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


```python
# importing libraries for data cleaning, manipulating and visualization.

import os
import pandas as pd
from pandas.api.types import CategoricalDtype
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

import plotly.express as px
from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot
init_notebook_mode(connected=True)

import cufflinks as cf


```


<script type="text/javascript">
window.PlotlyConfig = {MathJaxConfig: 'local'};
if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
if (typeof require !== 'undefined') {
require.undef("plotly");
requirejs.config({
    paths: {
        'plotly': ['https://cdn.plot.ly/plotly-2.9.0.min']
    }
});
require(['plotly'], function(Plotly) {
    window._Plotly = Plotly;
});
}
</script>




```python
# choosing working directory..

os.chdir("/Users/rohitpawar/Desktop/google-case-study")

# importing 12 csv files ..

mar_2021 = pd.read_csv('202103-divvy-tripdata.csv')
apr_2021 = pd.read_csv('202104-divvy-tripdata.csv')
may_2021 = pd.read_csv('202105-divvy-tripdata.csv')
jun_2021 = pd.read_csv('202106-divvy-tripdata.csv')
jul_2021 = pd.read_csv('202107-divvy-tripdata.csv')
aug_2021 = pd.read_csv('202108-divvy-tripdata.csv')
sep_2021 = pd.read_csv('202109-divvy-tripdata.csv')
oct_2021 = pd.read_csv('202110-divvy-tripdata.csv')
nov_2021 = pd.read_csv('202111-divvy-tripdata.csv')
dec_2021 = pd.read_csv('202112-divvy-tripdata.csv')
jan_2022 = pd.read_csv('202201-divvy-tripdata.csv')
feb_2022 = pd.read_csv('202202-divvy-tripdata.csv')
```


```python
# lets join all data sets into one data frame..

df = pd.concat([mar_2021,apr_2021,may_2021,jun_2021,jul_2021,aug_2021,sep_2021,oct_2021,nov_2021,dec_2021,jan_2022,feb_2022])
```


```python
# know your data..

df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>start_station_name</th>
      <th>start_station_id</th>
      <th>end_station_name</th>
      <th>end_station_id</th>
      <th>start_lat</th>
      <th>start_lng</th>
      <th>end_lat</th>
      <th>end_lng</th>
      <th>member_casual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CFA86D4455AA1030</td>
      <td>classic_bike</td>
      <td>2021-03-16 08:32:30</td>
      <td>2021-03-16 08:36:34</td>
      <td>Humboldt Blvd &amp; Armitage Ave</td>
      <td>15651</td>
      <td>Stave St &amp; Armitage Ave</td>
      <td>13266</td>
      <td>41.917513</td>
      <td>-87.701809</td>
      <td>41.917741</td>
      <td>-87.691392</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>1</th>
      <td>30D9DC61227D1AF3</td>
      <td>classic_bike</td>
      <td>2021-03-28 01:26:28</td>
      <td>2021-03-28 01:36:55</td>
      <td>Humboldt Blvd &amp; Armitage Ave</td>
      <td>15651</td>
      <td>Central Park Ave &amp; Bloomingdale Ave</td>
      <td>18017</td>
      <td>41.917513</td>
      <td>-87.701809</td>
      <td>41.914166</td>
      <td>-87.716755</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>2</th>
      <td>846D87A15682A284</td>
      <td>classic_bike</td>
      <td>2021-03-11 21:17:29</td>
      <td>2021-03-11 21:33:53</td>
      <td>Shields Ave &amp; 28th Pl</td>
      <td>15443</td>
      <td>Halsted St &amp; 35th St</td>
      <td>TA1308000043</td>
      <td>41.842733</td>
      <td>-87.635491</td>
      <td>41.830661</td>
      <td>-87.647172</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>3</th>
      <td>994D05AA75A168F2</td>
      <td>classic_bike</td>
      <td>2021-03-11 13:26:42</td>
      <td>2021-03-11 13:55:41</td>
      <td>Winthrop Ave &amp; Lawrence Ave</td>
      <td>TA1308000021</td>
      <td>Broadway &amp; Sheridan Rd</td>
      <td>13323</td>
      <td>41.968812</td>
      <td>-87.657659</td>
      <td>41.952833</td>
      <td>-87.649993</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>4</th>
      <td>DF7464FBE92D8308</td>
      <td>classic_bike</td>
      <td>2021-03-21 09:09:37</td>
      <td>2021-03-21 09:27:33</td>
      <td>Glenwood Ave &amp; Touhy Ave</td>
      <td>525</td>
      <td>Chicago Ave &amp; Sheridan Rd</td>
      <td>E008</td>
      <td>42.012701</td>
      <td>-87.666058</td>
      <td>42.050491</td>
      <td>-87.677821</td>
      <td>casual</td>
    </tr>
  </tbody>
</table>
</div>




```python
# converting data type of started_at and ended_at columns into datetime..

