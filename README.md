# Google Capstone Project 

# Introduction

This case study is the **Capstone Project** of ***[Google Data Analytics Professional Certificate](https://www.coursera.org/professional-certificates/google-data-analytics)*** . In this case study, I'm a junior data analyst on the marketing analyst team of Cyclistic, a fictitious Chicago bike-share firm.

Cyclistic is a bicycle-sharing scheme with over 5,800 bikes and 600 docking stations. Cyclistic distinguishes itself by including reclining bikes, hand tricycles, and cargo bikes in its fleet, making bike-share more accessible to persons with disabilities and others who can't ride a regular two-wheeled bike. The bulk of riders choose standard bikes, with only roughly 8% opting for assistive solutions. Although cyclists are more likely to ride for pleasure, about 30% of them use bikes to travel to work every day.

Cyclistic developed a successful bike-share programme in 2016. Since then, the initiative has grown to include a fleet of 5,824 bicycles that are geotracked and locked into 692 stations throughout Chicago. Bikes can be unlocked at any station and returned to any other in the system at any time.

The company's future prosperity, according to the director of marketing, hinges on increasing the number of yearly subscribers. As a result, my team is eager to learn more **how casual riders and annual members use Cyclistic bikes differently**. Our team will develop a new marketing plan based on the findings in order to convert casual riders into annual members. However, our recommendations must first be approved by Cyclistic management, therefore they must be backed up with evidence **compelling data insights and professional data visualizations**.

Single-ride passes, full-day passes, and annual memberships are all available. **Casual riders are customers who purchase single-ride or full-day passes**. **Cyclistic members are customers who purchase annual memberships**.

I followed the steps of the data analysis process to answer the key business questions: **ask**, **prepare**, **process**, **analyze**, **share**, and **act**.

# Ask

**The future marketing effort will be guided by three questions:** 
1. What are the differences in how annual members and casual riders use Cyclistic bikes? 
2. Why would a non-cyclist purchase a Cyclistic annual membership? 
3. How can Cyclistic use digital media to persuade non-members to join the club?

**The key stakeholders are:**
1. Cyclistic Executive Team.
2. Lily Moreno (Director of Marketing) & my Manager.

**Key tasks:**
1. Design marketing strategies aimed at converting casual riders into annual members. 

# Prepare

Evaluate and discover trends using Cyclistic's historical trip data. Here you can **[download the previous 12 months](https://divvy-tripdata.s3.amazonaws.com/index.html)** worth of Cyclistic trip data. **The datasets are appropriate for the objectives of this case study and will allow you to answer the business questions**. **Motivate International Inc. has made the data available under this [licence](https://ride.divvybikes.com/data-license-agreement)**. This is open data that you may use to investigate how various consumer categories utilise Cyclistic bikes. However, due to data-privacy concerns, you are unable to use riders' personally identifying information. This means you won't be able to link pass purchases to credit card numbers to see if casual riders live in the Cyclistic service region or bought multiple single passes.

The data is **accurate**, **trustworthy**, **consistent**. There are no issues with **bias** or **legitimacy** because this data is collected by a legitimate Chicago bike sharing firm. As a result, it's **Reliable**, **Original**, **Current**, and **Cited** as in **ROCCC**. I don't believe it is adequate because certain information is missing.

> **Limitations**

* No **financial information**.
* No **riders personal indentifiable information**.

# Process

# importing libraries for data cleaning, manipulating and visualization.

`import os`
`import pandas as pd`
`from pandas.api.types import CategoricalDtype`
`import numpy as np`
`import seaborn as sns`
`import matplotlib.pyplot as plt`

`import plotly.express as px`
`from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot`
`init_notebook_mode(connected=True)`

`import cufflinks as cf`

