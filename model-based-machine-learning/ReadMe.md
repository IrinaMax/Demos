-   [Introduction to Model-Based Machine Learning](#introduction-to-model-based-machine-learning)
    -   [Introduction](#introduction)
    -   [Current Challenges in Adopting Machine Learning](#current-challenges-in-adopting-machine-learning)
    -   [What is Model-Based Machine Learning (MBML)](#what-is-model-based-machine-learning-mbml)
    -   [The Key Ideas of MBML](#the-key-ideas-of-mbml)
        -   [Factor Graphs](#factor-graphs)
        -   [Bayesian Inference](#bayesian-inference)
        -   [Probabilistic Programming](#probabilistic-programming)
    -   [Stages of MBML](#stages-of-mbml)
    -   [Case Study](#case-study)
        -   [A Model for Traffic Prediction](#a-model-for-traffic-prediction)
        -   [Learning the Model Parameters using a Probabilisitic Programming Language](#learning-the-model-parameters-using-a-probabilisitic-programming-language)
    -   [Conclusion](#conclusion)
    -   [References](#references)

Introduction to Model-Based Machine Learning
============================================

Introduction
------------

During the last few years, the field of machine learning has moved to centre stage in the world of technology. Today thousands of scientists and engineers are applying machine learning to an extraordinarily broad range of domains. However, making effective use of machine learning in practice can be daunting, especially for newcomers to the field. Here are some of the principal challenges encountered when trying to solve real-world problems using machine learning.

In this book we look at machine learning from a fresh perspective which we call model-based machine learning. This viewpoint helps to address all of these challenges, and makes the process of creating effective machine learning solutions much more systematic. It is applicable to the full spectrum of machine learning techniques and application domains, and will help guide you towards building successful machine learning solutions without requiring that you master the huge literature on machine learning.

Current Challenges in Adopting Machine Learning
-----------------------------------------------

“I am overwhelmed by the choice of machine learning methods and techniques. There’s too much to learn!”

“I don’t know which algorithm to use or why one would be better than another for my problem.”

“My problem doesn’t seem to fit with any standard algorithm.”

What is Model-Based Machine Learning (MBML)
-------------------------------------------

The field of machine learning has experienced the development of thousands of learning algorithms. Typically, scientists choose from these algorithms to solve specific problems. Their choices are constrained by their familiarity with the algorithms. In this classical/traditional framework of machine learning, scientists are constrained to making some assumptions so as to use an existing algorithm. This is in contrast to the model-based machine learning approach which seeks to create a bespoke solution tailored to each new problem. The goal of model-based machine learning is to provide a single development framework which supports the creation of a wide range of bespoke models. This different framework emerged from an important convergence of three key ideas: (i) the adoption of a Bayesian viewpoint, (ii) the use of probabilistic graphical models, and (iii) the application of fast, deterministic, efficient and approximate inference algorithms. The core idea is that all assumptions about the problem domain are made explicit in the form of a model. In this framework, a model is simply a set of assumptionsabout the world expressed in a probabilistic graphical format with all the parameters and variables expressed as random components.

The Key Ideas of MBML
---------------------

### Factor Graphs

The second cornerstone to model-based machine learning is the use of Probabilistic Graphical Models (PGM), particularly factor graphs. A PGM is a diagrammatic representation of the joint probability distribution over all random variables in a model expressed as a graph. Factor graphs is a type of PGM that consist of circular nodes representing random variables, square nodes for the conditional probability distributions (factors), and vertices for conditional dependencies between nodes (Figure 2). They provide a general framework for modeling the joint distribution of a set of random variables. The joint probability *P*(*Q*, *X*) over the whole model in Figure 1 is factorized as (Equation 1):
*P*(*Q*, *X*)=*P*(*Q*)\**P*(*X*|*Q*)
Where Q are the set of model parameters and X are the set of observed variables

![A Factor Graph](figures/factor-graph.png)

In factor graphs, we treat the traffic congestion states as random variables and learn their probability distributions using Bayesian inference algorithms along the graph. Inference/learning is simply the product of factors over a subset of variables in the graph. This allows for easy implementation of local message passing algorithms.

### Bayesian Inference

The first key idea enabling this different framework for machine learning is Bayesian inference/learning. In model-based machine learning, latent/hidden parameters are expressed as random variables with probability distributions. This allows for a coherent and principled manner of quantification of uncertainty in the model parameters. Once the observed variables in the model are fixed to their observed values, initially assumed probability distributions(i.e. priors)are updated using the Bayes theorem. This is in contrast to the traditional/classical machine learning framework where model parameters are assigned average values that are determined by optimizing an objective function. Bayesian inference on large models over millions of variables is similarly implemented using the Bayes theorem but in a more complex manner. This is because Bayes theorem is an exact inference technique that is intractable over large datasets. In the past decade,the increase of processing power of computers has enabled research and development of fast and efficient inference algorithms that can scale to large data. In Section V of this paper, we describe two such algorithms based on local message passing along factor graphs.

### Probabilistic Programming

Probabilistic programming is a flexible software development environment for model-based machine learning. It takes existing programming languages and adds support for random variables, constraints on variables and inference packages. In this environment, models can be described in a compact form with a few lines of code, and then an inference engine is called to automatically generate inference routines (and even source code) to solve a wide variety of models. Some notable examples of probabilistic programming languages include Infer.Net \[@minka2010\], Stan \[@stan2016\], BUGS \[@lunn2000\], church \[@goodman2008\], and PyMC \[@patil2010\]. In this project, we access Stan algorithms through the R interface \[@rstan2016\] as described in section 5.

Stages of MBML
--------------

There are 3 steps to model based machine learning namely:

1.  *Describe the Model*: Describe the process that generated the data using factor graphs.

2.  *Condition on Observed Data*: Condition the observed variables to their known quantities

3.  *Perform Inference*: Perform backward reasoning to update the prior distribution over the latent variables or parameters. In other words, calculate the posterior probability distributions of latent variables conditioned on observed variables.

Case Study
----------

### A Model for Traffic Prediction

Suppose you wish to track the changing skill of a player in an online gaming service (this is the problem we will explore in detail in Chapter 3). A machine learning textbook might tell you that there is an algorithm called a Kalman filter \[Kalman, 1960\] which can be used for these kinds of problems. Suppose you decide to try and make use of some Kalman filter software to predict how a player’s skill evolves over time. First you will have to work out how to convert the skill prediction task into the form of a standard Kalman filter. Having done that, if you are lucky, the software might give a sufficiently good solution. However, the results from using an off-the-shelf algorithm often fail to reach the accuracy level required by real applications. How will you modify the algorithm, and the corresponding software, to achieve better results? It seems you will have to become an expert on the Kalman filter algorithm, and to delve into the software implementation, in order to make progress.

Contrast this with the model-based approach. You begin by listing the assumptions which your solution must satisfy. This defines your model. You then use this model to create the corresponding machine-learning algorithm, which is a mechanical process that can be automated. If your assumptions happen to correspond to those which are implicit in the Kalman filter, then your algorithm will correspond precisely to the Kalman filtering algorithm (and this will happen even if you have never heard of a Kalman filter). Perhaps, however, the model for your particular application has somewhat different assumptions. In this case you will obtain a variant of the Kalman filter, appropriate to your application. Whether this variant already exists, or whether it is a novel algorithm, is irrelevant if your goal is to find the best solution to your problem. Suppose you try your model-based algorithm, and the results again fall short of your requirements. Now you have a framework for improving the results by examining and modifying the assumptions to produce a better model, along with the corresponding improved algorithm. As a domain expert it is far easier and more intuitive to understand and change the assumptions than it is to modify a machine learning algorithm directly. Even if your goal is simply to understand the Kalman filter, then starting with the model assumptions is by far the clearest and simplest way to derive the filtering algorithm, and to understand what Kalman filters are all about.

![Traffic](figures/traffic.jpg)

### Learning the Model Parameters using a Probabilisitic Programming Language

For this case study, we shall use Stan to learn the model parameters. RStan is the R (<http://www.r-project.org/>) interface for Stan. Before installing rstan, follow this [link to get the prerequisites for installing RStan](https://github.com/stan-dev/rstan/wiki/RStan-Getting-Started#prerequisites).

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

    ## Loading required package: ggplot2

    ## Loading required package: StanHeaders

    ## rstan (Version 2.10.1, packaged: 2016-06-24 13:22:16 UTC, GitRev: 85f7a56811da)

    ## For execution on a local, multicore CPU with excess RAM we recommend calling
    ## rstan_options(auto_write = TRUE)
    ## options(mc.cores = parallel::detectCores())

Then you can describe the model is a compact language using the Stan modeling language as follows:

1.  First, we specify this model in a file called traffic.stan as follows

2.  The first section of the below code specifies the data that is conditioned upon by Bayes Rule

3.  The second section of the code defines the parameters whose posterior distribution is sought using Bayes Rule

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

Now, let's use the following code to check out the results in `traffic_model_fit`. We can review a summary of the parameter of the model as well as the log-posterior by using the `print()` function.

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

There several potential advantages of using model-based machine learning including; \* This approach provides a systematic process of developing bespoke models tailored to our specific problem.

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