df['started_at'] = pd.to_datetime(df['started_at'], format='%Y-%m-%d %H:%M:%S')
df['ended_at'] = pd.to_datetime(df['ended_at'], format='%Y-%m-%d %H:%M:%S')
```


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 5667986 entries, 0 to 115608
    Data columns (total 13 columns):
     #   Column              Dtype         
    ---  ------              -----         
     0   ride_id             object        
     1   rideable_type       object        
     2   started_at          datetime64[ns]
     3   ended_at            datetime64[ns]
     4   start_station_name  object        
     5   start_station_id    object        
     6   end_station_name    object        
     7   end_station_id      object        
     8   start_lat           float64       
     9   start_lng           float64       
     10  end_lat             float64       
     11  end_lng             float64       
     12  member_casual       object        
    dtypes: datetime64[ns](2), float64(4), object(7)
    memory usage: 605.4+ MB



```python
# getting rid of some columns which are not relevant for this analysis..
df = df.drop(columns=['start_station_name','start_station_id','end_station_name','end_station_id','start_lat',
                      'start_lng','end_lat','end_lng'])
```


```python
# lets create a new column called 'ride_length' and covert into int..

df['ride_length'] = (df['ended_at'] - df['started_at'])/pd.Timedelta(minutes=1)
df['ride_length'] = df['ride_length'].astype('int32')
df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>member_casual</th>
      <th>ride_length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CFA86D4455AA1030</td>
      <td>classic_bike</td>
      <td>2021-03-16 08:32:30</td>
      <td>2021-03-16 08:36:34</td>
      <td>casual</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>30D9DC61227D1AF3</td>
      <td>classic_bike</td>
      <td>2021-03-28 01:26:28</td>
      <td>2021-03-28 01:36:55</td>
      <td>casual</td>
      <td>10</td>
    </tr>
    <tr>
      <th>2</th>
      <td>846D87A15682A284</td>
      <td>classic_bike</td>
      <td>2021-03-11 21:17:29</td>
      <td>2021-03-11 21:33:53</td>
      <td>casual</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>994D05AA75A168F2</td>
      <td>classic_bike</td>
      <td>2021-03-11 13:26:42</td>
      <td>2021-03-11 13:55:41</td>
      <td>casual</td>
      <td>28</td>
    </tr>
    <tr>
      <th>4</th>
      <td>DF7464FBE92D8308</td>
      <td>classic_bike</td>
      <td>2021-03-21 09:09:37</td>
      <td>2021-03-21 09:27:33</td>
      <td>casual</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# in ride_length column, it seem to have negative and values.
# lets make a new data frame which wouldnt contain negative and less then 1 values for 'ride_length' column..
# trips below 1 mins are probably not relevant for this analysis..
# creating a new data frame.

trimmed_df = df[df['ride_length'] >= 1]
trimmed_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>member_casual</th>
      <th>ride_length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CFA86D4455AA1030</td>
      <td>classic_bike</td>
      <td>2021-03-16 08:32:30</td>
      <td>2021-03-16 08:36:34</td>
      <td>casual</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>30D9DC61227D1AF3</td>
      <td>classic_bike</td>
      <td>2021-03-28 01:26:28</td>
      <td>2021-03-28 01:36:55</td>
      <td>casual</td>
      <td>10</td>
    </tr>
    <tr>
      <th>2</th>
      <td>846D87A15682A284</td>
      <td>classic_bike</td>
      <td>2021-03-11 21:17:29</td>
      <td>2021-03-11 21:33:53</td>
      <td>casual</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>994D05AA75A168F2</td>
      <td>classic_bike</td>
      <td>2021-03-11 13:26:42</td>
      <td>2021-03-11 13:55:41</td>
      <td>casual</td>
      <td>28</td>
    </tr>
    <tr>
      <th>4</th>
      <td>DF7464FBE92D8308</td>
      <td>classic_bike</td>
      <td>2021-03-21 09:09:37</td>
      <td>2021-03-21 09:27:33</td>
      <td>casual</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# coverting data type of some columns in order to perform calculations..

trimmed_df =trimmed_df.astype({'ride_id':'string', 'rideable_type':'category', 'member_casual':'category'})
trimmed_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 5580736 entries, 0 to 115608
    Data columns (total 6 columns):
     #   Column         Dtype         
    ---  ------         -----         
     0   ride_id        string        
     1   rideable_type  category      
     2   started_at     datetime64[ns]
     3   ended_at       datetime64[ns]
     4   member_casual  category      
     5   ride_length    int32         
    dtypes: category(2), datetime64[ns](2), int32(1), string(1)
    memory usage: 202.2 MB



```python
trimmed_df.groupby('member_casual').count()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>ride_length</th>
    </tr>
    <tr>
      <th>member_casual</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>casual</th>
      <td>2506221</td>
      <td>2506221</td>
      <td>2506221</td>
      <td>2506221</td>
      <td>2506221</td>
    </tr>
    <tr>
      <th>member</th>
      <td>3074515</td>
      <td>3074515</td>
      <td>3074515</td>
      <td>3074515</td>
      <td>3074515</td>
    </tr>
  </tbody>
</table>
</div>




```python
# lets prepare data for analysis..
# lets create new columns for year, month, day_of_week, date, and hour..

# year.
trimmed_df['year'] = trimmed_df['started_at'].dt.year

# month.
m_name = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
trimmed_df['month'] = trimmed_df['started_at'].dt.month_name()
trimmed_df['month'] = trimmed_df['month'].astype(CategoricalDtype(categories=m_name, ordered=False))

# date.
trimmed_df['date'] = trimmed_df['started_at'].dt.date

