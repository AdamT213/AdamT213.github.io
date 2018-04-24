---
layout: post
title:      "CS 100.7: Dynamic Programming and Memoization"
date:       2018-04-24 21:27:20 +0000
permalink:  cs_100_7_dynamic_programming_and_memoization
---


Today, we will briefly consider a few distinct but closely related concepts in algorithm design: dynamic programming, memoization, and the "greedy algorithm". Another closely related concept is caching, which I will not go into detail about, but encourage you to study up on. The following material assumes that you have some familiarity with recursion, which I have not gone over, but assume you have seen at least a few times. The idea of recursion is to divide a problem into a series of subproblems and solve each one. It can be referred to as "divide-and-conquer" or "top-down", and merge sort and quicksort, which I discussed previously, are examples of recursive algorithms. Right then, let's consider an example to illustrate why dynamic programming is useful. 

The textbook example for considering a dynamic progamming algorithm consists of writing a function that computes the nth Fibonacci number (if you are not familiar with the Fibonacci sequence, I encourage you to look it up, it's one of the coolest examples of math in the real world. See, for example, how Fibonacci numbers can be used to convert from miles to kilometers here http://www.catonmat.net/blog/using-fibonacci-numbers-to-convert-from-miles-to-kilometers/. Fibonacci numbers also pop up in numerous facets of algorithm design and computer science). Below is a recursive algorithm that does just that. 

function fib(n) { 
    if (n == 1 || n == 2) { 
        return n 
    } 
    else { 
        return fib(n-1) + fib(n-2) 
    } 
} 

The above is valid JavaScript. Try it in your browser's js console. It works. But try computing, say, fib(100), and watch your console sit... and sit... and sit. This algorithm is highly inefficient, and will take an enormous amount of time to compute large values. The problem is that in computing fib(100), you must compute fib(99), and fib(98). fib(99) relies on fib(98) and fib(97), creating a binary tree that will evidently become very large, very fast. Just imagine how many times we would have to compute fib(2)! Clearly, we are doing too much repeated work. Consider, instead, a DP algorithm that does the same thing, much, much faster: 

function fib(n)
{
  let a = 0;
	let b = 1;
	let c;
	let i;
  
	if( n == 0) {
    return a;
  }
	
	for (i = 2; i <= n; i++){
     c = a + b;
     a = b;
     b = c;
  }
  return b;
} 

This, too, is valid JavaScript. Now try fib(100). Much better! Dynamic programming, then, is a technique for storing repeatedly calculated values (so are memoization, greedy algorithms, and caching, but we'll get into the finer points momentarily). The principle is that we make a small tradeoff in space complexity for a substantial gain in time complexity, though incidentally, the above algorithm is optimized for space and time, and takes O(n) time and O(1) space.  

Dynamic programming can be thought of as the complement to recursion. If recursion works from the "top down", DP works from the "bottom up". In other words, in recursion, we solve a problem by checking a table for the values of any sub-problems involved in computing the new problem. If there are none, we add the new problem's solution to the table. In dynamic programming, we calculate the values of sub-problems, then store them in a table to use when solving larger problems. In this sense, you could say that we use DP for storing values, and recursion for retrieving values. Both topics rely on the idea of overlapping sub-problems, i.e. the idea that the algorithm repeatedly solves the same sub-problems over and over, rather than working with new ones. Recursion can be used even when sub-problems are non-overlapping, in which case it is divide-and-conquer, ala merge sort and quicksort, but DP cannot. 

What we are discussing here are very nuanced and subtle differences, so don't be discouraged if it doesn't all click right away (it doesn't totally click for me yet). DP and recursion are two of the most complex concepts for new programmers to grasp, and it takes many examples before they start to make intuitive sense. Dynamic programming problems can be incredibly complex, but they generally follow a pattern of dealing with optimization. The way to make change for a given monetary amount with the smallest number of coins, the fastest way to traverse between vertices on a graph, these are the types of problems that dynamic programming was made to solve. Now, for some more extremely nuanced definitions! 

Memoization is, in practice, almost synonymous with DP, but pedantic people will emphasize their differences, so it's good to understand them. Memoization refers to the actual process of retrieving stored values. Technically, this process is used in top-down recursion, wherein the memoized results are returned to solve the top problem, recursing all the way down as needed. In bottom-up DP, tabulation is used, wherein the sub-problems are solved and stored one by one, all the way up. In this sense, memoization is to top-down recursion as tabulation is to bottom-up DP, but in practice, most people use memoization to refer to both concepts. It is important to understand that DP by definition solves all related problems to a given problem, because it starts at the bottom and computes every value on the way up. Conversely, top-down recursion already has access to the memoized values, so it only retrieves the ones needed to solve the specific problem at hand. Intuitively, this would make the top-down approach faster, and it can be, but for many problems (like the optimization problems previously discussed), DP is in fact faster, even though it requires more steps. No one will fault you for getting these terms confused, what is important is learning how to write algorithms that properly apply these concepts, and that takes ALOT of determination and practice.

Now for one last concept, slightly more distinct from the aforementioned. Whereas DP and memoization find the *globally optimal* solution to a problem, greedy algorithms find a *locally optimal* solution at every step. In other words, they pick the best solution to every sub-problem, but do not necessarily (or often) arrive at an optimal solution to the top problem. They are used on problems of higher computational complexity, wherein an optimal algorithm would require too many steps to be feasible. For a good example, check it this algorithm on my github page https://github.com/AdamT213/highestProductOfThree/blob/master/index.js, which finds the highest product of three integers in an array of integers (don't be discouraged if you don't think you could come up with a solution like this-- I didn't! I worked through this problem for hours and finally did little more than translate this solution from it's original Python. P.s. - I find that to be a good way of internalizing algorithm knowledge: If you can't come to an optimal or near-optimal solution to a problem, look it up, then translate it from the language it's written in to one you are familiar with. This has several benefits: a) it gives you some familiarity with valuable system-level languages like Java and C, b) it forces you to understand the inner-workings of the problem and solution code, and c) it's like punishment for not working hard enough to solve the problem in the first place!). 

Basically, this code works by iterating over the array once, keeping track of several values at each iteration, namely, the maximum product of three elements, the minimum product of three elements, the maximum product of two elements, the minimum produt of two elements, the maximum element, and the minimum element. It then compares these to the new value for each one calculated at each step, and determines if the new value or old value should be saved. This is "greedy" not "dynamic", i.e. we are still making new calculations at every step, and are simply comparing them to stored values.  

Again, don't be discouraged if these concepts seem tricky at first. THEY ARE! Just think, college CS students get like 45% on their exams and end up with an A in the class! Keep working through problems and all of this will eventually make sense. 

Today's words of wisdom come to us from Antoine de Saint-Exupery:

"If you want to build a ship, don't drum up people to collect wood and don't assign them tasks and work, but rather teach them to long for the endless immensity of the sea" 

Til next time!
