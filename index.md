---
title       : ICDM 2015 - Drawbridge Cross - Device Connection Challenge 
subtitle    : Machine learning approach to identify users across their digital devices
author      : Thakur Raj Anand , Oleksii Renov
job         : Kagglers
framework   : io2012 # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax,bootstrap,quiz,shiny,interactive]# {mathjax, quiz, bootstrap}
ext_widgets : {rCharts: [libraries/nvd3]}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides

---

## OUTLINE

1. Introduction
2. Objective and Evaluation
3. Data
4. Methodology
5. Feature Engineering
6. Models
7. Ensembling Approach
8. Conclusion
9. References

---

## Introduction : Competition

<iframe src='./assets/img/icdm_competition.png' width=10px height=10px> 
</iframe>

---

## Introduction : Leaderboard

<iframe src='./assets/img/leaderboard.png' width=10px height=10px>
</iframe>

--- 

## Introduction : Drawbridge
Drawbridge is a leading cross-device identity company. They aggregate data from personal computing devices, mobile devices and emerging devices to reach more than one billion consumers across three billion devices.

<iframe src='./assets/img/drawbridge.png' width=10px height=10px>
</iframe>

---

## Objective and Evaluation
Objective of the competition was to map device-cookie pairs. Evaluation function was <B>\\(F_{0.5}\\)</B> score


$$
(1 + \beta^2) \frac{pr}{\beta^2 p+r}\ \ \mathrm{where}\ \ p = \frac{tp}{tp+fp},\ \ r = \frac{tp}{tp+fn},\ \beta = 0.5.
$$

<b>Precision</b> measures the number of correctly predicted positive results as a proportion of the total predicted positive results.

<b>Recall</b> measures the number of true correctly predicted positive results as a proportion of that actual positive results. 

---

## Data
4 main data tables were provided by Drawbridge

1. <B>Device</B> features like ID , Country , OS etc. <font color="red">[6 Anonymous features]</font>
2. <B>Cookie</B> features like ID , Browser Version , Country etc. <font color="red">[6 Anonymous features]</font>
3. <B>IP</B> features for <B>Device/Cookie</B> like Freq count etc. <font color="red">[5 Anonymous count features]</font>
4. <B>Property</B> features for <B>IP</B> like Total Freq etc. <font color="red">[3 Anonymous features]</font>

<B>Drawbridge</B> assigns unique <B>Drawbridge Handle</B> to Device-Cookie pairs which belong to same user. 180K  <B>Device/Cookie</B> pairs were provided with known <B> Drawbridge Handles</B>.

---

## Methodolgy : Unsupervised to Semi-Supervised
Handling this problem as an unsupervised learning problem was not feasible as there were many device-cookie pairs possible (~2.18 million cookies). We converted the problem to a semi- supervised learning problem using the following methodology:

1. Train and test data was created for device-cookie with atleast one common IP

2. We were able to retreive 98.3% of known Drawbridge Handles (180K) for train data set

3. We used device-cookie pairs with a known Drawbridge handle as a positive example, and one with unknown Drawbridge handle as a negative example

4. Directly optimizing F-score was tough when handling this problem as binary classification. We optimized AUC throughout the competition which was eventually not that helpful.

---

## Methodology : Device - Cookie pair follows Poisson distribution
Using Exploratory Data Analysis, we found device-cookie pairs follow poisson distribution with $\lambda$ = 0.2613504 and 95% confidence interval (0.2586986, 0.2640023)
<iframe src='./assets/img/poisson.jpeg' width=10px height=10px>
</iframe>

---

## Feature Engineering : Naive Similarity Statistics

<div style='width:800; overflow:auto; border-width: 2'><!-- html table generated in R 3.1.0 by xtable 1.7-4 package -->
<!-- Thu Sep 17 07:48:40 2015 -->
<table border=1>
<tr> <th> Feature.Name </th> <th> Num.of.Matched.Pairs </th> <th> Num.Of.Unique.Matched.Pairs </th> <th> Filtering.in.Percentage </th> <th> Percentage.of.Unique.Matched.Pairs.Saved </th>  </tr>
  <tr> <td> N/A </td> <td>        251869 </td> <td>        176709 </td> <td>        100% </td> <td>        98.12% </td> </tr>
  <tr> <td> same c0 </td> <td>        106504 </td> <td>        74550 </td> <td>        36.40% </td> <td>        41.39% </td> </tr>
  <tr> <td> same c1 </td> <td>        59139 </td> <td>        40728 </td> <td>        09.87% </td> <td>        22.61% </td> </tr>
  <tr> <td> same c2 </td> <td>        91395 </td> <td>        60935 </td> <td>        15.08% </td> <td>        33.83% </td> </tr>
  <tr> <td> same 5 </td> <td>        7440 </td> <td>        5226 </td> <td>        04.23% </td> <td>        02.90% </td> </tr>
  <tr> <td> same 6 </td> <td>        64212 </td> <td>        40514 </td> <td>        25.74% </td> <td>        22.49% </td> </tr>
  <tr> <td> same 7 </td> <td>        45707 </td> <td>        23996 </td> <td>        36.96% </td> <td>        13.32% </td> </tr>
  <tr> <td> idix same c1 </td> <td>        51637 </td> <td>        26545 </td> <td>        48.90% </td> <td>        14.74% </td> </tr>
  <tr> <td> idix same c2 </td> <td>        12810 </td> <td>        5972 </td> <td>        12.75% </td> <td>        03.31% </td> </tr>
  <tr> <td> idix same c3 </td> <td>        36466 </td> <td>        16290 </td> <td>        61.45% </td> <td>        09.04% </td> </tr>
  <tr> <td> idix same c4 </td> <td>        70391 </td> <td>        37991 </td> <td>        50.65% </td> <td>        21.09% </td> </tr>
  <tr> <td> idix same c5 </td> <td>        39550 </td> <td>        18820 </td> <td>        46.49% </td> <td>        10.45% </td> </tr>
  <tr> <td> same country </td> <td>        240512 </td> <td>        169072 </td> <td>        81.93% </td> <td>        93.88% </td> </tr>
   </table>
