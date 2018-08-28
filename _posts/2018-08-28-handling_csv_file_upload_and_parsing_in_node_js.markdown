---
layout: post
title:      "Handling CSV File Upload and Parsing in Node.js"
date:       2018-08-28 06:23:26 +0000
permalink:  handling_csv_file_upload_and_parsing_in_node_js
---

In a recent node/express project, I found myself in a situation where I needed to allow users to upload csv files, parse them asynchronously, and save their contents (but not the files themselves) to the database. While I found many useful examples and tutorials for uploading and *storing* files, csv and not, I had a considerably tougher time finding any online resources on how to upload and *immediately* parse a csv file, all within the same route, asynchronously. This seemed strange, since it seemed like a relatively common scenario. As a result, I struggled with the tools I am about to explain for longer than I care to admit, but in the end, I was able to accomplish my task. I hope this guide saves you some of the time and headaches I had! 

This assumes that you have a node/express app, and that you are familiar with node/express. The tools we will be using to accomplish async csv parsing are multer and csvToJson. I will also assume that you are using express router and bodyparser in your app. Note: the process on the frontend for uploading a csv file is much simpler, so I won't go into in any great detail. My client app was built in React, and there are already some great tutorials on handling file upload in React. For example, the one I followed can be found [here](https://codeburst.io/asynchronous-file-upload-with-node-and-react-ea2ed47306dd). Of note, you can see that the formdata is an object that must have whatever fields you want sent appended to it, which you may not be used to doing in React. Here is my completed upload form: 

``` 
import React, { Component } from 'react';
import { connect } from 'react-redux'; 
import {
  BrowserRouter as Router,
  Route,
  Link,
  Redirect,
  withRouter
} from "react-router-dom";
import { UploadFile } from '../Actions/DistributorActions'; 
import { Distributor } from '../Components/Presentational/Distributor'


export class UploadInventory extends Component {
  
  constructor(props) {
    super(props)
    this.state = { 
      uploadStatus: false,
    }
    this.handleUpload = this.handleUpload.bind(this);
  }

  handleUpload = event => { 
    event.preventDefault();
    let data = new FormData();
    data.append('file', this.uploadInput.files[0]);
    this.props.UploadFile(data);   
    this.setState({
      uploadStatus: true,
    });
  }

  render() { 

    return(
      <div>
        <form onSubmit={this.handleUpload}>
          <div className="form-group"> 
            <input className="form-control"  ref={(ref) => { this.uploadInput = ref; }} type="file" />
          </div>
          <button className="btn btn-success" type>Upload</button>
        </form>
      </div>
    )
    }
  }
export default connect(null, { UploadFile })(UploadInventory); 
``` 

Nothing fancy, just a pretty standard React form that connects to a pretty standard redux action. 

Now, onto more interesting matters. BodyParser, the express standard for parsing incoming data, cannot be used to parse multipart/form-data (i.e. the encoding for a binary file). Therefore, you must use an engine specifically designed for this, the most popular one being multer. However, since bodyParser will by default be the first thing to parse incoming data, you must make sure that it is not enabled on any route that will use multer. So, instead of the usual, including bodyParser as a global middleware like this:

``` 
const app = express();
const router = express.Router();
app.use(bodyParser.urlencoded({extended: false}));
app.use(bodyParser.json()); 
``` 

You will need to do something more like this: 

```
...
const jsonParser = bodyParser.json()
const urlencodedParser = bodyParser.urlencoded({ extended: false }) 
```

then, on any route that processes data that isn't a file: 

```
app.post('/some_route_with_incoming_data', urlencodedParser, jsonParser, (req, res) => { 

   ...do stuff here 

} 
```

Now, once you  npm install --save multer, you can configure it like this: 

``` 
const multer  = require('multer');
const upload = multer({ dest: 'uploads/' }); 
``` 

In order to access the file inside the route, you will need to set a temporary destination in which to store it. Multer also accepts fileFilter, limits, and preservePath args, but for our simple example, dest will suffice. Also note that, if passed a string to dest, multer will create the folder for you if it does not exist. 

Okay, now to actually use it in a route. The code for doing so looks like this: 

```
app.post('/distributor/:id/upload', upload.single('file'), function (err, req, res, next ) { 
  
  if (err) { 
    console.log(req);
    console.log(req.file);
    console.error(err);
    return res.sendStatus(500);
  }
  next()
}, function (req, res, next) {  
``` 

Our multer instance can be passed* single*, as above, indicating that we expect a single file with the fieldname specified in our frontend code above, *array*, indicating that we expect an array of files all with the fieldname specified, 
*fields*, indicating that we expect a bunch of different files with different specified fieldnames, *none*, indicating that we expect only text field, or *any*, indicating that we will accept any mix, maxCount,  and type of files that may come. If we specify multer.single(), we will get the file stored in a req.file object. Otherwise, it will be stored in a req.files object. 

So that's it, super simple, and now we have the file in req.file, ready to be referenced in the next() route. 

Now, to actually parse the csv data, line by line. To do so, we will convert it to JSON data using csvToJson. So, after we npm install --save csvToJson, 

```  
const csv = require('csvtojson'); 

...

}, function (req, res, next) {  
    
    csv()
      .fromFile(req.file.path)
      .subscribe((json)=>{ 
        
        return new Promise((resolve,reject)=>{  
				
				do long async work, for example, saving each line as a record in the db 
} 
``` 

That's really it! CsvToJson can be passed a readable stream, or, as in this case, a file with csv data. The key is returning a promise so that each line can be processed asynchronously. Other than that, not too much to get hung up on. 

I hope this little guide was useful to you! The biggest tip is to only enable bodyParser as a route-specific middleware when some routes will use multer. If you don't, bodyParser will process the form data as a weird object-thing with no keys, and it will be very frustrating! 

Ok, with that, thanks for reading!


