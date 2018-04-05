---
layout: post
title:      "CS 100.3: Linked Lists"
date:       2018-04-05 06:11:31 +0000
permalink:  cs_100_3_linked_lists
---


Without further ado, the much hyped and heretofore undelivered post about linked lists! Linked lists are one of the most fundamental and important data structures in computer science, and rest assured, if you are interviewing for a position as a programmer, you will be expected to know how to implement them, work with them, and describe some facts about them. But first, why use a linked list? 

As a novice programmer, your inclination whenever you are asked to represent data in a list is to run to the standby, old faithful, the array. And rightly so. Arrays are great... for some applications. Consider, however, the cost of adding an element to an array. If you add the element at the end, this is an O(1) operation. Simply find the last element, then add another one. Splendid. What if, however, you want to add an element at some location other than the final index? You must locate the index, replace the element, and push every subsequent element back one index, an O(n) operation. This, then, is where linked lists come to the rescue. 

A linked list is, as its simple name implies, a list of elements where each element "links" to the next element in the list, via a pointer. There are different implementations of linked lists, but the general idea is that each "node" in the list stores its own data, as well as a pointer to the next node. Linked lists can be singly linked, in which case they work exactly as described herein, or doubly linked, meaning that they also store a pointer to the previous node in the list. They almost always have a head pointer, i.e., a pointer to the first node in the list, and may optionally have a tail pointer, i.e., a pointer to the last node. Unlike arrays, adding and deleting elements at the front is a simple O(1) operation, the pseudocode for which, looks like this (assuming a doubly linked list). 

deletenode(node) { 
	 node.prev.next = node.next
}  

The other main draw of linked lists as opposed to arrays is that, while arrays have fixed length, requiring you to create a new array, copy all of the data from the original array, and add the new data every time the array's length is exceeded, linked lists can grow boundlessly, until the computer's memory is filled. Generally speaking, linked lists support adding and removing elements at the front, adding and removing elements at the end, adding and removing elements before and after a given element, finding the first element, finding the last element, finding any element, and checking whether the list is empty. 

Adding, finding, and deleting an element at the front are always O(1) operations, and the pseudocode for adding an element to the front of a singly list looks like this: 

pushFront(key) {  
   if head == null ; error: empty list
   key.next = head.next 
	 head.next = key 
} 

Adding and finding an element at the end are O(n) operations assuming no tail pointer is present, and O(1) if there is a tail pointer. In singly linked lists, removing the last element is O(n) with or without a tail pointer, since one must iterate over the list until the second to last element is reached in order to set its pointer to null. This can be cut down to O(1) in a doubly linked list with a tail pointer, since tail.prev.next can simply be set to null. The pseudocode for removing the last element in a singly linked list looks like this 

popBack() { 
   if head == null ; error: empty list
    node = head
    while node.next.next != tail 
        node = node.next  
    node.next = null 
		tail = node 
} 

Finding a given element in any linked list, without a pointer to that element, is O(n), requiring iteration to that node in the list. Adding an element after a given element is O(1), and adding before an element is O(n) in a singly linked list and O(1) in a doubly linked list, for much the same reason as this is true for removing the final element. Erasing a given element without a pointer to that element is O(n) in any linked list, and checking whether the list is empty is a simple O(1) operation, i.e., checking whether the head pointer is null.  

Of particular interest is the point about finding a given node in a linked list. In an array, finding an element is an O(1) operation, simply a  matter of looking to the proper index. In a linked list, this becomes O(n), so in this sense arrays perform better. In fact, this dichotomy between arrays and linked lists, the idea that arrays are better at finding elements, and linked lists are better at adding and removing them, is the central point of analysis when considering which to use. Generally, use an array when elements will need to be located frequently, and a linked list when elements will be frequently added and removed from the front or back. For this reason, linked lists are a common implementation for stacks and queues, which were discussed in a previous blog post. They are also used when handling collisions in a hash table, which will be covered in a future post. 

One final note about linked lists: When practicing with them, use singly linked lists for the most part. If you can implement and work with a singly linked list, then you can implement and work with a doubly linked list, but the opposite cannot necessarily be assumed. 

Thanks for reading, and stay tuned for more introductory CS jargon. Today's words of wisdom come to us all the way from the Bhagavad Gita -  
"In battle, in the forest, at the precipice in the mountains, On the dark great sea, in the midst of javelins and arrows, In sleep, in confusion, in the depths of shame, The good deeds a man has done before defend him" 




    