# day of week.
d_name = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
trimmed_df['day_of_week'] = trimmed_df['started_at'].dt.day_name()
trimmed_df['day_of_week'] = trimmed_df['day_of_week'].astype(CategoricalDtype(categories=d_name, ordered=False))

# hour.
trimmed_df['hour'] = trimmed_df['started_at'].dt.hour

# converting year and hour data types..
trimmed_df = trimmed_df.astype({'year':'int16', 'hour':'int8'})




```


```python
trimmed_df.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>member_casual</th>
      <th>ride_length</th>
      <th>year</th>
      <th>month</th>
      <th>date</th>
      <th>day_of_week</th>
      <th>hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>115604</th>
      <td>211BE0DC162D85B7</td>
      <td>electric_bike</td>
      <td>2022-02-23 17:47:49</td>
      <td>2022-02-23 18:02:29</td>
      <td>member</td>
      <td>14</td>
      <td>2022</td>
      <td>February</td>
      <td>2022-02-23</td>
      <td>Wednesday</td>
      <td>17</td>
    </tr>
    <tr>
      <th>115605</th>
      <td>D4D53E78000C8CA1</td>
      <td>electric_bike</td>
      <td>2022-02-04 10:43:47</td>
      <td>2022-02-04 10:50:52</td>
      <td>member</td>
      <td>7</td>
      <td>2022</td>
      <td>February</td>
      <td>2022-02-04</td>
      <td>Friday</td>
      <td>10</td>
    </tr>
    <tr>
      <th>115606</th>
      <td>9E85F07D2F94492B</td>
      <td>electric_bike</td>
      <td>2022-02-28 09:16:33</td>
      <td>2022-02-28 09:28:11</td>
      <td>member</td>
      <td>11</td>
      <td>2022</td>
      <td>February</td>
      <td>2022-02-28</td>
      <td>Monday</td>
      <td>9</td>
    </tr>
    <tr>
      <th>115607</th>
      <td>B61B559F81F1D823</td>
      <td>electric_bike</td>
      <td>2022-02-10 16:55:16</td>
      <td>2022-02-10 16:57:53</td>
      <td>member</td>
      <td>2</td>
      <td>2022</td>
      <td>February</td>
      <td>2022-02-10</td>
      <td>Thursday</td>
      <td>16</td>
    </tr>
    <tr>
      <th>115608</th>
      <td>841C701610CF0609</td>
      <td>electric_bike</td>
      <td>2022-02-21 16:35:20</td>
      <td>2022-02-21 16:42:53</td>
      <td>member</td>
      <td>7</td>
      <td>2022</td>
      <td>February</td>
      <td>2022-02-21</td>
      <td>Monday</td>
      <td>16</td>
    </tr>
  </tbody>
</table>
</div>



# Analyze & Share

**Compare Annual members and Casual riders on the basis of some parameters, like:** 
* types of bike, 
* ride length, 
* year, months(seasons), 
* hours, 
* day of week..
 


## figure1


```python
# Grouping..
figure1 = trimmed_df.groupby('member_casual', as_index=False).count()
figure1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>member_casual</th>
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>ride_length</th>
      <th>year</th>
      <th>month</th>
      <th>date</th>
      <th>day_of_week</th>
      <th>hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>casual</td>
      <td>2506221</td>
      <td>2506221</td>
      <td>2506221</td>
      <td>2506221</td>
      <td>2506221</td>
      <td>2506221</td>
      <td>2506221</td>
      <td>2506221</td>
      <td>2506221</td>
      <td>2506221</td>
    </tr>
    <tr>
      <th>1</th>
      <td>member</td>
      <td>3074515</td>
      <td>3074515</td>
      <td>3074515</td>
      <td>3074515</td>
      <td>3074515</td>
      <td>3074515</td>
      <td>3074515</td>
      <td>3074515</td>
      <td>3074515</td>
      <td>3074515</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Plotting..
px.bar(figure1, y = 'member_casual', x = 'ride_length',range_x = [0,3000000], color = 'member_casual', height = 250,text = 'ride_length', 
        labels = {'ride_length': 'Number Of Rides', 'member_casual': 'Member Vs. Casual'},
        hover_name = 'member_casual', hover_data = {'member_casual': False, 'month': False, 'ride_length': True}, 
        color_discrete_map = {'casual': '#F5F5DC', 'member': '#00FFFF'})
```


