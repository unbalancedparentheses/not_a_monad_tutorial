# Become an Erlang Cowboy and tame the Wild Wild Web — Part I
#### Erlang: From zero to coding a commenting system

Jun 18, 2014

#### Objective

In the following series of posts we will be creating a commenting system like http://disqus.com/. The system will have normal HTTP/REST handlers but also some SSE and websockets handlers, and background jobs for uploading images to Amazon S3 and sending push notifications to iOS and Android clients via Amazon SNS. At the end of the series, we will connect Erlang and the system with other programming languages for those tasks that can be more difficult to do with Erlang.

By no means will this be an attempt to create a new reference for learning the syntax, types and functions of the language. There are already three great books that cover that purpose:

* Erlang Programming by Francesco Cesarini and Simon Thompson
* Programming Erlang: Software for a Concurrent World — Joe Armstrong
* [Learn you some Erlang for Great Good — Fred Hébert](http://learnyousomeerlang.com/)

At some point you will need to read them if you want to work with Erlang. My objective is to create a “Hands on Erlang” guide, and hopefully a small book, where we use the most important and useful Erlang concepts to create a working and real system so that you can see by yourself how highly scalable, concurrent, parallelizable, battleproof and especially how well designed the Erlang platform is. I separate the Erlang language from its plataform even if they are intertwined because you could change Erlang’s syntax or be using another language that runs on top of the BEAM like Elixir or LFE (Lisp Flavored Erlang) and still get almost all, if not all, of the Erlang benefits and even some mores. Don’t get me wrong. I really like the language per se. But its real power comes from its ecosystem and from the system in general. From my point of view, C++, Java, C#, Objective-C, Python, Ruby and even Javascript are very similar. Sure, they have different syntaxes and small different ways of doing the same thing. But you do not have to learn a new way of thinking when changing from one to another. You can learn the syntax of Erlang language in only a few hours but you will not be able to learn the concurrent/fault tolerant paradigm in one day. Nobody learnt Object Oriented paradigm in that lapse of time. If you are looking to learn a new syntax or a new rails-like framework you have come to the wrong place.

If you are interested on moving out of your comfort zone, you have come to the right place. I will do my best to help you learn a new and different way of thinking and designing applications. The length of the journey however will depend entirely on your will. You will have to play, reimplement the same idea in different ways and obviously fight with a new compiler to conquer victory.

**Audience**

This series of posts is oriented towards developers that need to create backend servers that normally use languages and frameworks such as Python and Flask, Twisted, Celery; Ruby and Rails/Sinatra/Grape, Sidekiq/Rescue, Concurrent Ruby with JRuby or Rubinius; Javascript with Nodejs, Express/Koa or Go. I have worked with these technologies for some years, creating HTTP servers that produced JSON consumed by single page applications, iOS and Android clients. I was very comfortable with them. But for the last year I have been using Erlang, and even if I still like Ruby, Python and Javascript, I have no regrets when I say that Erlang is superior, in most areas, for building these types of systems.

My idea is to show how easily and cleanly you can create a distributed, resilient system thanks to Erlang semantics, its awesome BEAM virtual machine and some great libraries like Cowboy. If I cannot convince you to use Erlang on your next project, then I hope that at least you respect its awesome power.

On this first post I will show you some basic Erlang code so that in next one I can start working on the first handlers of our system.

**Requirements**
* Erlang 17 installed. Check [Erlang Solutions downloads page](https://www.erlang-solutions.com/downloads/download-erlang-otp)
* [Basic http](http://robots.thoughtbot.com/back-to-basics-http-requests) knowledge

Before starting I must say that I am very thankful to my employer —[Inaka](http://inaka.net/) — for letting me write part of these posts during my working hours.

**The ugly duckling?**
Programming languages have a defined set of goals. Most of them put **performance, developer expressiveness or developer productivity at the top of the list**. Let’s see what they have to say about themselves so that we can compare with Erlang’s description:

>_**Rust** is a systems programming language that **runs blazingly fast, prevents almost all crashes**, and eliminates data races. — http://www.rust-lang.org/_
>
>_**Java** is designed to enable development of portable, **high-performance applications** for the widest range of computing platforms possible. — https://www.java.com/en/about/_

![](https://cdn-images-1.medium.com/max/400/1*EPh-KvfabwINW5psttwbig.png)    
>_**Haskell** is an advanced purely-functional programming language. An open-source product of more than twenty years of cutting-edge research, **it allows rapid development of robust, concise, correct software**. With strong support for integration with other languages, built-in concurrency and parallelism, debuggers, profilers, rich libraries and an active community, Haskell makes it easier to produce flexible, maintainable, high-quality software. — http://www.haskell.org/haskellwiki/Haskell_
>
>_**Python** is a programming language that lets you **work quickly and integrate systems more effectively.** — https://www.python.org/_
>
>_**Ruby** is a dynamic, open source programming language with a **focus on simplicity and productivity**. It has an **elegant syntax that is natural to read and easy to write.** — https://www.ruby-lang.org/en/_
>
>_The only reliable plan is to design for performance. Performance doesn’t mean speed; that’s taking the metaphor too literally. Speed counts, but a programming language is first of all a tool for thinking in. We want thinking in **Arc** to **feel like driving a 911.**— http://www.paulgraham.com/design.html_

![](https://cdn-images-1.medium.com/max/800/1*ezjVLCDsv-Qur7C2BvhnsQ.jpeg)
`1973 Porsche 911E`

>_The **Go** programming language is an open source project to **make programmers more productive**. Go is **expressive, concise, clean, and efficient**. Its concurrency mechanisms make it **easy to write programs that get the most out of multicore and networked machines**, while its novel type system enables flexible and modular program construction. Go compiles quickly to machine code yet has the convenience of garbage collection and the power of run-time reflection. It’s a **fast, statically typed, compiled language that feels like a dynamically typed, interpreted language.** — http://golang.org/doc/_

Let’s see what Erlang has to say about itself:

>**Erlang** is a programming language **used to build massively scalable soft real-time systems with requirements on high availability**. Some of its uses are in telecoms, banking, e-commerce, computer telephony and instant messaging. Erlang’s runtime system has built-in support for concurrency, distribution and fault tolerance. — http://www.erlang.org/

Erlang seems to be the ugly duckling compared to other programming languages since it doesn’t describe itself as being fast, clean or expressive.
![](https://cdn-images-1.medium.com/max/800/1*ezjVLCDsv-Qur7C2BvhnsQ.jpeg)
Erlang was created for building fault-tolerant systems. This is natural since Erlang’s roots are in the telecommunication world. Most important design choices of the language were taken to fulfill this requirement. However this does not mean it is not clean or expressive. Let’s take a closer look.
####Syntax

[Erlang grammar](https://github.com/erlang/otp/blob/maint/lib/stdlib/src/erl_parse.yrl#L-0-L-535) is simple, it has less than 550 lines of code. That makes erlang syntax easy to understand even if it is different from mainstream languages. But what is more important it is really consistent. Enough talk:

>%% this is a comment
>-module(foo).     %% we define a module called foo
>-export([bar/0]). %% and export the function bar that has 0
>                  %% arguments
>
>bar() ->          %% we define the function bar
> io:format("Hello World!~n").`

Lets compile it and run inside the erlang shell the bar function from module foo.

>$ erlc foo.erl 
>$ erl          
>
>1> foo:bar().  %% we call the bar function
>Hello World!
>ok

Let me show you an example that is a little more complex:

>-module(test).
>-compile(export_all).
>
>%% factorial implemented as you would normally do in most
>%% imperative languages.
>fac_if(N) ->
>  if 
>    N =:= 0 ->
>      1;
>    true ->
>      N * fac_if(N - 1)
>  end.
>
>%% factorial implemented with a case. if N matches 0 it
>%% returns 0. If N matches any other value it will call
>%% fac_case with N-1
>fac_case(N) ->
>  case N of
>    0 ->
>      1;
>    N ->
>      N * fac_case(N - 1)
>  end.
>
>%% factorial implemented with function clauses
> fac(0) ->
>  1;
>fac(N) ->
>  N * fac(N - 1).

Now let’s save it as test.erl, compile it and call the erlang shell:

>$ erlc test.erl
>$ erl 

The last case uses something called pattern matching in the function head. In just a few lines you will learn more about pattern matching. Now we can call the fac function:

>2> test:fac(20).
>2432902008176640000
>
>3> test:fac(40). 
>815915283247897734345611269596115894272000000000

The syntax is not difficult. It is just different from Algol or C based syntax. As you will see there are not many reserverd words or language constructs. I think only Lisps are way simpler languages from the syntax and grammar point of view.

I wanted to add that as you have noticed erlang uses “,”, “;” and “.” as terminators. I am not a big fan of them, since I have to change them when moving lines of code up or down. In general I like indentation a la Python as a way to delimit blocks of code. However Erlang terminators are not a big pain in the a** once you get used to them. Also I agree with Robert Virding, co creator of Erlang, that Erlang syntax is small, simple, regular and concise. It is difficult not to agree with that even if you do not like Erlang syntax. All the mainstream languages I have used (e.g. C++, Java, C# or Objective-C) have a much more complex and less consistent syntax. Returning to the terminator issue, Fred Hebert has a [great article](https://github.com/erlang/otp/blob/maint/lib/stdlib/src/erl_parse.yrl#L-0-L-535) with some tips on how to understand how to use and read them.

Let’s move onto more important things:

####Expressiveness
When thinking about Erlang expressiveness the first thing that comes to mind is message passing, process creation and management. Nevertheless pattern matching is a big player too in this field and serves a big purpose in making things easier for receiving messages. Let’s start by showing a simple example of pattern matching before moving on to message passing and process management.

>4> Body.
>* 1: variable 'Body' is unbound
>
>5> Headers.
>* 1: variable 'Headers' is unbound

First we tried to access the content of Body and Headers variables (all variables in Erlang must start with a capital letter). The shell answers with the obvious: the variables are unbound. Now it is time to do something more interesting.

**Exit the shell we have being using up to now doing twice Control-C. Since we are going to use an http library (ibrowse) on the following example, instead of setting up everything:**

>$ git clone git@github.com:unbalancedparentheses/erlskeletor_cowboy.git
>$ cd erlskeletor_cowboy/
>$ make

Make will fetch all the dependencies and compile the project. Up to now we executed erl to launch the erlang shell. Now we are going to use:

>$ make shell

Make shell will start erlang and all the dependencies we need. You will see a lot of output. Now we can return to our work:

>1> ibrowse:send_req("http://www.google.com/", [], get).
>
>{ok,"302",
>[{"Cache-Control","private"},
>{"Content-Type","text/html; charset=UTF-8"},
>{"Location",
>"https://www.google.com.ar/?gfe_rd=cr&ei=Nk11U4zsFIeF8Qf6hoC4BA"},
>{"Content-Length","263"},
>{"Date","Thu, 15 May 2014 23:26:46 GMT"},
>{"Server","GFE/2.0"},
>{"Alternate-Protocol","443:quic"}],
>"<HTML><HEAD><`meta http-equiv=\"content-type\" content=\"text/html;charset=utf-8\">\n<TITLE>302 Moved</TITLE></HEAD><BODY>\n<H1>302 Moved</H1>\nThe document has moved\n<A HREF=\"https://www.google.com.ar/?gfe_rd=cr&amp;ei=Nk11U4zsFIeF8Qf6hoC4BA\">here</A>.\r\n</BODY></HTML>\r\n"`}

We call the [send_req](https://github.com/cmullaparthi/ibrowse/blob/master/src/ibrowse.erl#L-165) function from the module [ibrowse](https://github.com/cmullaparthi/ibrowse), an HTTP erlang client. The first argument is a string with the URL. Strings are a [linked list of integers](http://www.erlang.org/faq/academic.html#id58248) since Erlang doesn’t have a real string type. It is not very common to use strings in Erlang. You have [atoms](https://en.wikipedia.org/wiki/Symbol_%28programming%29) ([Ruby](http://www.reactive.io/tips/2009/01/11/the-difference-between-ruby-symbols-and-strings/) or [Lisp](https://stackoverflow.com/questions/8846628/what-exactly-is-a-symbol-in-lisp-scheme) symbols), binaries and [IOLists](http://prog21.dadgum.com/70.html) . Atoms and binaries are really common and handy. For the moment this is not important but I wanted to mention this so that you do not start rambling afterwards.

The second argument of the call to send_req is a list of headers we want to send with the request. We are not sending any header in this case. At last, with the third argument, we specify the verb of the request. We used the atom get for that. Variables can not begin with a lowercase letter because atoms do. Atoms start with lower-case letters or enclosed in single quotes. As you can see we sent a get request to http://www.google.com.

Let’s inspect the result of the call:

>{ok,"302",
>[{"Cache-Control","private"},
>{"Content-Type","text/html; charset=UTF-8"},
>{"Location",
>"https://www.google.com.ar/?gfe_rd=cr&ei=Nk11U4zsFIeF8Qf6hoC4BA"},
>{"Content-Length","263"},
>{"Date","Thu, 15 May 2014 23:26:46 GMT"},
>{"Server","GFE/2.0"},
>{"Alternate-Protocol","443:quic"}],
> "<HTML><HEAD><`meta http-equiv=\"content-type\" content=\"text/html;charset=utf-8\">\n<TITLE>302 Moved</TITLE></HEAD><BODY>\n<H1>302 Moved</H1>\nThe document has moved\n<A HREF=\"https://www.google.com.ar/?gfe_rd=cr&amp;ei=Nk11U4zsFIeF8Qf6hoC4BA\">here</A>.\r\n</BODY></HTML>\r\n"`}

The result is a tuple with four elements. **{Term1,…,TermN}** in erlang denotes a tuple. Tuples have a fixed number of terms or elements. Erlang tuples are very similar to Python tuples. The first element is the atom **ok**, that lets us know that everything went fine. The second element is the string with content 302. 302 Found HTTP status code is usually used to redirect the user to somewhere else. The third element of the result is the list of headers in the answer that google sent us.

>[{"Cache-Control","private"},
>{"Content-Type","text/html; charset=UTF-8"},
>{"Location",
>"https://www.google.com.ar/?gfe_rd=cr&ei=Nk11U4zsFIeF8Qf6hoC4BA"},
>{"Content-Length","263"},
>{"Date","Thu, 15 May 2014 23:26:46 GMT"},
>{"Server","GFE/2.0"},
>{"Alternate-Protocol","443:quic"}]

As you can see the headers are represented as a list of tuples. Each tuple has two elements: a key and a value. This is a type of [proplist](http://www.erlang.org/doc/man/proplists.html).

The first header has a key of type string “Cache-Control” with a string value “private”. Also check that the “Location” key has the URL value of the redirect where google wants us to go. I know it can be different from what you are used to, but it is not that difficult to understand. After a few hours of reading proplists, tuples or lists in Erlang it will feel very natural.

The last element of the answer to the call to ibrowse:send_req is a big string with the HTML that google sent us:

`"<HTML><HEAD><meta http-equiv=\"content-type\" content=\"text/html;charset=utf-8\">\n<TITLE>302 Moved</TITLE></HEAD><BODY>\n<H1>302 Moved</H1>\nThe document has moved\n<A HREF=\"https://www.google.com.ar/?gfe_rd=cr&amp;ei=Nk11U4zsFIeF8Qf6hoC4BA\">here</A>.\r\n</BODY></HTML>\r\n"`

Now let’s store the result:

`2> {ok, "302", Headers, Body} = ibrowse:send_req("http://www.google.com/", [], get).`

![](https://cdn-images-1.medium.com/max/400/1*9mLW7UAh0eDj2ljx5GfCJw.jpeg)
`Pattern, pattern, pattern matching everything!!!`

We sent a get request to google, got an answer. ‘ok’ is an atom, and “302" a string. They are not assigned, since they are not variables. So why did we use the equal sign to assign them? Well because in Erlang the equal sign is not exactly the same as assignment. Since Erlang supports the pattern matching mechanism, the equal sign is used as a match operator. It tries to find equivalence between the two sides, and then binds values to unbound variables, thus assigning them a value.

So after getting the result from our call to ibrowse:send_req, Erlang asserts that we got a tuple with four elements that started with the elements ok and “302". Then since it knows that Headers and Body are unbound it assigned the headers proplist to the variable Headers and the HTML returned by Google to the variable Body.

>3> Headers.
>[{"Cache-Control","private"},
>{"Content-Type","text/html; charset=UTF-8"},
>{"Location",
>"https://www.google.com.ar/?gfe_rd=cr&ei=Nk11U4zsFIeF8Qf6hoC4BA"},
>{"Content-Length","263"},
>{"Date","Thu, 15 May 2014 23:26:46 GMT"},
>{"Server","GFE/2.0"},
>{"Alternate-Protocol","443:quic"}]

To sum up, pattern matching is way more than another syntax for writing your C switch or if/else/elif from Python and most languages. With pattern matching you get normal branching, conditionals on complex structure, not only by comparing simple values, and you can also extract specific values when you do the comparison or/and assignment. You [make the compiler work for you](http://deliberate-software.com/function-pattern-matching/).

At last I wanted to show a beautiful example of pattern matching where we dissect a TCP segment by its bits:

><<SourcePort:16, DestinationPort:16,
> SequenceNumber:32,
> AckNumber:32,
> DataOffset:4, _Reserved:4, Flags:8, WindowSize:16,
> Checksum:16, UrgentPointer:16,
> Payload/binary>> = TcpSegment.

Ain’t this a good example of Erlang expressiveness?

**God cannot alter the past, though historians can (and some developers too)— Samuel Butler**

Variables are either bound or unbound. As you might know values are inmutable in Erlang and once the variable is bound, you can not assign a new value to it. Only one assignment is allowed. You can not modify a variable or a value once it was created.

>4> Message = "Hello ladies!".
>"Hello ladies!"
>
>5> Message = "Die die my darling".
>** exception error: no match of right hand side value "Die die my darling"

Inmutability and single assignment at first might seem awkward, uncomfortable, but they are very useful properties since they minimize side effects. Even if it is not impossible to write Erlang code with race conditions, it is way more difficult than with general imperative and stateful languages. You will see this in the next section.

A really interesting property, helped by single assignment and inmutability, is [referential transparency](http://deliberate-software.com/function-pattern-matching/). In plain english this means that the result of a function is always the same when you provide the same arguments/input. You might think this is a property of most programming languages, but it is not.

The output of calling a method in most object oriented languages like for example C++, Java, Ruby depends on the state of the object. Previous calls to other method change the state of the object that is used by the methods you are calling. So the result will not depend only on the arguments, but also on what other methods have been called before. With referential transparency, you are on the greener side of the grass since you have some degree of determinism. Also writing test cases is way easier since, in general, you do not need to mock entire objects or use dependency injection since you only call functions with the arguments you want.

You might think we are using complicated words for showing off. But as you will see in the following posts thanks to referential transparency and pattern matching we will be able to refactor nested branching implemented with cases into calling small and simple functions. In many languages, inspired by how Java and C++ implemented OOP, the code is so damn interdependent that it is more difficult to refactor it.
####Processes and Messages

Functions in functional programming languages are first class citizens. This means that they are not discriminated. They can be assigned to variables, passed as arguments to other functions and returned as values from other functions. Say no to racism. Treat functions as any other type!
![](https://cdn-images-1.medium.com/max/800/1*FLmbrG6z0Pt25nbfeG775g.jpeg)
>6> F = fun(X, Y, Operation) -> Operation(X,Y) end.
>#Fun<erl_eval.18.106461118>
>
>7> Plus = fun(X, Y) -> X + Y end.
>#Fun<erl_eval.12.106461118>
>
>8> F(2, 2, Plus).
>4
>
>9> DividePlusTwo = fun(A,B) -> A / B + 2 end.
>#Fun<erl_eval.12.106461118>
>
>10> F(2, 2, DividePlusTwo). 
>3.0

Creating a new process in Erlang is really simple. You use the spawn primitive with the function you want to launch in another process.

>11> G = fun() -> 2 + 2 end.
>#Fun<erl_eval.20.106461118>
>
>12> G().
>4
>
>13> spawn(G).
><0.60.0>
>
>14> spawn(G).
><0.65.0>

Spawn creates the new process and returns the PID (unique Process Identifier) of that process. You might ask where does the return value go? Well apparently it disappeared. Black magic? No. Processes do not return anything. You have to send the result as a message to another. Before that let me show you that the shell itself is a process and that we can get its PID:

>15> self().
><0.32.0>
>
>16> exit(self()).
>** exception exit: <0.32.0>
>
>17> self().
><0.35.0>

With self() we got the PID of the actual process, in this case obviously the shell’s PID. We then exited that process and a new shell process automatically was launched, that is why the PID changed from <0.32.0> to <0.35.0>.

>18> Pid = self().
><0.32.0>
>
>19> H = fun() -> Pid ! 2+2 end.
>#Fun<erl_eval.20.106461118>
>
>20> spawn(H).
><0.36.0>
>
>21> flush().
>Shell got 4
>ok

The ! or bang symbol, is a primitive that sends the message at its right to the process identified by the PID at its left. Each process has a mailbox, a queue that stores the messages the process receives. With flush you can see all the messages the process has.

However in general you want to do something based on the message you received, not only see them on the shell. That is where the receive statement appears to save the game!

>22> Echofun = fun Echo() ->
>           receive
>             X ->
>               io:format("Message ~p~n", [X])
>           end
>         end.
>
>#Fun<erl_eval.20.106461118>
>
>23> EchoPid = spawn(Echofun).
><0.35.0>
>
>24> EchoPid ! test.
>Message test
>test

We created an Echofun function that receives a message and prints it. Receive blocks until it receives a message. Receive is very similar to the case statement. If a message correctly pattern matches, the associated expression gets executed.

Returning to our example, we saved the Echofun function in the Echo variable. Then we spawned the function and stored the Pid. Finally we sent an atom as a message using the bang symbol. The message is received by the process and as any message will pattern match to the variable X the io:fomat line gets executed. The first parameter of io:format is a string that contains a ~p. ~p gets replaced by each element of the list, that is the second argument of the call to io:format.

>25> EchoPid ! test.
>test

If we send the message again, the process will not print “Message test” as before because the function already finished it’s execution. That’s why we need to recursively call the function so that it keeps running after receiving a message.

>26> Echofun2 = fun Echo() ->
>             receive
>               X ->
>                 io:format("Message ~p~n", [X]),
>                 Echo()
>             end
>           end.
>
>#Fun<erl_eval.44.106461118>
>
>27> EchoPid2 = spawn(Echofun2).
><0.35.0>
>
>28> EchoPid2 ! test
>Message test
>test
>
>29> EchoPid2 ! test
>Message test
>test
>
>30> EchoPid2 ! test
>Message test
>test

Now after receiving and printing the first message, the function call itself and waits for the next message to be received.

Erlang and most functional programming languages have a great property called [tail recursion](https://en.wikipedia.org/wiki/Tail_call). Thanks to it your process can have a long and great life without making your stack grow and explode in your face.

Let’s add a new clause inside the receive so that we can kill the process:

>31> Echofun3 = fun Echo() ->
>  receive 
>    die ->
>      io:format("Process with PID ~p has died~n", [self()]);
>    X ->
>      io:format("Message ~p~n", [X]),
>      Echo()
>    end
> end.
>
>#Fun<erl_eval.44.106461118>

Now instead of spawning the function in a process and storing the PID, we will register the process:

>_Besides addressing a process by using its pid, there are also built in functions (BIFs) for registering a process under a name. The name must be an atom and is automatically unregistered if the process terminates_

>32> register(echo_process, spawn(Echofun3)).

We register the process created by spawn(Echofun3) under the name echo_process.

>33> echo_process ! test.
>Message test
>test
>
>34> echo_process ! die.
>Process with PID <0.37.0> has died
>die

![](https://cdn-images-1.medium.com/max/800/1*wysu8drrpca_-GPtNsoYNQ.gif)
`killing a process thanks to message passing`

>35> echo_process ! test.
>** exception error: bad argument
>in operator !/2
>called as echo_process ! test

The receive in the process function that is running under a new process pattern matchs the messages it receives. Since the first message does not match the die atom, it then checks if it matchs the X variable. Since the X variable is unbound, the message will always match it. The message gets printed out.

We send a die atom as a message to the same process. Since die matchs the first clause of the receive, a sentence stating that the process has died. As we do not call the function again effectively the process dies.

When we try to send again a test atom as message to the process that was registered via the echo_process we get a bad argument exception. This happens since the process died the echo_process atom does not reference a process anymore, and obviously that’s why we can not send a message.
####The final fight

Now we are going to play with three processes: a client (the shell), a project manager and a developer. The shell will send a message to the project manager. The project manager will forward the task to developer. Simple but cool:

>36> Dev = fun Devfun() ->
>  receive
>    Task ->
>      io:format("DEV(~p):~n I got a new task: ~p ~n---~n", [self(), Task]),
>      Devfun()
>    end
>end.

As you can see the Dev variable contains a function that receives a message and prints it out. After that it calls itself.

>37> Pm = fun Pmfun() ->
>  receive
>    Task ->
>      io:format("PM(~p):~n I received the following task: ~p.~n My job is to forward it to the developer ~n---~n", [self(), Task]),
>      dev ! Task,
>      Pmfun()
>  end
>end.

Well the Project Manager is not very different from the Developer. He receives a task, prints it out and finally it sends the Task he received to the dev process. We need to spawn and register the dev and pm processes. If we do not register the process that spawns the Dev function, then we would not be able to send it a message as we are doing. So let’s do it:

>38> register(dev, spawn(Dev)),
>register(pm, spawn(Pm)).

Finally we are going to create a really simple function that sends the task to the pm:

>39> NewTask = fun (Task) -> 
>  io:format("Client(~p):~n ~p~n---~n", [self(), Task]),
>  pm ! Task
>end.

Time to send our task!

>40> NewTask("Add cover to TPS report").

And we get this ouput printed out:

>Client(<0.32.0>):
> "Add cover to TPS report"
>---
>PM(<0.37.0>):
> I received the following task: "Add cover to >TPS report".
> My job is to forward it to the developer
>---
>DEV(<0.36.0>):
> I got a new task: "Add cover to TPS report"
>---
![](https://cdn-images-1.medium.com/max/800/1*1MQrMONE3YjecZdrkdOhTg.jpeg)

The Dev process could be running in one server in the US, the PM process in an Europe based server and the Client, my shell, could be running on my computer here in Buenos Aires, Argentina. In another language this would require a really big code change. When using Erlang this only requires adding a few lines of code. Technically, we would have to register the processes globally and set up some kind of vpn so that the virtual machines see themselves as if on a local network. The point is that erlang has distribution built in, and getting to the point where systems run on clusters is not difficult.

For the following posts we leave error detection and supervision of processes, an area where Erlang really really shines.
####Moving foward

You might be asking yourself why would you want to create processes and send messages between them? Sooner rather than later in relative big project you will need to parallelize some code for example a call to a third party api that is taking too much time for example. That is why you will need to use a concurrency construct. In most programming languages threads, processes or any construct related to concurrency or parallelism is something you rarely use. In most universities it is something you will learn only after your first programming courses. You might have used a ThreadPool in Java or even a pthread in C. But it is not something you generally use or do as often as defining a class, instantiating an object, calling a function or writing a conditional statement. Even if this is changing and we now have really interesting libraries, frameworks and toolkits like Akka for the Java world, Concurrent-Ruby or Celluloid for Ruby or even languages as Clojure that already set a pretty high bar, truth be told concurrency is not the cornerstone of most languages. In Erlang you will use them as frequently as you use an if construct in C, because they are cheap and great for designing your systems.
![](https://cdn-images-1.medium.com/max/800/1*o61yZIq3SpkTDPn-pzAJTA.gif)
`Message passing`

Up to now we have only played with processes so that you can see how easy it is to use concurrency based primitives in Erlang since you do not require external library support like in most languages. You will see some of its real uses in the nexts posts. I must add that pattern matching plays an essential role too, and it is very well integrated with the the rest because it makes it very easy to select what to do based on the message received.

To sum up thanks to message passing, pattern matching, light processes you can avoid threads, mutex, semaphores and a lot of deadly weapons that sooner rather than later will backfire.

Stay tuned, we will work on some real stuff next week: comments and threads, http endpoints of our commenting system!
![](https://cdn-images-1.medium.com/max/400/1*1LLFcr72yLfiLfFloQHaeg.jpeg)
`This is me after my first hour playing with Erlang!`
