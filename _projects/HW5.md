---
name: BigFoot Exploration Dataset
tools: [Python, HTML, vega-lite, altair]
image: assets/pngs/HW5.pdf
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
<p> I designed an interactive map of Bigfoot sightings across the U.S., overlaid on a geoshape map of state borders. The main purpose of this visualization is to allow users to explore the geographic spread of Bigfoot sightings and filter them by classification (Class A, B, or C). This type of interaction empowers the user to investigate potential patterns in report types and locations.

I added a dropdown menu where the user can select a classification. Once selected, only sightings of that type appear on the map. This prevents overcrowding and makes it easier to focus on relevant data. The points on the map are colored by classification and sized consistently for clarity. Tooltips provide contextual information for each sighting. </p>

**Data Transformations**
<p> The dataset was cleaned by:
Converting the date column to datetime format to extract the year:</p>
<ul>
<li>df['date'] = pd.to_datetime(df['date'], errors='coerce')</li>
<li>df['year'] = df['date'].dt.year</li>
</ul>
<p>Removing rows with missing values in essential columns:</p>
<ul>
<li>df = df.dropna(subset=['latitude', 'longitude', 'classification'])</li>
</ul>
<p>This ensured all points plotted on the map have valid coordinates and a classification type.</p>

**Encoding**
<p>In the interactive map:
longitude and latitude are quantitative variables used for geographic positioning.
classification is a nominal variable used to color the dots.
Tooltips show date, state, location_details, and classification for each point.
The map uses an Albers USA projection, which is well-suited for visualizing data across all U.S. states.
A dropdown menu enables filtering via alt.selection_point() and alt.binding_select().
The base map of the U.S. provides geographic context, while the colored points indicate specific sightings filtered by category. </p>

**Interesting Observation!**
<p> When filtering by Class A sightings (the most credible ones), there is a visible clustering around the Pacific Northwest, including Washington and Oregon. This supports the idea that these regions are hotspots for serious Bigfoot reports. Meanwhile, Class B and C sightings appear more randomly distributed, perhaps indicating less consistency or reliability. The interactive nature of the map makes it easy to switch between classifications and compare patterns directly. </p>

<div class="left">
{% include elements/button.html link="https://raw.githubusercontent.com/UIUC-iSchool-DataViz/is445_data/main/bfro_reports_fall2022.csv" text="The Data" %}
</div>

<div class="right">
{% include elements/button.html link="https://github.com/vrajderiya/vrajderiya.github.io/blob/main/Workbook.ipynb" text="The Analysis" %}
</div>

