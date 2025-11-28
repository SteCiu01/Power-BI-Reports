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

![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/ff0d9c1f-1ab6-405a-9973-e0ea5e4cbe69.png)In this KPI card the trendline and the "Vs. PQ: -61 / (-4.9%)" part was done with the following code:

```
Won Opps Sparkline Point Card = 

// Static line color - use %23 instead of # for Firefox compatibility (Measure Derived from Eldersveld Modified by Kolosko)VAR LineColour = "black"// Color logic for the last quarterVAR PercDiff = [Opps % Won Delta PQ]VAR Color = IF(PercDiff &lt; 0, "#ff0000", if(PercDiff &gt; 0,"#008e96", "grey"))VAR PointColour = "white"

// SVG dimensionsVAR SVGWidth = 150 // Change this value to adjust the widthVAR SVGHeight = 30 // Change this value to adjust the height

// "Quarter" field used in this example along the X axisVAR XMinDate = MIN('Close Dates Table'[Quarter])VAR XMaxDate = MAX('Close Dates Table'[Quarter])

// Obtain overall min and overall max measure values when evaluated for each quarterVAR YMinValue = MINX(Values('Close Dates Table'[Quarter]),CALCULATE([Won Opportunities]))VAR YMaxValue = MAXX(Values('Close Dates Table'[Quarter]),CALCULATE([Won Opportunities]))

// Build table of X &amp; Y coordinates and fit to SVGWidth x SVGHeight viewboxVAR SparklineTable = ADDCOLUMNS(&nbsp; &nbsp; SUMMARIZE('Close Dates Table','Close Dates Table'[Quarter]),&nbsp; &nbsp; &nbsp; &nbsp; "X",INT(SVGWidth * DIVIDE('Close Dates Table'[Quarter] - XMinDate, XMaxDate - XMinDate)),&nbsp; &nbsp; &nbsp; &nbsp; "Y",INT(SVGHeight * DIVIDE([Won Opportunities] - YMinValue,YMaxValue - YMinValue)-25))

VAR SparklineTableSeller = ADDCOLUMNS(&nbsp; &nbsp; SUMMARIZE('sales_teams',sales_teams[sales_agent]),&nbsp; &nbsp; &nbsp; &nbsp; "Y",INT(SVGHeight * DIVIDE([Won Opportunities] - YMinValue,YMaxValue - YMinValue)))

VAR SparklineTableManager = ADDCOLUMNS(&nbsp; &nbsp; SUMMARIZE('sales_teams',sales_teams[manager]),&nbsp; &nbsp; &nbsp; &nbsp; "Y",INT(SVGHeight * DIVIDE([Won Opportunities] - YMinValue,YMaxValue - YMinValue)))

// Concatenate X &amp; Y coordinates to build the sparklineVAR Lines = CONCATENATEX(SparklineTable,[X] &amp; "," &amp; SVGHeight-[Y]," ", 'Close Dates Table'[Quarter])VAR PointTable=&nbsp; &nbsp; ADDCOLUMNS(&nbsp; &nbsp; &nbsp; &nbsp; SUMMARIZE('Close Dates Table','Close Dates Table'[Quarter]),&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; "@Colour",IF([Won Revenues]=YMaxValue,"Green",IF([Won Opportunities]=YMinValue,"Red",LineColour)),&nbsp; &nbsp; &nbsp; &nbsp; "@Points", "
