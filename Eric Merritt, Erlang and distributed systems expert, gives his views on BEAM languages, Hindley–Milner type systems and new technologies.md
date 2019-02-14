![](https://cdn-images-1.medium.com/max/800/1*Bvd7l2Q-OmEhkVC2qcclJA.png)
#Eric Merritt, Erlang and distributed systems expert, gives his views on BEAM languages, Hindley–Milner type systems and new technologies

Aug 8, 2015

In this case, [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial) interviewed [Eric Merritt](https://twitter.com/ericbmerritt), author of [Erlang and OTP in Action](http://www.manning.com/logan/), [Joxa](http://joxa.org/) (a small semantically clean, functional lisp running on the Erlang VM), [relx](https://github.com/erlware/relx) (best release creation tool in Erlang).

![](https://cdn-images-1.medium.com/max/800/1*XCrgX6wctMhx0GLjNQS9nw.jpeg)

In the following weeks we will be talking with Robert Virding — Erlang co-inventor and [Lisp Flavored Erlang](http://lfe.io/) creator — , [Brian McKenna](https://github.com/puffnfresh) —[Roy](https://github.com/puffnfresh/roy) language creator— and with [MirageOS unikernel](https://mirage.io/) dev team. Stay tuned!

**In the Functional Geekery podcast you stated that the Erlang VM (BEAM) is brilliant. What did it get right that other VMs did not?**

[Functional Geekery Episode 20 - Eric B. Merritt](http://www.functionalgeekery.com/episode-20-eric-b-merritt/)

BEAM is the only reasonably popular VM that took the language model, in this case Actors, and leveraged that model to make the platform itself more efficient. I find that brilliant. The two major examples of that approach in BEAM are how the Garbage Collector works with the runtime and how IO works.

In many systems, Java included, the Garbage Collector (GC) must examine the entire heap in order to collect all the garbage. There are optimizations to this, like using Generations in a Generational GC, but those optimizations are still just optimizations for walking the entire heap. BEAM takes a different approach, leveraging the actor model on which it is based. That approach basically has the following tenets:

* If a process hasn’t been run, it doesn’t need to be collected
* If a process has run, but ended before the next GC run, it doesn’t need to be collected
* If, in the end, the process does need to be collected, only that single process needs to be stopped while collection occurs

Those three tenets are one of the primary reasons that Erlang can be a soft-real time system [[Elang has a preemptive scheduler that also plays big a part for this]](http://jlouisramblings.blogspot.com.ar/2013/01/how-erlang-does-scheduling.html). The fact that the model subsets the work that the GC has to do allows that work to remain small and manageable. Its an impressive achievement.

Another big win for the BEAM and its approach to leveraging Erlang’s Actor model is that it leverages low level, efficient, non-blocking asyncronous IO primitives from the operating system to do IO, but presents a comfortable blocking interface to the language layer. Developers using the platform can use a very human understandable synchronous IO primitives while gaining all the advantages of asyncronous IO. This too, is an impressive achievement. I just gave a talk on this topic for the Seattle Scalability Meetup:

[Scalability Meetup @ Whitepages - Erlang VM](https://www.youtube.com/watch?v=PwWIN6vk62Q) 

**Apart from the Erlang VM (BEAM), what do you think about Erlang as a language?**

I think it’s not a bad language. It has the benefit of being both very declarative and very simple. That is a big win in distributed systems where complexity composes and quickly becomes unmanagable. I tend to prefer languages with an algebraic type system and a type inferencing and reasonable meta programming capabilities. Erlang has neither and that’s unfortunate. That said, I have implemented a large number of very reliable systems in Erlang and wouldn’t hesitate to do so again.

**You implemented Joxa, a Lisp for the Erlang VM. Why did you do it?**

For a while there I was working on a problem that was best solved via a suite of DSLs. The platform we built for that was based on Erlang and BEAM, but Erlang doesn’t really lend itself to DSLs. So I decided to write Joxa to facilitate DSL development on the BEAM. It just so happens that creating DSLs for problems is a generally good idea and that makes Joxa a decent general purpose language.

**What is your opinion about LFE (Lisp Flavored Erlang)?**

Joxa took a very different direction than LFE, even though LFE predated it by quite some time. When I ran into the problem that caused Joxa to be created, I investigated it rather deeply to see if it would solve the problem. I ran into a few issues while I was investigating it. In general, I found the implementation very hard to follow. It’s not a bad implementation, it’s just so different from the way I think about languages that it confounded me. That made it difficult for me to expand it.

I was also looking for something with simple semantics that I could build other languages on. LFE is, quite literally, Lisp Flavored Erlang. It is Erlang with S-expression based syntax. That’s not a problem unless you are looking for something with much lower level syntax to build upon. Finally, and this really is a nitpick, Macros are interpreted by LFE and that interpreter is very limited. The rest of the language is interpreted by BEAM. Having to remember if something was going to be run inside the macro interpreter or inside of the normal runtime bothered me a lot.

**And what do you think about Elixir?**

I think Elixir has brought a lot of people to the Erlang world that wouldn’t have otherwise come over. That is a very good thing and a powerful contribution to the Erlang eco-system. However, I am not a big fan of Elixir itself. I find the macro system to be a bit inconsistent and I really dislike that Elixir tries to hide immutability. That does make it slightly easier for beginners, but it’s a leaky abstraction. The immutability eventually bleeds through and then you have to think about it. It also introduces additional complexity within bindings in Elixir Macros among other things. It doesn’t help that I have never been a fan of Ruby syntax and Elixir borrows heavily from that sphere.

**What do you think about laziness in programming languages? In which cases do you think it is useful, if any?**

I love lazyness in concept. I think the idea that computation only occurs when it’s needed is right in line with the trend that has been occurring in functional programming for many decades. The problem that I have with lazyness is more pragmatic. It is very easy to create space leaks and, as of this writing, good tools to detect and debug those space leaks don’t yet exist. That makes me very hesitant to use a language that is lazy by default in production. The Haskell guys are working hard to resolve this, and I think they will, but they haven’t yet.

**Why do you like Hindley–Milner type system? [the type system used in the ML family (Standard ML, Caml, OCaml, F#) and Haskell]**
![](https://cdn-images-1.medium.com/max/400/1*TKFIhHLhfGTz5uMBn6NfkQ.png)
`Image stolen from http://learnyousomeerlang.com/`

Essentially, it’s because I am lazy. Much like resource management, contract management is a slow, manual painful process. By contract management, I mean verifying that the form of data a function recieves is the form of data that it expects. A Hindley-Milner style type system allows me to offload that tedious work to the compiler. Computers are essentially better at that kind of tedious work than humans.

A type system like this is just an evolution of our ongoing effort to offload work to the computer. Originally, we wrote in machine code, then we moved up to Assembly, which was one step higher. Not long after we started using higher level languages like Fortran, Cobol and Lisp. A bit later on we started offloading resource management to the computer as well in the form of GC. An algebraic type system is just a continuation of that. With this type system we are offloading contract checking to the computer. Just like with resource management the contract checking must happen, its just that many languages force the human to do it when the compiler can do it much more effectively.

**Do you think that it would be possible to create a language with a Hindley Milner type system for the Erlang VM without affecting the power of Erlang semantics?**

Not only do I think its possible, I have been planning to do it for a while now, time being the limiting factor. The main problem you will run into is the mismatch between the untyped bits of the Erlang native system and the typed bits of the new language. Dialyzer attempts to solve this through Success Typing, but there may be a better way. Something like what [Roy](http://roy.brianmckenna.org/) [programming languages that tries to meld JavaScript semantics with some features common in static functional languages] is doing in its type system or [Clojure’s core.typed](https://github.com/clojure/core.typed). I am not sure, but it’s a fun and solvable problem.

**Do you think it would be worthwhile adding algebraic data types to the Erlang VM? Or is using records (Erlang, Joxa) and tagged maps (Elixir) enough for all practical purposes?**

Type systems have very little to do with the VM and very much to do with the language. That is, it’s usually a compile time thing rather than a runtime thing. It might actually be useful to add, simply so that BEAM can take advantage of the type annotations to run more optimized versions of the code, but it’s not especially helpful to the efforts to run a well typed language on top of the VM.

**In the past we had to create a few clients and console applications. Python and Ruby were great for building them quickly. However not being able to easily generate standalone binaries for each OS and architecture is a shortcoming of those languages. We are testing Nim and Go since they have good cross compilation and library support. Have you tried them? Could OCaml be a good alternative?**

I have not tried either Nim or Go unfortunately. I have used Python extensively and Ruby as well, though to a lesser extent. I have also used OCaml extensively for these types of work and I find that I like OCaml the best. I like it for all the reasons I talked about above. That said, it is very different from other shell programming approaches and takes a bit of getting used to. I should also note that the vast majority of my work with OCaml has been in conjunction with [Jane Street Capital’s Core and Async libraries](https://janestreet.github.io/).

**What other languages or technologies are you keeping an eye on that we should check?**

I haven’t seen any new languages pop up recently that have grabbed my interest. On technologies, I think that microkernels are very, very interesting. Things like [OSv](http://osv.io/) for the JVM based languages, [Mirage](https://mirage.io/) for OCaml and [BSD Rump Kernels](http://rumpkernel.org/) for the rest. I think those are going to become the fundamental building block of **system orchestration** in the very near future. The other thing to keep an eye on is the [Nix Package manager](https://nixos.org/nix/), [NixOS](https://nixos.org/), and technologies like Atlas from Hashicorp. It’s not going to be too much longer before we declaratively describe out systems as well as our code. I am looking forward to that.