<div>                            <div id="62acc562-cb72-472c-b054-371af62c1c67" class="plotly-graph-div" style="height:250px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("62acc562-cb72-472c-b054-371af62c1c67")) {                    Plotly.newPlot(                        "62acc562-cb72-472c-b054-371af62c1c67",                        [{"alignmentgroup":"True","customdata":[[2506221]],"hovertemplate":"<b>%{hovertext}</b><br><br>Number Of Rides=%{text}<extra></extra>","hovertext":["casual"],"legendgroup":"casual","marker":{"color":"#F5F5DC","pattern":{"shape":""}},"name":"casual","offsetgroup":"casual","orientation":"h","showlegend":true,"text":[2506221.0],"textposition":"auto","x":[2506221],"xaxis":"x","y":["casual"],"yaxis":"y","type":"bar"},{"alignmentgroup":"True","customdata":[[3074515]],"hovertemplate":"<b>%{hovertext}</b><br><br>Number Of Rides=%{text}<extra></extra>","hovertext":["member"],"legendgroup":"member","marker":{"color":"#00FFFF","pattern":{"shape":""}},"name":"member","offsetgroup":"member","orientation":"h","showlegend":true,"text":[3074515.0],"textposition":"auto","x":[3074515],"xaxis":"x","y":["member"],"yaxis":"y","type":"bar"}],                        {"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Number Of Rides"},"range":[0,3000000]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Member Vs. Casual"},"categoryorder":"array","categoryarray":["member","casual"]},"legend":{"title":{"text":"Member Vs. Casual"},"tracegroupgap":0},"margin":{"t":60},"barmode":"relative","height":250},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('62acc562-cb72-472c-b054-371af62c1c67');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


* **Annual members** have more rides than **Casual riders**, as shown in the graph.

## figure2


```python
# Grouping..
figure2 = round(trimmed_df.groupby(['member_casual','rideable_type'], as_index=False).count(),2).dropna()
figure2

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>member_casual</th>
      <th>rideable_type</th>
      <th>ride_id</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>ride_length</th>
      <th>year</th>
      <th>month</th>
      <th>date</th>
      <th>day_of_week</th>
      <th>hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>casual</td>
      <td>classic_bike</td>
      <td>1254496</td>
      <td>1254496</td>
      <td>1254496</td>
      <td>1254496</td>
      <td>1254496</td>
      <td>1254496</td>
      <td>1254496</td>
      <td>1254496</td>
      <td>1254496</td>
    </tr>
    <tr>
      <th>1</th>
      <td>casual</td>
      <td>docked_bike</td>
      <td>309359</td>
      <td>309359</td>
      <td>309359</td>
      <td>309359</td>
      <td>309359</td>
      <td>309359</td>
      <td>309359</td>
      <td>309359</td>
      <td>309359</td>
    </tr>
    <tr>
      <th>2</th>
      <td>casual</td>
      <td>electric_bike</td>
      <td>942366</td>
      <td>942366</td>
      <td>942366</td>
      <td>942366</td>
      <td>942366</td>
      <td>942366</td>
      <td>942366</td>
      <td>942366</td>
      <td>942366</td>
    </tr>
    <tr>
      <th>3</th>
      <td>member</td>
      <td>classic_bike</td>
      <td>1971782</td>
      <td>1971782</td>
      <td>1971782</td>
      <td>1971782</td>
      <td>1971782</td>
      <td>1971782</td>
      <td>1971782</td>
      <td>1971782</td>
      <td>1971782</td>
    </tr>
    <tr>
      <th>4</th>
      <td>member</td>
      <td>docked_bike</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>member</td>
      <td>electric_bike</td>
      <td>1102733</td>
      <td>1102733</td>
      <td>1102733</td>
      <td>1102733</td>
      <td>1102733</td>
      <td>1102733</td>
      <td>1102733</td>
      <td>1102733</td>
      <td>1102733</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Plotting..
px.bar(figure2, y = 'member_casual', x = 'ride_length',range_x = [0,3000000], color = 'rideable_type', height = 250,text = 'ride_length', 
        labels = {'ride_length': 'Number Of Rides/Bike', 'member_casual': 'Member Vs. Casual'},
        hover_name = 'member_casual', hover_data = {'member_casual': False, 'month': False, 'ride_length': True}, 
        color_discrete_map = {'casual': '#FFF8DC', 'member': '#00FFFF'})
```


<div>                            <div id="c12b4c36-ae7a-4c7f-b4c4-827818dc8d07" class="plotly-graph-div" style="height:250px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("c12b4c36-ae7a-4c7f-b4c4-827818dc8d07")) {                    Plotly.newPlot(                        "c12b4c36-ae7a-4c7f-b4c4-827818dc8d07",                        [{"alignmentgroup":"True","customdata":[[1254496],[1971782]],"hovertemplate":"<b>%{hovertext}</b><br><br>rideable_type=classic_bike<br>Number Of Rides/Bike=%{text}<extra></extra>","hovertext":["casual","member"],"legendgroup":"classic_bike","marker":{"color":"#00cc96","pattern":{"shape":""}},"name":"classic_bike","offsetgroup":"classic_bike","orientation":"h","showlegend":true,"text":[1254496.0,1971782.0],"textposition":"auto","x":[1254496,1971782],"xaxis":"x","y":["casual","member"],"yaxis":"y","type":"bar"},{"alignmentgroup":"True","customdata":[[309359],[0]],"hovertemplate":"<b>%{hovertext}</b><br><br>rideable_type=docked_bike<br>Number Of Rides/Bike=%{text}<extra></extra>","hovertext":["casual","member"],"legendgroup":"docked_bike","marker":{"color":"#ab63fa","pattern":{"shape":""}},"name":"docked_bike","offsetgroup":"docked_bike","orientation":"h","showlegend":true,"text":[309359.0,0.0],"textposition":"auto","x":[309359,0],"xaxis":"x","y":["casual","member"],"yaxis":"y","type":"bar"},{"alignmentgroup":"True","customdata":[[942366],[1102733]],"hovertemplate":"<b>%{hovertext}</b><br><br>rideable_type=electric_bike<br>Number Of Rides/Bike=%{text}<extra></extra>","hovertext":["casual","member"],"legendgroup":"electric_bike","marker":{"color":"#FFA15A","pattern":{"shape":""}},"name":"electric_bike","offsetgroup":"electric_bike","orientation":"h","showlegend":true,"text":[942366.0,1102733.0],"textposition":"auto","x":[942366,1102733],"xaxis":"x","y":["casual","member"],"yaxis":"y","type":"bar"}],                        {"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Number Of Rides/Bike"},"range":[0,3000000]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Member Vs. Casual"}},"legend":{"title":{"text":"rideable_type"},"tracegroupgap":0},"margin":{"t":60},"barmode":"relative","height":250},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('c12b4c36-ae7a-4c7f-b4c4-827818dc8d07');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


