---
layout: post
title:      "Mapping State to Props"
date:       2018-03-08 22:03:52 +0000
permalink:  mapping_state_to_props
---


    The functions map state to props and map dispatch to props are undoubtedly two of the trickiest and most important concepts for nascent React developers. It is fair to say that you don't truly "get" using React with the Redux pattern until you fully grasp the concept of mapStateToProps(). Personally,  I did not understand the power and simplistic beauty of this function until I had to use in my first React/Redux project - again, and again and again. In fact, it comes up so often in React/Redux, that any aspiring developer should commit to learning its ins, outs, and uses, ASAP. To that end, I offer this quick guide. 
    Think of mapStateToProps() as a bridge between components. Whenever you need a piece of state within a component that it is not native to that component, mapStateToProps()! This way, the component can remain stateless, as is the preference in React, but has access to any properties from the state that it may need. As an example, in my recent project, I made the user id a piece of the state, effectively managing the current session from the front end in this way. However, when creating resources via forms from the client, it became necessary to access this user id, since it was required by the back end to be sent with the form data. So what did I do? Inside my input components, I mapped state to props. This gave me access to the user id I needed, while leaving the components without control of any state.  
		The bottom line is, whenever you are in a a bind early on in your React career, and cannot figure out how to gain access to a property that you need, there is a good chance the answer lies in mapping state to props. Learn this function, practice it, understand its use cases, and you will greatly expedite your learning curve as a React developer.

