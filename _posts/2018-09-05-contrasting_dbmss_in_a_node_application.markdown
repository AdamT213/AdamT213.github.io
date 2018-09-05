---
layout: post
title:      "Contrasting DBMSs in a Node Application"
date:       2018-09-05 20:15:26 +0000
permalink:  contrasting_dbmss_in_a_node_application
---


Marcos Henrique da Silva wrote a very good and informative post about creating secure REST APIs in Node/Express, which can be found [here](https://www.toptal.com/nodejs/secure-rest-api-in-nodejs). In it, he described many best practices for securing the application, including implementing JWT authorization and setting user permission levels (this part, in particular, was fascinating to me, and I will definitely be referring back to it in the future). However, in his simple tutorial, he also made  a common choice that many Node developers make: he included a NoSQL database, specifically MongoDB. Now, we have all heard of the MEAN stack and the MERN stack, so we know that using Mongo with Node is a common pattern. But, we are all, too, aware that it is not the only option. Today, I will show you a modified version of his code, replacing MongoDB with PostgreSql, implemented with some common Express middlewares, Bookshelf.js and Knex.js. But first, to motivate the discussion, I will explain the difference between NoSqL and SQL databases, and the cases where a developer might prefer one over the other. 

So, without further ado: What is a NoSQL database? In order to understand what a NoSQL database is, we will need to understand what a SQL database is, since NoSQL essentially developed as a response to SQL, and more or less means "everything that isn't SQL." So, then, what is a SQL database? As you may have guessed from the preceding sentence, SQL databases (also known as "relational databases", or RDBMSs) developed before NoSQL databases, and they are the more traditional, more established way of doing things. Essentially, they are tabular, meaning your database is stored as a giant table with a collection of columns. In this sense, they are "relational", since the relationships between the columns in the database are well-defined, and it is easy to locate and categorize data based on its relationship to other data. Obviously, they also make use of SQL (or some other SQL-like language), which stands for *structured query language*, and provides a robust way to organize and collect data that is stored in a relational fashion. 

Now that we have some sense of what a SQL database is, we can compare it to NoSQL. There are several broad kinds of NoSQL databases. The first kind we will discuss is a document-oriented database, of which MongoDB is an example. In a document database, each stored key pairs to something called a document, which can contain more key-value pairs, key-array pairs, and even more documents. In this sense, we can think of items stored in document databases as being sort of like JavaScript objects, and indeed, a MongoDB database can be described as looking like a giant JSON-like structure. 

Next, we have graph stores, which are good for storing data that is arranged in a network. There are also key-value stores, which are, in some sense, a simpler version of document databases, where each key is matched to a value. There are also wide-column stores, which are mainly used to query large datasets, and optimize for this by storing all similar data in columns with no rows. 

So, now that we understand the basic differences between SQL and NoSQL, the all-important question: why use one over the other? Of course, NoSQL is newer than SQL, and in tech, there tends to be a perception that newer is better. Of course, there is some truth to this, since newer solutions arise to address problems with the older ways of doing things. However, not all solutions are created equal, and solving one problem often creates ten more. So, how does this debate play out when it comes to NoSQL vs. SQL? 

Those in camp MongoDB would tell you that it is superior for several reasons. First and foremost, they believe it allows for faster iteration and better collaboration among teams. Non-relational databases have what is known as a dynamic schema, meaning that you do not need to define the schema before adding data. This allows you to iterate more rapidly, because adding new attributes to the database does not require a complete overhaul and migration. This puts less pressure on carefully planning how the data will be organized, and allows you to begin coding more quickly. This essentially means that SQL databases are better suited to the waterfall development approach, in which each stage of a project is carefully planned and executed in a strict sequence, whereas, NoSQL databases are more suited to the Agile approach, where the requirements of the project are more fluid, and each step can be evaluated along the way, allowing the team to change course more easily in light of unforseen circumstances. Likewise, Mongo aficionados claim it scales better. Since SQL databases need to be organized on a single server to maintain the network of relationships, storing large amounts of data can get very expensive, and the solution is a complex process of spreading across multiple servers and making them look like a single server. NoSQL does not have this problem, so its proponents argue that it allows teams to spend less time and energy managing their data.  

With all of these benefits, you may wonder why I am even discussing an alternative to Mongo and NoSQL. However, it is worth menthioning that SQL databases still have a large place in the industry(Apple, for example, uses Postgres), so surely, there must be some good reasons why all of these developers opt to stick with the traditional way, right? In fact, there are. For one thing, PostgreSQL (the RDBMS I will implement) is fully ACID-compliant, whereas MongoDB is not. I won't go into the details of the acronym, but essentially, this means that all data inserted into a Postgres db is rigorously checked for validity, and there is no way around this, allowing Postgres users to rightly claim that it is more reliable than other dbms. Likewise (while there is plenty of controversy), many benchmarks indicate that PostgreSQL can be faster at making queries than MongoDB. 

The bottom line is that these are high-level, theoretical debates that will not have a strong effect on most small applications. Both SQL and NoSQL are clearly good options, I am only trying to point out that Mongo is far from the only option for a Node app. With that, let's look at how the application Mr. da Silva built would look different if it had instead been implemented with Postgres. 

First, we would need to define our Users model. All operations related to the models would be handled by Bookshelf.js, which is built on top of Knex.js so we would first *npm install --save knex && npm install --save bookshelf*. Then in *app/db,* we would add a file called bookshelf.js, where we would initialize the schema like so: 

```
const config  = require('../../knexfile.js');
const knex = require('knex')(config[process.env.NODE_ENV]);
const bookshelf = require('bookshelf')(knex);

bookshelf.plugin('registry');

module.exports = bookshelf;  
``` 

We make reference here to a knexfile, which we would define in app, and which might look something like this: 

```
require('dotenv').config()

module.exports = {

  testing: {
    client: 'postgresql',
    connection: {
      database: 'dbName'
    },
    pool: {
      min:2,
      max: 10
    },
    migrations: {
      tableName: 'knex_migrations'
    }
  },

  development: {
    client: 'postgresql',
    connection: {
      database: 'dbName',
    },
    pool: {
      min: 2,
      max: 10
    },
    migrations: {
      tableName: 'knex_migrations'
    }
  }, 

  production: { 
    client: 'postgresql', 
    connection: process.env.DATABASE_URL,
    pool: {
      min: 2,
      max: 10
    },
    migrations: {
      tableName: 'knex_migrations'
    }
  }
}; 
  
	This takes care of describing our various database connections.  Then, in app/db/models/user, we would define the relationships for our actual user model. In this simple example, something like this would suffice: 
	
	"use strict";
	
 const bookshelf = require('../db/bookshelf'); 

const User = bookshelf.Model.extend({
  tableName: 'users',
  initialize: function() {
  },
  hasTimestamps: true,
});

module.exports = bookshelf.model('users', User);  
``` 

Here we have referenced a table called *users*, so we will need to actually create this table. Knex allows us to build migrations from the command line, so we could type knex migrate:make generate_users. Then, in the files generated in the migrations directory, we could add some code like this: 

```
exports.up = function(knex, Promise) {
  return Promise.all([
    knex.schema.createTable('users', (tbl) => {
      tbl.increments('id').primary();
      tbl.string('name'); 
			tbl.string('password')
      tbl.timestamps();
    }),
  ]);
};

exports.down = function(knex, Promise) {
  return knex.schema
    .dropTable('users')
};  
```

Now, we have everything we need to begin making routes at our user endpoints. For example, our Post /users route to create a new user could look like this: 

```
router.post('/users', urlencodedParser, jsonParser, (req, res) => {
  if(_.isEmpty(req.body))
    return res.sendStatus(400);
  User
    .forge(req.body)
    .save()
    .then((user) => {
      res.json({id: user.id});
    })
    .catch((error) => {
      console.error(error);
      return res.sendStatus(500);
    });
});  
``` 

You will notice how similar this code is to the route in da Silva's post. Working with Bookshelf and Knex gives SQL a very NoSQL-like feel, even though the underlying mechanics are SQL. Again, the point of this post is just to illustrate that MongoDB is not the only option for working with Node/Express apps, ubiquitous as it may seem. I am not trying to argue that one is better than the other, this debate has already been had many times, and there is no clear winner. I am merely trying to argue that, if you are already familiar with SQL, you do not NEED to go out and learn NoSQL, thinking that it is the only option for developing Node/Express applications. With that, thanks for reading!
	

