Predicting Used Car Prices
================
Andrés Pitta, Braden Tam, Serhiy Pokrovskyy </br>
2020/01/25 (updated: 2020-01-25)

# Summary

In this project we attempt to build a regression model which can predict
the price of used cars based on numerous features of the car. We tested
the following models: support vector regression, stochastic gradient
descent regression, linear regression, K-nearest neighbour regression,
and random forest regression. data set is not linearly separable, more
clustered. We found that support vector regression had the best results,
having a score of `INPUT TRAIN SCORE` on the training set and a score of
`INPUT TEST SCORE` on the test set. Given that the dataset was
imbalanced, this led to poor prediction of the classes that were quite
sparse.

# Introduction

Websites such as Craigslist, Kijiji, and eBay have tons of users that
creates vast markets of used goods. Typically people looking to save
some money use these website to purchase second hand items. The problem
with these websites is that the user determines the price of their used
good. This can either be a good or bad thing, depending on whether or
not the user is trying to scam the buyer or give the buyer a good deal.
For the average individual who is not familiar with prices of the used
market, it is especially difficult to guage what the price of a used
good should be. Being able to predict used car prices based on data on a
whole market will gives users the ability to evaluate whether a used car
listing is consistent with the market so that they know they are not
getting ripped off.

# Methods

## Data

