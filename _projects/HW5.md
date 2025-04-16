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
<p>I chose to visualize the longitude and latitude of different bigfoot sightings across all U.S. states using a selection zorder visualization. The purpose of this graph is to visually observe the frequency of bigfoot sightings in different U.S. states. I chose this visualization method to improve user interactivity and clarity by moving highlighted dots forward. Normally, it is difficult to select dots on a graph due to the small size of the dots. However, with selection zorder, highlighted dots come forward and other dots are minimized. As the user moves their cursor across the screen, the closest dot to the cursor appears in the foreground the other dots remain in the background to prevent overlapping. This reducing confusion and frustration during interpretation.</p>


**Data Transformations**
<p>I first started to see which data values I could replace with zero. However, I decided not to replace certain columns, such as 'date', with zero because 'date' cannot logically have a zero value. Instead, I chose to replace NaN values for the following columns:</p>

<ul>
<li> df['temperature_high'] = df['temperature_high'].replace(0, np.nan) </li>
<li> df['temperature_low'] = df['temperature_high'].replace(0, np.nan) </li>
<li> df['dew_point'] = df['dew_point'].replace(0, np.nan) </li>
<li> df['humidity'] = df['humidity'].replace(0, np.nan) </li>
<li> df['cloud_cover'] = df['cloud_cover'].replace(0, np.nan) </li>
<li> df['moon_phase'] = df['moon_phase'].replace(0, np.nan) </li>
<li> df['precip_intensity'] = df['precip_intensity'].replace(0, np.nan) </li>
<li> df['precip_probability'] = df['precip_probability'].replace(0, np.nan) </li>
<li> df['pressure'] = df['pressure'].replace(0, np.nan) </li>
<li> df['visibility'] = df['visibility'].replace(0, np.nan) </li>
<li> df['uv_index'] = df['uv_index'].replace(0, np.nan) </li>
<li> df['wind_bearing'] = df['wind_bearing'].replace(0, np.nan) </li>
<li> df['wind_speed'] = df['wind_speed'].replace(0, np.nan) </li>
</ul>

Then I proceeded to drop NaN values for the rest of the columns that I found had null values in.

**Encoding**
<p> I used longitude as a quantitative variable for the X-axis and latitude as a quantitative variable for the Y-axis to map the sightings across the U.S. As a result, the points on the graph resemble the U.S. map, making the visualization easier to interpret. I chose the 'category20' color scheme to represent the different states. While some colors repeat, 'category20' was one of the few color schemes with a larger color palette. To address this, I increased the size of the legend to include more symbols, making it easier for users to discern which state a specific sighting belongs to. Additionally, I split the states into two columns to improve the legend's readability. I incorporated the following fields as tooltip values: 'longitude:Q', 'latitude:Q', 'visibility:Q', 'wind_speed:Q', 'date:N', 'state:N', 'county:N', 'season:N', and 'cloud_cover:Q'. These values were selected to help assess the validity and reliability of bigfoot sightings. Visibility indicates whether a bigfoot could be seen, while cloud cover determines how much light passes through the clouds, indirectly affecting visibility and the likelihood of a sighting. Wind speed, which can make it harder to spot anything, was also included as an interesting metric. Longitude and latitude provide the exact location, while county and state further describe the sighting's location. The date specifies when the sighting occurred. I added the 'add_params' encoding parameter to enable the hover effect for zselection. Finally, I created a side-by-side comparison to demonstrate how the graph appears when the points are obscured versus when they are not. </p>

**Interesting Observation!**
<p>I was surprised to see that many bigfoot sightings were not spotted in Alaska due to its relative remoteness. This does make sense since Alaska has a lower population compared to the rest of the U.S., which means fewer sightings are reported. I do wonder if the reported sightings would increase if the population in Alaska were to increase. I was surprised to see these sightings be reported even when the cloud cover was above 0.8 for all three reportings. However, other factors such as wind speed and visibility also determine the likelihood of a sighting, which is why it is possible these sightings were reported.</p>


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

