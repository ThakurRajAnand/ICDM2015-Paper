---
title       : ICDM 2015 - Drawbridge Cross - Device Connection Challenge 
subtitle    : Machine learning approach to identify users across their digital devices
author      : Thakur Raj Anand , Oleksii Renov
job         : Kagglers
framework   : io2012 # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides

---

## OUTLINE

1. Introduction
2. Objective and Evaluation
3. Data
4. Feature Engineering
5. Models
6. Ensembling Approach
7. Conclusion
8. References

---

## Introduction - Competition

<iframe src='./assets/img/icdm_competition.png' width=10px height=10px> 
</iframe>

---

## Introduction - Leaderboard

<iframe src='./assets/img/leaderboard.png' width=10px height=10px>
</iframe>

--- 

## Introduction - Drawbridge
Drawbridge is a leading cross-device identity company. They aggregate the data from personal computing devices, mobile devices, and emerging devices to reach more than one billion consumers across more than three billion devices.

<iframe src='./assets/img/drawbridge.png' width=10px height=10px>
</iframe>

---

## Objective and Evaluation
Objective of the competition was to do device-cookie pairs mapping. Evaluation function was \\(F_{0.5}\\) score

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