The data set used in this project is Used Cars Dataset created by Austin
Reese. It was collected from Kaggle.com (Reese 2020) and can be found
[here](https://www.kaggle.com/austinreese/craigslist-carstrucks-data).
This data consists of used car listings in the US scraped from
Craigslist that contains information such as listed price, manufacturer,
model, listed condition, fuel type, odometer, type of car, and which
state it’s being sold in.

## Analysis

As it was mentioned, our original data holds half a million observations
with a few dozen features, most categorical, so accurate feature
selection and model selection were extremely important. Especially
because model training took significant amount of computational
resources.

Since we could not efficiently use automated feature selection like RFE
or FFE (because of time / resources constraint), we had to perform
manual feature selection. As we had some intuition in the target area as
well as some practical experience, we were able to prune our feature
list to just 12 most important on our opinion:

  - 10 categorical features:
      - manufacturer (brand)
      - transmission type
      - fuel type
      - paint color
      - number of cylinders
      - drive type (AWD / FWD / RWD)
      - size
      - condition
      - title\_status
      - state
  - 2 continuous features:
      - year
      - odometer

For each model we performed 5-fold-cross-validated grid search involving
a range of most important model-specific hyper-parameters.

The R and Python programming languages (R Core Team 2019; Van Rossum and
Drake 2009) and the following R and Python packages were used to perform
the analysis: docopt (de Jonge 2018), knitr (Xie 2014), tidyverse
(Wickham et al. 2019), readr (Wickham, Hester, and Francois 2018) docopt
(Keleshev 2014), altair (VanderPlas et al. 2018), plotly (Inc. 2015),
selenium (SeleniumHQ 2020), pandas (McKinney 2010), numpy (Oliphant
2006), statsmodel (Seabold and Perktold 2010). scikit-learn (Buitinck et
al. 2013).

The code used to perform the analysis and create this report can be
found here: \*\*\*\*\*\*\*\*

# Results & Discussion

Based on our EDA and assumptions, we picked a number of models to fit
our train data. Since training and validating took a lot of resources,
we performed it on a gradually increasing subsets of training data. See
the results below, sorted by validation score (increasing):

| Model                       | Training Score | Validation Score |
| --------------------------- | -------------- | ---------------- |
| Linear Regression           | 0.555803       | 0.526354         |
| Stochastic Gradient Descent | 0.550439       | 0.528612         |
| kNN                         | 0.638008       | 0.626848         |
| Random Forests              | 0.964447       | 0.734342         |
| Gradient Boosted Trees      | 0.803595       | 0.736818         |
| **Support Vector Machines** | **0.840271**   | **0.813724**     |

Since SVM shown the best results from the very beginning, we performed a
thorough adaptive grid search on a bigger subset of 200,000 observations
(running for 4 hours) resulting in 81.3% accuracy on validation data.
Eventually we ran the model on the **test data** containing more than
40,000 observations, which confirmed the model with even better
**accuracy of 81.5%**. The good sign was also that it did not overfit
greatly on train set, which was a good sign to perform further testing.

    ## Warning: Missing column names filled in: 'X1' [1]

    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_double(),
    ##   year = col_double(),
    ##   odometer = col_double(),
    ##   manufacturer = col_character(),
    ##   condition = col_character(),
    ##   title_status = col_character(),
    ##   price = col_double(),
    ##   prediction = col_double(),
    ##   abs_error = col_double(),
    ##   abs_error_pct = col_double()
    ## )

<table class="table" style="margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:right;">

X1

</th>

<th style="text-align:right;">

year

</th>

<th style="text-align:right;">

odometer

</th>

<th style="text-align:left;">

manufacturer

</th>

<th style="text-align:left;">

condition

</th>

<th style="text-align:left;">

title\_status

</th>

<th style="text-align:right;">

price

</th>

<th style="text-align:right;">

prediction

</th>

<th style="text-align:right;">

abs\_error

</th>

<th style="text-align:right;">

abs\_error\_pct

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:right;">

345

</td>

<td style="text-align:right;">

2013

</td>

<td style="text-align:right;">

89662

</td>

<td style="text-align:left;">

lincoln

</td>

<td style="text-align:left;">

No value

</td>

<td style="text-align:left;">

clean

</td>

<td style="text-align:right;">

13995

</td>

<td style="text-align:right;">

13605.94

</td>

<td style="text-align:right;">

389.06

</td>

<td style="text-align:right;">

2.78

</td>

</tr>

<tr>

<td style="text-align:right;">

698

</td>

<td style="text-align:right;">

2007

</td>

<td style="text-align:right;">

143724

</td>

<td style="text-align:left;">

ford

</td>

<td style="text-align:left;">

good

</td>

<td style="text-align:left;">

clean

</td>

<td style="text-align:right;">

6588

</td>

<td style="text-align:right;">

8246.61

</td>

<td style="text-align:right;">

1658.61

</td>

<td style="text-align:right;">

25.18

</td>

</tr>

<tr>

<td style="text-align:right;">

714

</td>

<td style="text-align:right;">

2017

</td>

<td style="text-align:right;">

35370

</td>

<td style="text-align:left;">

ford

</td>

<td style="text-align:left;">

No value

</td>

<td style="text-align:left;">

clean

</td>

<td style="text-align:right;">

27990

</td>

<td style="text-align:right;">

30395.27

</td>

<td style="text-align:right;">

2405.27

</td>

<td style="text-align:right;">

8.59

</td>

</tr>

<tr>

<td style="text-align:right;">

401

</td>

<td style="text-align:right;">

2001

</td>

<td style="text-align:right;">

157000

</td>

<td style="text-align:left;">

dodge

</td>

<td style="text-align:left;">

No value

</td>

<td style="text-align:left;">

clean

</td>

<td style="text-align:right;">

2500

</td>

<td style="text-align:right;">

2780.30

</td>

<td style="text-align:right;">

280.30

</td>

<td style="text-align:right;">

11.21

</td>

</tr>

<tr>

<td style="text-align:right;">

83

</td>

<td style="text-align:right;">

2016

</td>

<td style="text-align:right;">

88564

</td>

<td style="text-align:left;">

toyota

</td>

<td style="text-align:left;">

No value

</td>

<td style="text-align:left;">

rebuilt

</td>

<td style="text-align:right;">

19900

</td>

<td style="text-align:right;">

11858.45

</td>

<td style="text-align:right;">

8041.55

</td>

<td style="text-align:right;">

40.41

</td>

</tr>

<tr>

<td style="text-align:right;">

364

</td>

<td style="text-align:right;">

2005

</td>

<td style="text-align:right;">

199683

</td>

<td style="text-align:left;">

jeep

</td>

<td style="text-align:left;">

good

</td>

<td style="text-align:left;">

clean

</td>

<td style="text-align:right;">

3500

</td>

<td style="text-align:right;">

2957.85

</td>

<td style="text-align:right;">

542.15

</td>

<td style="text-align:right;">

15.49

</td>

</tr>

<tr>

<td style="text-align:right;">

213

</td>

<td style="text-align:right;">

2014

</td>

<td style="text-align:right;">

39026

</td>

<td style="text-align:left;">

chevrolet

</td>

<td style="text-align:left;">

excellent

</td>

<td style="text-align:left;">

clean

</td>

<td style="text-align:right;">

23400

</td>

<td style="text-align:right;">

24538.64

</td>

<td style="text-align:right;">

1138.64

</td>

<td style="text-align:right;">

4.87

</td>

</tr>

<tr>

<td style="text-align:right;">

286

</td>

<td style="text-align:right;">

2007

</td>

<td style="text-align:right;">

178450

</td>

<td style="text-align:left;">

hyundai

</td>

<td style="text-align:left;">

No value

</td>

<td style="text-align:left;">

clean

</td>

<td style="text-align:right;">

4950

</td>

<td style="text-align:right;">

4859.28

</td>

<td style="text-align:right;">

90.72

</td>

<td style="text-align:right;">

1.83

</td>

</tr>

<tr>

<td style="text-align:right;">

643

</td>

<td style="text-align:right;">

2017

</td>

<td style="text-align:right;">

66765

</td>

<td style="text-align:left;">

dodge

</td>

<td style="text-align:left;">

No value

</td>

<td style="text-align:left;">

No value

</td>

<td style="text-align:right;">

22900

</td>

<td style="text-align:right;">

23366.48

</td>

<td style="text-align:right;">

466.48

</td>

<td style="text-align:right;">

2.04

</td>

</tr>

<tr>

<td style="text-align:right;">

49

</td>

<td style="text-align:right;">

2008

</td>

<td style="text-align:right;">

203516

</td>

<td style="text-align:left;">

gmc

</td>

<td style="text-align:left;">

No value

</td>

<td style="text-align:left;">

clean

</td>

<td style="text-align:right;">

9988

</td>

<td style="text-align:right;">

9497.48

</td>

<td style="text-align:right;">

490.52

</td>

<td style="text-align:right;">

4.91

</td>

</tr>

</tbody>

</table>

# Further Directions

To further imrpove the accuracy of this model we can aleviate the
problem of imbalanced classes by grouping manufacturers by region
(American, Germnan, Italian, Japanese, British, etc.) and status type
(luxery vs economy).

Although we achieved a nice accuracy of 81.5%, we can now observe some
other metrics. Eg., having an RMSE almost twice higher than MAE suggests
that there is a good number of observations where the error is big (the
more RMSE differs from MAE, the higher is the variance) This is
something we may want to improve by finding features and clusters in
data space that introduce more variance in the predictions. Eg. the
model predicting clean car price may greatly differ from the model
predicting salvage (damage / total loss) car price. This comes from
getting deeper expertise in the area, and we will try to play with this
further more.

We may also want to use a different scoring function for our model - eg.
some custom implementation of MSE of relative error, since we have high
variance of price in the original dataset.

Lastly, due to time / resources limitations we only trained the model on
half the training data, so we should try to run it on all training data
and see how this changes our model (this would take approximately 16
hours). So far we have only seen improvements to the score as we
increased the sample size.

The ultimate end goal is to eventually create a command-line tool for
the end-user to interactively request vehicle details and output
expected price with a precision interval.

# References

<div id="refs" class="references">

<div id="ref-sklearn_api">

Buitinck, Lars, Gilles Louppe, Mathieu Blondel, Fabian Pedregosa,
Andreas Mueller, Olivier Grisel, Vlad Niculae, et al. 2013. “API Design
for Machine Learning Software: Experiences from the Scikit-Learn
Project.” In *ECML Pkdd Workshop: Languages for Data Mining and Machine
Learning*, 108–22.

</div>

<div id="ref-docopt">

de Jonge, Edwin. 2018. *Docopt: Command-Line Interface Specification
Language*. <https://CRAN.R-project.org/package=docopt>.

</div>

<div id="ref-plotly">

Inc., Plotly Technologies. 2015. “Collaborative Data Science.” Montreal,
QC: Plotly Technologies Inc. 2015. <https://plot.ly>.

</div>

<div id="ref-docoptpython">

Keleshev, Vladimir. 2014. *Docopt: Command-Line Interface Description
Language*. <https://github.com/docopt/docopt>.

</div>

<div id="ref-mckinney-proc-scipy-2010">

McKinney, Wes. 2010. “Data Structures for Statistical Computing in
Python.” In *Proceedings of the 9th Python in Science Conference*,
edited by Stéfan van der Walt and Jarrod Millman, 51–56.

</div>

<div id="ref-oliphant2006guide">

Oliphant, Travis E. 2006. *A Guide to Numpy*. Vol. 1. Trelgol Publishing
USA.

</div>

<div id="ref-R">

R Core Team. 2019. *R: A Language and Environment for Statistical
Computing*. Vienna, Austria: R Foundation for Statistical Computing.
<https://www.R-project.org/>.

</div>

<div id="ref-reese_2020">

Reese, Austin. 2020. “Used Cars Dataset.” *Kaggle*.
<https://www.kaggle.com/austinreese/craigslist-carstrucks-data>.

</div>

<div id="ref-seabold2010statsmodels">

Seabold, Skipper, and Josef Perktold. 2010. “Statsmodels: Econometric
and Statistical Modeling with Python.” In *9th Python in Science
Conference*.

</div>

<div id="ref-seleniumhq_2020">

SeleniumHQ. 2020. “SeleniumHQ/Selenium.” *GitHub*.
<https://github.com/SeleniumHQ/selenium>.

</div>

<div id="ref-Altair2018">

VanderPlas, Jacob, Brian Granger, Jeffrey Heer, Dominik Moritz, Kanit
Wongsuphasawat, Arvind Satyanarayan, Eitan Lees, Ilia Timofeev, Ben
Welsh, and Scott Sievert. 2018. “Altair: Interactive Statistical
Visualizations for Python.” *Journal of Open Source Software*, December.
The Open Journal. <https://doi.org/10.21105/joss.01057>.

</div>

<div id="ref-Python">

Van Rossum, Guido, and Fred L. Drake. 2009. *Python 3 Reference Manual*.
Scotts Valley, CA: CreateSpace.

</div>

<div id="ref-tidyverse">

Wickham, Hadley, Mara Averick, Jennifer Bryan, Winston Chang, Lucy
D’Agostino McGowan, Romain François, Garrett Grolemund, et al. 2019.
“Welcome to the tidyverse.” *Journal of Open Source Software* 4 (43):
1686. <https://doi.org/10.21105/joss.01686>.

</div>

<div id="ref-readr">

Wickham, Hadley, Jim Hester, and Romain Francois. 2018. *Readr: Read
Rectangular Text Data*. <https://CRAN.R-project.org/package=readr>.

</div>

<div id="ref-knitr">

Xie, Yihui. 2014. “Knitr: A Comprehensive Tool for Reproducible Research
in R.” In *Implementing Reproducible Computational Research*, edited by
Victoria Stodden, Friedrich Leisch, and Roger D. Peng. Chapman;
Hall/CRC. <http://www.crcpress.com/product/isbn/9781466561595>.

</div>

</div>
