# Challenge Goal

Create an **interactive dashboard that enables sales managers to track their team's quarterly performance**.

[Full Challenge Link](https://mavenanalytics.io/challenges/maven-sales-challenge)

# Maven Sales Challenge - Sales Teams Performance Tracker

![6-MavenSalesChallenge-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/c9378dbb-e0a6-4f82-9fca-e86956bdea56)
<br><br>

Interactive sales dashboard designed to assist Sales Team Managers in monitoring their teams’ performance.

[📊 Live Report](https://app.powerbi.com/view?r=eyJrIjoiNDY5MmM0NTItMzhjMC00ZTgyLWE5ZjgtMDU1NmYxMDViNmUxIiwidCI6IjhhNDk1ZGQwLThkNDEtNDcyYy1iMTljLTFhMzQzZjYxYmFhMSIsImMiOjl9)

# Approach to the Challenge 

Step 1: imported the data into Power BI Desktop and understood its structure and what information the data contains.

Step 2: based on the pieces of information available in the CRM data, the KPIs the sales managers most likely need to focus on were defined.

Step 3: decided how to visualize the KPIs (e.g., KPIs cards, by team leaders, by sales agent, etc.), and drafted a dashboard's user flow that maximizes user experience.

Step 4: data clean-up and transformation using Power Query, in order to make the data suitable create all the measures for the KPIs and the visualizations.

Step 5: creation of the data model and all the measures.

Step 6: actual Power BI development - for this dashboard it was decide to use almost entirely the SVG technique, within the KPI Cards and the Matrix, to guarantee a clean design that recalls the IBCS guidelines.

In the next sections it will be covered in detail the KPIs definition, the dashboard visualization and user flow ideas, and finally the SVG technique for Power BI development.

Data transformation, modeling and measures will be only marginally covered, while explaining the other main sections, as they are quite simple and straight forward and do not require an ad hoc paragraph.

# KPIs Definition

**1. Success Rate**

The percentage of won opportunities, higher value means positive achievement.

**2. Won Deals**

The actual number of won opportunities, higher value means positive achievement.

**3. Won Revenue**

The total won revenue in $, higher value means positive achievement.

**4. Avg. %△ SP (Sales Price) vs. LP (List Price or Recommended Retail Price)**

The average percentage delta between the sales price and the list price, **%△ =** 0 is considered a positive achievement, as it would mean sales agents are not selling only thanks to massive discounts but they are also capale to well promote the value of the poducts they are selling.

To calculate this metric, starting from the original dataset, few transformations were required:

First, it was brought into the "sales-pipeline" table the Retail/List Price from the "products" table.

Secondly, it was calculated for each opportunity (each row of the "sales-pipeline" table) the %△ SP (Sales Price) vs. LP (List Price) using this M-Code:

```

each 

if [close_value] = null or [close_value] = 0 

then null 

else [close_value] / [sales_price] - 1

```

This code provides the %△ for only those won opportunities with a Sales Price.

The final step here was to use a simple DAX Average measure to calculate the average %△.

**5. Avg. Days to Win**

The average number of days to close a successfull deal, lower value means positive achievement.

# KPIs Visualization Idea &amp; Dashboard User Flow 

## 1. After setting the KPIs the first question was how to visualize them, and, as the primary goal of the dashboard is to enable sales managers to track the teams' quarterly results, it was decided a Quarter vs. Quarter approach.

**Won Deals and Won Revenue**

It was decided to display the current quarter value, the △ vs. Past Quarter and the %△ vs. Past Quarter.

**Success Rate and Avg. %△ SP (Sales Price) vs. LP (List Price)**

It was decided to display the current quarter percentage and the △ pp vs. Past Quarter.

**Avg. Days to Win**

It was decided to display the current quarter value and the △ Days vs. Past Quarter.

## 2. The second question was in what whay the visualisation should happen. Here it was drafted a dashboard flow idea, to maximise the user experience. 

**Sales Teams Performance Tracker Page + Products and Customers drill through Page**

When users enter in the dashboard they see the 5 KPIs with the current quarter value in 5 cards. These cards also display the variations vs. the past quarter and a trend line that includes all the historical variations, quarter after quarter.

Then, under the KPIs they see a visually appealing Matrix that, for each KPI, summarizes the current quarter value, all the variations vs. past quarter and the trend lines. This Matrix includes the Team Leaders and the Sales Agents as categorical values, so that users can see each KPI, for each team and for each team member.

*Note: to make this matrix more visually appealing and easier to read, it was decided to implement some of the IBCS principles.*

When users click either on a Team Leader name or on a Sales Agent name, they are offered the possibility to click on a drill through button and see for that Team or for that particular Sales Agent, the same KPIs but broken down by product or by customer. This view is very important to enable leaders to understand the source of a good or bad performance.

**Sales Opportunities Details Page**

Another option offered to the users is to change page, from the Sales Teams Performance Tracker Page to the Sales Opportunities Details Page where they can find a table that contains all the major opps details from the CRM database (e.g., opps ID, engage start date, close date, outcome, outcome value, % var. vs. List Price, Sales Team Leader, etc.).

This table can also be filtered by each one of its categorical data point, and offers an opportunity to easily query the CRM database, with a user friendly interface.

Why this view?

Very often Sales Leaders might need to see things in details, they might need to check what opps are pending in their team or they might want to see in details what opps a Sales Agent lost in the past quarter, or they might want to to check all the opps for a customer etc.

When these needs appear, very often they also struggle to easily get the data they need, and, this view could be the solution for them.

# Power BI Development &amp; SVG

A note before starting this section: I would like to mention 3 people that shared fantastic work online on SVGs, and have great repositories of codes.

Their work was instrumental for the development of this dashboard, as I used their codes as starting point to develop mine.

- [Injae Park](https://www.linkedin.com/in/injae-park?utm_source=share&amp;utm_campaign=share_via&amp;utm_content=profile&amp;utm_medium=ios_app) — https://youtube.com/@PowerBIPark?si=bOInsg7pS9HHvcAM - great tutorials on the usage of SVGs and on the IBCSs visualizations.

- [Kerry Kolosko](https://www.linkedin.com/in/kerrykolosko?utm_source=share&amp;utm_campaign=share_via&amp;utm_content=profile&amp;utm_medium=ios_app) —   [SVG Templates](https://kerrykolosko.com/portfolio-category/svg-templates/) - Template for Trend-Lines

- [Andrzej Leszkiewicz](https://www.linkedin.com/in/avatorl?utm_source=share&amp;utm_campaign=share_via&amp;utm_content=profile&amp;utm_medium=ios_app) —  [GitHub](https://github.com/avatorl/IBCS-for-Power-BI)  - Templates for IBCS SVGs

After this introduction, I would like to share the codes that enabled the visuals seen in the Matrix and the KPI Card for Won Deals.  I share them for Won Deals, but they can be  easily adapted for any other KPI.

These codes are a combination of the original codes shared by the individuals mentioned above, along with my customizations, which were necessary for the specific use case of this dashboard (particularly when it comes to the KPI cards and to the double ranking for sales hierarchy in the Matrix).

Starting from the **KPI Card**, this is the final outcome:

![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/ff0d9c1f-1ab6-405a-9973-e0ea5e4cbe69.png)

In this KPI card the trendline and the "Vs. PQ: -61 / (-4.9%)" part was done with the following code:

```
Won Rev Sparkline Points Card =
// Measure Derived from Eldersveld Modified by Kolosko and adapted here by Stefano Ciurlia
VAR LineColour = "black"
// Color logic for the last quarter
VAR PercDiff = [Rev % Delta]
VAR Color = IF(PercDiff < 0, "#ff0000", if(PercDiff > 0,"#008e96", "grey"))
VAR PointColour = "white"

// SVG dimensions
VAR SVGWidth = 150 // Change this value to adjust the width
VAR SVGHeight = 30 // Change this value to adjust the height

// "Quarter" field used in this example along the X axis
VAR XMinDate = MIN('Close Dates Table'[Quarter])
VAR XMaxDate = MAX('Close Dates Table'[Quarter])

// Obtain overall min and overall max measure values when evaluated for each quarter
VAR YMinValue = MINX(Values('Close Dates Table'[Quarter]),CALCULATE([Won Revenues]))
VAR YMaxValue = MAXX(Values('Close Dates Table'[Quarter]),CALCULATE([Won Revenues]))

// Build table of X & Y coordinates and fit to SVGWidth x SVGHeight viewbox
VAR SparklineTable = ADDCOLUMNS(
    SUMMARIZE('Close Dates Table','Close Dates Table'[Quarter]),
        "X",INT(SVGWidth * DIVIDE('Close Dates Table'[Quarter] - XMinDate, XMaxDate - XMinDate)),
        "Y",INT(SVGHeight * DIVIDE([Won Revenues] - YMinValue,YMaxValue - YMinValue)-25))

VAR SparklineTableSeller = ADDCOLUMNS(
    SUMMARIZE('sales_teams',sales_teams[sales_agent]),
        "Y",INT(SVGHeight * DIVIDE([Won Revenues] - YMinValue,YMaxValue - YMinValue)))

VAR SparklineTableManager = ADDCOLUMNS(
    SUMMARIZE('sales_teams',sales_teams[manager]),
        "Y",INT(SVGHeight * DIVIDE([Won Revenues] - YMinValue,YMaxValue - YMinValue)))

// Concatenate X & Y coordinates to build the sparkline
VAR Lines = CONCATENATEX(SparklineTable,[X] & "," & SVGHeight-[Y]," ", 'Close Dates Table'[Quarter])
VAR PointTable=
    ADDCOLUMNS(
        SUMMARIZE('Close Dates Table','Close Dates Table'[Quarter]),
            "@Colour",IF([Won Revenues]=YMaxValue,"Green",IF([Won Revenues]=YMinValue,"Red",LineColour)),
        "@Points", "<circle cx='"&INT(SVGWidth * DIVIDE('Close Dates Table'[Quarter] - XMinDate, XMaxDate - XMinDate))&"' cy='" & SVGHeight -  INT(SVGHeight * DIVIDE([Won Revenues] - YMinValue,YMaxValue - YMinValue))+25 & "' r='3' stroke='"&LineColour&"' stroke-width='1'  fill='"&LineColour&"'/>")

// Last data points on the line
VAR LastSparkYValue = MAXX( FILTER(SparklineTable, 'Close Dates Table'[Quarter] = XMaxDate), [Y])
VAR LastSparkXValue = MAXX( FILTER(SparklineTable, 'Close Dates Table'[Quarter] = XMaxDate), [X])

VAR _YValue = 8

// Add to SVG, and verify Data Category is set to Image URL for this measure
VAR SVGImageURL =
    "data:image/svg+xml;utf8," &
    "<svg xmlns='http://www.w3.org/2000/svg' x='0px' y='0px' viewBox='-7 -7 " & SVGWidth+14 & " " & SVGHeight+45 & "'>
    " &
    "<text x='1' y='" & _YValue & "'  fill='black' font-family='Tahoma' font-size='11'> Vs. PQ: <tspan fill='" & Color & "'>" & FORMAT([Rev Delta], IF(PercDiff <= 0, "$#,###", "+$#,###")) &"</tspan> / (<tspan fill='" & Color & "'>" & FORMAT([Rev % Delta], IF(PercDiff <= 0, "0.0%", "+0.0%")) &"</tspan>)"& "</text>" &
    "<polyline
        fill='transparent' stroke='" & LineColour & "'
        stroke-linecap='round' stroke-linejoin='round'
        stroke-width='2' points=' " & Lines &
      " '/>" &
        CONCATENATEX(PointTable,[@Points]) &
        "<circle cx='"& LastSparkXValue & "' cy='" & SVGHeight - LastSparkYValue & "' r='4' stroke='" & Color & "' stroke-width='3' fill='" & PointColour & "' />" &
        "</svg>"
       
RETURN
SVGImageURL
```
<img width="940" height="498" alt="image" src="https://github.com/user-attachments/assets/edaa42c2-3ba4-4feb-b6b5-965073c2a834" />

The codes used are below:

```
Won Deals|CQ 
IBCS CQ vs PQ Won Opps = 

VAR _ColorAC = "#black"
VAR _ColorPY = "white"

VAR _FontWeight =
    IF ( HASONEVALUE ( 'sales_teams'[sales_agent] ), "normal", "bold" )

VAR _ValueAvg = [Last Q Average Won Opps]

// For Sales Agents
VAR PY = [Previous Q Won Opps]
VAR Actual = [Last Q Won Opps]

VAR _maxValue =
    --max value (AC and PY)
    IF ( HASONEVALUE ( sales_teams[sales_agent] ),
    MAX (
        MAXX ( ALLSELECTED ( 'sales_teams'  ), [Last Q Won Opps] ),
        MAXX ( ALLSELECTED ( 'sales_teams'  ), [Previous Q Won Opps] )
    ) / 0.6,
    IF ( HASONEVALUE ( sales_teams[manager] ),
    MAX (
        MAXX ( ALLSELECTED ( 'sales_teams'[manager] ), [Last Q Won Opps] ),
        MAXX ( ALLSELECTED ( 'sales_teams'[manager] ), [Previous Q Won Opps] )
    ) / 0.8,
    MAX (
         [Last Q Won Opps],
        [Previous Q Won Opps]
    ) / 0.8)
    )
VAR _WidthAvg =
    --width (up to 100%)
    FORMAT ( DIVIDE ( _ValueAvg, _maxValue ), "0%" )
VAR _WidthAC =
    --width (up to 100%)
    FORMAT ( DIVIDE ( Actual, _maxValue ), "0%" )
VAR _WidthPY =
    --width (up to 100%)
    FORMAT ( DIVIDE ( PY, _maxValue ), "0%" )

VAR _Rank =
    --row value rank to ensure correct column sorting (by AC value)
    100000
        + RANKX ( ALLSELECTED ( 'sales_teams'[manager]), [Last Q Won Opps],, ASC )
VAR _Rank2 =
    --row value rank to ensure correct column sorting (by AC value)
    100000
        + RANKX ( ALLSELECTED ( 'sales_teams'[sales_agent]), [Last Q Won Opps],, ASC )
RETURN 
    "data:image/svg+xml;utf8," & "<svg xmlns=""http://www.w3.org/2000/svg"" width=""" & 250& """ height=""" & 45 & """>
    /*" & _Rank & _Rank2 &"*/
    " & /*"<line  x1=""" & _WidthAvg & """ x2=""" & _WidthAvg & """ y1=""0%"" y2=""100%"" stroke=""" & _ColorAC & """ stroke-width=""1""></line >" & */ "
    <rect y=""9%"" x=""0"" width=""" & _WidthPY & """ height=""67%"" fill=""" & _ColorPY & """ stroke=""#333333"" stroke-width=""1""></rect>
    <rect y=""27%"" x=""0"" width=""" & _WidthAC & """ height=""67%"" fill=""" & _ColorAC & """></rect>   
    <text x=""" & _WidthAC & """ dx=""5"" y=""65%"" font-weight=""" & _FontWeight & """>" & FORMAT(Actual,"#,###") & "</text>
" & "<style>
    <![CDATA[
      text{
        font-family: Thaoma;
        font-size: 24px;
        alignment-baseline: middle;
      }
    ]]>
  </style>" & "
</svg>"
```
```
△ PQ 
IBCS Delta CQ vs PQ Won Opps = 
//ΔPY = AC - PY (difference between AC and PY)
//Returns an SVG image code with ΔPY bars (green - positive, red - negative)
//  and a vertical line for Axis Y (ΔPY = 0)
VAR _ColorGrey = "#c6c6c6"
VAR _ColorRed = "#ff0000"
VAR _ColorGreen = "#008e96"

VAR _FontWeight =
    --'normal' font for sales persons, 'bold' font for total row (average)
    IF (
        HASONEVALUE ( 'sales_teams'[sales_agent] ),
        "normal",
        "bold"
    )

VAR _Value = [Opps Won Delta PQ]

VAR _maxValue =
    
    IF ( HASONEVALUE ( sales_teams[sales_agent] ),
    MAX (
        MAXX ( ALL( 'sales_teams'  ), [Opps Won Delta PQ] ),
        MAXX ( ALL( 'sales_teams'  ), [Opps Won Delta PQ] )
    ) /0.3 ,
    IF ( HASONEVALUE ( sales_teams[manager] ),
    MAX (
        MAXX ( ALL( 'sales_teams'  ), [Opps Won Delta PQ]),
        MAXX ( ALL( 'sales_teams'  ), [Opps Won Delta PQ] )
    )  /0.2,
    MAX (
        MAXX ( ALL( 'sales_teams'), [Opps Won Delta PQ]),
        MAXX ( ALL( 'sales_teams' ), [Opps Won Delta PQ] )
    )  /0.2
    ))

VAR _WidthValue =
    --bar width (numeric value)
    ( DIVIDE ( ABS ( _Value ), _maxValue ) / 2 ) * 0.5
VAR _Width =
    --bar width - text for SVG
    FORMAT ( _WidthValue, "0.0%" )
VAR _barColor =
    --green or red
    IF ( _Value > 0, _ColorGreen, _ColorRed )
VAR _X =
    --x position of a bar
    SWITCH (
        TRUE (),
        _Value >= 0, "50%",
        _Value < 0, FORMAT ( 0.5 - _WidthValue, "0%" )
    )
VAR _XText =
    --x position of a label
    SWITCH (
        TRUE (),
        _Value >= 0, FORMAT ( 0.5 + _WidthValue, "0%" ),
        _Value < 0, _X
    )
VAR _Anchor =
    --text anchor
    SWITCH ( TRUE (), _Value >= 0, "start", _Value < 0, "end" )
VAR _DX =
    --text offset along axis X
    SWITCH ( TRUE (), _Value >= 0, 5, _Value < 0, -5 )
VAR _Rank =
    --row value rank to ensure correct column sorting (by AC value)
    100000
        + RANKX ( ALLSELECTED ( 'sales_teams'[manager]), [Opps Won Delta PQ],, ASC )
VAR _Rank2 =
    --row value rank to ensure correct column sorting (by AC value)
    100000
        + RANKX ( ALLSELECTED ( 'sales_teams'[sales_agent]), [Opps Won Delta PQ],, ASC )

VAR _SVG =
    --SVG code
    --header
    --comment with Rank text for sorting
    --line (axis Y)
    --rectangle for DeltaPY (green or red)
    --text data label for DeltaPY
    --SVG style

    IF(
    ISBLANK([Opps Won Delta PQ]) = true, BLANK(),
    "data:image/svg+xml;utf8," & "<svg xmlns=""http://www.w3.org/2000/svg"" width=""" & 150& """ height=""" & 30 & """>
    /*" & _Rank &_Rank2 & "*/ 
    <line  x1=""50%"" x2=""50%"" y1=""0%"" y2=""100%"" stroke=""" & _ColorGrey & """ stroke-width=""1""></line >
    <rect y=""17%"" x=""" & _X & """ width=""" & _Width & """ height=""67%"" fill=""" & _barColor & """></rect>    
    <text text-anchor=""" & _Anchor & """ x=""" & _XText & """ dx=""" & _DX & """ y=""55%"" font-weight=""" & _FontWeight & """>"
        & FORMAT ( _Value, "+#,0;-#,0;#,0" ) & "</text>    
" & "<style>
    <![CDATA[
      text{
        font-family: Thaoma;
        font-size: 13px;
        alignment-baseline: middle;
      }
    ]]>
  </style>" & "
</svg>"
    )
RETURN
    _SVG
```
```
%△ PQ 
IBCS % Delta CQ vs PQ Won Opps = 

VAR PercDiff = [Opps % Won Delta PQ]

VAR Width = PercDiff * 85 // Adjust scale factor as needed

VAR Color = IF(PercDiff < 0, "#ff0000", if(PercDiff > 0,"#008e96", "grey"))
VAR _FontWeight =
    --'normal' font for sales persons, 'bold' font for total row (average)
    IF (
        HASONEVALUE ( 'sales_teams'[sales_agent] ),
        "normal",
        "bold"
    )

VAR Direction = IF(PercDiff < 0,  50 + Width, 50 + Width)
VAR TextPosition = IF(PercDiff < 0, Direction -38, Direction + 7)

VAR _Rank =
    --row value rank to ensure correct column sorting (by AC value)
    100000
        + RANKX ( ALLSELECTED ( 'sales_teams'[manager]), [Opps % Won Delta PQ],, ASC )
VAR _Rank2 =
    --row value rank to ensure correct column sorting (by AC value)
    100000
        + RANKX ( ALLSELECTED ( 'sales_teams'[sales_agent]), [Opps % Won Delta PQ],, ASC )

RETURN
IF(ISBLANK([Opps Won Delta PQ]), BLANK(), 
"data:image/svg+xml;utf8," & "
    <svg width='260' height='32' viewBox='-2 -2 102 22' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' overflow='visible'>
    /*" & _Rank &_Rank2 & "*/ 
    <line  x1='50' x2='50%' y1='0' y2='100' stroke=""" & "grey" & """ stroke-width=""1""></line >
        <line x1='50' y1='10' x2='"&Direction&"' y2='10' stroke='"&Color&"' stroke-width='2'/>
        <circle cx='"&Direction&"' cy='10' r='5' fill='"&Color&"'/>
        <text x='"&TextPosition&"' y='15' font-weight='"&_FontWeight&"' font-family='Thaoma, sans-serif' font-size='14px'>"&FORMAT(PercDiff, "0%")&"</text>
    </svg>" 
)
```
```
Quarterly Trend 
Won Opps Sparkline Point = 
// Static line color - use %23 instead of # for Firefox compatibility (Measure Derived from Eldersveld Modified by Kolosko)
VAR LineColour = "black"
// Color logic for the last quarter
VAR PercDiff = [Opps % Won Delta PQ]
VAR Color = IF(PercDiff < 0, "#ff0000", if(PercDiff > 0,"#008e96", "grey"))
VAR PointColour = "white"

// SVG dimensions
VAR SVGWidth = 150 // Change this value to adjust the width
VAR SVGHeight = 30 // Change this value to adjust the height

// "Quarter" field used in this example along the X axis
VAR XMinDate = MIN('Close Dates Table'[Quarter])
VAR XMaxDate = MAX('Close Dates Table'[Quarter])

// Obtain overall min and overall max measure values when evaluated for each quarter
VAR YMinValue = MINX(Values('Close Dates Table'[Quarter]),CALCULATE([Won Opportunities]))
VAR YMaxValue = MAXX(Values('Close Dates Table'[Quarter]),CALCULATE([Won Opportunities]))

// Build table of X & Y coordinates and fit to SVGWidth x SVGHeight viewbox
VAR SparklineTable = ADDCOLUMNS(
    SUMMARIZE('Close Dates Table','Close Dates Table'[Quarter]),
        "X",INT(SVGWidth * DIVIDE('Close Dates Table'[Quarter] - XMinDate, XMaxDate - XMinDate)),
        "Y",INT(SVGHeight * DIVIDE([Won Opportunities] - YMinValue,YMaxValue - YMinValue)))

// Concatenate X & Y coordinates to build the sparkline
VAR Lines = CONCATENATEX(SparklineTable,[X] & "," & SVGHeight-[Y]," ", 'Close Dates Table'[Quarter])
VAR PointTable=
    ADDCOLUMNS(
        SUMMARIZE('Close Dates Table','Close Dates Table'[Quarter]),
            "@Colour",IF([Won Opportunities]=YMaxValue,"Green",IF([Won Opportunities]=YMinValue,"Red",LineColour)),
        "@Points", "<circle cx='"&INT(SVGWidth * DIVIDE('Close Dates Table'[Quarter] - XMinDate, XMaxDate - XMinDate))&"' cy='" & SVGHeight -  INT(SVGHeight * DIVIDE([Won Opportunities] - YMinValue,YMaxValue - YMinValue)) & "' r='3' stroke='"&LineColour&"' stroke-width='1'  fill='"&LineColour&"'/>")

// Last data points on the line
VAR LastSparkYValue = MAXX( FILTER(SparklineTable, 'Close Dates Table'[Quarter] = XMaxDate), [Y])
VAR LastSparkXValue = MAXX( FILTER(SparklineTable, 'Close Dates Table'[Quarter] = XMaxDate), [X])

VAR _Rank =
    --row value rank to ensure correct column sorting (by AC value)
    100000
        + RANKX ( ALLSELECTED ( 'sales_teams'[manager]), [Last Q Won Opps],, ASC )
VAR _Rank2 =
    --row value rank to ensure correct column sorting (by AC value)
    100000
        + RANKX ( ALLSELECTED ( 'sales_teams'[sales_agent]), [Last Q Won Opps],, ASC )
// Add to SVG, and verify Data Category is set to Image URL for this measure
VAR SVGImageURL = 
    "data:image/svg+xml;utf8," & 
    "<svg xmlns='http://www.w3.org/2000/svg' x='0px' y='0px' viewBox='-7 -7 " & SVGWidth+14 & " " & SVGHeight+14 & "'>
    /*" & _Rank &_Rank2 & "*/ " &
    "<polyline 
        fill='transparent' stroke='" & LineColour & "' 
        stroke-linecap='round' stroke-linejoin='round' 
        stroke-width='2' points=' " & Lines & 
      " '/>" &
        CONCATENATEX(PointTable,[@Points]) &
        "<circle cx='"& LastSparkXValue & "' cy='" & SVGHeight - LastSparkYValue & "' r='4' stroke='" & Color & "' stroke-width='3' fill='" & PointColour & "' />" &
      IF(ISBLANK([Rev % Delta]), BLANK(),   "</svg>"
  )      
RETURN 
IF(ISBLANK([Opps Won Delta PQ]), BLANK(), 
SVGImageURL
)
```
