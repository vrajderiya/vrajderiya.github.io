---
name: BigFoot Exploration Dataset
tools: [Python, HTML, vega-lite, altair]
image: assets/pngs/bigfoot.png
description: This notebook shows two different visualizations for the bigfoot dataset!
custom_js:
  - vega.min
  - vega-lite.min
  - vega-embed.min
  - justcharts
permalink: /projects/HW5
---

# HW5 - Bigfoot Dataset Visualization + Analysis

<p> These are the two visualizations that I created for the bigfoot dataset.</p>

<br>

##  Plot 1 - Mapping bigfoot sightings across longitude and latitude using zselection

<iframe src="/assets/plots/zselection_plot.html" width="100%" height="600" style="border:none;"></iframe>

**Description**
<p>I created a static bar chart to show the total number of Bigfoot sightings reported in each U.S. state. The goal of this visualization is to provide a clear overview of where Bigfoot sightings are most commonly reported. This helps users quickly identify states with the highest or lowest number of reports. I chose this method because bar charts are ideal for comparing categorical data (states) against a quantitative variable (number of sightings).

To enhance interpretability, I used the "oranges" color scheme, where darker bars represent higher sighting counts. The states are sorted in descending order by number of sightings, allowing the user to scan from most to least active regions. I also included tooltips showing the exact count and state name for easy reference.</p>


**Data Transformations**
<p>I used the raw dataset of Bigfoot reports and created a summary dataframe that counted sightings per state:</p>

<ul>
<li> state_counts = df['state'].value_counts().reset_index() </li>
<li> state_counts.columns = ['state', 'sightings'] </li>
</ul>

This transformation helped convert the raw report-level data into an aggregate form suitable for a bar chart.

**Encoding**
<p> In this bar chart:
state is encoded as a nominal variable on the x-axis and sorted by descending count.
sightings is a quantitative variable on the y-axis.
Color is based on the number of sightings, using a continuous gradient to represent frequency.
Tooltips display both the state name and the number of sightings.

This encoding scheme ensures users can easily compare values across different states and spot trends in the distribution of Bigfoot reports. </p>

**Interesting Observation!**
<p>Washington state leads with the highest number of reported Bigfoot sightings, which supports its reputation in folklore. States like California and Ohio also rank highly. Interestingly, some large states such as Nevada and Nebraska show surprisingly low numbers of sightings. This might reflect differences in population density, local belief systems, or even terrain visibility that affects whether or not people report unusual sightings.</p>


<br>
<br>
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<br>
<br>

##  Plot 2 - Viewing classified bigfoot sightings across different U.S. counties from 1950-2018 using a scatter plot

<iframe src="/assets/plots/scatter_plot.html" width="130%" height="950" style="border:none;"></iframe>


**Description**
<p> I chose to visualize the precipation intensity and visibility of bigfoot sightings in different U.S. counties from 1990-2015. The visualization method I chose is a scatter plot with point paths on hover with search box. This allows the user to search for a county and that specific point is highlighted on the graph if a data point exists for a certain year. The user can also use the slider to modify the year in 5 year increments. I chose a step size of 5 to be able to parse through the years quickly rather than one by one. I also narrowed from 1990-2015 because many bigfoot sightings were not spotted before 1990. I chose 2015 as the upper limit so there is no discrepancy when the user interacts with the slider. When set to another value, the slider cannot reach its maximum value which is misleading to the user. When a user clicks on a dot, a trail is created between other dots for the same county even if the years are different. These dots are also color coded by classification to determine class of a bigfoot finding. I also allowed the user to pan and zoom with this graph becauses some of the data points are clustered, making it difficult to read the years and county names as well as being able to view a line between the data points. </p>

**Data Transformations**
<p> I first converted date to datetime so I can then convert it into a year to be used as an integer for my visualization: </p>
<ul>
<li>df_cleaned['date'] = pd.to_datetime(df_cleaned['date'], errors='coerce')</li>
<li>df_cleaned['year'] = df_cleaned['date'].dt.year</li>
</ul>

**Encoding**
<p>I encoded 'precip_intensity' as a quantitative variable because the values are numerical. The same goes for visibility. I encoded 'county' as a nominal variable since county is a categorical variable. The text variable determines what text is displayed on the graph and this allows the user to see a county when they click on a specific data point. The opacity variable helps determine the level of shading a data point inherits when a user clicks on it. When the user clicks on a data point, the value is one and when unselected, the value is zero. I used the 'dark2' color scheme for classification because classification is an ordinal categorical variable. The color of the line is darker for enhanced readability, especially if there are other unrelated data points around. For the base trail, the year variable is a quantitative variable to connect the data points and to also determine the thickness of the line in numerical order. For the base text, year was encoded as an ordinal variable because year is being treated as a label which usually works well with categorical variables. As a quantitative variable, certain scaling transformations may apply which can affect the way year is displayed on the graph. </p>

**Interesting Observation!**
<p> I noticed that some counties do not have lines connecting their data points while some do. This may be due to the fact that there is not a notable change in data or there is not enough data for a specific county. Some counties have only one data point so no connecting line is formed. It is also possible that showing lines for certain counties may be convoluting and not display a specific trend, so no connecting line is displayed. </p>

<div class="left">
{% include elements/button.html link="https://github.com/spatel54/spatel54.github.io/blob/725f63d7ebc40c615a8ff2f72891eef93d7db924/assets/json/bfro_reports_fall2022.json" text="The Data" %}
</div>

<div class="right">
{% include elements/button.html link="https://github.com/vrajderiya/vrajderiya.github.io/blob/main/Workbook.ipynb" text="The Analysis" %}
</div>