* The **Classic Bike** appears to be the most popular bike among both, but it is primarily used by **Annual Members**. 

## figure3


```python
# Grouping..
figure3 = trimmed_df.groupby(['year', 'month', 'member_casual'], as_index=False).count()
figure3 = figure3[figure3['ride_id'] != 0]
figure3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>month</th>
      <th>member_casual</th>
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>ride_length</th>
      <th>date</th>
      <th>day_of_week</th>
      <th>hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>2021</td>
      <td>March</td>
      <td>casual</td>
      <td>83148</td>
      <td>83148</td>
      <td>83148</td>
      <td>83148</td>
      <td>83148</td>
      <td>83148</td>
      <td>83148</td>
      <td>83148</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2021</td>
      <td>March</td>
      <td>member</td>
      <td>142375</td>
      <td>142375</td>
      <td>142375</td>
      <td>142375</td>
      <td>142375</td>
      <td>142375</td>
      <td>142375</td>
      <td>142375</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2021</td>
      <td>April</td>
      <td>casual</td>
      <td>134945</td>
      <td>134945</td>
      <td>134945</td>
      <td>134945</td>
      <td>134945</td>
      <td>134945</td>
      <td>134945</td>
      <td>134945</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2021</td>
      <td>April</td>
      <td>member</td>
      <td>197477</td>
      <td>197477</td>
      <td>197477</td>
      <td>197477</td>
      <td>197477</td>
      <td>197477</td>
      <td>197477</td>
      <td>197477</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2021</td>
      <td>May</td>
      <td>casual</td>
      <td>253346</td>
      <td>253346</td>
      <td>253346</td>
      <td>253346</td>
      <td>253346</td>
      <td>253346</td>
      <td>253346</td>
      <td>253346</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2021</td>
      <td>May</td>
      <td>member</td>
      <td>269897</td>
      <td>269897</td>
      <td>269897</td>
      <td>269897</td>
      <td>269897</td>
      <td>269897</td>
      <td>269897</td>
      <td>269897</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2021</td>
      <td>June</td>
      <td>casual</td>
      <td>365023</td>
      <td>365023</td>
      <td>365023</td>
      <td>365023</td>
      <td>365023</td>
      <td>365023</td>
      <td>365023</td>
      <td>365023</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2021</td>
      <td>June</td>
      <td>member</td>
      <td>352676</td>
      <td>352676</td>
      <td>352676</td>
      <td>352676</td>
      <td>352676</td>
      <td>352676</td>
      <td>352676</td>
      <td>352676</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2021</td>
      <td>July</td>
      <td>casual</td>
      <td>435927</td>
      <td>435927</td>
      <td>435927</td>
      <td>435927</td>
      <td>435927</td>
      <td>435927</td>
      <td>435927</td>
      <td>435927</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2021</td>
      <td>July</td>
      <td>member</td>
      <td>373833</td>
      <td>373833</td>
      <td>373833</td>
      <td>373833</td>
      <td>373833</td>
      <td>373833</td>
      <td>373833</td>
      <td>373833</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2021</td>
      <td>August</td>
      <td>casual</td>
      <td>407517</td>
      <td>407517</td>
      <td>407517</td>
      <td>407517</td>
      <td>407517</td>
      <td>407517</td>
      <td>407517</td>
      <td>407517</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2021</td>
      <td>August</td>
      <td>member</td>
      <td>385426</td>
      <td>385426</td>
      <td>385426</td>
      <td>385426</td>
      <td>385426</td>
      <td>385426</td>
      <td>385426</td>
      <td>385426</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2021</td>
      <td>September</td>
      <td>casual</td>
      <td>359226</td>
      <td>359226</td>
      <td>359226</td>
      <td>359226</td>
      <td>359226</td>
      <td>359226</td>
      <td>359226</td>
      <td>359226</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2021</td>
      <td>September</td>
      <td>member</td>
      <td>385959</td>
      <td>385959</td>
      <td>385959</td>
      <td>385959</td>
      <td>385959</td>
      <td>385959</td>
      <td>385959</td>
      <td>385959</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2021</td>
      <td>October</td>
      <td>casual</td>
      <td>253829</td>
      <td>253829</td>
      <td>253829</td>
      <td>253829</td>
      <td>253829</td>
      <td>253829</td>
      <td>253829</td>
      <td>253829</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2021</td>
      <td>October</td>
      <td>member</td>
      <td>367460</td>
      <td>367460</td>
      <td>367460</td>
      <td>367460</td>
      <td>367460</td>
      <td>367460</td>
      <td>367460</td>
      <td>367460</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2021</td>
      <td>November</td>
      <td>casual</td>
      <td>105373</td>
      <td>105373</td>
      <td>105373</td>
      <td>105373</td>
      <td>105373</td>
      <td>105373</td>
      <td>105373</td>
      <td>105373</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2021</td>
      <td>November</td>
      <td>member</td>
      <td>248699</td>
      <td>248699</td>
      <td>248699</td>
      <td>248699</td>
      <td>248699</td>
      <td>248699</td>
      <td>248699</td>
      <td>248699</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2021</td>
      <td>December</td>
      <td>casual</td>
      <td>68752</td>
      <td>68752</td>
      <td>68752</td>
      <td>68752</td>
      <td>68752</td>
      <td>68752</td>
      <td>68752</td>
      <td>68752</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2021</td>
      <td>December</td>
      <td>member</td>
      <td>174878</td>
      <td>174878</td>
      <td>174878</td>
      <td>174878</td>
      <td>174878</td>
      <td>174878</td>
      <td>174878</td>
      <td>174878</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2022</td>
      <td>January</td>
      <td>casual</td>
      <td>18154</td>
      <td>18154</td>
      <td>18154</td>
      <td>18154</td>
      <td>18154</td>
      <td>18154</td>
      <td>18154</td>
      <td>18154</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2022</td>
      <td>January</td>
      <td>member</td>
      <td>83765</td>
      <td>83765</td>
      <td>83765</td>
      <td>83765</td>
      <td>83765</td>
      <td>83765</td>
      <td>83765</td>
      <td>83765</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2022</td>
      <td>February</td>
      <td>casual</td>
      <td>20981</td>
      <td>20981</td>
      <td>20981</td>
      <td>20981</td>
      <td>20981</td>
      <td>20981</td>
      <td>20981</td>
      <td>20981</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2022</td>
      <td>February</td>
      <td>member</td>
      <td>92070</td>
      <td>92070</td>
      <td>92070</td>
      <td>92070</td>
      <td>92070</td>
      <td>92070</td>
      <td>92070</td>
      <td>92070</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Plotting..
px.line(figure3, x = 'month', y = 'ride_id', range_y = [0,500000], color = 'member_casual', line_shape = 'spline', markers=True, 
        labels = {'ride_id': 'Number Of Rides', 'month': 'Months (Mar 2021 - Feb 2022)', 'member_casual': 'Member Vs. Casual'},
        hover_name = 'member_casual', hover_data = {'member_casual': False, 'month': True, 'ride_id': True}, 
        color_discrete_map = {'casual': '#151516', 'member': '#E24A33'})
```


