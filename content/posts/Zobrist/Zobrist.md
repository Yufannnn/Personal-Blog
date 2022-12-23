---
weight: 1
title: "Unexpected Use of Zobrist Hashing :eyes:"
date: 2022-10-17
lastmod: 2022-10-17
draft: false
author: "Zhu Yufan"
description: "A surprising place to use Zobrist Hashing, which is designed for board games"
featuredImage: "Zobrist.jpg"
featuredImagePreview: "Zobrist.jpg"

tags: ["computer science", "curriculum"]
categories: ["Academia"]

lightgallery: true
linkToMarkdown: false

toc:
  auto: false
---
This post is kind of a following up to the previous post. I found a surprising question where Zobrist hashing is applicable. Alert: this post is going to be a bit technical!
<!--more-->

This post is kind of a following up to the previous post. In the previous post, I wrote about the AI agent I constructed for the CS2109S individual project to play the board game, Breakthrough. There is, however, one feature I didn't manage to implement, Zobrist Hashing, which supposedly can dramatically reduce the runtime and space complexity. I have read about the Zobrist Hashing Algorithm but decided to not actually implement it due to lack of time. Instead, I just used a very naive approach: represent the board using a string and directly hash the string representation of the board, which runs in O(n^2) as we have to go through the n * n every time to figure out its string representation.

{{< admonition type=Note title="What is Zobrist Hashing?" open=false >}}
Zobrist hashing (also referred to as Zobrist keys or Zobrist signatures) is a hash function construction used in computer programs that play abstract board games, such as chess and Go, to implement transposition tables, a special kind of hash table that is indexed by a board position and used to avoid analyzing the same position more than once.
{{< /admonition >}}

### My Initial stereotype about Zobrist Hashing
Upon reading Zobrist Hashing, I initially thought that this hashing algorithm is only dedicated to hashing for board games. I did not see how it could be used elsewhere as it seems to be so specifically designed for hashing board games.

### A New Quest
Several weeks later, the professor from another module that I concurrently took, CS3230 ***Design and Analysis of Algorithm***, released a programming assignment. Here is the question of the assignment. Given an integer array of size n, find the number of balanced subarrays and output the number. An array is defined to be balanced if all the numbers in that array occur an even number of times (zero times included). For example, if the given array is [1, 1, 2, 3, 3, 2], the output should be 2 as we have subarray [1, 1] and [1, 1, 2, 3, 3, 2] being balanced. The algorithm should run in O(n^2) time complexity. 

### The Solution 1.0
It didn't take me long to figure out a naive version of the solution. We can represent the whole configuration using a very long bit string, as long as the maximum number of the given array and set the bit string to be all zeros. Let the ith bit of the bit string represents the parity of number i in the array, 0 for even parity and 1 for odd parity. Every time we encounter a number, we just toggle the corresponding bit in the bit string to indicate a flip in parity. After changing the bit string for each number, we hash the current bit string and record the number for each bit representation. We can easily see that if we have two exactly the same bit strings, then the subarray in between them should be balanced. Therefore, after we loop through the whole array and get the number for each bit string, we simply have to find the summation of each count choose two. This is because by the argument before, we notice that for each pair of the same bit string the subarray between them is always balanced. Therefore, if we will find the total number of balanced subarrays, we need to calculate the number of distinct pairs for each bitstring and sum them up.

### The Problem
Seems to be pretty elegant, right? However, the problem is that the number in the given array can be extremely large, say 50,000. If we store the bitstring representation in the form of a series of long-type numbers, it means 391 long-type numbers are needed to store the representation! Since each long can represent 128 bits. Therefore, the space and time needed to hash all these values are going to be tremendous. Therefore, we need to figure out an even smarter solution.

### The Solution 2.0
As I only did the AI project one or two weeks ago, when sat down and reconsider the problem, I thought, "Wait a minute, isn't this question a little bit similar to a board game". The essence of Zobrist Hashing is that in a board game, the board representation only changes minimally for every step, namely the move of a pawn. Therefore, we use the exclusive OR operation on the hash value of the previous step once to indicate the change to avoid repeated computation. The current problem shows much resemblance to the situation described above. When we loop through one element in the array, we only flip one bit of the bitstring, which is similar to the movement of a single pawn. Also, we don't care about the order or sequence of each bitstring representation; we only care about their counts. After I thought all these through, I then convinced myself that maybe Zobrist Hashing can actually be used in some other situations.

### The Implementation
Instead of using a 391 longs combination as the representation. This time I long use a single long as the representation. We randomly initialize it first to avoid collision then find the largest element in the array and assign each number to a random long. The algorithm is rather straightforward, we simply loop through the array. When we encounter a number, we XOR it with our long representation and then hash the long representation, which is almost the same as the original version of Zobrist Hashing.

### The Improvement
The improvement is unbelievable; the naive solution needs more than 20 seconds to solve an input of scale 50,000 while the Zobrist version only uses less than 3 seconds !!! I was wondering if I did not do the AI project several weeks ago, how would I even be able to link this problem with Zobrist hashing and come up with this elegant solution? It just struck me how amazing and flexible algorithms can be when it comes to problem-solving.

### To Be Continued ...
Lastly, after studying CS3230, I do wish to formally prove the runtime and the correctness of the algorithm but this post is already running a bit too long and too technical. Maybe, I will come back sometime and write a new post with the proof in the future!
 


