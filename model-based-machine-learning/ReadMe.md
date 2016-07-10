-   [Model-Based Machine Learning and Probabilistic Programming Using RStan](#model-based-machine-learning-and-probabilistic-programming-using-rstan)
    -   [Introduction](#introduction)
        -   [Why use R for Spatial Data Analysis](#why-use-r-for-spatial-data-analysis)
            -   [R Packages for Spatial Data Analysis](#r-packages-for-spatial-data-analysis)
    -   [Conclusion](#conclusion)
    -   [References](#references)

Model-Based Machine Learning and Probabilistic Programming Using RStan
======================================================================

Introduction
------------

I recently started working on my Ph.D dissertation which utilizes a vast amount of different spatial data types. During the process, I discovered that there were alot of concepts about using R for spatial data analysis that I was not aware of. The purpose of this report is to document some of those concepts and my favorite packages for spatial data analysis. This report is orgainized as follows: Firstly we need to ask why R is a good tool of choice for spatial analysis; secondly we shall go through a typical data analysis life-cycle from getting spatial data to data preparation, exploration, visualization and geostatistical analysis.

### Why use R for Spatial Data Analysis

You might be asking yourself; why use R for spatial analysis when there are commercial and open source Geographical Information Systems (GIS) like ESRI ArcMap and QGIS respectively. These are some of my reasons:

-   R is free and open source
-   Reproducibility: Researchers can reproduce their own analyses or other people's analyses and verify their findings
-   Packages: There are a vast number of R packages for spatial data analysis, statistical modeling, visualisation, machine learning and more.

#### R Packages for Spatial Data Analysis

Some of my favorite packages for spatial data analysis include:

Conclusion
----------

In the next blog post, we shall see how to perform geostatistical analysis on spatial data.

References
----------

For further reading, refer to the following references.

1.  [Applied Spatial Data Analysis with R](http://www.asdar-book.org/). Roger S. Bivand, Edzer Pebesma and V. Gómez-Rubio. UseR! Series, Springer. 2nd ed. 2013, xviii+405 pp., Softcover. ISBN: 978-1-4614-7617-7
2.  [ggmap: Spatial Visualization with ggplot2](https://cran.r-project.org/web/packages/ggmap/ggmap.pdf)
3.  [Do more with dates and times in R with lubridate 1.3.0](https://cran.r-project.org/web/packages/lubridate/vignettes/lubridate.html)
4.  [Introduction to visualising spatial data in R](https://github.com/Robinlovelace/Creating-maps-in-R). Robin Lovelace (<R.Lovelace@leeds.ac.uk>), James Cheshire, Rachel Oldroyd and others V. 1.3, September 2015
5.  [Leaflet for R](http://rstudio.github.io/leaflet/basemaps.html)
6.  [gglot2 documentation](http://docs.ggplot2.org/current/index.html)
7.  [Introduction to dplyr](https://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html)