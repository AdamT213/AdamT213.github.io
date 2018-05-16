---
layout: post
title:      "CS 100.4: Sorting Algorithms"
date:       2018-04-11 00:46:09 -0400
permalink:  cs_100_4_sorting_algorithms
---


Welcome back aspiring engineers! Today, I will tell you about a few common algorithms for sorting lists. You have, undoubtedly in your time as programmers, come across instances where it helped to have items in a list appear in sorted order.  Whether finding the first instance of a user alphabetically, finding a user's first pet's name alphabetically, or for countless other applications, you have probably used your language of choice's array.sort() method and thought little of it. That is fine, but in moving from programming small, personal apps to large, data-intensive, team-oriented software, it helps to have an understanding of how these methods are built and the common practices in sorting. Moreoever, sorting data is one of the most important and fundamental exercises in computer science, and taking the time to understand its best practices is a good review of topics that will come up again and again when solving cs problems. Plus, if asked an interview question in which having a sorted list would help in some way (such as to implement binary search), you should be able to demonstrate that you know how to sort a list without a specific language's built-in method, and that you have a good understanding of the tradeoffs between different algorithms. 

The algorithms I will cover today are insertion sort, selection sort, merge sort, and quicksort. There are others about which you should at least be conversant, such as bubble sort, heap sort, bucket sort, and radix sort, but the  four covered here are perhaps the most common, and form a solid introduction. The key take away is that some sorting algorithms are bad (in terms of time/space complexity, of course), but are important to know because they help reveal truths about the better ones, and because they are sometimes acceptable to use. All the algorithms I will discuss can be applied to arrays or linked lists, but some have advantages for one data structure vs. another. I will discuss the algorithms for arrays specifically. So with that, let's talk about selection sort.  

Selection sort is the most basic sorting algorithm, and it is, essentially, how a naive person would sort a list if asked to. 
Essentially, selection sort keeps a pointer to the first unsorted element in the list, and runs through the list, looking for the min element. If the min element is not the first element, they are swapped, and this process repeats until the end of the list is reached. Notice I said, "the first element in the unsorted list." Selection sort maintains two lists, one sorted, one unsorted. However, it can be implemented "in place" for arrays, i.e., without extra memory, because to begin with, the sorted list is considered to be length 0, and the unsorted list is the whole thing. Each time a swap is made, the sorted list grows by one, and the unsorted list shrinks by one. Obviously, finding each min element is a tedious O(n) process, which is our first hint that selection sort is not a very efficient algorithm. In fact, the total algorithm runs in O(n^2) time in the best and worst case.  It will be easier to understand how this works given a short example, so let's say we had the list 9,2,7,8. We walk through the unsorted list to find the min, which is 2, so we swap this with the first element, 9. This leaves the sorted list as 2 and the unsorted list as 9,7,8. We then walk through the unsorted list again, finding the min, 7, and swapping it with the first element, 9, leaving 2,7,9,8. The unsorted list now contains 8 and 9, so we swap them to produce the final list, 2,7,8,9. The pseudocode for selection sort is below. 
```
function selectionSort(array, length)  
		for i = 1 to  length -1
		    min = i 
				for j = i + 1 to length 
				    if array[j] < array[min] 
				        min = j 
		    swap array[i], array[min] 
```
								 
The next algorithm we will discuss is also simple, and also BAD. It is called insertion sort, and it is conceptually similar to selection sort. Insertion sort also runs in O(n^2) time in the worst case, but can perform substantially better on lists that are almost sorted to begin with, in which case the time complexity is proportional to the number of inversions (i.e. elements that are out of order).  Also like selection sort, it maintains a sorted and an unsorted list, and can be implemented in place in much the same fashion. The first element is taken to be sorted, and rather than finding the min and swapping, as in selection sort, the next element is simply added to the sorted list in order. Let's go back to our 9,2,7,8 example to illustrate this. The 9 is taken to  be sorted, so we start with the 2. 2 comes before 9, so we move it to the beginning, leaving 2,9,7,8. 7 comes before 9 and after 2, so we move it over 1 space, leaving 2,7,9,8. 8 comes before 9 and after 7, so we move it one space. The key is that when inserting elements into the sorted list, we start from the end and move the element over until we find one that comes before it. The pseudocode for this procedure is below. 
```
function insertionSort(array, length) 
    for j = 2 to length 
		    key = j 
		    i = j-1 
				while i > 0 && array[i] > key 
				    array[i+1] = array[i] 
						i = i-1 
			 array[i+1] = key 
	```		 
Okay. That's enough inefficient sorting for one day. The main point is that these two algorithms are bad, but they are good to study because they mimic how the brain naturally thinks about sorting, and they help us to understand how and why the good algorithms are derived. Tomorrow, we will learn merge sort and quick sort, the two that you actually want to know and understand. But for now, I leave you with some words of wisdom from particle physicist Savas Dimopolous, from the documentary particle fever. Great movie, check it out sometime. 

"The key to success is moving from failure to failure with increased enthusiasm." 

I may have just butchered that quote. Great quote though. 

Til next time!


				 
    
