---
title: Is Ruby.nil? or Ruby.hype.dead? A current overview of Ruby 
type: docs
Date: 2014-03-26
author: unbalancedparentheses
---
### The main languages I’ve used this year

My last post was a really short mention of the resources I have used to learn and develop my skills in JavaScript and Erlang. On this post I’m going to share with you some thoughts I have regarding Ruby’s present.

## Ruby

Oh my dear Ruby, son of Lisp, Perl, Eiffel and Smalltalk, time has passed but I think you still are such an amazing creature. Sure, you need to resolve some issues. Everbody does. Some really intelligent doctors have stated that you are sick, that you might be dying, others are not quite sure about that:

* [Is Ruby Dying?](http://jmoses.co/2013/12/21/is-ruby-dying.html)
* [Introducing Rubinius X](http://jmoses.co/2013/12/21/is-ruby-dying.html)
* Good clarification about the previous post: [Rubinius seeks to modernize, not bury, the Ruby language](http://www.infoworld.com/t/ruby-rails/rubinius-seeks-modernize-not-bury-the-ruby-language-229445)
* [Discussion on Hacker News about Introducing Rubinius X](https://news.ycombinator.com/item?id=6553767)
* [Discussion on Reddit about the Hacker News discussion](http://www.reddit.com/comments/1oi8wd)
**********

### Quantitative analysis

Let’s start with some graphs. Everybody loves graphs!

![](https://cdn-images-1.medium.com/max/800/1*QCE2zU-R9MKX6Sqj5lZNyQ.png)

**Programming language popularity indexes**

[Ruby seems to be the second language](http://adambard.com/blog/top-github-languages-for-2013-so-far/) with more repositories (218812) created (without counting forks) between January 1(st and Aug 30th of 2013 on Github. JavaScript is the first with 264131 repositories created.

As reported by the [TIOBE Index](https://en.wikipedia.org/wiki/TIOBE_index), Ruby has descended one position on its ranking during this year. On January 2013, Ruby was the eleventh most used language and is now the [twelfth](http://www.tiobe.com/index.php/content/paperinfo/tpci/index.html).

[TIOBE ratings for Ruby](http://www.tiobe.com/index.php/content/paperinfo/tpci/Ruby.html)

![](https://cdn-images-1.medium.com/max/800/1*gt6Mt5Fq20JpvL3DNwYBNA.png)

According to the [Popularity of Programming Language index](https://cdn-images-1.medium.com/max/800/1*gt6Mt5Fq20JpvL3DNwYBNA.png) (PYPL), Ruby is the ninth most popular programming language. PYPL is created by analyzing how often language tutorials are searched on Google.

According to [RedMonk Programming Language Rankings](http://redmonk.com/sogrady/2014/01/22/language-rankings-1-14/), at this moment, Ruby is the seventh most popular language based on correlations between GitHub’s and Stack Overflow’s rankings.
*******************
### Other popularity indicators

Let’s move onto other indicators.

Ruby conferences registered by [lanyrd](http://lanyrd.com/) per year:

* 2009: [16](http://lanyrd.com/topics/ruby/2009/)
* 2010: [50](http://lanyrd.com/topics/ruby/2010/)
* 2011: [139](http://lanyrd.com/topics/ruby/2011/)
* 2012: [188](http://lanyrd.com/topics/ruby/2012/)
* 2013: [207](http://lanyrd.com/topics/ruby/2013/)

The absolute number of conferences kept on growing, however the growth speed diminished.

[Module counts](http://lanyrd.com/topics/ruby/2009/)

![](https://cdn-images-1.medium.com/max/800/1*syyHrXaZtCo9O1V6iTtTEQ.png)

The number of modules seems to have kept growing at about the same speed for the last years.

[Ruby’s lines of code](https://www.ohloh.net/p/ruby)

![](https://cdn-images-1.medium.com/max/800/1*Dibiyr75hsGDLCatZTiRxQ.png)

Ohloh reports that the Ruby language number of lines has grown at a steady speed.

[Interest over time reported by Google Trends](https://www.google.com/trends/explore#cat=0-5&q=%2Fm%2F06ff5&cmpt=q):

![](https://cdn-images-1.medium.com/max/800/1*bHHmLXNdXqaT9IbDoEcfpg.png)

However the interest over time registered by Google seems to have plunked.

[Ruby and Javascript Indeed job trends (absolute)](http://www.indeed.com/jobtrends?q=ruby)

![](https://cdn-images-1.medium.com/max/800/1*kFX3kYHAfcR7_NuXO0PtbA.png)

[Ruby and Javascript Indeed job trends (relative)](http://www.indeed.com/jobtrends?q=ruby%2C+javascript&l=&relative=1)

![](https://cdn-images-1.medium.com/max/800/1*IwJKDz6AwEQxl93gKlvM7g.png)

Nevertheless, even if Ruby job postings are way down compared to JavaScript, they are still on the rise and its percentage growth continues to be quite important. In the startup world, there are at least [five startups](https://angel.co/javascript/jobs) registered on AngelList that need a JavaScript developer for [each startup](https://angel.co/ruby/jobs) that needs at least someone with Ruby knowledge.

These statistics show that even though Ruby is not as popular as it was a few years ago, it’s still a big player.
*************
### Qualitative data

So far, we have seen some numbers. Now it’s a good time to check other indicators that account for liveliness of Ruby.

![](https://cdn-images-1.medium.com/max/800/1*P_BdP89ac9dcWAMDTBoaPw.png)

Let me mention a few interesting tools created in Ruby apart from Rails and other web frameworks such as Sinatra or Padrino. I think this might give an idea about Ruby’s usage. [Homebrew](http://brew.sh/), coded in Ruby, is the simplest and more useful package manager for Mac OS X. Even if I prefer other alternatives, created in Nodejs, such as [Docpad](https://github.com/bevry/docpad), Jekyll, which is coded in Ruby, was one of the main reasons for the popularity of static page generators. [Discourse](http://www.discourse.org/), the best discussion platform. [Artoo](http://artoo.io/) is a micro-framework for robotics. I don’t know a heck about it. Ruby also has got the two most used IT automation tools that exist: [Chef](http://www.getchef.com/chef/) and [Puppet](http://puppetlabs.com/). These are two really big players that won’t disappear anytime soon and it doesn’t look as though they want to move away from Ruby.

Regarding books, Ruby always had great and fascinating books such as:

* [Why’s (Poignant) Guide to Ruby](http://mislav.uniqpath.com/poignant-guide/)
* [Confident Ruby](http://www.confidentruby.com/)
* [Practical Object-Oriented Design in Ruby (POODR)](http://www.poodr.com/)
* [Eloquent Ruby](http://eloquentruby.com/)
* [Exceptional Ruby: Master the Art of Handling Failure in Ruby](http://pragprog.com/book/ager/exceptional-ruby)
* [The Well-Grounded Rubyist](http://www.manning.com/black2/)

And now we got [Ruby under a microscope](http://patshaughnessy.net/ruby-under-a-microscope). One of the latest and greatest books about Ruby that keeps up with this tradition of precious books.

Last but not least, let’s see some Ruby talks:

* [Ruby-Core dilemmas by Marc-André Lafortune](https://www.youtube.com/watch?v=fyq8Z6E5E14)
* [Towards Tooling; A Look at What is Missing From our Toolbox](https://www.youtube.com/watch?v=wPu2kVI09Yc)
* [Ruby On Robots Using Artoo by Ron Evans](https://www.youtube.com/watch?v=mhXyNGX38mw)
* [Thinking about Machine Learning with Ruby by Bryan Liles](https://www.youtube.com/watch?v=mhXyNGX38mw)
* [Machine Learning with Ruby](http://youtu.be/cO26TjhQWvA)
* [To Know A Garbage Collector](http://michaelrbernste.in/2013/06/10/to-know-a-garbage-collector-goruco-2013.html)

As we can see, very different pieces of software are crafted with Ruby and Ruby developers seem to be attracted by very different things. From my point of view these are only examples that show how much life there is on the Ruby world. In my humble opinion Ruby is not going to die anytime soon. I think that, while Ruby’s hype is dead, the language and its great community have a great future.

********************

### Ruby’s hardest problem

This doesn’t mean there are no problems on Ruby. Ruby seems to be in the same order of magnitude in terms of speed than [Python, PHP, and Perl](http://www.unlimitednovelty.com/2012/06/ruby-is-faster-than-python-php-and-perl.html) but in general Ruby it is not the fastest kid on the block. Even Matsumoto, its creator, admits it!

>_[Yukihiro Matsumoto details the past, present, and future of the popular programming language, calling mobile ‘the way to go’](http://www.infoworld.com/d/application-development/infoworld-interview-ruby-creator-sets-sights-mobile-171503?page=0,1)_
>
>_**InfoWorld**: Are there any limitations with developing Ruby applications?_
>
>_**Matsumoto**: Well, in some cases, performance could be the limitation. For example, Twitter was originally written in Ruby, but it has now has billions of users so, it’s larger, its core [is now] on top of the JVM. It was originally running on C Ruby, my Ruby. [With Twitter’s JVM-based program], the program is written in Scala and Clojure_.

I think Matsumoto represents very well the self-critical spirit of the Ruby community. But it would be great if we could sidestep the performance problem. One way to do it is with concurrency and parallelism.

**Ruby implementations**

Some say that:

>_Both Python and Ruby have full support for multi-threading. There are some implementations (e.g. CPython, MRI, YARV) which cannot actually run threads in parallel, but that’s a limitation of those specific implementations, not the language. This is similar to Java, where there are also some implementations which cannot run threads in parallel, but that doesn’t mean that Java is single-threaded. Note that in both cases there are lots of implementations which can run threads in parallel: PyPy, IronPython, Jython, IronRuby and JRuby are only few of the examples. [Are languages like python and ruby single threaded unlike say java?](https://stackoverflow.com/questions/3086467/confused-are-languages-like-python-ruby-single-threaded-unlike-say-java-for)_

But come on, that’s pure theory. Almost everybody uses MRI, the main Ruby implementation. I think that the main reason is that it’s [reliable](http://andrewjgrimm.wordpress.com/2013/07/08/mri-boring-but-reliable/). However the issue with MRI is that it:

>_has something called a global interpreter lock (GIL). It’s a lock around the execution of Ruby code. This means that in a multi-threaded context, only one thread can execute Ruby code at any one time. So if you have 8 threads busily working on a 8-core machine, only one thread and one core will be busy at any given time. The GIL exists to protect Ruby internals from race conditions that could corrupt data. There are caveats and optimizations, but this is the gist. [Nobody understands the GIL](http://www.jstorimer.com/blogs/workingwithcode/8085491-nobody-understands-the-gil):_

With GIL or without it (read 2[012: The Year Rubyists Learned to Stop Worrying and Love Threads](http://tonyarcieri.com/2012-the-year-rubyists-learned-to-stop-worrying-and-love-the-threads)) it’s pretty clear that:

>_the single-core nature of the canonical Ruby interpreter, MRI (the “Matz Ruby Interpreter”), is limiting Ruby’s potential applications._

[Parallelism is a Myth in Ruby](http://www.igvita.com/2008/11/13/concurrency-is-a-myth-in-ruby/). While we wait for this to get solved for MRI we can check out [why critics of Rails have it all wrong (and Ruby’s bright multicore future)](http://www.unlimitednovelty.com/2012/03/why-critics-of-rails-have-it-all-wrong.html) and learn how to overcome this problem:

* Rubinius is an implementation of Ruby designed for concurrency using native threads to run Ruby code on all the CPU cores. It also has a low-pause generational garbage collector, LLVM-based just-in-time (JIT) native machine code compiler. Pretty badass piece of technology. It’s mainly implemented on Ruby. [Chicken or the egg?](https://en.wikipedia.org/wiki/Chicken_or_the_egg). But it has a big downside: Rubinius currently implements MRI Ruby version 1.8.7. MRI Ruby 1.8.7 was released on April 2008, that’s more than 5 years ago. From what I have read Rubinius is working on implementing the upcoming MRI 2.1 release.
* JRuby runs your Ruby code on the JVM, and that’s why we get real threading. Latest version of JRuby is compatible with MRI Ruby 1.8.7 and 1.9.3. If you haven’t got enough links yet start reading [Concurrency in JRuby](http://blog.engineyard.com/2011/concurrency-in-jruby). [JRuby: Insights from Six Years in Production](http://youtu.be/t_JJD-AEtNM) and [The Future of JRuby by Charles Nutter and Thomas Enebo](http://youtu.be/4TAwkq0aQdE).

Also you got [Visualizing Garbage Collection in Rubinius, JRuby and Ruby 2.0](http://youtu.be/yl_zYzPiDto) that will be useful for any of the three implementation we mentioned.

Even if it’s a synthetic benchmark, this comparison between [MRI 1.9.3, MRI 2.0.0, MRI 2.1.0dev JRuby 1.7.4 and Rubinius 2.0.0](http://miguelcamba.com/blog/2013/10/05/benchmarking-the-ruby-2-dot-1-and-rubinius-2-dot-0/) has some interesting information.

Again I think it’s a good idea to hear what Matsumoto thinks:

>_**InfoWorld**: What’s your perspective on alternative Ruby implementations just as JRuby and Rubinius?_
>
>_**Matsumoto**: I don’t see any problem about other implementations just because the diversity is very sound, the healthy things they have. And actually Ruby, the language, is very good for productivity but the programming environment differs from application to application. For example, some clients require very stable and multicore applications on top of the JVM. In that kind of field, JRuby works better than my Ruby, actually, which is called C Ruby. For most of the cases, C Ruby is good for Web applications. But in certain situations, JRuby and maybe Rubinius are a better fit for a particular requirement._

**********
### We need a concurrency model!

Despite having real concurrency or being able to run code in many cores, doesn’t mean it’s easy to do so:

[Why Johnny Can’t Write Multithreaded Programs](http://blog.smartbear.com/programming/why-johnny-cant-write-multithreaded-programs/):

>_Introductory multithreading materials explain what threads are. Then they launch into discussions of how to make those threads work together in various ways, such as controlling access to shared data with locks and semaphores, and perhaps controlling when things happen with events. There’s detailed discussion of condition variables, memory barriers, critical sections, mutexes, volatile fields, and atomic operations. You’re given examples of how to use those low level constructs to do all manner of systems level things. By the time a programmer is halfway through that material, she thinks she knows how to use those primitives in her applications. After all, if you understand how to use something at the systems level, using it at the application level should be trivial, right?_
>
>_This is like teaching a teenager how to build an internal combustion engine from discrete parts and then, without the benefit of any driving instruction, setting him behind the wheel of a car and turning him loose on the roads. The teenager understands how the car works internally, but he has no idea how to drive it from point A to point B._
>
>_Knowing how threads work at the systems level is mostly irrelevant to understanding how to use them in an application program. I’m not saying that programmers shouldn’t know how things work under the hood, just that they shouldn’t expect that knowledge to be directly applicable to the design or implementation of a business application. After all, knowing the details of the intake, compression, combustion, and exhaust cycle doesn’t help you in getting from home to the grocery store and back._
>
>_Introductory multithreading textbooks (and computer science courses) shouldn’t be teaching those low level constructs. Rather, they should concentrate on common classes of problems and show developers how to use higher level constructs to solve those problems. For example, a large number of business applications are in concept extremely simple programs: They read data from one or more input devices, apply some arbitrarily complex processing to that data (perhaps querying some other stored data in the process), and then output the results._

You need a concurrency model or you will keep recreating one on each project. In my humble opinion, these words about Javascript written by Joe Armstrong, creator of Erlang, apply almost perfectly for Ruby too even if it has [threads, process and fibers](http://blog.smartbear.com/programming/why-johnny-cant-write-multithreaded-programs/).

[Red and Green Callbacks](http://blog.smartbear.com/programming/why-johnny-cant-write-multithreaded-programs/):

>_It’s actually worse, every Javascript programmer who has a concurrent problem to solve must invent their own concurrency model. The problem is that they don’t know that this is what they are doing. Every time a Javascript programmer writes a line of code that says “on this do that” they are actually inventing a new concurrency model, and they haven’t a clue how the code will interleave when it executes._
>
>_(I actually have a love-hate relationship with Javascript, most parts I love but I hate the concurrency model- that might sound funny since Javascript has no concurrency model — so there’s nothing to hate :-)_
>
>_What’s even more difficult to understand is errors. Errors in multi-threaded callback code with shared memory is something that would give me an extremely large headache._

The best concurrency model I know in the Ruby land is the one provided by the family of [Celluloid](https://github.com/celluloid/celluloid). Celluloid, with the help of its sons [DCell](https://github.com/celluloid/dcell) — this is the best one — and C[elluloi-io](https://github.com/celluloid/celluloid-io), tries to help you achieve the nirvana described by these two famous quotes:

>_“I thought of objects being like biological cells and/or individual computers on a network, only able to communicate with messages” — **Alan Kay, creator of Smalltalk, on the meaning of “object oriented programming”**_
***********
>_“Objects can message objects transparently that live on other machines over the network, and you don’t have to worry about the networking gunk, and you don’t have to worry about finding them, and you don’t have to worry about anything. It’s just as if you messaged an object that’s right next door.” -**Steve Jobs describing the NeXT Portable Distributed Object system**_
**************
**Why I like Ruby**

![](https://cdn-images-1.medium.com/max/800/1*U24Mc9wNeiSNp2_eOYIGWg.png)

Time to land on Earth. We have spent too much time floating on the sky. I have never explained why I use and like Ruby.

In comparison with other dynamic type languages it has an awesome set of tools as [rubocop](https://github.com/bbatsov/rubocop), [ruby-lint](https://github.com/bbatsov/rubocop) and [reek](https://github.com/troessner/reek) that help you analyze your code. Furthermore it has the best [REPL](https://github.com/troessner/reek) that I am aware of: [pry](http://pryrepl.org/). Watch [REPL driven development with Pry](http://youtu.be/D9j_Mf91M0I) and [Pry, The Good Parts!](http://youtu.be/D9j_Mf91M0I) to learn how it can help you speed up your development.

On the other hand, as I mentioned before, it’s not as fast as C or Java but Ruby core developers are doing an excellent work on that area. For example, check the history of improvements to the [Ruby garbage collector](http://tmm1.net/ruby21-rgengc/).

Even though I sometimes think that it’s Perl-inherited attitude of having more than [one way to do the same thing](http://www.senktec.com/2013/09/one-way-to-do-it/) is overwhelming (for some reason I need to reread [Understanding Ruby Blocks, Procs and Lambdas](http://www.robertsosinski.com/2008/12/21/understanding-ruby-blocks-procs-and-lambdas/)every month or so), in general I really like its syntax and semantics. I think it’s a very expressive and intuitive language. In my opinion how good a language is is not defined only by semantics, syntax and performance. I am not an academic. I have not even finished university.

[The Master, The Expert, The Programmer](http://zedshaw.com/essays/master_and_expert.html):

>_Programming is a very new discipline, so there’s not too many master programmers out there. What’s worse is that the few people I would consider masters aren’t very exemplary of the software profession and art. They are typically professors who never write anything under a deadline and are given complete artistic freedom to develop whatever they want._

I am not saying this in any mean way. I have huge respect for professors and researchers. But I am not one. I code stuff on a deadline. That’s why for me the community and the tools that it has created are really important. They are the salt and pepper of what I am cooking. This is why I love Ruby. It has a lot of salt and pepper. I particularly like Rails, the most known Ruby framework, and its cut down version [Rails API](https://github.com/rails-api/rails-api), because of the following features:

[Debunking the Node.js Gish Gallop](http://www.unlimitednovelty.com/2012/08/debunking-nodejs-gish-gallop.html):

>_**Handled at the middleware layer:**_
>
>_Reloading: Rails applications support transparent reloading. This works even if your application gets big and restarting the server for every request becomes non-viable._
>
>_Development Mode: Rails application come with smart defaults for development, making development pleasant without compromising production-time performance._
>
>_Test Mode: Ditto test mode._
>
>_Logging: Rails applications log every request, with a level of verbosity appropriate for the current mode. Rails logs in development include information about the request environment, database queries, and basic performance information._
>
>_Security: Rails detects and thwarts IP spoofing attacks and handles cryptographic signatures in a timing attack aware way. Don’t know what an IP spoofing attack or a timing attack is? Exactly._
>
>_Parameter Parsing: Want to specify your parameters as JSON instead of as a URL-encoded String? No problem. Rails will decode the JSON for you and make it available in params. Want to use nested URL-encoded params? That works too._
>
>_Conditional GETs: Rails handles conditional GET, (ETag and Last-Modified), processing request headers and returning the correct response headers and status code. All you need to do is use the stale? check in your controller, and Rails will handle all of the HTTP details for you._
>
>_Caching: If you use dirty? with public cache control, Rails will automatically cache your responses. You can easily configure the cache store. HEAD requests: Rails will transparently convert HEAD requests into GET requests, and return just the headers on the way out. This makes HEAD work reliably in all Rails APIs._
>
>_**Handled at the ActionPack layer:**_
>
>_Resourceful Routing: If you’re building a RESTful JSON API, you want to be using the Rails router. Clean and conventional mapping from HTTP to controllers means not having to spend time thinking about how to model your API in terms of HTTP._
>
>_URL Generation: The flip side of routing is URL generation. A good API based on HTTP includes URLs (see the GitHub gist APIfor an example)._
>
>_Header and Redirection Responses: head :nocontent and redirectto userurl(currentuser) come in handy. Sure, you could manually add the response headers, but why?_
>
>_Caching: Rails provides page, action and fragment caching. Fragment caching is especially helpful when building up a nested JSON object. Basic, Digest and Token Authentication: Rails comes with out-of-the-box support for three kinds of HTTP authentication._
>
>_Instrumentation: Rails 3.0 added an instrumentation API that will trigger registered handlers for a variety of events, such as action processing, sending a file or data, redirection, and database queries. The payload of each event comes with relevant information (for the action processing event, the payload includes the controller, action, params, request format, request method and the request’s full path)._
>
>_Generators: This may be passé for advanced Rails users, but it can be nice to generate a resource and get your model, controller, test stubs, and routes created for you in a single command._
>
>_Plugins: Many third-party libraries come with support for Rails that reduces or eliminates the cost of setting up and gluing together the library and the web framework. This includes things like overriding default generators, adding rake tasks, and honoring Rails choices (like the logger and cache backend)._

These features are priceless when a deadline is near cutting your head like a guillotine. I think these features are partly the reason [Why Targeter moved from NodeJS to RoR](http://blog.targeterapp.com/post/22984987832/why-we-moved-from-nodejs-to-ror). This doesn’t mean I don’t like Nodejs, but that discussion is beyond the scope of this post.

For example, this week, a client asked me to create a button on the admin site so that they could get the emails from the users of the application. It took me less than ten minutes to do it:

`invitations_emails = InvitationRequest.pluck(:email)`
`user_emails = User.pluck(:email)`
`content = (invitations_emails + user_email).to_csv`
`send_data content, filename: “emails.csv”`

However all this magic ain’t always great, in particular when you have performance issues or library-related bugs. But in most cases it will save you from reinventing the wheel.

In this same line devise (authentication), paperclip (file attachment library that helps you treat files as any other attribute), kaminari (paginator), geocoder (really simple and complete geocoding solution), koala (Facebook library supporting the Graph and REST API), aws-sdk (self-explanatory), tire (API and DSL for the Elasticsearch search engine) or other alternatives that you can find in [The Ruby Toolbox](https://www.ruby-toolbox.com/) will make you cry of joy if your non techincal boss ask you to add a new functionality in a couple of hours.

Also, cucumber and rspec will be of great utility to check that the commit you did on Monday morning while you are really sleepy won’t destroy everything. I also wanted to mention RVM — the greatest version manager of any language I know — and Ruby Gems. After Nodejs great npm package manager, this is the best one I know.

At last, I wanted to show you [watir-webdriver](http://watirwebdriver.com/). I bet you can’t find a better api to drive a browser. This is a perfect example of how rich and simple Ruby libraries are:

`require ‘watir-webdriver’`
`b = Watir::Browser.new`
`b.goto ‘bit.ly/watir-webdriver-demo’`
`b.text_field(:id => ‘entry_0').set ‘your name’`
`b.select_list(:id => ‘entry_1').select ‘Ruby’`
`b.select_list(:id => ‘entry_1').selected? ‘Ruby’`
`b.button(:name => ‘submit’).click`
`b.text.include? ‘Thank you’`

I have not mentioned before that Ruby is also an excellent language for creating prototypes or for doing IT related work like dealing with files and parsing strings.

Finally, if you need to create a webpage or a REST API, specially when you don’t need real-time stuff, with limited time and money I can’t currently imagine a better replacement for Ruby and Rails.
************
### Closing words

I won’t deny that Ruby has lost some momentum. But that doesn’t mean the language is dead. [I think it has matured](http://www.codinghorror.com/blog/2013/03/why-ruby.html) and that it is a great tool to add to your arsenal as a developer.

[frycicle](http://www.reddit.com/r/programming/comments/1oi8wd/ruby_is_a_dying_language/?utm_source=rubyweekly&utm_medium=email) explained it really well in a few words.

>_Shoot, in terms of libraries, language design, and community, Ruby has it all. It’s won’t scale to 10 website levels. And that is ok for it’s use. They usually have to make super specialized software anyways._

Even accepting that that Ruby is not dying, we could ask ourselves [why hasn’t Ruby won](http://www.reddit.com/r/programming/comments/1oi8wd/ruby_is_a_dying_language/?utm_source=rubyweekly&utm_medium=email)?. The answer is that:

>_**Sarah Mei**: this is a game where there is no winning turns out, but there is losing, there is definently losing. […] The best thing that you can do for Ruby is go learn something else and then come back._
>
>[@bascule](https://twitter.com/bascule) daily amazed at the number of ruby people I come across in the scala and Go worlds too. Ruby opens your mind.
>
>    — Paul Lamb (@PaulLamb) [January 23, 2014](https://twitter.com/PaulLamb/statuses/426338111094661120)

That’s why you should learn some Python, [Go](http://blog.grok.se/2013/10/on-comparing-languages-c-and-go/), Rust, Erlang, Scala, Haskell, Clojure or any Lisp dialect. Oh, and keep an eye on [Julia](http://julialang.org/) and [Elixir](http://elixir-lang.org/)!
