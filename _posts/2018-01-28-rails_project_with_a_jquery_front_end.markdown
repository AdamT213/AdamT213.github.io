---
layout: post
title:      "Rails Project with a Jquery Front End"
date:       2018-01-28 23:11:03 +0000
permalink:  rails_project_with_a_jquery_front_end
---


    Built on top of my original Rails Fitness Tracker, this app essentially reworks some features to include a jquery front end, including several resources that load without a page refresh. This provides a more user-friendly and professional looking UI, and improves the app's overall performance. Because much of the creative and design-oriented heavy lifting had already been taken care of, this iteration of project mode was all about architecting the jquery features, freeing me from thinking too much about other concerns. In this sense, it was the easiest project thus far to build, and I feel that I have come away with a thorough understanding of the process and benefits of using jquery. So, without further ado, here is a quick walkthrough. 
		On the homepage, a user sees the same list of workout routines form the original Rails Fitness Tracker, this time with a button instead of a link below each one. On click, the button appends the details of each routine to the DOM. On the back end, this required removing the Rails view template and having the workout routines show action instead route to an API with a JSON response. On the front end, this response is retreived using jquery and ajax, then converted to two javascript model objects, a workout and its corresponding exercises. A prototype for each of these objects takes care of formatting, and then they are appended to the DOM. 
		Much the same thing happens on the user's show page, where a  user's fitness plans can be appended without a page refresh as well. Additionally, the form to create a workout routine submits a jquery post request, appending the newly created routine directly to the page. Other than that, the original structure and integrity of the Rails Fitness Tracker remains intact. 
		Working on this project, it was cool to see how much simpler and more elegant this new resource (jquery) could make an existing application. The process of creating and refactoring is ceaseless in programming, so it is valuable to get a sense of just how far a small refactoring in application can go. 
