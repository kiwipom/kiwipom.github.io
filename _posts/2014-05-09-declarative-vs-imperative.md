---
layout: post
title: The difference between declarative and imperative
tags:
- FP
---


I sometimes like to ask the following simple interview question:

>> Write a function that will give me the sum of the squares of the odd numbers between 1 and 50

I like watching people go through the process of solving this simple problem - I think it initiates good conversations. More often than not, I will end up looking at code that in some shape or form resembles this:


    int SumOfSquares()
    {
      int result = 0;
      for (int i=0; i<50; i++)
      {
        if (i%2 == 1)
        {
          int square = i*i;
          result += square;
        }
      }
      return result;
    }
    
This code should be simple enough to read, because it is a trivial example:

- create an acumulator variable `result` and initialize it to 0
- Create a for loop to increment a variable `i` through from 0 to 49
- check the modulo 2 result of `i`
- if that mod 2 is 1, the number is odd, so:
- calculate the square of that number, and
- add it to the accumulator, `result`.
- return `result`.

To me, that's a large amount of noise. As programmers, we really want to be working on the high level problem solving bits - defining the *what* - and leaving the implementation details - the *how* - to the machine. I know that under the hood it's going to be turning 0s into 1s and back again, but I'm not interested in any of that. 

Let's examine the actual problem here, and define it in natural language. Basically, it looks like:

- Start with the numbers from 1 to 50,
- Filter out just the odd numbers,
- Square them, and
- Return the sum.

And in C#, this would look like:

    int SumOfSquares()
    {
      return Enumerable.Range(1,50)
        .Where(i => i%2 == 1)
        .Select(i => i*i)
        .Sum();
    }


This code reads the way you think about the problem - cleaner, more intuitive, just better.


---

P.S. This function in Haskell would look like:

    sum (map (\x->x^2) [1,3..49])


You know. Just sayin'...


