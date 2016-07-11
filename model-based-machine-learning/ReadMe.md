-   [Introduction to Model-Based Machine Learning](#introduction-to-model-based-machine-learning)
    -   [Introduction](#introduction)
    -   [Challenges in Adopting Machine Learning](#challenges-in-adopting-machine-learning)
    -   [The Defining moment](#the-defining-moment)
    -   [What is Model-Based Machine Learning (MBML)](#what-is-model-based-machine-learning-mbml)
    -   [The Key Ideas of MBML](#the-key-ideas-of-mbml)
        -   [Factor Graphs](#factor-graphs)
        -   [Bayesian Inference](#bayesian-inference)
        -   [Probabilistic Programming](#probabilistic-programming)
    -   [Stages of MBML](#stages-of-mbml)
    -   [Case Study](#case-study)
        -   [A Model for Traffic Prediction](#a-model-for-traffic-prediction)
        -   [Learning the Model Parameters using a Probabilisitic Programming Language](#learning-the-model-parameters-using-a-probabilisitic-programming-language)
        -   [Evaluating Model Results](#evaluating-model-results)
    -   [Conclusion](#conclusion)
    -   [References](#references)

Introduction to Model-Based Machine Learning
============================================

Introduction
------------

This blog post follows my journey from traditional statistical modeling to Machine Learning (ML) and introduces a new paradigm of ML called **Model-Based Machine Learning**\[^1\]. It will particulary be interesting to new comers or other people who want to make a shift to machine learning for their research or practice.

During my Masters in Transportation Engineering (2011-2013), I used traditional statistical modeling in my research to study transportation related problems like [highway crashes](http://dx.doi.org/10.1080/19439962.2013.812168). When I started my PhD, I wanted to make a shift to using machine learning because of the powerful use cases I was constantly reading about in several fields in academia and industry. Particulary I wanted to develop methods that learn how people travel within cities so that we can better plan our transportation infrastructure.

Challenges in Adopting Machine Learning
---------------------------------------

However, I found this shift from traditional statistical modeling to machine learning to be overwhelming because:

1.  There was a vast amount of literature to read, about the 1000s of existing ML algorithms. There was too much learn including the new vocabulary like "features", "feature engineering", etc.

2.  I had to understand which algorithms to use or why one would be better than another for my urban mobility research projects.

3.  What if my problem didn't seem to fit with any standard algorithm? Would I have to devise a new algorithm.

The Defining moment
-------------------

As, I continued to read this vast amount of literature and watch several MOOCs on machine learning, I discovered Prof. Daphne Koller's [course of Probabilistic Graphical Models (PGMs) on Coursera](https://www.coursera.org/learn/probabilistic-graphical-models). This later led to a text book by Prof. Christopher Bishop titled ["Pattern Recognition and Machine Learning"](http://www.amazon.com/Pattern-Recognition-Learning-Information-Statistics/dp/0387310738/ref=sr_1_2?ie=UTF8&s=books&qid=1263391804&sr=8-2#reader_0387310738) which I found easy to understand. So naturally I looked up Bishop's other publications and that's when I found his paper titled ["Model-Based Machine Learning"](http://rsta.royalsocietypublishing.org/content/371/1984/20120222)\[2\]. This was a career defining moment for me. I feel in love with Bayesian Machine Learning. I later discovered that Bishop and his colleagues at Microsoft Research - Cambridge were writing a [book](http://www.mbmlbook.com/)\[3\] with the same title.

What is Model-Based Machine Learning (MBML)
-------------------------------------------

The field of machine learning has experienced the development of thousands of learning algorithms. Typically, scientists choose from these algorithms to solve specific problems. Their choices are constrained by their familiarity with the algorithms. In this classical/traditional framework of machine learning, scientists are constrained to making some assumptions so as to use an existing algorithm. This is in contrast to the model-based machine learning approach which seeks to create a bespoke solution tailored to each new problem.

The goal of MBML is to provide a single development framework which supports the creation of a wide range of bespoke models. This different framework emerged from an important convergence of three key ideas:

1.  the adoption of a Bayesian viewpoint,

2.  the use of factor graphs (a type of probabilistic graphical model), and

3.  the application of fast, deterministic, efficient and approximate inference algorithms.

The core idea is that all assumptions about the problem domain are made explicit in the form of a model. In this framework, a model is simply a set of assumptions about the world expressed in a probabilistic graphical format with all the parameters and variables expressed as random components.

The Key Ideas of MBML
---------------------

### Factor Graphs

The second cornerstone to MBML is the use of Probabilistic Graphical Models (PGM), particularly factor graphs. A PGM is a diagrammatic representation of the joint probability distribution over all random variables in a model expressed as a graph. Factor graphs is a type of PGM that consist of circular nodes representing random variables, square nodes for the conditional probability distributions (factors), and vertices for conditional dependencies between nodes (Figure 2). They provide a general framework for modeling the joint distribution of a set of random variables.

The joint probability *P*(*U*, *X*) over the whole model in Figure 1 is factorized as:

*P*(*U*, *X*)=*P*(*U*)\**P*(*X*|*U*)

Where U are the set of model parameters and X are the set of observed variables

![A Factor Graph](figures/factor-graph.png)

In factor graphs, we treat the traffic congestion states as random variables and learn their probability distributions using Bayesian inference algorithms along the graph. Inference/learning is simply the product of factors over a subset of variables in the graph. This allows for easy implementation of local message passing algorithms.

### Bayesian Inference

The first key idea enabling this different framework for machine learning is Bayesian inference/learning. In MBML, latent/hidden parameters are expressed as random variables with probability distributions. This allows for a coherent and principled manner of quantification of uncertainty in the model parameters. Once the observed variables in the model are fixed to their observed values, initially assumed probability distributions(i.e. priors)are updated using the Bayes theorem.

This is in contrast to the traditional/classical machine learning framework where model parameters are assigned average values that are determined by optimizing an objective function. Bayesian inference on large models over millions of variables is similarly implemented using the Bayes theorem but in a more complex manner. This is because Bayes theorem is an exact inference technique that is intractable over large datasets. In the past decade,the increase of processing power of computers has enabled research and development of fast and efficient inference algorithms that can scale to large data like Belief Propagation (BP), Expectation Propagation (EP), Variational Bayes (VB).

### Probabilistic Programming

Probabilistic programming is a flexible software development environment for model-based machine learning. It takes existing programming languages and adds support for random variables, constraints on variables and inference packages. In this environment, models can be described in a compact form with a few lines of code, and then an inference engine is called to automatically generate inference routines (and even source code) to solve a wide variety of models. Some notable examples of probabilistic programming languages include Infer.Net, Stan, BUGS, church, and PyMC. In this blog post, we shall access Stan algorithms through the R interface.

Stages of MBML
--------------

There are 3 steps to model based machine learning namely:

1.  **Describe the Model**: Describe the process that generated the data using factor graphs.

2.  **Condition on Observed Data**: Condition the observed variables to their known quantities

3.  **Perform Inference**: Perform backward reasoning to update the prior distribution over the latent variables or parameters. In other words, calculate the posterior probability distributions of latent variables conditioned on observed variables.

Case Study
----------

### A Model for Traffic Prediction

Suppose you wish to track the propagation of traffic congestion on a road segment. A machine learning textbook might tell you that there is an algorithm called a Kalman filter \[Kalman, 1960\] which can be used for these kinds of problems. Suppose you decide to try and make use of some Kalman filter software to predict how a traffic congestion evolves over time. First you will have to work out how to convert the traffic congestion prediction task into the form of a standard Kalman filter. Having done that, if you are lucky, the software might give a sufficiently good solution. However, the results from using an off-the-shelf algorithm often fail to reach the accuracy level required by real applications. How will you modify the algorithm, and the corresponding software, to achieve better results? It seems you will have to become an expert on the Kalman filter algorithm, and to delve into the software implementation, in order to make progress.

Contrast this with the model-based approach. You begin by listing the assumptions which your solution must satisfy. This defines your model. You then use this model to create the corresponding machine-learning algorithm, which is a mechanical process that can be automated. If your assumptions happen to correspond to those which are implicit in the Kalman filter, then your algorithm will correspond precisely to the Kalman filtering algorithm (and this will happen even if you have never heard of a Kalman filter). Perhaps, however, the model for your particular application has somewhat different assumptions. In this case you will obtain a variant of the Kalman filter, appropriate to your application. Whether this variant already exists, or whether it is a novel algorithm, is irrelevant if your goal is to find the best solution to your problem. Suppose you try your model-based algorithm, and the results again fall short of your requirements. Now you have a framework for improving the results by examining and modifying the assumptions to produce a better model, along with the corresponding improved algorithm. As a domain expert it is far easier and more intuitive to understand and change the assumptions than it is to modify a machine learning algorithm directly. Even if your goal is simply to understand the Kalman filter, then starting with the model assumptions is by far the clearest and simplest way to derive the filtering algorithm, and to understand what Kalman filters are all about.

![A Model for Traffic Congestion](figures/kalman.png)

### Learning the Model Parameters using a Probabilisitic Programming Language

For this case study, we shall use Stan to learn the model parameters. Stan provides an R interface, RStan, which can be used to call Stan algorithms from within the [R programming lanuage](http://www.r-project.org/) Firstly, follow this [link to get the prerequisites for installing RStan](https://github.com/stan-dev/rstan/wiki/RStan-Getting-Started#prerequisites).

Then install the latest rstan package and the packages it depends on like this:

``` r
## omit the 's' in 'https' if you cannot handle https downloads
install.packages('rstan', repos = 'https://cloud.r-project.org/', dependencies = TRUE)

## If all else fails, you can try to install rstan from source via
install.packages("rstan", type = "source")
```

You are recommended to restart R after the installation before loading the rstan package. Then load the rstan library like this:

``` r
library(rstan)
```

Then you can describe the model is a compact language using the Stan modeling language as follows:

\*. The first section of the below code specifies the data that is conditioned upon by Bayes Rule

\*. The second section of the code defines the parameters whose posterior distribution is sought using Bayes Rule

``` r
traffic_model <- "
  data {
  int<lower=0> N;          //  the number of speed measurements, N; constrained to be non-negative
  real y[N];               // vector of observed speed measurements,y1, . . . , yN (real = double)
  real<lower=0> sigma[N]; //  the standard errors, σ1, . . . σN, of speed measurements
}
parameters {
  real mu;            // the mean of the traffic speeds
  real<lower=0> tau; // the standard deviation of the traffic speeds
  real theta[N];     // the unstandardized speed measurement
}
model {
  theta ~ normal(mu, tau);
  y ~ normal(theta, sigma); // sigma is the standard deviation
}
"
```

It is recommended that we specify the above model in a separate text file with extension *.stan*. However for this tutorial we shall combine it in the same R Markdown file.

After describing the model, you can perform inference by calling Stan inference engine as follows:

``` r
traffic <- read.table("data/traffic.txt", header = TRUE) 

traffic_data <- list(y = traffic[, "sensor_speed"], 
                     sigma = traffic[, "sigma"], 
                     N = nrow(traffic))

traffic_model_fit <- stan(model_code = traffic_model, model_name = "traffic-prediction", 
                          data = traffic_data, iter = 1000, chains = 4, save_dso = TRUE)
```

    ## 
    ## SAMPLING FOR MODEL 'traffic-prediction' NOW (CHAIN 1).
    ## 
    ## Chain 1, Iteration:   1 / 1000 [  0%]  (Warmup)
    ## Chain 1, Iteration: 100 / 1000 [ 10%]  (Warmup)
    ## Chain 1, Iteration: 200 / 1000 [ 20%]  (Warmup)
    ## Chain 1, Iteration: 300 / 1000 [ 30%]  (Warmup)
    ## Chain 1, Iteration: 400 / 1000 [ 40%]  (Warmup)
    ## Chain 1, Iteration: 500 / 1000 [ 50%]  (Warmup)
    ## Chain 1, Iteration: 501 / 1000 [ 50%]  (Sampling)
    ## Chain 1, Iteration: 600 / 1000 [ 60%]  (Sampling)
    ## Chain 1, Iteration: 700 / 1000 [ 70%]  (Sampling)
    ## Chain 1, Iteration: 800 / 1000 [ 80%]  (Sampling)
    ## Chain 1, Iteration: 900 / 1000 [ 90%]  (Sampling)
    ## Chain 1, Iteration: 1000 / 1000 [100%]  (Sampling)
    ##  Elapsed Time: 0.21 seconds (Warm-up)
    ##                0.04 seconds (Sampling)
    ##                0.25 seconds (Total)
    ## 
    ## 
    ## SAMPLING FOR MODEL 'traffic-prediction' NOW (CHAIN 2).
    ## 
    ## Chain 2, Iteration:   1 / 1000 [  0%]  (Warmup)
    ## Chain 2, Iteration: 100 / 1000 [ 10%]  (Warmup)
    ## Chain 2, Iteration: 200 / 1000 [ 20%]  (Warmup)
    ## Chain 2, Iteration: 300 / 1000 [ 30%]  (Warmup)
    ## Chain 2, Iteration: 400 / 1000 [ 40%]  (Warmup)
    ## Chain 2, Iteration: 500 / 1000 [ 50%]  (Warmup)
    ## Chain 2, Iteration: 501 / 1000 [ 50%]  (Sampling)
    ## Chain 2, Iteration: 600 / 1000 [ 60%]  (Sampling)
    ## Chain 2, Iteration: 700 / 1000 [ 70%]  (Sampling)
    ## Chain 2, Iteration: 800 / 1000 [ 80%]  (Sampling)
    ## Chain 2, Iteration: 900 / 1000 [ 90%]  (Sampling)
    ## Chain 2, Iteration: 1000 / 1000 [100%]  (Sampling)
    ##  Elapsed Time: 0.22 seconds (Warm-up)
    ##                0.04 seconds (Sampling)
    ##                0.26 seconds (Total)

    ## The following numerical problems occured the indicated number of times after warmup on chain 2

    ##                                                                                 count
    ## Exception thrown at line 13: normal_log: Scale parameter is 0, but must be > 0!     1

    ## When a numerical problem occurs, the Metropolis proposal gets rejected.

    ## However, by design Metropolis proposals sometimes get rejected even when there are no numerical problems.

    ## Thus, if the number in the 'count' column is small, do not ask about this message on stan-users.

    ## 
    ## SAMPLING FOR MODEL 'traffic-prediction' NOW (CHAIN 3).
    ## 
    ## Chain 3, Iteration:   1 / 1000 [  0%]  (Warmup)
    ## Chain 3, Iteration: 100 / 1000 [ 10%]  (Warmup)
    ## Chain 3, Iteration: 200 / 1000 [ 20%]  (Warmup)
    ## Chain 3, Iteration: 300 / 1000 [ 30%]  (Warmup)
    ## Chain 3, Iteration: 400 / 1000 [ 40%]  (Warmup)
    ## Chain 3, Iteration: 500 / 1000 [ 50%]  (Warmup)
    ## Chain 3, Iteration: 501 / 1000 [ 50%]  (Sampling)
    ## Chain 3, Iteration: 600 / 1000 [ 60%]  (Sampling)
    ## Chain 3, Iteration: 700 / 1000 [ 70%]  (Sampling)
    ## Chain 3, Iteration: 800 / 1000 [ 80%]  (Sampling)
    ## Chain 3, Iteration: 900 / 1000 [ 90%]  (Sampling)
    ## Chain 3, Iteration: 1000 / 1000 [100%]  (Sampling)
    ##  Elapsed Time: 0.22 seconds (Warm-up)
    ##                0.04 seconds (Sampling)
    ##                0.26 seconds (Total)
    ## 
    ## 
    ## SAMPLING FOR MODEL 'traffic-prediction' NOW (CHAIN 4).
    ## 
    ## Chain 4, Iteration:   1 / 1000 [  0%]  (Warmup)
    ## Chain 4, Iteration: 100 / 1000 [ 10%]  (Warmup)
    ## Chain 4, Iteration: 200 / 1000 [ 20%]  (Warmup)
    ## Chain 4, Iteration: 300 / 1000 [ 30%]  (Warmup)
    ## Chain 4, Iteration: 400 / 1000 [ 40%]  (Warmup)
    ## Chain 4, Iteration: 500 / 1000 [ 50%]  (Warmup)
    ## Chain 4, Iteration: 501 / 1000 [ 50%]  (Sampling)
    ## Chain 4, Iteration: 600 / 1000 [ 60%]  (Sampling)
    ## Chain 4, Iteration: 700 / 1000 [ 70%]  (Sampling)
    ## Chain 4, Iteration: 800 / 1000 [ 80%]  (Sampling)
    ## Chain 4, Iteration: 900 / 1000 [ 90%]  (Sampling)
    ## Chain 4, Iteration: 1000 / 1000 [100%]  (Sampling)
    ##  Elapsed Time: 0.23 seconds (Warm-up)
    ##                0.05 seconds (Sampling)
    ##                0.28 seconds (Total)

Calling the `stan()` function performs three fundamental operations:

1.  First, your Stan program is translated to C++ code using the `stanc()` function,

2.  Then the resulting C++ code is compiled to create a DSO (also called a dynamic link library (DLL)) that can be loaded by R,

3.  Finally, the DSO is run to sample from the posterior distribution.

### Evaluating Model Results

Now, we can use the the `print()` function to check out the results in `traffic_model_fit` including a summary of the parameter of the model as well as the log-posterior .

``` r
print(traffic_model_fit, digits = 1)
```

    ## Inference for Stan model: traffic.
    ## 4 chains, each with iter=1000; warmup=500; thin=1; 
    ## post-warmup draws per chain=500, total post-warmup draws=2000.
    ## 
    ##                 mean se_mean   sd   2.5%    25%    50%    75%  97.5% n_eff
    ## alpha[1]       239.8     0.1  2.9  234.1  237.9  239.9  241.7  245.5  2000
    ## alpha[2]       247.8     0.1  2.7  242.4  246.0  247.8  249.6  253.0  2000
    ## alpha[3]       252.5     0.1  2.6  247.4  250.8  252.6  254.2  257.8  2000
    ## alpha[4]       232.6     0.1  2.8  227.1  230.8  232.6  234.4  238.1  2000
    ## alpha[5]       231.7     0.1  2.6  226.8  229.8  231.6  233.4  236.8  2000
    ## alpha[6]       249.8     0.1  2.7  244.5  248.1  249.7  251.5  255.3  2000
    ## alpha[7]       228.7     0.1  2.7  223.2  226.9  228.7  230.6  233.9  2000
    ## alpha[8]       248.4     0.1  2.8  243.2  246.5  248.4  250.3  253.8  2000
    ## alpha[9]       283.2     0.1  2.8  277.6  281.4  283.3  285.0  288.6  2000
    ## alpha[10]      219.3     0.1  2.7  214.0  217.4  219.3  221.2  224.5  2000
    ## alpha[11]      258.3     0.1  2.7  253.0  256.4  258.2  260.1  264.0  2000
    ## alpha[12]      228.1     0.1  2.7  223.0  226.3  228.1  229.9  233.6  2000
    ## alpha[13]      242.4     0.1  2.7  237.1  240.7  242.4  244.1  248.0  2000
    ## alpha[14]      268.3     0.1  2.6  263.1  266.5  268.3  270.1  273.6  2000
    ## alpha[15]      242.7     0.1  2.8  237.4  241.0  242.8  244.5  248.2  2000
    ## alpha[16]      245.4     0.1  2.7  240.0  243.6  245.4  247.2  250.7  2000
    ## alpha[17]      232.3     0.1  2.7  227.0  230.5  232.3  234.0  237.4  2000
    ## alpha[18]      240.5     0.1  2.7  235.2  238.8  240.4  242.3  246.0  2000
    ## alpha[19]      253.8     0.1  2.8  248.1  252.0  253.8  255.6  259.5  2000
    ## alpha[20]      241.7     0.1  2.7  236.2  239.9  241.7  243.5  247.2  2000
    ## alpha[21]      248.6     0.1  2.8  243.2  246.8  248.5  250.3  254.2  2000
    ## alpha[22]      225.2     0.1  2.8  219.9  223.4  225.1  227.0  230.8  2000
    ## alpha[23]      228.5     0.1  2.6  223.3  226.7  228.6  230.3  233.6  2000
    ## alpha[24]      245.1     0.1  2.6  240.1  243.5  245.1  246.8  250.3  2000
    ## alpha[25]      234.6     0.1  2.8  229.3  232.6  234.5  236.4  240.1  2000
    ## alpha[26]      254.0     0.1  2.8  248.5  252.3  254.1  255.9  259.5  2000
    ## alpha[27]      254.4     0.1  2.8  248.7  252.5  254.4  256.3  259.9  2000
    ## alpha[28]      243.0     0.1  2.8  237.5  241.1  242.9  244.8  248.4  2000
    ## alpha[29]      217.9     0.1  2.7  212.6  216.1  217.9  219.7  223.0  2000
    ## alpha[30]      241.5     0.1  2.7  236.1  239.6  241.5  243.3  246.6  2000
    ## beta[1]          6.1     0.0  0.2    5.6    5.9    6.1    6.2    6.5  2000
    ## beta[2]          7.1     0.0  0.3    6.5    6.9    7.1    7.2    7.6  2000
    ## beta[3]          6.5     0.0  0.2    6.0    6.3    6.5    6.6    6.9  2000
    ## beta[4]          5.3     0.0  0.3    4.8    5.2    5.3    5.5    5.9  2000
    ## beta[5]          6.6     0.0  0.2    6.1    6.4    6.6    6.7    7.1  2000
    ## beta[6]          6.2     0.0  0.3    5.6    6.0    6.2    6.3    6.7  2000
    ## beta[7]          6.0     0.0  0.2    5.5    5.8    6.0    6.1    6.5  2000
    ## beta[8]          6.4     0.0  0.2    6.0    6.3    6.4    6.6    6.9  2000
    ## beta[9]          7.1     0.0  0.3    6.6    6.9    7.0    7.2    7.5  2000
    ## beta[10]         5.9     0.0  0.2    5.4    5.7    5.9    6.0    6.3  2000
    ## beta[11]         6.8     0.0  0.3    6.3    6.6    6.8    7.0    7.3  2000
    ## beta[12]         6.1     0.0  0.2    5.6    6.0    6.1    6.3    6.6  2000
    ## beta[13]         6.2     0.0  0.2    5.7    6.0    6.2    6.3    6.7  2000
    ## beta[14]         6.7     0.0  0.2    6.2    6.5    6.7    6.9    7.2  2000
    ## beta[15]         5.4     0.0  0.3    4.9    5.2    5.4    5.6    5.9  2000
    ## beta[16]         5.9     0.0  0.2    5.4    5.8    5.9    6.1    6.4  2000
    ## beta[17]         6.3     0.0  0.2    5.8    6.1    6.3    6.4    6.8  2000
    ## beta[18]         5.8     0.0  0.2    5.4    5.7    5.8    6.0    6.3  2000
    ## beta[19]         6.4     0.0  0.2    5.9    6.2    6.4    6.6    6.9  2000
    ## beta[20]         6.1     0.0  0.2    5.6    5.9    6.1    6.2    6.5  2000
    ## beta[21]         6.4     0.0  0.2    5.9    6.2    6.4    6.6    6.9  2000
    ## beta[22]         5.9     0.0  0.2    5.4    5.7    5.9    6.0    6.4  2000
    ## beta[23]         5.8     0.0  0.3    5.3    5.6    5.7    5.9    6.2  2000
    ## beta[24]         5.9     0.0  0.2    5.4    5.7    5.9    6.1    6.4  2000
    ## beta[25]         6.9     0.0  0.3    6.4    6.7    6.9    7.1    7.4  2000
    ## beta[26]         6.5     0.0  0.2    6.1    6.4    6.5    6.7    7.0  2000
    ## beta[27]         5.9     0.0  0.2    5.4    5.7    5.9    6.1    6.3  2000
    ## beta[28]         5.8     0.0  0.3    5.3    5.7    5.8    6.0    6.3  2000
    ## beta[29]         5.7     0.0  0.3    5.2    5.5    5.7    5.8    6.2  2000
    ## beta[30]         6.1     0.0  0.2    5.7    6.0    6.1    6.3    6.6  2000
    ## mu_alpha       242.5     0.1  2.8  237.1  240.7  242.4  244.2  248.1  2000
    ## mu_beta          6.2     0.0  0.1    6.0    6.1    6.2    6.3    6.4  2000
    ## sigmasq_y       37.9     0.2  6.0   28.0   33.6   37.2   41.4   51.7  1184
    ## sigmasq_alpha  217.9     1.4 63.3  123.7  174.1  207.6  250.4  381.2  2000
    ## sigmasq_beta     0.3     0.0  0.1    0.1    0.2    0.3    0.3    0.5  1027
    ## sigma_y          6.1     0.0  0.5    5.3    5.8    6.1    6.4    7.2  1198
    ## sigma_alpha     14.6     0.0  2.1   11.1   13.2   14.4   15.8   19.5  2000
    ## sigma_beta       0.5     0.0  0.1    0.4    0.5    0.5    0.6    0.7   990
    ## alpha0         106.4     0.1  3.7   99.3  103.9  106.4  108.8  113.9  2000
    ## lp__          -439.3     0.3  7.2 -454.5 -444.0 -438.8 -434.2 -426.4   568
    ##               Rhat
    ## alpha[1]         1
    ## alpha[2]         1
    ## alpha[3]         1
    ## alpha[4]         1
    ## alpha[5]         1
    ## alpha[6]         1
    ## alpha[7]         1
    ## alpha[8]         1
    ## alpha[9]         1
    ## alpha[10]        1
    ## alpha[11]        1
    ## alpha[12]        1
    ## alpha[13]        1
    ## alpha[14]        1
    ## alpha[15]        1
    ## alpha[16]        1
    ## alpha[17]        1
    ## alpha[18]        1
    ## alpha[19]        1
    ## alpha[20]        1
    ## alpha[21]        1
    ## alpha[22]        1
    ## alpha[23]        1
    ## alpha[24]        1
    ## alpha[25]        1
    ## alpha[26]        1
    ## alpha[27]        1
    ## alpha[28]        1
    ## alpha[29]        1
    ## alpha[30]        1
    ## beta[1]          1
    ## beta[2]          1
    ## beta[3]          1
    ## beta[4]          1
    ## beta[5]          1
    ## beta[6]          1
    ## beta[7]          1
    ## beta[8]          1
    ## beta[9]          1
    ## beta[10]         1
    ## beta[11]         1
    ## beta[12]         1
    ## beta[13]         1
    ## beta[14]         1
    ## beta[15]         1
    ## beta[16]         1
    ## beta[17]         1
    ## beta[18]         1
    ## beta[19]         1
    ## beta[20]         1
    ## beta[21]         1
    ## beta[22]         1
    ## beta[23]         1
    ## beta[24]         1
    ## beta[25]         1
    ## beta[26]         1
    ## beta[27]         1
    ## beta[28]         1
    ## beta[29]         1
    ## beta[30]         1
    ## mu_alpha         1
    ## mu_beta          1
    ## sigmasq_y        1
    ## sigmasq_alpha    1
    ## sigmasq_beta     1
    ## sigma_y          1
    ## sigma_alpha      1
    ## sigma_beta       1
    ## alpha0           1
    ## lp__             1
    ## 
    ## Samples were drawn using NUTS(diag_e) at Sun Jul 10 21:19:34 2016.
    ## For each parameter, n_eff is a crude measure of effective sample size,
    ## and Rhat is the potential scale reduction factor on split chains (at 
    ## convergence, Rhat=1).
    ##  The estimated Bayesian Fraction of Missing Information is a measure of
    ##  the efficiency of the sampler with values close to 1 being ideal.
    ##  For each chain, these estimates are
    ##  0.7 0.7 0.8 0.8

Conclusion
----------

There are several potential advantages of using model-based machine learning including;

-   This approach provides a systematic process of developing bespoke models tailored to our specific problem.

-   It also provides transparency in our model because we explicitly defined our modelassumptions by leveraging prior knowledge about human travel.

-   This approach allows handling of uncertainty in a principled manner using probability theory.
-   It also does not suffer from overfitting because the model parameters are learned using Bayesian inference and not optimization.

-   Finally, this approach separates the model development from inference which allows us to build several models and use the same inference algorithm to learn the model parameters. This in turn helps to quickly compare several alternative modelsand select the best model that is explained by the observed data.

References
----------

For further reading, refer to the following references.

1.  J. Winn, C. Bishop, and T. Diethe, Model-Based Machine Learning,Microsoft Research, <http://www.mbmlbook.com/>,2015.

2.  C. M. Bishop, “Model-based machine learning” Phil Trans R Soc, A 371: 20120222. <http://dx.doi.org/10.1098/rsta.2012.0222>, Jan. 2013

3.  T. Minka, J. Winn, J. Guiver, and D. Knowles, Infer.NET, Microsoft Research Cambridge, <http://research.microsoft.com/infernet>.2010.

4.  Stan Development Team, “Stan Modeling Language Users Guide and Reference Manual,” Version 2.9.0,<http://mc-stan.org>,2016.

5.  J. Lunn, A.Thomas, N. Best, and D. Spiegelhalter, “[WinBUGS --a Bayesian modelling framework: concepts, structure, and extensibility,]()”, Statistics and Computing,10:325—337, 2000.

6.  N. D. Goodman, V. K. Mansinghka, D. M. Roy, K. Bonawitz, and J. B.Tenenbaum,“[Church: a language for generative models]()", In Uncertainty in Artificial Intelligence (UAI), pages 220–229, 2008.

7.  Patil, A., D. Huard and C.J. Fonnesbeck. 2010. [PyMC: Bayesian Stochastic Modelling in Python. Journal of Statistical Software](), 35(4), pp. 1-81, 2010.

8.  Stan Development Team, "[RStan: the R interface to Stan,” Version 2.9.0](http://mc-sta%20n.org,2016)".

9.  D. Emaasit, A. Paz, and J. Salzwedel (2016). ["A Model-Based Machine Learning Approach for Capturing Activity-Based Mobility Patterns using Cellular Data"](). IEEE ITSC 2016. Under Review.

10. D. Emaasit (2017). ["A Model-Based Machine Learning Framework for Predicting Traffic Congestion: A Case Study of Nairobi, Kenya"](). TRB 2017. Under Review.
