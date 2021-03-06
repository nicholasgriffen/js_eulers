# js_eulers
JS programs I wrote to solve problems from Project Euler

# 1_multiples.js

Problem:  
Find the sum of all multiples of 3 or 5, below 1000.

Solution:  
The mostly-imperative style program arrives at a solution by breaking the problem into the following tasks:

1. Check all integers less than 1000:  
Start a while loop at 999, decrementing by 1 at each iteration, ending at i = 0
2. Determine if a number is a multiple of 5:  
True if number modulo 5 is equal to 0
3. Determine if a number is a multiple of 3:  
True if number modulo 3 is equal to 0
4. Collect the multiples of 3 or 5:  
Push integer into an array if it passes the test in step 2 or step 3
5. Sum the collection of multiples:  
Use Array.prototype.reduce to reduce the collection to the sum of its parts

# 2_fibonacci.js

Problem:  
Find the sum of all even members of the Fibonacci sequence, below 4,000,000 

Solution:  
The recursive program arrives at a solution by breaking the problem into the following tasks:

1. Build the fibonacci sequence:  
a. Each term in the Fibonacci sequence is simply the sum of the preceding two terms. Starting in line 8, the function takes the current sequence and stores a copy with Array.prototype.slice(). Operating on a copy as opposed to the original sequence preserves the order and accuracy of the sequence with minimal effort.  
b. Produce the next term by popping 2 terms from the copy and summing them.  
c. The next term is pushed into the original sequence.  
d. The function calls itself using the updated sequence and a decremented count argument. This can all be done in an imperative style using loops, but I prefer the elegance and simplicity of the recursive style.

2. Stop building the sequence:  
a. When an upper limit on the value of a term is reached. Before building the sequence, the function checks the most recent value against a designated limit. If the last value in the sequence exceeds the limit, it is popped and discarded, and the function returns. For convenience this limit has a default value of 4,000,000. Together with the count parameter, the limit guards against infinite recursion.  
b. When the designated number of terms have been calculated. User may designate a particular number of terms to be calculated, as opposed to an upper bound on the value of the greatest term. Strictly unnecessary in this task but helpful for experimentation and for use of the fibonacci function outside of the particular constraints of the task.

3. Filter odd numbers out of the sequence:  
As each term in the sequence depends upon the preceding two terms, even terms cannot easily be discarded while the sequence is being created. Instead, a simple Array.prototype.filter is applied to the completed sequence: (term => term % 2 === 0). Here we build on the knowledge gained in Task 1, making use of the modulo operator to find even numbers.

4. Sum the collection of odd terms:  
Use Array.prototype.reduce to reduce the collection to the sum of its parts - again building on the knowledge gained in Task 1.

# 3_factorization.js 

Problem:  
Find the largest prime factor of the number 600851475143

Solution:  
The problem is broken into four general tasks, solved through the composition of 5 primary functions.

Note:  
This program may not run to completion if executed in the browser. It is only tested to completion in node.js. More elegant solutions are surely possible but at the time of writing have not been discovered and implemented by the author.  

1. Reduce the size of the task:  
a. JavaScript in the browser did not take kindly to my initial, recursive approach to integer factorization. Likewise, node.js would not complete recursive operations on large numbers. In order to complete the task, I had to be able to run my programs, so I had to optimize. The first optimization is to loop while factoring, as opposed to making recursive function calls.  
b. Prime numbers above 2 must necessarily be odd numbers. Therefore, the function trialDiv() is called with an odd number (lines 11-13) and decrements by 2 at each iteration of the loop (line 35), ignoring even numbers completely.  
c. Typically, trial division checks every integer from 2 to n. Checking values greater than x = n/2 is unnecessary - anything greater could only be x = n/y where (1 < y < 2), so I set an upper limit of the nearest odd integer to n/2. Likewise, I set a lower bound on the trial division by taking a first pass at factorization, fermFac(), implementing Fermat's factorization method as described at [Wikipedia](https://en.wikipedia.org/wiki/Fermat%27s_factorization_method). The larger of the two factors returned by this method is used as the lower bound.

2. Find the factors of 600851475143:  
a. smarterFac() runs the optimizations listed above, gathers factors using Fermat's method and a simple trial division function (trialDiv()), then finally attempts to fill in gaps caused by the use of a lower bound by pushing n/factor into the factors array (buildFactors()). 

3. Find the prime factors of 600851475143:  
a. Once the factors are gathered, I factor each factor. One could check the primality of a factor before pushing it into the factors array, but I was afraid that this would greatly slow the program or possibly break it.  
b. Each array of factors-of-factor-of-600851475143 is put through a simple Array.protoype.filter, returning only those factors whose value is not 1 or n. If any such factors exist, n is not prime.  

4. Find the largest of the prime factors of 600851475143:  
a. Presently this is left as an exercise to the reader's mind and eyes. One of many possible improvements to the program would be to implement a simple loop, running through the prime factors and returning only the largest factor. 
