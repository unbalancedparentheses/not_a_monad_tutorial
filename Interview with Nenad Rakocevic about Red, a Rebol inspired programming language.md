#Interview with Nenad Rakocevic about Red, a Rebol inspired programming language

Aug 28, 2015

After our [last interview with Brian McKenna](https://medium.com/this-is-not-a-monad-tutorial/interview-with-brian-mckenna-about-roy-purescript-haskell-idris-and-dependent-types-63bb1289ea3d) for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial) we interviewed [Nenad Rakocevic](https://github.com/dockimbel), creator of the [Red](http://www.red-lang.org/) programming language.

From my completely subjective point of view Red and Rebol are quite strange creatures! But don’t get me wrong, that doesn’t mean something bad. For example, I am not aware of many high-level languages which features an embedded DSL for general-purpose low-level programming or that have 50 native types. You should check it out, you might find some interesting ideas inside Red development.
![](https://cdn-images-1.medium.com/max/400/1*C0sMUPxpbBs40YIBX848jQ.jpeg)
*****
Reach me via twitter at [@unbalancedparen](http://twitter.com/unbalancedparen) if you have any comments or interview request for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial). **Stay tuned!**
********


**Please tell us a little bit about Red’s inception. Why was it created?**

I started programming micro-computers, an Amiga in my case, in my teens. I have now been programming for more than 30 years. After my early experiences, I was unhappy with existing programming languages and tools. This was mostly because I found them not productive or friendly enough for my taste. So, when I stumbled across the Rebol language, in 1999, it was an eye-opener on what was wrong with so called “modern” computing practice. (Nowadays it’s even worse). Fighting complexity on all software fronts became the logical course of action.

In 2010, Rebol was closed source. I deeply felt that the approach had a lot more to offer but Rebol was barely evolving. This was the trigger for me to start work on an open source relative to Rebol with much higher ambitions and a broader field of usage.
![](https://cdn-images-1.medium.com/max/400/1*869fRKUVs2sTdVDYrqblMQ.png)

**What are the main selling points of Red?**

* First fullstack programming solution: combines in one tool, the ability to write high-level code (GUI apps, scripting and DSL) and fast low-level code (writing device drivers, operating systems, native interfacing, etc). Moreover, Red is also a _both-sided_ technology (client & server).
* Cross-platform native code compiler: from any platform the toolchain runs on, you can compile to about 15 other platforms, with a simple command-line option (-t Windows, -t Linux, -t Darwin, -t RPi, …).
* Extremely lightweight: Red is a 1MB, single-file, no install, no setup, toolchain. It takes typically a few seconds to download and you can immediatly start writing and running code, there’s * nothing * to setup (it’s just terrible that this is the exception instead of being the norm…).
* Batteries-included solution: it comes with a very rich runtime library, despite its tiny size, covering pretty much anything you need for common tasks.
* DSL-oriented environment: Red comes with many embedded DSL addressing important needs (like GUI or system-programming). DSL are a very powerful way to reduce complexity arising from frameworks or API, while drastically increasing productivity. Red includes a DSL (called Parse) for constructing DSLs.
* Red (like Rebol) is a Lisp derivative, but with a human-friendly syntax (no parenthesis hell). Red is its own data format. All code is treated as data until you evaluate it, code/data serialization comes for free. The minimal punctuation makes it easy on the eye.
* The underlying philosophy of Red (as was that of Rebol) is to make the simple easy and the difficult possible.

**What made Rebol the main inspiration for Red?**

Rebol is one of the most innovative programming language created in the last 20 years. Sadly, it remained under the radar being closed source at a time when open-source languages like Perl, Python and Ruby hit the streets. Rebol’s approach shakes the foundation of what programmers consider “simple” or “efficient” in programming. Typically, API which other languages would call “simple”, look uselessly complicated when you are used to wear Rebol glasses. ;-) Here are a few one-liners as examples (using the Rebol2 REPL):

Create a GUI window with a button printing, on click, Hello in the console:

`view layout [button “Click Me” [print “Hello”]]`

Dump the content of a web page in console:

`print read http://rebol.com`

Extract the title of a web page:

`parse read http://rebol.com [thru <title> copy text to </title> (print text)]`

Send the list of files in current folder by email:

`send user@domain.com mold read %.`

Retrieve records from a MySQL database and print them in console:

`foreach row read/custom mysql://root@localhost/books [“SELECT * FROM authors”] [print row]`

Notice that even if you never looked at Rebol code before, you can nonetheless read it and guess what most of the code is doing.

**What are the main differences between Rebol and Red?**

* Red can be (cross-)compiled to native code, Rebol is only interpreted. Compiled code can run much faster than interpreted code.
* Red allows system-programming and fast low-level code, Rebol is stuck at scripting-level.
* Red relies on native widgets and native backends for GUI support, Rebol has a custom GUI engine. So Red will make your GUI apps feel more natural and better integrated to the OS user.

Besides that, the languages itself are very similar, somewhere around 95% the same. If you know Rebol, you know Red.

**Red covers the whole range of abstraction between low-level and high-level programming by offering Red/System as a dialect with C-type semantics and Rebol-type syntax. Was the distinction between Red and Red/System present in the original design? What advantages do you gain by using Red/System?**

Absolutely, Red/System was one of the main incentive to build a new programming stack instead of simply duplicating the Rebol implementation. Red/System is a statically compiled language targetting native code (like C). Red/System serves two main purposes:

* Provide an embedded DSL to Red users for fast code support and system programming needs
* Fill the role of an IR (Intermediate Representation) language for compiled Red code

AFAIK, Red is the first high-level language which features such embedded DSL for general-purpose low-level programming.

**Who uses Red/Rebol?**

The Rebol community used to be relatively big in the early 2000, but it shrank a lot as its evolution tailed off. The profile of users was astonishly broad on the skills scale. Many were attracted by its simplicity for simple tasks and its cross-platform GUI engine, but other were more interested by its depth (dynamic binding, easy DSL crafting, strong meta-programming abilities, …).

Since then, only the hardcore fans or companies which have built their software on top of it, continue to use and promote it. Many of those same people now form the early adopters for Red, along with many other people which were interested in Rebol when it launched, but rejected it due to its closed-source nature. Some of them wrote libraries for Red, other small games or even a Windows device driver! :-) As soon as Red is ready for production, we’ll make sure many more people will join them and have fun using Red.

**For which scenarios do you think Red is an appropriate language?**

Red is a general-purpose programming solution which should be good enough for * any * programming task. In practice, it’s (only) limited by the available frameworks and libraries. So these tasks are a very good match for Red:

* Scripting / glue code
* GUI apps (in upcoming v0.6.0)
* Android apps (in v0.6.1)
* Data processing
* Grammar parsers / DSL creation
* System programming
* Device drivers
* IoT devices programming (runinng on Intel or ARM cpu)

Once we reach 1.0 (next year), Red will also be very good for:

* Webapps programming
* Servers creation
* 2D games
* Robotics

**Rebol and Red offer a great variety of built in types with practical applications. Some would argue that it is better to offer a small core of language features. What is your take on that?**

Rebol and Red offer about 50 datatypes in a runtime of about 500KB. Among them, two-thirds have a specific literal form (like money, email, url, time, date, colors,…) which gives you, out of the box, a rich set of literals you can use for building embedded DSL.

Another big gain is that most of the features you need for daily work are already there, as first-class citizens, perfectly integrated with the rest, working exactly the same on every supported platform. This is a productivity boost and makes learning/using the language much more pleasant (no need to make “imports” for any simple task). Such languages are pragmatic and aiming at reducing the costs of software building.

**What is a sky-high view of how Red is implemented? Are all the components (parsers, code generators, garbage collectors, etc) hand-written? What dependencies does Red have?**

Excepting for a good part of the unit tests, which are generated by script, everything else is hand-written. We are bootstrapping Red using Rebol, so the toolchain (compilers, linker, builders) is written in Rebol2. Rebol offers a parsing DSL which is very effective, adding to that its deep metaprogramming abilities, there is simply no need to use any other tool for building such toolchain. Red scripts can be interpreted from the REPL or compiled to native code, using Red/System as intermediary target. The runtime library is built in a mix of Red and Red/System code.

Red executables are typically around 0.5MB and have no dependencies.

**How complete is Red as of Mid-2015?**

There’s already a lot implemented, so I’ll describe rather what’s missing. Right now, we are completing the cross-platform GUI support with a first backend for Windows. Android, Linux and OS X backends will follow. The I/O is currently limited to simple file operations and HTTP client support only. Modular compilation, full garbage collector and concurrency support are the main features missing before reaching 1.0. We aim at launching the 1.0 release in 2016.

**Where do you see Red in the future?**

Red has the potential to seduce many developers (especially indie-developers who have the freedom of choice) who are frustrated by existing tools (even the so-called “simple” ones). I expect Red to be wide-spread in a couple of years, helping programmers achieving many different tasks while having fun doing it and making their life easier. Red will expand with many strong DSL for various domains, offering nice replacements for existing libraries direct usage. For example, we’ll push it in robotic and AI fields.

**What are the most important lessons learned from the development of Red?**

1. Open-source is a superior way to build quality software (just confirmed that fact with Red project).
2. Working “in the open” is not always a good thing, sometimes you need to isolate yourself from the outside “noise” to execute complex tasks (mostly design tasks). Being able to do so is increasingly difficult as the project growths up.
3. Having to deal with a growing community of users consumes a * lot * of time. Finding people to deal with the community for you is critical.
4. Designing good syntactic rules is * way * more difficult than designing good semantics. That’s a part overlooked by many language designers, which end up with great semantics but terrible syntaxes.
5. Writing a native code compiler for a statically typed language is really not difficult, most programmers with a minimal CS background could do it, they’re just not aware they can.
6. Premature optimization can (often) bite you in the back. Knowing when you’re optimizing prematurely is a bit of a black art.
7. Every big software project should be started by a team of at least 2 highly tuned, equally skilled developers. Working alone on big projects is insane, and not a guarantee of best results.
8. If you work on an open-source project that is attractive enough and gather enough followers, you can live from user’s donations (I did so for 2 years, covering all my basic life expenses). I never thought that would be possible when I started, nor did I counted on it. I guess I’m just lucky that Red has an amazing community.

**What reading material do you recommend for implementing your first programming language?**

You can have a good overview of all the required parts in a book like this [Modern Programming Languages: A Practical Introduction](https://www.goodreads.com/book/show/112256.Modern_Programming_Languages). If you want to go in-depth and dive into more abstract concerns, the “Dragon book” is still the reference.

But the most useful way is to study several small languages implementation, that will give you the best insights about how to achieve it yourself. For example, Red 0.1.0 release is just a 24KB zip archive, but features already a working compiler/linker for Red/System with many features already (including FFI). [Get it from here](https://github.com/red/red/releases/tag/v0.1.0)

**What other languages or technologies are you keeping an eye?**

1. Go: it’s the language with the fastest growth in the last years, understanding why, could be a key to help Red growing faster too. Go’s concurrency model also seems attractive to users, so worth studying.
2. Lua: trying to understand where it’s heading and how it grows.
3. Python3.x: trying to understand where it’s heading, not sure I understand its strategy though.
4. Webassembly: the foundation for the future of web programming.
5. MagicLeap: the future of HCI!