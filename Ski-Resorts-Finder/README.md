# [The Challenge](https://mavenanalytics.io/challenges/maven-slopes-challenge)

"...you need to **build a dashboard to help skiers find their ideal destination**."

[📊 Live Report](https://app.powerbi.com/view?r=eyJrIjoiNzUwNWRlOGUtMDU4NS00YWM1LTgwOTktNWM2NTQwNmY0YjY5IiwidCI6IjhhNDk1ZGQwLThkNDEtNDcyYy1iMTljLTFhMzQzZjYxYmFhMSIsImMiOjl9)

# Approach Used:

**The first step** was to ask a question to myself: who is the end-consumer of this dashboard? The answer is immediate: the skiers looking for an ideal destination. Therefore I thought to build a user-friendly tool with an interface that recalls a familiar holidays-boking website. This way, each person, with different requirements (more or less beginner or advanced skier, wish of a summer skiing, wish of a night skiing, need of a child friendly place, etc..) can explore the various resorts in the database and pick the one that best satisfy his/her needs.

**The second step** was to try to think to a possible user journey to offer in this tool, so, I came up with a case study to cover a realistic user journey possibility:

**Australian Family Case Study**

- It's Christmas 2023 and this family wants to go skiing from the 23rd of December 2023 until the 5th of January 2024 and spending Christmas Holidays on the snow. 
- First they think to go to an Australian resort, to stay closer to home. 
- So, they open the Ski Resorts Finder and look for a resort to go. 
- In Australia unfortunately they won't find any resort as it's summer.  
- Clicking on "Snow Coverage & Resorts Availability" they can check the global snow coverage for December and January and see where in these months there is the higher concentration of open resorts. 
- They also check when there are open resorts in Australia (May-October), however, they really want to go skiing during Christmas time.
- Viewing the maps, they notice that in their chosen period there is a wide choice among North American and European resorts, where, there should also be a quite good snow coverage. 
- This family, in the end, choses to go to Europe for their trip and, they go back to the "Resorts Search" and type it. Wow, so many resorts indeed! 
- They think they should narrow it a bit and pick a country.
- They choose Italy.
- They see there is a quite decent number of resorts in Italy, and therefore they want to narrow their choice even more and they use the filter pane for doing it. 
- They immediately notice that they are looking at prices in Euro, therefore, they change it to Australian Dollars, their currency, for then filtering out everything that costs more than 70 AUD.
- They also want to ski during night and they would like to go to small/medium size, child friendly resorts (hopefully not too crowded and suitable for real beginners).
- They have few options left and they check them out through the cards first and through the map later, to visualize their actual location.
- For some resorts they also click  on "Book You Holiday" button and they are redirected to the summary of their selected resorts to see them better in the wider summary view.
- Here they can give a final look to the places, their location on the map (with full street view) and they can also check how much the adults would spend in total for the ski-pass for the whole selected period inputting how many adults there are. 
- In the end they pick Bormio, which is close to an Italian National Park (Stelvio Park) and not that far from the Milan’s airports.
- Finally, they can also input how many kids are going and the number of rooms they would like to have. 
- At this point they can launch Booking.com to book their staying, and, in Booking.com they will have already their selections and the currency they were using in the Ski Resorts Finder tool, together with a list of available properties. 

**The third and last step** was to build this tool/dashboard so that this hypothetical user journey was possible.

# Interesting Power BI development topics

I would like to highlight the most interesting topics/techniques I used during the development of this dashboard and that allowed to level-up the overall users experience:

- **Mix of data transformation, data modelling and DAX  measures, that allows the users to see only the available resorts when selecting the dates (from - to) for an holiday** *(e.g. Selecting from 28th of November to 5th of December as holidays period, the user would see only those resorts that have in their season availability both November and December)*. The starting point was, for each resort, in the resorts data table, a column with season availability *(e.g., October - March)*.  What I did, in a nutshell, was to create a dates table first, for the users to select the dates of their staying. This table has a second column with the months names. As second step, I created a table/matrix (seasons lookup table), where each season had assigned only the months belonging to that season  (available months), leaving the other months blank. Then, as third step, I created another table (resorts lookup table) where each resort had assigned its available months: I started having resorts names, resorts IDs and season for each resort,  leaving a blank placeholder column “available months” so that each resort had 12 rows, then I used the seasons as lookup value to return the available months or blank to each resort from the seasons lookup table. As fourth step, I worked on the data model and I connected the dates table with the seasons lookup table: dates[months] -> seasons lookup[available months], then the seasons lookup table with the resorts lookup table: seasons lookup[available months] -> resorts lookup[available months] and finally, through resorts ID I connected the resorts lookup table with the resorts data table: resorts lookup[ID] -> resorts data table[ID]. However, it was not finished, as for now, if a user selects from 28th of November to 5th of December would get the resorts available in November or December or in both, not in both only. To overcome this problem my fifth step was to create a measure that checks, for each resort, if the number of months selected in the calendar table equals the count of a resort ID in the resorts lookup table and used this measure as visual level filter in the cards and maps visuals. This because if the users selects 2 months and a resort is available in both months the ID would appear twice, if the ID appears once, it means that the resort is not available in one of the 2 selected months and therefore needs to be filtered out.
- **Connection to live daily exchange rates through APIs**, for having the current exchange rates in the dashboard. This way the users can choose what currency they want for their resorts search.  
-  **Dashboard fully brought to the cloud, for automated data refresh:** in order to always have the most updated exchange rates I needed to schedule periodically automated refreshes of the dataset. However, without having all the data sources in the cloud, this is impossible, unless installation of a on-premises gateway, on a machine that stays 24h online. My original file sources were a mix of Excel files and the APIs connection for the exchange rates, therefore a mix of on-premises and on the cloud sources. Here the steps I used to have all the data sources in the cloud: (a) uploaded all the Excel files to Google Drive and save them as Google Sheet files. (b) Imported the Google Sheet files in Power BI desktop, using the dedicated Google Sheets connector, and, used their path to change the data source of the originally imported Excel files (in the Power Query editor). (c) Removed the redundant newly imported files, and kept the original files with now a new source in the cloud. (d) Applied changes in Power Query editor, saved and republished the report in Power BI Service. In Power BI Service I could schedule the data refresh without Gateway. 
- **Usage of Dax to set the starting date of the dates selection in the slicers to today**. So users can start their resorts search from today with all the prior days grayed out. 
- **Usage of Power Query to group the latitude/longitude combinations of the snow data table into wider "regions"** than the given 0.25x0.25 degrees in size, in order to: on one hand, having anyways a good representation of the global snow coverage, on the other, allowing the map visual to visualize without bugging/crashing etc...
- **Overridden filters in drill-through mode** so that in the drilled-through page the user can again use the full filter experience and not being limited by his/her initial filtering choice. In this particular case, I wanted to allow the user to change the selected dates beyond his/her original selection, after drilling-through a specific resort and opening the summary of the selected resort. To achieve it, I first removed all the filters in the drilled-through page and then I sync the slicers throughout all the dashboard making them invisible in all the pages but where the user can see the filters pane. 
- **HTML integration in Power BI** to create dynamic cards, page tooltips, resort summaries and the custom legend for the heat-map. The visualization of the scripts is possible using the HTML Content and the Card Browsers custom visuals. Dax and HTML integration can give nice results.
- **Interaction of Power BI with the web through manipulation of a standard link** - The standard link is made dynamic applying Dax variables within it that change based on the user's selections. In order to find a precise location for the Booking.com research I reverse-geocoded the latitude and longitude of the resorts to find a precise address to integrate in the link. 
- **Integration of the images of each resort into the dashboard:** the goal is to provide the users with better context and the perspective of what to expect from the resort. The images are included through adding their link into the data table and using the URL option to display them into the visuals.  
- **Usage of Power Point to build the background.**
- **Level-up of the dashboard navigation using custom icons in .svg format** and their combination with blank buttons or using "clickable" gray backgrounds that provide a the "real website effect". 

# About the dataset

**Main Dataset:** dataset provided by Maven Analytics for the Maven Slopes Challenge.

This dataset contains 2 tables in CSV format:

- The Resorts Data table contains information on 499 ski resorts around the world, including their location, slopes, lifts, prices, and ski season
- The Snow Data table contains supplemental data on the surface of the earth covered by snow for each month in 2022, by latitude & longitude

**Exchange Rate - Live Data:** APIs connection with a free source ([https://exchangerate.host/#/](https://exchangerate.host/#/))

**Reverse Geocoding the resorts coordinates (latitude/longitude):** I used [Geoapify](https://www.geoapify.com/tools/reverse-geocoding-online) that allows it for free.

**Additional Integrations**

**Images of every resort:** I integrated each resort with an image that represents the whole resort's map of the slopes. This, to provide the user with the perspective of what to expect from the resort.

**Icons and Report Images:** big thanks to [www.flaticon.com](https://www.flaticon.com/) that provides access to these high quality contents

# Data clean-up & Duplicates Removal

Before using the Resorts Data Table it was necessary to **clean-up 2 columns** ("Resorts" and "Season") and to **erase few duplicates.**

**Resorts Column:** it was necessary to transform some resort names as they were presenting some not user-friendly characters *(e.g., original: Madonna di Campiglio-?Pinzolo-?Folga?rida-?Marilleva -> changed into: Madonna di Campiglio-Pinzolo-Folgarida-Marilleva)*. Also, I re-wrote several resorts name in a more understandable way, following how the real resorts' names should be in accordance with the website [skiresort.info](www.skiresort.info), source that provides pieces of information on all the world-wide resorts.

**Season Column:** the season column provides the piece of information on when  the resort should be open in normal circumstances and I identified few issues here: "unknown" entries, entries where only one month was provided, entries where it was written in different ways that a resort is open the all year (e.g., June - May or Year-round, etc...), therefore here I made a mix of sense check and desktop research to have a normalized column with data that can be safely used.

*(See below image for details)*

<img width="1182" height="625" alt="image" src="https://github.com/user-attachments/assets/99065266-11be-4971-aff3-6983e25ce447" />

**Duplicates (1/3):** Checking the latitude and longitude of the resorts it turned out that 3 resorts, that include different small locations/towns, were split in different rows, one for each small location/town but it does not mean each one of them is a different resort (that in fact has one lat./long. combination). Making a simple desktop research is easily findable that these 8 rows were in fact only 3 resorts. What I did was to compact them and to rename them, in accordance with my research.

*(See below image for details)*

<img width="1250" height="415" alt="image" src="https://github.com/user-attachments/assets/03ba29a5-f364-4a8e-8307-34a7311327e4" />

**Duplicates (2/3):** also, the resorts *"Les 3 Vallees, Monte Rosa Ski, Grandvalira,  Via Lattea, SkiWelt Wilder Kaiser-Brixental, Les Portes du Soleil, Ski Amede, Skicircus, Ski Arlberg and Grandvalira"* are split into different duplicated rows. In this case each row contains a different small town name and has a different lat./long. combination. However, each row contains the same pieces of information when it comes to the resort price, kilometers of slopes, highest and lowest points, number of ski-lifts, ski lifts capacity etc., with exception for the first row among all the duplicated rows belonging to one resort that is the only one having information on the longest run. Therefore  I made a desktop research that proved that all these rows are actually duplications of one resort only, for each one of the above mentioned resorts. As result, also these resort have been compacted into 1 row only each, accordingly to my research.

**Duplicates (3/3):** *"Espace San Bernardo , La Rosière/La Thuile"* resort  is duplicated with one row with as location Italy and another as location France. Besides that all the other pieces of information, followed the rule of the "Duplicates (2/3) part".  I made a research and this resort is mostly acknowledged to be in France, therefore I kept the "French row".
