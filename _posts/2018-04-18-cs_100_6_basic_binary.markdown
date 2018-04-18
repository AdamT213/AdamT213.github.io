---
layout: post
title:      "CS 100.6: Basic Binary"
date:       2018-04-18 18:53:14 +0000
permalink:  cs_100_6_basic_binary
---


Today, I will introduce the concept of the binary numeral system on which computers rely. Please note that this is a VERY basic introduction to binary numbers, their purpose and uses, and the operations that can be performed on/with them. Until very recently, I had almost no experience working with binary, and my knowledge is still a work in progress. Bit manipulations such as bit vectors etc. are complex topics, and as a junior engineer, you will need to have only a very surface-level understanding of bits and the like. That said, I encourage you to study further on the topics I introduce today, since understanding how computers work at the lowest level can only help you as a programmer. Okay, that aside, let's get cracking. 

As a coder, you likely have some concept of what binary is, and that idea that everything a computer does is based on a series of 1's and 0's. You may be familiar with 32 and 64 bit operating systems, and you likely know that there are (generally) 8 bits in a byte, 1,000,000 bytes in a megabyte, and 1024 megabytes in a gigabyte. But why are these values what they are? 

Binary refers to the base2 numeral system. In our everyday lives, we deal with the base10 numeral system, wherein every number from negative infinity to infinity can be represented as a series of 10 digits (0,1,2,3,4,5,6,7,8,9), and, reading a number left to right, each place is a power of 10 (100's place, 10's place, 1's place, etc.). From this understanding, it is very easy to move to base2, wherein every integer can be represented as a series of 2 digits (0 &1), and reading left to right, every place (or bit) is a power of 2 (the 16's bit, the 8's bit, the 4's bit, the 2's bit, and the 1's bit, assuming a 5-bit unsigned integer). Today, I will show all examples using only 8-bit unsigned integers, because 8 is sufficient for the purposes of demonstrating how basic bitwise operations work, and because using signed and negative integers complicates things immensely. That said, once you are comfortable with very basic binary, I encourage you to study up on the representations of signed and negative integers, and particularly the two's-complement method of representing them, which is nearly ubiquitous in modern computers and programming languages. 

Okay, so on to representing 8-bit unsigned integers in binary. It's really pretty simple, once you get the hang of the pattern. 1 is 00000001, i.e., the 1's bit turned "on" (1 always means on, 0 always means off), and all other bits turned off. 2 looks like 00000010 (the 2's bit turned on, all other bits turned off). With this pattern, we can represent 27 as 00011011 and 135 as 10000111. Great. Also note that, from these examples, it is easy to see why, with n bits, we can represent a max unsigned integer of 2^n -1, in this case, 255 (128+64+32+16+8+4+2+1). 

Also note that, when dealing with unsigned positive integers, it is very easy to convert from binary to base10 and vice versa. The javascript implementation for both is below. This code becomes considerably more complicated with signs/when representing negatives. 

function convertNumberToBin(num) { 
    return Number(num).toString(2) 
} 

function convertBinaryStringToNumber(bin) { 
    return parseInt(bin, 2) 
} 

Okay, now let's tackle the basic bitwise operations. In any programming language, there are only a few different operations that can be performed at the bit level: | (or), & (and), ^ (Xor), ~ (not), << (left shift), >> (right shift), and >>> (logical right shift). The first few behave almost the same way they do when used on regular data. | returns 1 (true) if any data passed to it is true. For example 0 | 1 returns 1, and 0 | 0 returns 0. & returns 1 (true) if all data passed to it is true. So 1 & 1 returns 1, 1 & 0 returns 0. Xor means exclusive or, and it works by figuring out if the bits passed to it are different. So, 1 ^ 1 returns 0, 0 ^ 0 returns 0, and 1 ^ 0 returns 1. ~ works like the bang operator, negating the value of a bit. ~1 is 0 and ~0 is 1. 

The shift operators are used for many bit manipulations, as we will see in a moment. << shifts a bit a to left a specified number of positions, and pads 0's on the right to make up for the loss. So, 9 << 2 becomes 36 (left shifting 2 is equivalent to multiplying by 2, twice), like so 00001001 ~> 
00100100.  Right shift shifts the bit right a specified number of positions, and bits shifted off the right end are discarded. So, 9 >> 2 yields 2 (right shifting by 2 is equivalent to dividing by 2, twice, then rounding towards negative infinity), like so 00001001 ~> 00000010. Note that these are known as arithmetic shifts, meaning that the bits shifted off either end are discarded. There are also logical shifts, wherein the discarded bits are replaced with 0's. This happens already in arithmetic left shift, leaving no need for a logical left shift, but there is a need for a logical right shift. This has no effect on positive numbers, so 9 <<< 2 still yields 2, like so: 000010001 ~> 00000010. For signed negative ints this is not the case, but we will not go into detail on that at this time. Just note that >>> is better for unsigned binary numbers. 

Okay, now let's see what these operators allow us to do. Let's say we have a binary input string, and we want to set a given bit to true. We can use the shift operators to create what is called a bit mask. The javaScript implementation for this is below. 

function setBit(x, position) { 
    let mask = 1 << position 
    return x | mask 
}  

The code works like this. Say x is a binary string like 0000110 (6), and you want to set the bit in position 5 to true. You would pass in a binary string equaling 5 as position (0000101), and since the mask shifts 1 to this position, it would look like 00100000. X or mask then does  a bit by bit comparison, like so, 

0000110 | 
0010000  

resulting in 0010110 and successfully setting the bit in position 5. 

Clearing a bit that is set works similarly. 

function clearBit(x, position) { 
    let mask = 1 << position 
		return x & ~mask 
} 

The mask is created in the same way, with a 1 in the position you want to manipulate, and 0's everywhere else. You then take ~mask, which has a 0 in the position you want to manipulate and 1's everywhere else, and & it with x. Since 1 & 0 is 0, this clears the bit.  

Flipping a bit is even simpler, the javaScript looks like this: 

function flipBit(x, position) { 
    let mask = 1 << position 
		return x ^ mask 
} 

Here, we set the bit in the position, then return 1 for all the bits that are different in mask and x, successfully flipping the bit at position x, regardless of its initial value. 

Beyond these basic manipulations, I will show you how to add binary strings. For positive unsigned ints, this is fairly simple, and works much like base10 addition. 

00001100 (12) + 
00100001 (33) = 
00101101 (45) 

You simply add up each column. If they were both 1's, you would put a 0 and carry the 1, just like base10.

Okay, that is a fair introduction to binary and bits. Keep learning and practicing with them, they never go away. 

Today's words of wisdom are of a slightly different vein, and they come to us from the legendary Hunter Thompson: 

"Life should not be a journey to the grave with the intention of arriving safely in a pretty and well preserved body, but rather to skid in broadside in a cloud of smoke, thoroughly used up, totally worn out, and loudly proclaiming "Wow! What a Ride!” 

Til next time!


    



