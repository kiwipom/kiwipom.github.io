---
layout: post
title: Lesson 1. The easy stuff.
---

### Starting out

Having established that functional programming is the very cat's pyjamas, and that if I'm going to learn any language, [I may as well try and learn Haskell](/2013/04/26/learning-me-a-haskell.html), I just need to do a couple of things.

1. Install the [Haskell platform](http://www.haskell.org/platform/) and
2. Fire up [learnyouahaskell.com](http://learnyouahaskell.com/) and type stuff into the terminal directly like it's 1987!

Actually, this really does have echoes of those hours spent bashing in BASIC programs from the back of a magazine, and then wondering whether I'd typed it in wrong (usually the case) or there actually was a bug in the code (a small but significant percent of the time).

Maybe that's why this is so much fun.


### Chapter 1. Ready, set, go!

OK, this really is the easy stuff. Type a number in and it spits it back at you. Type in a simple sum and it gives you the answer. I reckon even I can keep up at this pace! We learn some syntax around boolean logic and testing for equality. Gotcha.

We learn that Haskell cares about types and you can't cram a string and a number together and hope for something sensible. Seems reasonable.  I do need to resist the temptation to say "…oh, like C#?" when I learn this, because I'm trying desperately to not roll this new stuff down the well-trodden C# pathways of my brain.

And here comes the first 'huh?'. We then learn that by typing the simple stuff like

    ghci> 5 + 9
    14
    ghci>
    
we're actually using a function. You know, like functional programming. The `+` operator is the function, and it's used in the 'infix' position; that is, the two arguments being passed into the add function go either side of the `+` (as opposed to prefix notation where the arguments go after the function name).

This, as it transpires, is a great example of a pure function. It is immutable, or side-effect free. You cannot change the nature of 'add' just by calling 'add'. If you put 9 and 5 into the 'add' function you will *always* get 14. 

Functional programming works like this. We create immutable structures (functions) that cannot change, and call them knowing they will behave reliably and consistently.

Awesomesauce.


Next step is to write a simple function or two. We'll do that next time.


