# Challenge Goal

Use any data tool(s) at your disposal to&nbsp;**create an interactive visual or dashboard that illustrates post-pandemic ridership recovery trends across the MTA's services**.

[Full Challenge Link](https://mavenanalytics.io/challenges/maven-commuter-challenge)

# Maven Commuter Challenge [Finalist]

Interactive visuals aimed to illustrate the post-pandemic ridership recovery trends across the MTA's services.

[📊 Live Report](https://app.powerbi.com/view?r=eyJrIjoiZjJkNWYyZWUtNDVkOS00NDJhLTliYmEtMDEzM2Y5ZDc0M2FjIiwidCI6IjhhNDk1ZGQwLThkNDEtNDcyYy1iMTljLTFhMzQzZjYxYmFhMSIsImMiOjl9)

# How did I work through the Challenge? 

**Step 1:** imported the data into Power BI.

**Step 2:** data exploration to understand what information the data contains (this step included the reading of the PDF document "MTA_DailyRidershipData_Overview", available among the data challenge resources).

**Result:**  the data contains the ridership figures for each MTA‘s service from March, 2020 until October, 2024, and the percentage comparison of these figures against a comparable pre-pandemic date.

**Step 3:** decided what visuals to create:

**Scatter Plot:** with the post-pandemic ridership on the x-axis and the % change pre-post pandemic on the y-axis. Also I envisioned this scatter having a play axis based on month and year grouping of the data, and each bubble would represent a service.

**Aim**: visualise, for each service, its level of post-pandemic ridership and its positioning vs. a comparable pre-pandemic period.

**Area Chart:** with the years and months on the x-axis and the monthly pre and post pandemic ridership on the y-axis. Here I thought the user could also have an easy selection pane (**slicer**) for the transport type, to see the pre-post pandemic trend for each service.

**Aim**: visualise, the full evolution of services' ridership through the pandemic and after, keeping an eye on comparable pre-pandemic levels.

**Weekdays/Weekends Slicer:** in order to explore different trends based on working days vs. weekends.

**Tooltip:** custom tooltip to show, for each data point, the pre-pandemic ridership, the post-pandemic ridership, and the percentage of change between the two.

In orderde to realise what I was planning to, it was clear I needed to proceed to some data transformations, to shape the dataset and make it suitable to my visualization requirements.

**Step 4:** used Power Query, to transform the data, following the below steps:

**Data Transformation Part 1:**

From raw data to unpivot of "totals" and "percentages"

1. MTA_Daily_Ridership Table

```
let
  Source = Csv.Document(
    File.Contents(
      "C:\Users\stefa\OneDrive\\Desktop\Maven Data Challenge11 - Commuter Challenge\MTA_Daily_Ridership.csv"
    ), 
    [Delimiter = ",", Columns = 15, Encoding = 1252, QuoteStyle = QuoteStyle.None]
  ), 
  #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars = true]), 
  #"Changed Type" = Table.TransformColumnTypes(
    #"Promoted Headers", 
    {
      {"Subways: Total Estimated Ridership", Int64.Type}, 
      {"Buses: Total Estimated Ridership", Int64.Type}, 
      {"LIRR: Total Estimated Ridership", Int64.Type}, 
      {"Metro-North: Total Estimated Ridership", Int64.Type}, 
      {"Access-A-Ride: Total Scheduled Trips", Int64.Type}, 
      {"Bridges and Tunnels: Total Traffic", Int64.Type}, 
      {"Staten Island Railway: Total Estimated Ridership", Int64.Type}, 
      {"Date", type date}
    }
  ), 
  #"Unpivoted Total Columns" = Table.UnpivotOtherColumns(
    #"Changed Type", 
    {
      "Date", 
      "Subways: % of Comparable Pre-Pandemic Day", 
      "Buses: % of Comparable Pre-Pandemic Day", 
      "LIRR: % of Comparable Pre-Pandemic Day", 
      "Metro-North: % of Comparable Pre-Pandemic Day", 
      "Access-A-Ride: % of Comparable Pre-Pandemic Day", 
      "Bridges and Tunnels: % of Comparable Pre-Pandemic Day", 
      "Staten Island Railway: % of Comparable Pre-Pandemic Day"
    }, 
    "Attribute", 
    "Value"
  ), 
  #"Removed % Columns" = Table.RemoveColumns(
    #"Unpivoted Total Columns", 
    {
      "Subways: % of Comparable Pre-Pandemic Day", 
      "Buses: % of Comparable Pre-Pandemic Day", 
      "LIRR: % of Comparable Pre-Pandemic Day", 
      "Metro-North: % of Comparable Pre-Pandemic Day", 
      "Access-A-Ride: % of Comparable Pre-Pandemic Day", 
      "Bridges and Tunnels: % of Comparable Pre-Pandemic Day", 
      "Staten Island Railway: % of Comparable Pre-Pandemic Day"
    }
  ), 
  #"Extracted Text Before Delimiter" = Table.TransformColumns(
    #"Removed % Columns", 
    {{"Attribute", each Text.BeforeDelimiter(_, ":"), type text}}
  )
in
  #"Extracted Text Before Delimiter"
```

2. MTA_Daily_Ridership - percentages Table

```
let
  Source = Csv.Document(
    File.Contents(
      "C:\Users\stefa\OneDrive\\Desktop\Maven Data Challenge11 - Commuter Challenge\MTA_Daily_Ridership.csv"
    ), 
    [Delimiter = ",", Columns = 15, Encoding = 1252, QuoteStyle = QuoteStyle.None]
  ), 
  #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars = true]), 
  #"Changed Type" = Table.TransformColumnTypes(
    #"Promoted Headers", 
    {
      {"Subways: Total Estimated Ridership", Int64.Type}, 
      {"Buses: Total Estimated Ridership", Int64.Type}, 
      {"LIRR: Total Estimated Ridership", Int64.Type}, 
      {"Metro-North: Total Estimated Ridership", Int64.Type}, 
      {"Access-A-Ride: Total Scheduled Trips", Int64.Type}, 
      {"Bridges and Tunnels: Total Traffic", Int64.Type}, 
      {"Staten Island Railway: Total Estimated Ridership", Int64.Type}, 
      {"Date", type date}
    }
  ), 
  #"Unpivoted % Columns" = Table.UnpivotOtherColumns(
    #"Changed Type", 
    {
      "Date", 
      "Subways: Total Estimated Ridership", 
      "Buses: Total Estimated Ridership", 
      "LIRR: Total Estimated Ridership", 
      "Metro-North: Total Estimated Ridership", 
      "Access-A-Ride: Total Scheduled Trips", 
      "Bridges and Tunnels: Total Traffic", 
      "Staten Island Railway: Total Estimated Ridership"
    }, 
    "Attribute", 
    "Value"
  ), 
  #"Removed Total Columns" = Table.RemoveColumns(
    #"Unpivoted % Columns", 
    {
      "Subways: Total Estimated Ridership", 
      "Buses: Total Estimated Ridership", 
      "LIRR: Total Estimated Ridership", 
      "Metro-North: Total Estimated Ridership", 
      "Access-A-Ride: Total Scheduled Trips", 
      "Bridges and Tunnels: Total Traffic", 
      "Staten Island Railway: Total Estimated Ridership"
    }
  ), 
  #"Renamed Value into %" = Table.RenameColumns(#"Removed Total Columns", {{"Value", "%"}}), 
  #"Extracted Text Before Delimiter" = Table.TransformColumns(
    #"Renamed Value into %", 
    {{"Attribute", each Text.BeforeDelimiter(_, ":"), type text}}
  ), 
  #"Changed Type for % col" = Table.TransformColumnTypes(
    #"Extracted Text Before Delimiter", 
    {{"%", type number}}
  ), 
  #"Divided transoform % in decimal" = Table.TransformColumns(
    #"Changed Type for % col", 
    {{"%", each _ / 100, type number}}
  )
in
  #"Divided transoform % in decimal"
```

**Data Transformation Part 2:**

Merging the two tables to create all the needed elements for the intended visualizations (all the details are in the Power Query comments)

```
// Bringing in the "percentages" table
    #"Left Join Percentages Table" = Table.NestedJoin(#"Extracted Text Before Delimiter", {"Date", "Attribute"}, #"MTA_Daily_Ridership - percentages", {"Date", "Attribute"}, "MTA_Daily_Ridership (2)", JoinKind.LeftOuter),
    #"Expanded {0}" = Table.ExpandTableColumn(#"Left Join Percentages Table", "MTA_Daily_Ridership (2)", {"%"}, {"%"}),
    #"Replaced % 0s with Null" = Table.ReplaceValue(#"Expanded {0}",0,null,Replacer.ReplaceValue,{"%"}),
    #"Renamed Columns" = Table.RenameColumns(#"Replaced % 0s with Null",{{"Value", "Passengers After Covid"}, {"%", "% vs Pre-Pandemic"}}),
    
    // Estimation of the pre-pandemic passengers based on the percentage vs. pre-pandemic
    #"Added PrePandemic Passengers" = Table.AddColumn(#"Renamed Columns", "Pre-Pandemic Passengers", each 1*[Passengers After Covid]/[#"% vs Pre-Pandemic"], type number),
   
   // User friendly name
    #"Renamed Transportation" = Table.RenameColumns(#"Added PrePandemic Passengers",{{"Attribute", "Transportation"}}),
    
    // Added Month and Year column and create its sorting (for the scatter chart play axis)
    #"Inserted Month Name" = Table.AddColumn(#"Renamed Transportation", "Month Name", each Date.MonthName([Date]), type text),
    #"Inserted Year" = Table.AddColumn(#"Inserted Month Name", "Year", each Date.Year([Date]), Int64.Type),
    #"Added Month and Year" = Table.AddColumn(#"Inserted Year", "Month and Year", each [Month Name] & ", " & Number.ToText([Year]), type text),
    #"Sorted Date Ascending" = Table.Sort(#"Added Month and Year",{{"Date", Order.Ascending}}),
    #"Grouped Rows" = Table.Group(#"Sorted Date Ascending", {"Month and Year"}, {{"Count", each _, type table [Date=nullable date, Transportation=text, Passengers After Covid=number, #"% vs Pre-Pandemic"=nullable number, #"Pre-Pandemic Passengers"=number, #"% diff vs Pre-Pandemic"=number, Month Name=text, Year=number, Month and Year=text]}}),
    #"Added Index" = Table.AddIndexColumn(#"Grouped Rows", "Sort Month and Year", 1, 1, Int64.Type),
    #"Removed Other Columns" = Table.SelectColumns(#"Added Index",{"Count", "Sort Month and Year"}),
    #"Expanded {0}1" = Table.ExpandTableColumn(#"Removed Other Columns", "Count", {"Date", "Transportation", "Passengers After Covid", "% vs Pre-Pandemic", "Pre-Pandemic Passengers", "% diff vs Pre-Pandemic", "Month Name", "Year", "Month and Year"}, {"Date", "Transportation", "Passengers After Covid", "% vs Pre-Pandemic", "Pre-Pandemic Passengers", "% diff vs Pre-Pandemic", "Month Name", "Year", "Month and Year"}),
    
    // Added weekdays and weekends column
    #"Inserted Day Name" = Table.AddColumn(#"Expanded {0}1", "Day Name", each Date.DayOfWeekName([Date]), type text),
    #"Added Conditional Column" = Table.AddColumn(#"Inserted Day Name", "Week/Weekend", each if [Day Name] = "Saturday" then "Weekends" else if [Day Name] = "Sunday" then "Weekends" else "Weekdays", type text),
    
    // Final columns selection
    #"Removed Other Columns1" = Table.SelectColumns(#"Added Conditional Column",{"Date", "Transportation", "Passengers After Covid", "% vs Pre-Pandemic", "Pre-Pandemic Passengers", "% diff vs Pre-Pandemic", "Month Name", "Year", "Month and Year", "Sort Month and Year", "Week/Weekend"})
in
    #"Removed Other Columns1"
```

**Step 4:** Developed the interactive dashboard, according to plan, with the two visuals (scatter chart and area chart), the possibility to switch from one to the other with a click, the weekends/weekdays views, and the custom tooltip for the detailed figures.

**Of note:** Regarding the themes, background, and layout, I used the official MTA branding colors and styles, they use in their website -&gt; [https://new.mta.info/](https://new.mta.info/)

Here below the measures for the main metrics and the custom html tooltip:

1. Pre-Pandemic Ridership

```
Pre-Pandemic Ridership = 

SUM(
   MTA_Daily_Ridership[Pre-Pandemic Passengers]
)
```

2. Post-Pandemic Ridership

```
Post-Pandemic Ridership = 

SUM(
   MTA_Daily_Ridership[Passengers After Covid]
)
```

3. Percentage of Change vs. Pre-Pandemic

```
% Change vs. Pre-Pandemic = 

DIVIDE(
   [Post-Pandemic Ridership], [Pre-Pandemic Ridership]
) - 1
```

4. Custom HTML Tooltip 

4.1 Main Part

```
Tooltip Main= 

VAR PerChange = 
if([% Change vs. Pre-Pandemic] &lt;= 0, 
FORMAT([% Change vs. Pre-Pandemic], "#0.#%"),
FORMAT([% Change vs. Pre-Pandemic], "+#0.#%")
)

VAR pre_pand = 
FORMAT([Pre-Pandemic Ridership], "#,###")

VAR post_pand = 
FORMAT([Post-Pandemic Ridership], "#,###")

VAR service =
IF(
&nbsp; &nbsp; ISFILTERED(MTA_Daily_Ridership[Transportation]),
&nbsp; &nbsp; IF(
&nbsp; &nbsp; &nbsp; &nbsp; DISTINCTCOUNT(MTA_Daily_Ridership[Transportation]) = 1,
&nbsp; &nbsp; &nbsp; &nbsp; SELECTEDVALUE(MTA_Daily_Ridership[Transportation]),
&nbsp; &nbsp; &nbsp; &nbsp; IF(
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; DISTINCTCOUNT(MTA_Daily_Ridership[Transportation]) = 7,
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; "All Services",
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; CONCATENATEX(
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; VALUES(MTA_Daily_Ridership[Transportation]),
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; MTA_Daily_Ridership[Transportation],
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ", ",
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; MTA_Daily_Ridership[Transportation],
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ASC
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; )
&nbsp; &nbsp; &nbsp; &nbsp; )
&nbsp; &nbsp; ),
&nbsp; &nbsp; "All Services"
)

VAR PostP_marker = "<span style="display:inline-block; width:8px; height:8px; background-color:#2360A5; border-radius:50%; margin-right:5px; border:1px solid #2360A5;"></span>"
VAR PreP_marker = "<span style="display:inline-block; width:8px; height:8px; background-color:#808080; border-radius:50%; margin-right:5px; border:1px solid #808080;"></span>"

RETURN
" <b> MTA Service: " &amp; "<span style="font-size: 13px;">" &amp; service &amp; "</span><br><br>" &amp; 
"ㅤ" &amp; PreP_marker &amp; "Pre-Pandemic Monthly Ridership*: " &amp; "<span style="font-size: 13px;">" &amp; pre_pand &amp; "</span><br><br>" &amp; 
"ㅤ" &amp; PostP_marker &amp; "Post-Pandemic Monthly Ridership: "&amp; "<span style="font-size: 13px;">" &amp; post_pand &amp; "</span><br><br>" &amp; 
"ㅤ% Change vs. Pre-Pandemic Comparable Month: " &amp; "<span style="font-size: 13px;">" &amp; PerChange &amp; "</span><br>"

```

4.2 Title (top left corner of the tooltip)

```
VAR mon_year = SELECTEDVALUE(MTA_Daily_Ridership[Month and Year])
Var dayofweek = 
IF(
&nbsp; &nbsp; DISTINCTCOUNT(
&nbsp; &nbsp; &nbsp; &nbsp; MTA_Daily_Ridership[Week/Weekend]
&nbsp; &nbsp; ) = 1, 
&nbsp; &nbsp; if(SELECTEDVALUE(MTA_Daily_Ridership[Week/Weekend]) = "Weekends", "<i> (Weekends Only)</i>",
&nbsp; &nbsp; if(SELECTEDVALUE(MTA_Daily_Ridership[Week/Weekend]) = "Weekdays", "<i> (Weekdays Only)</i>"
&nbsp; &nbsp; )),
&nbsp; &nbsp; ""
) &nbsp;

RETURN

" <b> " &amp; mon_year &amp; dayofweek 
```

 </b></b>
