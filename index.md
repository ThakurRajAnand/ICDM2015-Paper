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
Drawbridge is a leading cross-device identity company. They aggregate the data from personal computing devices, mobile devices, and emerging devices to reach more than one billion consumers across more than three billion devices.

<iframe src='./assets/img/drawbridge.png' width=10px height=10px>
</iframe>

---

## Objective and Evaluation
Objective of the competition was to do device-cookie pairs mapping. Evaluation function was <B>\\(F_{0.5}\\)</B> score


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
Handling this problem as unsupervised learning was not a feasible due to too many Device-Cookie pairs possible (~2.18 million Cookies). We converted the problem to Semi-Supervised learning as follows.

1. Train and test data were created for those Device-Cookie pairs which had at least 1 IP common

2. We were able to retreive 98.3% of known Drawbridge Handles (180K) for train data set

3. We used Device-Cookie with known Drawbridge Handle as postive example and unknown as negative example

4. Directly optimizing F-score was tough when handling this problem as binary classification. We optimized AUC throughout the competition which was eventually not that helpful.

---

## Methodology : Device-Cookie pair follows Poisson distribution
Using Exploratory Data Analysis, we found device-cookie pairs follow poisson distribution with $\lambda$ = 0.2613504 and 95% confidence interval (0.2586986, 0.2640023)
<iframe src='./assets/img/poisson.jpeg' width=10px height=10px>
</iframe>

---

## Feature Engineering

<div id = 'chart1' class = 'rChart nvd3'></div>
<script type='text/javascript'>
 $(document).ready(function(){
      drawchart1()
    });
    function drawchart1(){  
      var opts = {
 "dom": "chart1",
"width":    800,
"height":    400,
"x": "Hair",
"y": "Freq",
"group": "Eye",
"type": "multiBarChart",
"id": "chart1" 
},
        data = [
 {
 "Hair": "Black",
"Eye": "Brown",
"Sex": "Male",
"Freq":             32 
},
{
 "Hair": "Brown",
"Eye": "Brown",
"Sex": "Male",
"Freq":             53 
},
{
 "Hair": "Red",
"Eye": "Brown",
"Sex": "Male",
"Freq":             10 
},
{
 "Hair": "Blond",
"Eye": "Brown",
"Sex": "Male",
"Freq":              3 
},
{
 "Hair": "Black",
"Eye": "Blue",
"Sex": "Male",
"Freq":             11 
},
{
 "Hair": "Brown",
"Eye": "Blue",
"Sex": "Male",
"Freq":             50 
},
{
 "Hair": "Red",
"Eye": "Blue",
"Sex": "Male",
"Freq":             10 
},
{
 "Hair": "Blond",
"Eye": "Blue",
"Sex": "Male",
"Freq":             30 
},
{
 "Hair": "Black",
"Eye": "Hazel",
"Sex": "Male",
"Freq":             10 
},
{
 "Hair": "Brown",
"Eye": "Hazel",
"Sex": "Male",
"Freq":             25 
},
{
 "Hair": "Red",
"Eye": "Hazel",
"Sex": "Male",
"Freq":              7 
},
{
 "Hair": "Blond",
"Eye": "Hazel",
"Sex": "Male",
"Freq":              5 
},
{
 "Hair": "Black",
"Eye": "Green",
"Sex": "Male",
"Freq":              3 
},
{
 "Hair": "Brown",
"Eye": "Green",
"Sex": "Male",
"Freq":             15 
},
{
 "Hair": "Red",
"Eye": "Green",
"Sex": "Male",
"Freq":              7 
},
{
 "Hair": "Blond",
"Eye": "Green",
"Sex": "Male",
"Freq":              8 
} 
]
  
      if(!(opts.type==="pieChart" || opts.type==="sparklinePlus" || opts.type==="bulletChart")) {
        var data = d3.nest()
          .key(function(d){
            //return opts.group === undefined ? 'main' : d[opts.group]
            //instead of main would think a better default is opts.x
            return opts.group === undefined ? opts.y : d[opts.group];
          })
          .entries(data);
      }
      
      if (opts.disabled != undefined){
        data.map(function(d, i){
          d.disabled = opts.disabled[i]
        })
      }
      
      nv.addGraph(function() {
        var chart = nv.models[opts.type]()
          .width(opts.width)
          .height(opts.height)
          
        if (opts.type != "bulletChart"){
          chart
            .x(function(d) { return d[opts.x] })
            .y(function(d) { return d[opts.y] })
        }
          
         
        
          
        

        
        
        
      
       d3.select("#" + opts.id)
        .append('svg')
        .datum(data)
        .transition().duration(500)
        .call(chart);

       nv.utils.windowResize(chart.update);
       return chart;
      });
    };
</script>

---

## Models

---

## Ensembling Approach

---

## Conclusion

---

## References
H.~Brendan McMahan, Gary Holt, D.~Sculley, Michael Young. Ad Click Prediction: a View from the Trenches. In KDD, Chicago, Illinois, USA, August 2013 <a href="url">http://www.eecs.tufts.edu/\~dsculley/papers/ad-click-prediction.pdf</a>

H.~Brendan Mcmahon. Follow-the-Regularized-Leader and Mirror Descent. <a href="url">http://jmlr.org/proceedings/papers/v15/mcmahan11b/mcmahan11b.pdf</a>

Jerome H.~Friedman. Greedy Function Approximation: A Gradient Boosting Machine. <a href="url">https://statweb.stanford.edu/\~jhf/ftp/trebst.pdf</a>

T.~Hastie, R.~Tibshirani, J.~H.~Friedman. The Elements of Statistical Learning. New York:Springer. ISBN 0-387-84857-6

Jerome H.~Friedman. Stochastic Gradient Boosting. <a href="url">https://statweb.stanford.edu/\~jhf/ftp/stobst.pdf</a>

J~Tang, H.~Liu. Unsupervised feature selection for linked social media data. In KDD, 2012.
