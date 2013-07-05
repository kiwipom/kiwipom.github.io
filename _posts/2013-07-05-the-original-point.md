---
layout: post
title: The point I hope wasn't lost
---

Earlier this week, I got an idea that had been perculating in my brain for a few weeks out and [into a blog post](/2013/07/02/funcy-love.html) and it seems to have generated a bit of reaction from a couple of the C# devs I know.

[Ivan](http://twitter.com/ppog_penguin) said "Cool, and here's how you can take that idea [even](http://developer.greenbutton.com/make-my-func-the-higher-order-func/) [further](http://developer.greenbutton.com/func-ier-and-func-ier/)"

There were calls to [not do it](http://blog.computercraft.co.nz/2013/07/04/DontGetTooFuncy.aspx) (although I got confused why his use of `(delegate)` was OK and my advocation of lambdas wasn't)

[Brendan](http://twitter.com/shiftkey) said that the principle was sound enough, but the devil was in the detail and my overly-trivial example raised other interesting questions.

[Paul](https://twitter.com/xpaulbettsx) pointed out that Ivan's implementation is similar to [a class in ReactiveUI](https://github.com/reactiveui/ReactiveUI/blob/master/ReactiveUI/MemoizingMRUCache.cs)

[Simon](http://twitter.com/simoncropp) suggested [doing an IL version](https://twitter.com/SimonCropp/status/353078614444605441)…


So, I'm delighted to see the conversation spark up around all this, and these are all excellent points, digging into the detail, and trying to fill the huge gaps I left, but…


### The original point of the post

Here's my point that I worry might've got somewhat lost. Or maybe I didn't state it explicitly enough in the first place:

> Adding **just a sprinkling** of Func<> love to your C# code improves it.

That's it! That's all I was trying to say: That taking idiomatic, imperitive C# and applying functional paradigms with `Func<>` and `Action<>` - just a tiny bit, just where it makes sense - makes the code:

- DRYer
- Easier to read
- Easier to maintain

And that was really my main point.

***OK-I-love-you-bye-bye***