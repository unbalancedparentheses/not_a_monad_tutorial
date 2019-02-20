# Interview with Robert Virding, creator of Lisp Flavored Erlang, an alien technology masterpiece

Feb 29, 2016

![](https://cdn-images-1.medium.com/max/800/1*0_N8_5MqdROjW5duRb6Dpw.png)

>As you might know zombies, skeletons and momies are good friends of aliens

This time I interviewed [Robert Virding](https://twitter.com/rvirding), co-creator of Erlang and creator of [Lisp Flavored Erlang](http://lfe.io/) (LFE). I am an Erlang developer and Lisp fan — if you are learning Clojure check out my post [How to earn your Clojure white belt](https://medium.com/this-is-not-a-monad-tutorial/how-to-earn-your-clojure-white-belt-7e7db68a71e5#.dtmcog9gk) — so logically I am very excited about LFE.
*****
Reach me via twitter at [@unbalancedparen](https://twitter.com/unbalancedparen) if you have any comments or interview request for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial/). **Stay tuned!**
******

**Why did you create LFE?**

I discovered and learnt Lisp long before we started working with Erlang, and have always loved it. But I also like the Erlang language (I don’t have problems with the syntax :-)) and how it can build systems. My goal was to make a lisp which was a “real” lisp which builds systems in the Erlang way. Hence LFE.

**What is the LFE philosophy?**

LFE is a proper lisp based on the features and limitations of the Erlang VM, which coexists seamlessly with vanilla Erlang and OTP and runs on the standard Erlang VM.

**In your talk called [“About Language Design”](https://www.youtube.com/watch?v=afLRmoSOnHA) you said:**
_“People complain about the Erlang libraries and once thing they complain very rightly about the Elang libraries is they’re inconsistent, the naming conventions is inconsistent, the argument ordering is inconsistent, everything is inconsistent about them and that is correct. They are right and people complain about that.”_
**At some point, will you create a new standard library in LFE?**

I would like to but there are some deep problems trying to do that. Basically you can’t really change any library module that is used by OTP without the effect propagating and going viral in OTP. Adding new libraries yes, modifying old modules not really. Elixir got around this by having their special module aliases which map onto module name ‘Elixir.XXX’ but then you end up with 2 module naming conventions. I preferred to keep the same names.

**[Common Lisp macros and functions](https://github.com/rvirding/lfe/blob/dev-macro/src/cl.lfe) have been added to [LFE](https://github.com/rvirding/lfe/blob/dev-macro/src/cl.lfe). [Clojure macros and functions](https://github.com/lfex/clj/issues/18) are also available as a separate library. LFE follows more traditional LISPs like Common Lisp, Scheme or more modern Lisps like Clojure?**

LFE more has the feel of CL and Scheme, especially CL as it is a lisp-2 not a lisp-1 like Scheme. Clojure is definitely interesting but I felt that the way it does concurrency doesn’t really map well onto Erlang and the style of building systems feels different. `Clojure feels more like language with concurrency while Erlang feels more like a operating system with a language`.

**What is [LFE Flavors](https://github.com/rvirding/flavors) and the [LFE Object System](https://github.com/oubiwann/los)? Aren’t they pretty similar?**

Flavors is an object system on the Lisp Machine. I did LFE Flavors out of pure fun and curiosity. A long time ago (about 30 yrs) I did an implementation of Flavors for another lisp system, Portable Standard Lisp, and I was curious to see what it would be like to do one for LFE. It worked quite well for the central parts but there is a lot of Lisp machine specifics which can’t be transferred. CLOS is based on Flavors and you can see the heritage.

My plan with LFE Flavors is not to bake it in as part of LFE but have it as a supported compatible plugin. I like to keep the core simple. I also have plans to implements more general structs which will allow more control over data structures and access to them. It would subsume records and elixir structs amongst other things.

**A few months ago you [sent an email](https://groups.google.com/d/topic/lisp-flavoured-erlang/l_Te7ZHkm9M/discussion) titled “New macro handling and compiled macros”. How does the macro system work in LFE, what are you changing and why?**

Currently macros work by defining them locally in each file where they are used. If you need to share macros then you define them in include files. The new macro handling will allow macros to be exported from modules in much the same way as functions and you would call them in the same way. So for a module foo you call functions with (foo:function …) and macros with (foo:macro …) making the interface much more consistent generic. Most uses of include files will disappear.

**What do you think about Elixir?**

Ambivalent! I don’t speak Ruby at all, so much of the syntax feels very strange and foreign. It also manages to push some of my programming buttons, for example having multiple ways of representing the same thing and adding syntax for special cases; both which I feel are just wrong. I am jealous of their ability to clean up some of the OTP modules by writing their own interfaces and having a way to avoid overlapping module names. I wonder about some of the complexity but that is just because I have a thing about simplicity.

**Have you incorporated any idea from it into LFE?**

I have not taken any ideas directly from Elixir though we do share some features, for example having multiple modules in one file (which I am not sure I like but it can be practical).

**I have seen [multimethods](http://blog.lfe.io/design/2015/07/11/1720-towards-multi-methods-in-lfe/) and [protocols](https://github.com/lfex/los/issues/8) mentioned a few times by the LFE community. What do you think about Clojure multimethods and protocols?**

One difficulty doing something like this in Erlang is that Erlang modules must be compiled as one unit, it is impossible to add, or remove, functions afterwards without recompiling the whole module. This makes it very difficult to have methods which are to be in one module in different places. Which lessens the usefulness of multimethods. IMAO.

Flavors gets around this by compiling each component flavor separately and building a object flavor from all its mixin when the first instance is created. This allows us to create the components separately as long as all are done before their first time they are used. After that they cannot be modified.

**Do you think adding something like [transducers](https://stackoverflow.com/questions/26317325/can-someone-explain-clojure-transducers-to-me-in-simple-terms) to LFE would be a good idea?**

They probably would be, but they are not really the way I think. I find I tend to be very explicit in which data types I use; for me this is as fundamental as choosing which algorithm to use. This means that having polymorphic transformation functions becomes less interesting for me.

I know that many people prefer working in this way and I see no problems in adding them to the set of standard LFE libraries. They just won’t be integrated into LFE at the lowest level. You will be able to choose.

**I have seen some discussions about Dialyzer in LFE’s mailing list and there is a dialyzer branch in the [LFE repository](https://github.com/rvirding/lfe/tree/dev-dialyzer). How well does LFE support [Dialyzer](http://erlang.org/doc/man/dialyzer.html)?**

Supporting dialyzer is a little tricky as the official interface to dialyzer is very restricted: you either pass in erlang files, or you pass in beam files containing the Erlang AST of the code (compiling with the debug_info option). This does not work with LFE as the LFE compiler generates Core erlang, a language used internally in the compiler.

However, by being a bit cunning I have generated some alternate dialyzer interface modules which can load in the Core erlang forms directly. It works but it is really only an experiment; a better solution would be to do a proper fix of the dialyzer interface but this is not that simple as the current interface is not very cleanly coded. It is actually quite ironic as dialyzer uses Core erlang internally.

**What do you think about [Steel Bank Common Lisp Type system](http://www.sbcl.org/manual/#Handling-of-Types), [Typed Racket](https://docs.racket-lang.org/ts-guide/index.html) or [Clojure’s core.typed](https://github.com/clojure/core.typed) and [Shen Types](http://www.shenlanguage.org/learn-shen/types/types.html)?**

I have never tried these so I can’t say. Generally I am very dynamically typed. :-)

**What is the roadmap for LFE v1.0?**

So far the roadmap has been a little “I just want to add this one last feature and then I’ll release 1.0”. Anyway, my plan now is that when the new macro handling works and is properly integrated then the system will feel well rounded and I will release 1.0.

*******
**Did you like it? [Follow me on Twitter — @unbalancedparen](https://twitter.com/unbalancedparen)**

Oh and I recommend that you read this email from Robert about LFE titled _“A bit of philosophy and some design principles”_:

>_There was a bit of discussion on the IRC about the CL module and where LFE gets its impulses from, that it doesn’t try to stand on its feet. There are things in it from scheme and CL but most of it is based on Erlang and the features it provides and the type of systems you build using it._
>
>_The base of LFE rests directly on what the underlying Erlang VM provides and this determines what LFE can do and how it works. It is based on language features like modules, pattern matching, immutable data and how functions work. This is what defines LFE. I don’t think that LFE tries as hard as Elixir to hide this background._
>
>_This, for example, is why LFE is a lisp-2 not a lisp-1 as it is a better fit for how Erlang handles functions. On top of this there is a set of convenience macros which were originally more scheme inspired but became more CL inspired when LFE became a lisp-2. They are just a better fit. Of course this doesn’t make LFE a scheme or a CL as there are many things in both scheme and CL which LFE can’t do because of the underlying Erlang VM like mutable data and the handling of nil/() and symbols with values, plists and function bindings [*]._
>
>_An alternative would have been more inspired by clojure which shares some properties with Erlang. However they are fundamentally different in many ways so it would mainly have been using clojure’s naming conventions. Also clojure’s concurrency model is very different from Erlang/LFE so its way of building systems is also very different. You get the feeling, at least I do, that it is based on a central thread of execution where we you can run things in parallel, but there is this central thread. This is very un-Erlangy as Erlang/LFE systems typically don’t have a central thread._
>
>_This gets us, finally, to the CL module. It is just a library containing many of the standard CL library functions which gives you the possibility of writing in a CL style. Not everything can be included, for example the nXXXX functions which mutate data, and some features don’t mesh well with Erlang/LFE. For example equating nil/() and predicates which in Erlang/LFE return true/false while in CL are truthy and return nil/() and anything else [**]. It is in no way a fundamental part of LFE and is just an add-on. If anyone feels inclined to do a similar module for clojure then I will definitely consider including it. Again it would just be an add-on. These could easily be included in the base release._
>
>_Packages like flavors, which I did because they are interesting and I think fun, should probably not be part of the base. This wou|d also apply to things like LFE CLOS (if anyone decided to do it) and LFE clojure-like protocols. They are interesting and useful in themselves but not things I would consider part of the LFE base. A set of these should probably kept in a standard place to make them easily accessible for everyone. I would like to keep the base relatively simple, clean and “basic”, a “lean, mean, fighting machine” if you will. It is all too easy to add things, even sensible and useful things, and end up with a bloated mess. I really want to avoid this, hence keeping the base simple and clean._
