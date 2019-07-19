---
title: Interview with Thomas Wiecki about PyMC and probabilistic programming
type: docs
Date: 2018-11-16
author: unbalancedparentheses
---
![-](https://cdn-images-1.medium.com/max/800/1*Ke59y1Iyox6MUJ1HpEkLyg.jpeg)
A colleague of von Neumann and Ulam, Nicholas Metropolis, suggested using the name Monte Carlo, which refers to the Monte Carlo Casino in Monaco where Ulam’s uncle would borrow money from relatives to gamble”

After studying and working with distributed systems my interests drifted into data science, artificial intelligence, machine learning and now statistics. After a while I found I really liked applying what I have learnt into finance. That is how I learnt about probabilistic programming and PyMC while reading Thomas Wiecki posts at Quantopian blog. That’s why we decided to interview Thomas. I hope you like this interview as much as we did.

*****
I ‘ve created a Not a Monad Tutorial newsletter so that you receive an email whenever we publish a new story. [Sign up!](https://mailchi.mp/9302d4f60de9/not-a-monad-tutorial)
Reach me via twitter at [@unbalancedparen](https://twitter.com/unbalancedparen) if you have any comments or interview request for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial/).
****
**What is probabilistic programming?**

Probabilistic Programming is a paradigm that allows the expression of Bayesian statistical models in computer code. Imagine you run an A/B test and want to know which version is better. With a few lines of code you could build a model with 2 convergence rate parameters that are linked to the data you observed this far. So far that’s only model specification. The real power in probabilistic programming comes from the inference algorithms that have become so powerful that they usually work very well no matter what model you throw at them. So all you have to do is build the model, hit the inference button and analyze your outputs. In this example, the outputs will be the estimates of the convergence rates for both conditions as well as the uncertainty in those estimates. From there it is trivial to extract statements such as “with x% probability, version A has a higher convergence rate than version B”.
This is a really simple example but these models can get staggeringly complex, depending on the problem you are trying to solve.
In general, whenever you have a problem where uncertainty plays a big role, where there is structure to be exploited (e.g. hierarchical), or you have prior knowledge you want to include in the model, probabilistic programming is the tool of choice.

**What is PyMC?**

PyMC3 is such a probabilistic programming framework. It is different from most previous frameworks in that it does not require you to write models in a domain specific language but in plain Python. It features state-of-the-art inference algorithms and diagnostics, flexible support for Gaussian Processes, model comparison metrics, and has a very active community.

**Why was PyMC created? What is the difference with other projects like Edward and Stan?**

PyMC3 is a result of the desire to implement next-generation Hamiltonian Monte Carlo (HMC) samplers which are vastly superior to previous MCMC algorithms. Contrary to these previous algorithms, HMC requires gradients to be computed. Theano was the first library that allowed to build a compute graph in Python from which it is easy to automatically compute gradients. John Salvatier went through various designs until he came up with the core design that would constitute PyMC3. While HMC is the core motivation, Theano provides many other benefits to PyMC3, like high performance due to graph optimizations and compilation to CPU and GPU, while keeping the model definition and code-base pure Python.
The biggest difference to Stan is that this one is written in C++ with a custom DSL to define models. This is great as it allows interfaces from various languages but it comes at the cost of a more complex code-base, and having to learn a DSL rather than being able to use Python. Having said that, PyMC3 is hugely inspired by Stan in many ways.
Edward is a more recent PPL built on TensorFlow so in that way it is quite similar to PyMC3 in that you can construct models in pure Python. Its focus is more on variational inference (which can also be expressed in the same PPL), scalability and deep generative models. I don’t think it is actively developed anymore so I think some interested should take a look at TensorFlow Probability instead.
In general, I would say that if you are a ML researcher developing new deep networks or variational inference algorithms, use TensorFlow Probability; if you are an R user with a statistical background, use Stan; if you are a Data Scientist most comfortable in Python, use PyMC3.

**Could you please tell us about real world examples where PyMC is being used?**

PyMC3 is widely used in academia, there are currently close to 200 papers using PyMC3 in various fields, including astronomy, chemistry, ecology, psychology, neuroscience, computer security, and many more. It is also used to solve various business problems by large and small companies. One common use case I have heard is for A/B testing, which makes sense as uncertainty plays a big role. Another problem well solved by PyMC3 is supply chain optimization.

**How does PyMC differ from other probabilistic programming languages (eg Figaro, Church, Anglican)?**

These more academic PPLs really push the envelope of what is possible to do in this framework. They are Turing complete and thus there is little that can not be done. Often these high standards on theoretical guarantees and pureness of the PPL come at the cost of usability and performance. PyMC3 on the other hand is focusing on solving 80% of the problems encountered in reality as succinctly and performant as possible.

**How far is support for Deep Learning methods integrated into PyMC?**

I wrote a [couple](https://twiecki.io/blog/2016/06/01/bayesian-deep-learning/) [of](https://twiecki.io/blog/2016/07/05/bayesian-deep-learning/) [blog](https://twiecki.io/blog/2017/03/14/random-walk-deep-net/) [posts](https://twiecki.io/blog/2018/08/13/hierarchical_bayesian_neural_network/) exploring these topics. As deep learning was Theano’s original purpose we can tap into a lot of that functionality and extend deep nets in interesting ways. I have not seen it been much explored elsewhere, however. PyMC4 will be based on TensorFlow Probability (TFP) which definitely has a strong focus on deep generative models so this type of model will be much easier to build and TFP’s powerful inference algorithms will also allow it to scale.

**What are the tradeoffs of using Variational Inference vs standard Markov chain Monte Carlo with regards to finance?**

Variational inference is generally much faster at the cost of a less accurate approximation of the posterior. In finance you are often operating under razor-thin margins so everything counts. My recommendation is to thus use MCMC.

**Is there support for modelling Generalized Extreme Value distributions in PyMC3?**

I haven’t come across that one yet but it looks like it should be fairly easy to add. We certainly have the Gumbell and Weibull distribution which are special cases of it.

**How is model criticism implemented?**

There is pymc3.sample_posterior_predictive() which allows you to generate new data from your model which you can then compare to your original data to test if the model can capture the important properties in your model. See https://docs.pymc.io/notebooks/posterior_predictive.html for more information.

**How do you use PyMC3 at Quantopian?**

We use it in various places, but most critical for portfolio weighting. At Quantopian our challenge is to evaluate return streams of trading algorithms over limited time periods to decide if they have an edge in the market or just got lucky over a certain time-period. Thus, correctly quantifying uncertainty is critical. The return streams themselves have many challenging characteristics on top of that which are important to consider when trying to select them. Some key characteristics are that over time, they often change volatility; the “edge” they have can also vary over time; and they can be correlated with other strategies you are considering.
Adrian Seyboldt built a fairly sophisticated model which takes all these properties (and more) into account and ends up having tens of thousands of parameters. The incredible thing about PyMC3 and HMC is that this hugely complex model can be fit in well under an hour. But the really business critical advantage is that once this model is fitted, correctly attributing all the uncertainty in these various characteristics, we can put everything into an optimizer to give us the optimal portfolio. This way, we solve the algorithm selection and portfolio weighting problem in one go. Before, there was a lot of discretionary leeway in what constitutes enough evidence that an algorithm should be turned on or off leading to suboptimal decisions.

**Has PyMC been ever used to price financial derivatives?**

Not to my knowledge but I would not be surprised if they had been used for that. People come up to me constantly at conferences and tell me about the cool domain problems they solved with PyMC3 at their company. I love hearing about this because it is hard to assess how many and for what purpose people use your software.

**How big a problem is autocorrelation in sampling financial (tail risk, pricing) distributions?**

It is definitely an important source of risk that is often ignored when applying frequentist statistics to finance, as the normality assumption is baked into most tests. Using probabilistic programming we can model changes in volatility like the [stochastic volatility model](https://docs.pymc.io/notebooks/stochastic_volatility.html) does. Alternatively one can also use a Student-T distribution for the likelihood which allows for heavier tails.

**What could be improved in PyMC?**

I think the documentation and website needs more work to make it easier to find things and more beginner friendly. We also recently spun out the plotting code into a separate project called [ArviZ](https://arviz-devs.github.io/arviz/). In the upcoming release, we will slowly transition to using that library.
Our next major project will be PyMC4 which will be based on TensorFlow Probability which provides a great core architecture and allows us to put more focus on usability and we have some interesting ideas for improved model creation API. In the meantime, maintenance of Theano is going to be taken over by the PyMC development team as the original authors are moving on.

**Is the project looking for contributors?**

Yes, we constantly have new contributors show up on [GitHub](https://github.com/pymc-devs/pymc3) and [discourse](https://discourse.pymc.io/) and keep growing our list of core developers. We also have a summit coming up where all developers will be meeting in London to hack on PyMC4.

**What books or MOOCs would you recommend for a software dev that wants to learn more about probability and statistics?**

[Probabilistic Programming for Hackers](http://camdavidsonpilon.github.io/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/) is a great read. For a more formal introduction I like [Kruschke’s puppy book](http://www.indiana.edu/~kruschke/DoingBayesianDataAnalysis/) (which was [ported to PyMC3](https://github.com/JWarmenhoven/DBDA-python)) as well as Osvaldo Martin’s (who is a PyMC core developer) book on [Bayesian Analysis with Python](https://www.packtpub.com/big-data-and-business-intelligence/bayesian-analysis-python-second-edition). Also if you find that screencasts are a good way to learn — Peadar Coyle (who is a PyMC core developer) put together a course called [Probabilistic Programming Primer](https://www.probabilisticprogrammingprimer.com/), he also looks at other PPLs including Pyro (from Uber based on Pytorch) and Rainier (a Scala based PPL). Finally, [Statistical Rethinking](https://xcelab.net/rm/statistical-rethinking/) (ported to [PyMC3](https://github.com/pymc-devs/resources/tree/master/Rethinking)) is a fantastic resource as well.