<div>                            <div id="8b194aa3-55b1-4c0e-9aca-30b607109aff" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("8b194aa3-55b1-4c0e-9aca-30b607109aff")) {                    Plotly.newPlot(                        "8b194aa3-55b1-4c0e-9aca-30b607109aff",                        [{"customdata":[["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"]],"hovertemplate":"<b>%{hovertext}</b><br><br>Months (Mar 2021 - Feb 2022)=%{x}<br>Number Of Rides=%{y}<extra></extra>","hovertext":["casual","casual","casual","casual","casual","casual","casual","casual","casual","casual","casual","casual"],"legendgroup":"casual","line":{"color":"#151516","dash":"solid","shape":"spline"},"marker":{"symbol":"circle"},"mode":"markers+lines","name":"casual","orientation":"v","showlegend":true,"x":["March","April","May","June","July","August","September","October","November","December","January","February"],"xaxis":"x","y":[83148,134945,253346,365023,435927,407517,359226,253829,105373,68752,18154,20981],"yaxis":"y","type":"scatter"},{"customdata":[["member"],["member"],["member"],["member"],["member"],["member"],["member"],["member"],["member"],["member"],["member"],["member"]],"hovertemplate":"<b>%{hovertext}</b><br><br>Months (Mar 2021 - Feb 2022)=%{x}<br>Number Of Rides=%{y}<extra></extra>","hovertext":["member","member","member","member","member","member","member","member","member","member","member","member"],"legendgroup":"member","line":{"color":"#E24A33","dash":"solid","shape":"spline"},"marker":{"symbol":"circle"},"mode":"markers+lines","name":"member","orientation":"v","showlegend":true,"x":["March","April","May","June","July","August","September","October","November","December","January","February"],"xaxis":"x","y":[142375,197477,269897,352676,373833,385426,385959,367460,248699,174878,83765,92070],"yaxis":"y","type":"scatter"}],                        {"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Months (Mar 2021 - Feb 2022)"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Number Of Rides"},"range":[0,500000]},"legend":{"title":{"text":"Member Vs. Casual"},"tracegroupgap":0},"margin":{"t":60}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('8b194aa3-55b1-4c0e-9aca-30b607109aff');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


* The frequency of rides for **Annual Members** increases from **June to October and remains consistent over that time period**.
* For **Casual Riders**, the number of rides is highest from **June to September, with July appearing to be the peak month in between**.

## figure4


```python
# Grouping..
figure4 = round(trimmed_df.groupby(['year', 'month', 'member_casual'], as_index=False).mean(),2).dropna()
```


```python
# Plotting..
px.bar(figure4, x = 'month', y = 'ride_length',
        color = 'member_casual',
        barmode='group',
        text = 'ride_length', 
        labels = {'ride_length': 'Average Ride Length (mins)', 'member_casual': 'Member Vs. Casual', 'month': 'Months (Mar 2021 - Feb 2022)'},
        hover_name = 'member_casual', hover_data = {'member_casual': False, 'ride_length': True}, 
        color_discrete_map = {'casual': '#F5F5DC', 'member': '#00FFFF'})
```


<div>                            <div id="3c359acb-5593-404a-b427-427934ed2e0c" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("3c359acb-5593-404a-b427-427934ed2e0c")) {                    Plotly.newPlot(                        "3c359acb-5593-404a-b427-427934ed2e0c",                        [{"alignmentgroup":"True","customdata":[["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"]],"hovertemplate":"<b>%{hovertext}</b><br><br>Months (Mar 2021 - Feb 2022)=%{x}<br>Average Ride Length (mins)=%{text}<extra></extra>","hovertext":["casual","casual","casual","casual","casual","casual","casual","casual","casual","casual","casual","casual"],"legendgroup":"casual","marker":{"color":"#F5F5DC","pattern":{"shape":""}},"name":"casual","offsetgroup":"casual","orientation":"v","showlegend":true,"text":[38.07,37.99,38.27,37.2,32.75,28.65,27.68,28.56,22.97,23.34,30.49,26.76],"textposition":"auto","x":["March","April","May","June","July","August","September","October","November","December","January","February"],"xaxis":"x","y":[38.07,37.99,38.27,37.2,32.75,28.65,27.68,28.56,22.97,23.34,30.49,26.76],"yaxis":"y","type":"bar"},{"alignmentgroup":"True","customdata":[["member"],["member"],["member"],["member"],["member"],["member"],["member"],["member"],["member"],["member"],["member"],["member"]],"hovertemplate":"<b>%{hovertext}</b><br><br>Months (Mar 2021 - Feb 2022)=%{x}<br>Average Ride Length (mins)=%{text}<extra></extra>","hovertext":["member","member","member","member","member","member","member","member","member","member","member","member"],"legendgroup":"member","marker":{"color":"#00FFFF","pattern":{"shape":""}},"name":"member","offsetgroup":"member","orientation":"v","showlegend":true,"text":[13.68,14.43,14.4,14.44,13.99,13.83,13.46,12.23,11.01,10.69,11.69,11.17],"textposition":"auto","x":["March","April","May","June","July","August","September","October","November","December","January","February"],"xaxis":"x","y":[13.68,14.43,14.4,14.44,13.99,13.83,13.46,12.23,11.01,10.69,11.69,11.17],"yaxis":"y","type":"bar"}],                        {"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Months (Mar 2021 - Feb 2022)"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Average Ride Length (mins)"}},"legend":{"title":{"text":"Member Vs. Casual"},"tracegroupgap":0},"margin":{"t":60},"barmode":"group"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('3c359acb-5593-404a-b427-427934ed2e0c');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


* According to this graph, the **Average** ride length for **Casual Riders** is **higher during the entire spring season and the first months of summer**.

* The **Average** ride length for **Annual Members** is very **consistent** throughout the year.

## figure5


```python
# Grouping..
figure5 = round(trimmed_df.groupby(['day_of_week', 'member_casual'], as_index=False).mean(),2)
```


```python
# Plotting..
px.bar(figure5, x = 'day_of_week', y = 'ride_length',
        color = 'member_casual',
        barmode='group',
        text = 'ride_length', 
        labels = {'ride_length': 'Average Ride Length (mins)', 'member_casual': 'Member Vs. Casual', 'day_of_week': 'Weekdays'},
        hover_name = 'member_casual', hover_data = {'member_casual': False, 'ride_length': True}, 
        color_discrete_map = {'casual': '#F0FFFF', 'member': '#058ED9'})
```


<div>                            <div id="d68de7dd-4879-4cab-ad02-d04324374b63" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("d68de7dd-4879-4cab-ad02-d04324374b63")) {                    Plotly.newPlot(                        "d68de7dd-4879-4cab-ad02-d04324374b63",                        [{"alignmentgroup":"True","customdata":[["casual"],["casual"],["casual"],["casual"],["casual"],["casual"],["casual"]],"hovertemplate":"<b>%{hovertext}</b><br><br>Weekdays=%{x}<br>Average Ride Length (mins)=%{text}<extra></extra>","hovertext":["casual","casual","casual","casual","casual","casual","casual"],"legendgroup":"casual","marker":{"color":"#F0FFFF","pattern":{"shape":""}},"name":"casual","offsetgroup":"casual","orientation":"v","showlegend":true,"text":[37.53,31.75,27.73,27.56,27.72,30.09,34.52],"textposition":"auto","x":["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"],"xaxis":"x","y":[37.53,31.75,27.73,27.56,27.72,30.09,34.52],"yaxis":"y","type":"bar"},{"alignmentgroup":"True","customdata":[["member"],["member"],["member"],["member"],["member"],["member"],["member"]],"hovertemplate":"<b>%{hovertext}</b><br><br>Weekdays=%{x}<br>Average Ride Length (mins)=%{text}<extra></extra>","hovertext":["member","member","member","member","member","member","member"],"legendgroup":"member","marker":{"color":"#058ED9","pattern":{"shape":""}},"name":"member","offsetgroup":"member","orientation":"v","showlegend":true,"text":[15.28,12.78,12.39,12.37,12.38,12.95,14.88],"textposition":"auto","x":["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"],"xaxis":"x","y":[15.28,12.78,12.39,12.37,12.38,12.95,14.88],"yaxis":"y","type":"bar"}],                        {"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Weekdays"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Average Ride Length (mins)"}},"legend":{"title":{"text":"Member Vs. Casual"},"tracegroupgap":0},"margin":{"t":60},"barmode":"group"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('d68de7dd-4879-4cab-ad02-d04324374b63');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


* **Casual members**, according to this graph, ride their bikes for **longer periods of time, especially on weekends**.
* **Annual members** generally tend to ride their bikes in a steady manner during the week, while on **weekends they ride for a little longer**.

## figure6


```python
# Grouping..
figure6 = trimmed_df.groupby(['rideable_type', 'member_casual'], as_index=False).count()
```


```python
# Plotting..
px.bar(figure6, x = 'rideable_type', y = 'ride_id',
        color = 'member_casual',
        barmode='group',
        text = 'ride_id', 
        labels = {'ride_id': 'Number Of Rides', 'member_casual': 'Member Vs. Casual', 'rideable_type' : 'Rideable Type'},
        hover_name = 'member_casual', hover_data = {'member_casual': False, 'ride_length': False}, 
        color_discrete_map = {'casual': '#7FFF00', 'member': '#058ED9'})
```


<div>                            <div id="31eb58f9-3fd2-4469-a198-a600db56221e" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("31eb58f9-3fd2-4469-a198-a600db56221e")) {                    Plotly.newPlot(                        "31eb58f9-3fd2-4469-a198-a600db56221e",                        [{"alignmentgroup":"True","customdata":[["casual",1254496],["casual",309359],["casual",942366]],"hovertemplate":"<b>%{hovertext}</b><br><br>Rideable Type=%{x}<br>Number Of Rides=%{text}<extra></extra>","hovertext":["casual","casual","casual"],"legendgroup":"casual","marker":{"color":"#7FFF00","pattern":{"shape":""}},"name":"casual","offsetgroup":"casual","orientation":"v","showlegend":true,"text":[1254496.0,309359.0,942366.0],"textposition":"auto","x":["classic_bike","docked_bike","electric_bike"],"xaxis":"x","y":[1254496,309359,942366],"yaxis":"y","type":"bar"},{"alignmentgroup":"True","customdata":[["member",1971782],["member",0],["member",1102733]],"hovertemplate":"<b>%{hovertext}</b><br><br>Rideable Type=%{x}<br>Number Of Rides=%{text}<extra></extra>","hovertext":["member","member","member"],"legendgroup":"member","marker":{"color":"#058ED9","pattern":{"shape":""}},"name":"member","offsetgroup":"member","orientation":"v","showlegend":true,"text":[1971782.0,0.0,1102733.0],"textposition":"auto","x":["classic_bike","docked_bike","electric_bike"],"xaxis":"x","y":[1971782,0,1102733],"yaxis":"y","type":"bar"}],                        {"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Rideable Type"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Number Of Rides"}},"legend":{"title":{"text":"Member Vs. Casual"},"tracegroupgap":0},"margin":{"t":60},"barmode":"group"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('31eb58f9-3fd2-4469-a198-a600db56221e');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


# Act

### Final Conclusion Based On Analysis

> * Cyclistic bike share is used **differently** by **annual members and casual users**.

> * During the data analysis process, it was discovered that **Annual members** prefer to ride their bikes for **everyday commuting** because they don't ride for long periods of time and have **consistency** in their weekly rides. **Casual riders**, on the other hand, primarily use bikes on **weekends for outings**. nevertheless, some casual riders also use bikes for daily commutes because some of them have short rides.

> * **Casual Riders** average ride length is **higher throughout the spring season and the first months of summer**, but **Annual Members** average ride length is **fairly steady throughout the year**. The frequency of rides for **Annual Members increases from June to October** and remains consistent throughout that time period; on the other hand, the frequency of rides for Non-Annual Members increases. From **June through September**, the number of **Casual riders** is at its highest, with **July looking to be the peak month in between**.


### Effective Use Of Insights

> * Make an appealing annual membership package for casual riders who utilise bikes for regular commutes, especially for daily commuters. And that plan would include some sort of seasonal or festive discount, and when promoting the membership plan, the main focus should be on email marketing, with less emphasis on other media.

> * And for the casual riders who primarily use their bikes on weekends. This set of riders should be offered the same annual membership package, but presented in a different way. There could be coupons for some popular hot spots for this segment of casual riders, and social media should be a big focus for promotion.

> * These strategies can also be used to acquire new clients.

### Recommendation 

> * Add some extra information for deeper analysis, such as financial information, membership plan details, personally identifiable information about clients, and bike type information. It would be more beneficial in improving the marketing strategy's effectiveness.
