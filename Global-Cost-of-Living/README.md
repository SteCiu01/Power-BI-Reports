# Global Cost of Living Report

<img width="929" height="502" alt="image" src="https://github.com/user-attachments/assets/10276647-97d6-453f-9a63-a6b53ee453e6" />

Interactive Dashboard that allows the end user to explore the World-Wide Cost of Living factors and compare prices between countries and cities.

[📊 Live Report](https://app.powerbi.com/view?r=eyJrIjoiMjcwMTg2NzMtY2IzYy00MDliLThlNzQtN2FiNGFhNGU5NjMzIiwidCI6IjhhNDk1ZGQwLThkNDEtNDcyYy1iMTljLTFhMzQzZjYxYmFhMSIsImMiOjl9)

## Why this dashboard? 

In a historical moment (Jan-2023) where one of the major topics is the inflation I decided to take the Cost of Living data from the website www.numbeo.com and to create a dashboard/tool that replicates in Power BI some functionalities of their websites allowing the user to visualize and compare the cost of living factors among cities and countries for one single year or over-time. (Big thanks to Numbeo for allowing the usage of their data for personal projects and social media/blogging purposes)

## Dashboard Content 

The dashboard is divided into two main sections; Country View and City View, and, in each one of them it is possible to:

1. Explore the indexes (e.g., Cost of Living Index, Grocery Index, Rent Index, Purchasing Power Index, etc.). The indexes are relative to New York City and therefore they provide a relative positioning for each country or city vs NYC. So, if for instance a city has a Purchasing Power Index of 120 means that in this city, on average, people have a purchasing power 20% higher than in New York City. If a city has a Purchasing Power Index of 70 instead, it means that in this city, on average, people have a purchasing power 30% lower than in New York City.
2. View the value of several Cost of Living factors in Euro or all the existing local currencies (e.g., average salary, cost of grocery items, average rent, cost of car purchase, cost of clothes purchase, etc.)
3. Compare the prices between countries or between cities in Euro or all the existing local currencies.

## Interesting Power BI development topics

For building this dashboard, due to the complexity of emulating some of the Numbeo's website functionalities, I needed to use some advanced techniques in three main areas: productivity & organization, data sourcing & modelling, UX/UI & formatting measures. In details here below some of the topics I find more interesting:

PRODUCTIVITY & ORGANIZATION

1. Organization of measures into different folders, divided by type (e.g., indexes-country measures, indexes-city measures etc…).
2. Usage of Tabular Editor for replicating the measures (once the country-view part was done, I replicated the measures for the city-view part using Tabular Editor). 
3. Naming of the measures in an efficient way that simplifies replication (e.g., Country_Cost-of-living-index, Country_Purchasing-Power-Index etc… where, in replication phase the “Country” part can easily replaced with “City”).

DATA SOURCING & MODELLING

1. Upload images in the data model from a local folder using PowerQuery.
2. Sourcing of live and historical FX data using APIs.
3. Usage of the fields parameters as tables and their integration in the data model, to “call” a parameter using a lookup table’s column as filter.
4. Duplication of some data tables and part of the data model related to them, to allow the price comparisons between cities and countries. 

UX/UI & FORMATTING MEASURES

1. Building the dashboard interface's structure importing backgrounds previously created using Power Point
2. Creation of a customized page navigation.
3. Interactive buttons with hovering effects (gif animation, change of color, change of text, etc…).
4. Usage of “info buttons” to provide instructions/guidelines to the user, during its dashboard navigation. 
5. Building a Shape Map / Map with effective color-scales and with dynamic geographical view options (e.g., selection of only countries in Europe or selection of only cities in Asia, etc…).
6. Dynamic titles, using conditional formatting measures.
7. Usage of Tooltips, Small Multiples and Drill-Through functionalities to enhance the User Experience. 

## Dataset

The data used to build this work is entirely from Numbeo.com, in accordance with Numbeo data terms of use, cit. from their website (https://www.numbeo.com/common/terms_of_use.jsp) : "Use of Numbeo data is free for personal use, under the term if you use Numbeo data in personal blogging, websites and social media that you have to give appropriate credit, a link back to Numbeo.com"

The original dataset includes four data tables: Country Indexes, City Indexes, Country Values of Cost of Living factors and City Values of Cost of Living factors. I created these four tables extracting the row data from the Numbeo website. Also, the data model includes several lookup tables that I created using Power Query and Dax, but the FX tables; that I imported using the APIs connection with a free source (https://exchangerate.host/#/)