</div>

---

## Feature Engineering : Latent Features Statistics

1. <B>LogCount1</B> - Number of unique IPs on which specific combination of (device, cookie) occurs

2. <B>LogCount2</B> - Number of unique cookies on which specific combination of (device, IP) occurs

3. <B>LogCount3</B> - Number of unique devices on which specific combination of (cookie, IP) occurs

4. <B>Latent1</B> - Binary feature which was used to filter number of possible cookies for each device. We computed the sum of all IP frequencies of device grouped by device and cookie. If this specific sum is greater than or equal to the median of all sums by device, then Latent1 is set to 1 else it is set to 0.


--- 

## Models - FTRL and XGBOOST

We used following 2 models:

1. <b>FTRL</b> : FTRL is a online version of regularized logistic regression and gives lot of flexibility to run the models on big data sets quickly 

2. <b>XGBOOST</b> : XGBOOST is a library developed by DMLC and currently considered as the most powerful Gradient Boosting library (Linear & Tree)

We primarily used FTRL to test new features and take LB feedback to decide if feature is adding value or not.

---

## Models - Cross Validation Score
<B>3-Fold-CV-Score for XGBOOST </B>
<div style='width:800; overflow:auto; border-width: 2'><!-- html table generated in R 3.1.0 by xtable 1.7-4 package -->
<!-- Thu Sep 17 10:18:25 2015 -->
<table border=1>
<tr> <th> Probability </th> <th> Lambda </th> <th> X3.Fold.CV </th>  </tr>
  <tr> <td align="right"> 0.99 </td> <td align="right"> 0.01 </td> <td align="right"> 0.79 </td> </tr>
  <tr> <td align="right"> 0.99 </td> <td align="right"> 0.01 </td> <td align="right"> 0.79 </td> </tr>
  <tr> <td align="right"> 0.98 </td> <td align="right"> 0.05 </td> <td align="right"> 0.79 </td> </tr>
  <tr> <td align="right"> 0.98 </td> <td align="right"> 0.09 </td> <td align="right"> 0.77 </td> </tr>
  <tr> <td align="right"> 975.00 </td> <td align="right"> 0.12 </td> <td align="right"> 0.77 </td> </tr>
   </table>
</div>


---

## Ensembling Approach

---

## Conclusion
We did intensive feature engineering and analysis for naive similarity features, latent features and transformation of categorical variables to their empirical analogues.

Ensemble of 3 GBM gave us 0.809 (post deadline) on the leaderboard. Our strategy to ensemble using Poisson distribution seems to be the right choice to achieve high F-score for this problem. 

Our score 0.809 was achieved using $\lambda = 0.12$, while we observed that F-score should improve till $\lambda \leq 0.26$. We think by including more models in the ensemble we can improve the F-score significantly. Our strategy also gives flexibility to work on very large data set as it may not be feasible to do K-Fold cross validation for such large data sets. 

We think our strategy of using Poisson distribution can be used as generic method in settings where one user has to be connected to multiple items e.g recommender systems.

Our strategy remains the area of our research, which we will continue to explore and publish the result on other similar data sets.

---

## References
H.~Brendan McMahan, Gary Holt, D.~Sculley, Michael Young. Ad Click Prediction: a View from the Trenches. In KDD, Chicago, Illinois, USA, August 2013 <a href="url">http://www.eecs.tufts.edu/\~dsculley/papers/ad-click-prediction.pdf</a>

H.~Brendan Mcmahon. Follow-the-Regularized-Leader and Mirror Descent. <a href="url">http://jmlr.org/proceedings/papers/v15/mcmahan11b/mcmahan11b.pdf</a>

Jerome H.~Friedman. Greedy Function Approximation: A Gradient Boosting Machine. <a href="url">https://statweb.stanford.edu/\~jhf/ftp/trebst.pdf</a>

T.~Hastie, R.~Tibshirani, J.~H.~Friedman. The Elements of Statistical Learning. New York:Springer. ISBN 0-387-84857-6

Jerome H.~Friedman. Stochastic Gradient Boosting. <a href="url">https://statweb.stanford.edu/\~jhf/ftp/stobst.pdf</a>

J~Tang, H.~Liu. Unsupervised feature selection for linked social media data. In KDD, 2012.
