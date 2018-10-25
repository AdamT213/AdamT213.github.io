---
layout: post
title:      "Calculating Elapsed Time in JavaScript"
date:       2018-10-25 22:32:53 +0000
permalink:  calculating_elapsed_time_in_javascript
---


Greetings folks! Just a short post today about an interesting problem I recently solved in node.js (for this post, only an understanding of vanilla.js will be required). In my poker head-up display app (the code for which can be found here https://github.com/AdamT213/hand-trackerAPI), I wanted to have the ability to update a session with the elapsed time since its creation. That is to say, I had created a session model, and initialized it with a duration attribute, which is null upon creation, and accepts an integer value. This integer is the number of minutes since the session started, and once I end the session, this value gets populated. Recently, I got to the point in the development process at which it was finally time to dive into the nitty-gritty of actually doing this. I believe that my experience herein is a good general lesson in working with js timestamps. so without further ado, here is the code: 

```
router.patch('/session/:id', (req,res) => {    
  Session
    .forge({id: req.params.id})
    .fetch()
    .then((session) => {
      //to get duration: get current time via Date.getTime, and convert created at timestamp to date.getTime format. Then subtract and divide by number of milliseconds in a minute
      return session.save({
        isTermed: true,
        duration: parseInt((new Date().getTime() - new Date(session.attributes.created_at.toString().replace(/-/g,'/')).getTime())/60000)
      })
    })
    .then((session) => {
      res.json(session);
    })
    .catch((error) => {
      console.error(error);
      return res.sendStatus(500);
    });
}); 
``` 

Unless you are familiar with Express and Bookshelf, you can safely ignore most of this, it simply defines a patch route to update the record, and I included it solely for context. The important part is the line below the comment, where I set the duration on the updated record. Basically, I am subtracting two timestamps: the timestamp from when the record was created, from the current time when the function is being called. The added complexity owes to the fact that, when working with JavaScript Date objects, the Date.getTime() method returns the number of milliseconds since 1/1/1970, as a fixnum. However, a created_timestamp as used in PostgreSQL is an ISO string. Therefore, in order to subtract them, the created_at timestamp must be converted to a form the JavaScript Date object can understand. The regex does just that, replacing the dashes with slashes, which can then be used to instantiate a Date object and perform the getTime operation on it. We can then subtract the two, and the number we are left with is the number of milliseconds between the instantiation and termination of the session. Since we want this value in minutes, we divide by 60,000 milliseconds/minute. Since this gives a float, and our database will only accept ints, we parse an int from this value. Voila, we did it! Duration attribute on session model: set. 

Thanks for reading!
