# Unexpected Use of Zobrist Hashing :eyes:

 I'm excited to share a surprising discovery I made while working on a programming assignment for the CS3230 module, **Design and Analysis of Algorithms**, at NUS. This revelation involves the unexpected application of Zobrist Hashing, a technique primarily designed for board games, to efficiently solve a challenging problem unrelated to gaming. Get ready for a bit of technical insight!

## Revisiting Zobrist Hashing 🕹️

In my previous post, I shared my experiences developing an AI agent to play the board game "Breakthrough" for the CS2109S individual project. Although I didn't implement Zobrist Hashing due to time constraints, I learned about its potential to significantly reduce runtime and space complexity in board game AI algorithms. For those unfamiliar, Zobrist Hashing is a hash function construction used to implement transposition tables in computer programs playing abstract board games like chess and Go. Its primary purpose is to avoid analyzing the same game position repeatedly.

## The Unexpected Quest 🚀

A few weeks after my AI project and exploration of Zobrist Hashing, I embarked on a fascinating programming assignment during my studies in CS3230. The assignment presented a challenging problem: to find the number of balanced subarrays in an integer array of size "n."

### The Problem Description 🎯

In this intriguing problem, we were given an array of integers, and our task was to identify the number of balanced subarrays it contained. An array is considered balanced if all its numbers occur an even number of times, including zero occurrences. Let's illustrate this with more examples:

- Example 1:
  - Array: `[1, 1, 2, 3, 3, 2]`
  - Balanced Subarrays: `[1, 1]`, `[1, 1, 2, 3, 3, 2]`
  - Explanation: In this array, both subarrays contain numbers with even occurrences, making them balanced.
- Example 2:
  - Array: `[4, 4, 7, 8, 8, 7, 4, 8]`
  - Balanced Subarrays: `[4, 4]`, `[8, 8]`, `[4, 8]`, `[7, 8, 8, 7]`
  - Explanation: The subarrays in this array have even occurrences of their elements, satisfying the condition for balance.
- Example 3:
  - Array: `[1, 2, 3, 4, 5]`
  - Balanced Subarrays: `[]` (Empty subarray)
  - Explanation: In this case, there are no subarrays with elements occurring an even number of times, including zero occurrences. Hence, the empty subarray `[]` is the only balanced subarray.

### The Challenge ⚡️

The complexity of the problem lies in achieving the solution with a runtime of O(n^2). Balancing subarrays effectively requires careful consideration of the array elements and their occurrences. With potentially large input arrays, the sheer number of subarrays possible demands a well-thought-out algorithmic approach. Additionally, ensuring that each subarray meets the condition of having elements occurring an even number of times, including zero occurrences, poses an intriguing challenge.

To achieve the O(n^2) time complexity, we need to devise an algorithm that efficiently explores the array and identifies balanced subarrays without unnecessary computations. This requires smart data structures, techniques, or principles to keep the solution optimized and avoid redundant operations.

In the next section, we'll explore how an unexpected application of Zobrist Hashing, which was originally designed for board games, remarkably addressed this challenge and provided an elegant solution to the problem.

## Solution 1.0 - A Naive Attempt 🤔

In my initial attempt at solving the problem, I devised a straightforward solution using a lengthy bit string representation. Each bit in the string denoted the parity of the corresponding number (0 for even parity and 1 for odd parity). As I looped through the array, I updated the bit string with each number's parity, hashed the resulting configuration, and recorded the counts for each bit representation. The number of distinct pairs of the same bit string indicated the total number of balanced subarrays.

This approach, while conceptually simple, suffered from significant space complexity issues, especially when dealing with large input arrays. For example, when the size of the given array reached 50,000 elements, storing the entire configuration as a series of long-type numbers required nearly 391 long-type variables. Managing such a vast representation proved inefficient and impractical, leading to excessively high memory consumption.

### The Space Complexity Concern 💥

The naive solution's space complexity can be better understood by considering that each long-type variable can represent 128 bits. With an array size of 50,000, each bit string representation required 391 long-type numbers, leading to the excessive memory usage mentioned earlier.

As a result, the naive solution's performance degraded significantly as the array size increased, making it impractical for handling large datasets effectively.

To address this space complexity concern and find a more efficient solution, I turned to explore alternative approaches and techniques, leading me to an unexpected application of Zobrist Hashing. The application of Zobrist Hashing remarkably improved the solution's efficiency and provided a more elegant way to tackle the problem of identifying balanced subarrays with a runtime of \( O(n^2) \). I will delve into this surprising quest and its exciting results in the next section. Stay tuned! 🚀👨‍💻

## Solution 2.0 - The Surprising Application of Zobrist Hashing 🤯

A moment of inspiration struck me! Zobrist Hashing, a technique typically used in board games, might hold the key to a more elegant and efficient solution for our problem of identifying balanced subarrays. By adapting the principles of Zobrist Hashing to this non-gaming scenario, I hoped to overcome the space complexity challenge and achieve the required \( O(n^2) \) runtime.

### The Modified Approach 🛠️

In the updated solution, I used a single long-type variable as the representation. The variable was randomly initialized to avoid collisions, and each number in the array was assigned to a random long. This initial step ensured that each element in the array was uniquely represented in the long-type variable.

### The XOR Operation 🔄

During the loop through the array, I employed the XOR operation to effectively mimic the XOR operations in Zobrist Hashing. For each number encountered, I XOR-ed its corresponding long representation with the current value of the long variable. This process efficiently updated the long representation to reflect the parity of numbers seen so far.

### Hashing the Long Representation 🗝️

After updating the long representation, I proceeded to hash the long variable to obtain a unique hash value for each subarray configuration. The hash value acted as a fingerprint, enabling me to identify distinct subarrays with the same parity configuration efficiently.

### The Astounding Improvement 🚀

The shift to using Zobrist Hashing in this context yielded astonishing results! The Zobrist version of the solution solved the problem in under 3 seconds, a significant improvement over the naive solution's runtime of over 20 seconds for a large input array. This impressive enhancement showcases the power and adaptability of algorithmic techniques, even when applied in unexpected domains.

## The Quest Continues... 🗝️

The unexpected application of Zobrist Hashing to a problem unrelated to board games opened my eyes to the remarkable flexibility and versatility of algorithms. It's awe-inspiring to witness how concepts designed for specific purposes can be creatively adapted to address entirely different challenges.

While I wish to delve into formal proofs of the algorithm's correctness and runtime, this post has already grown quite lengthy and technical. Perhaps in the future, I'll return with a new post to explore these aspects more thoroughly, providing a deeper understanding of the elegance and efficiency of the Zobrist Hashing-based solution.

