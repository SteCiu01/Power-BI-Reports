# The Challenge

"For the **Maven LEGO Challenge**, you'll need to stack your imagination and analytical prowess to piece together an interactive dashboard or visual that lets users explore the history and evolution of LEGO sets from the past 5 decades."

## Maven LEGO Challenge - LEGO Sets Explorer [Finalist]

<img width="1775" height="1003" alt="5 - Maven Lego Challenge" src="https://github.com/user-attachments/assets/16c668c2-07f9-4cd9-853c-fff7f6758d2e" />
<br><br>

Are you a LEGO enthusiast? You can use the **LEGO Sets Explorer** to discover more than 18,000 LEGO Sets released in the past 5 decades. Also, LEGO Sets Explorer lets you directly connect with brickset.com, where you can purchase your favourite sets.

[📊 Live Report](https://app.powerbi.com/view?r=eyJrIjoiZjM4YTAyZjEtOTAyMS00ZDQ3LTk0MjgtYjU0MTg1OWUyNDNkIiwidCI6IjhhNDk1ZGQwLThkNDEtNDcyYy1iMTljLTFhMzQzZjYxYmFhMSIsImMiOjl9)

# The Team

In this challenge I teamed-up again with [Aleksandra Zaczek](https://www.linkedin.com/in/aleksandra-%C5%BCaczek-1875b4108/), UX/UI designer and student at the "User Experience Design/Product Design" Postgraduate Program at the SWPS Universytet in Warsaw, Poland.

**We divided the work as follows:**

- Aleksandra: creation of the design of the tool's interface in Figma, in order to optimize the users experience.
- Stefano: data exploration and analysis, data ETL process, data quality and Power BI Development.
- Joint efforts: strategic thinking in defining the user persona, cooperation in implementing the design from Figma to Power BI.

# Approach Used

## **First step: identified the target audience: "LEGO enthusiasts"**

As the dataset is about LEGO Sets and the topic of the challenge is to let users explore the history and evolution of LEGO sets from the past 5 decades, a user of this dashboard/explorative tool can be defined as *"someone that is a LEGO user/player or someone that is interested in exploring recent and past LEGO Sets and, eventually purchase some of them".* This type of "persona” can be named as **"LEGO enthusiast"**.

## **Second step: identified what can/should be delivered to "LEGO enthusiasts" and how, based on the available data.**

**- Data understanding -**

On this matter, it was crucial to first understand what data was available and what it could be used for, on this purpose a careful explorative analysis has been conducted in 3 steps:

**Step 1: first understanding of the csv file provided by Maven Analytics Team in terms of dimension and pieces of information in each column.**

The dataset has 18,457 rows, each row is a LEGO Set, with its unique identifier i.e., "set_id".

For each LEGO Set, there are the following columns containing valuable pieces of information: the year of release, the LEGO category, the LEGO theme group, the LEGO theme, the LEGO sub-theme, the LEGO set name, the number of pieces, the number of mini figures, the minimum advised age for using the LEGO set, the launch price in USD, the image of the LEGO set and the brickset.com URL (website where it is possible to find also the current LEGO sets prices and eventually purchase the set).

After understanding what was available in the csv file a decision on how to use each column needed to be made.

For three columns: year of release, image URL and brikset.com URL the decision was quite straight forward just exploring the csv file: the year of release can be used both as a fact for enriching the output and as a dimension for filter purposes, while the image of the LEGO set and the brickset.com URL provide additional pieces that can enhance the quality of the output. A note: the URLs of the LEGO sets images have some blanks, therefore there will be the need to have a placeholder image for when the real image is not available.

For the other columns a more in-depth analysis is required to decide how to use them, and it has been decided to run it using Microsoft SQL Server Management Studio, tool that allows quick data manipulation.

**Step 2: created a Lego database on Microsoft SQL server and imported the table.**

```
USE master

CREATE DATABASE Lego_DB;
GO

USE Lego_DB;
GO

-- Create lego_sets table
CREATE TABLE lego_sets (
set_id VARCHAR(20) NOT NULL PRIMARY KEY,
name NVARCHAR(300),
year INT,
theme NVARCHAR(300),
subtheme NVARCHAR(300),
themeGroup NVARCHAR(300),
category NVARCHAR(300),
pieces INT,
minifigs INT,
agerange_min INT,
US_retailPrice DECIMAL(10,2),
bricksetURL NVARCHAR(MAX),
thumbnailURL NVARCHAR(MAX),
imageURL NVARCHAR(MAX)
);

-- inserted the data
BULK INSERT dbo.lego_sets
FROM 'C:\Users\stefa\OneDrive\Desktop\SQL\Lego DB\lego_sets.csv'
WITH
(
	FORMAT='CSV',
  FIRSTROW=2
)
GO
```

**Step 3: run some queries to analyse various aspects of the data and have full understanding on what should be done in terms of transformations and how to use each column.**

**Query 1**

```
-- Hierarchical check

SELECT DISTINCT
category, themeGroup, theme, subtheme, name
FROM lego_sets
ORDER BY 1
```

**Query 1** **results:**

The LEGO category, the LEGO theme group, the LEGO theme, the LEGO sub-theme and, of course the LEGO set name provide a hierarchical categorization that can be used by users for narrowing down their preferences.

Some of these elements, like Lego set name, or theme, can also be used to enrich the output after users filtering.

Some transformations of the data will be required in order to make this hierarchy as user friendly as possible, and will be later carried out using Power Query (e.g, null values in the “subtheme” column, presence of "Random" and "Other" in the “category” column, the column “name” has some duplications, etc. ).

**Query 2**

```
-- check the total LEGO set launched over time and the percentage of null values for the items: LEGO pieces, mini figures, age range and price in each year, comparing this against the average percentage of null pieces - all years for each one of the items.

SELECT
    Year,
    COUNT(set_id) AS Launched_sets,

    FORMAT(SUM(CASE WHEN pieces IS NULL THEN 1 ELSE 0 END) *1.0 / COUNT(set_id), '0%') AS Perc_null_pieces,
    FORMAT(AVG(SUM(CASE WHEN pieces IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(set_id)) OVER(), '0%') AS avg_perc_null_pieces_all_years,

    FORMAT(SUM(CASE WHEN minifigs IS NULL THEN 1 ELSE 0 END) *1.0 / COUNT(set_id), '0%') AS Perc_null_minifigures,
    FORMAT(AVG(SUM(CASE WHEN minifigs IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(set_id)) OVER(), '0%') AS avg_perc_null_minifiguers_all_years,

    FORMAT(SUM(CASE WHEN agerange_min IS NULL THEN 1 ELSE 0 END) *1.0 / COUNT(set_id), '0%') AS Perc_null_agerange,
    FORMAT(AVG(SUM(CASE WHEN agerange_min IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(set_id)) OVER(), '0%') AS avg_perc_null_agerange_all_years,

    FORMAT(SUM(CASE WHEN US_retailPrice IS NULL THEN 1 ELSE 0 END) *1.0 / COUNT(set_id), '0%') AS Perc_null_price,
    FORMAT(AVG(SUM(CASE WHEN US_retailPrice IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(set_id)) OVER(), '0%') AS avg_perc_null_price_all_years
FROM lego_sets
GROUP BY Year
ORDER BY 1;
```

**Query 2 results:**

Over time there is a significant increase of LEGO sets launches.

Data on LEGO pieces tends to be missed more in the recent years (more than 20% of missing values after 2000s).

It is the opposite for age range and price that have a very high percentage of null values until the beginning of 2000s , when this percentage drops from close to 100% to around 30%-40%.

Mini figures percentage of null values is around 100% until mid 1970s then it is quite stable averaging around 50%.

**The conclusions that can be made are:**

- It was more complicated retrieving data for older LEGO sets in terms of price, and age range. 
- Age range might have been also not included in several older LEGO sets at launch, but often it was a missed data retrieval.
- Regarding mini figures it seems that several sets do not include mini figures but in many circumstances this piece was not possible to be retrieved.
- A good example for the previous points is the set called: [Michele Mouton Minifig](https://brickset.com/sets/MMOUTON-1) - this is actually a mini figure, based on 4 pieces with age set to 6+, that in the lego_set data comes with only the number of pieces. Here the missing age range and the missing fact it is a mini figure are a clear example of missed data retrieval.
- Furter investigations are required on LEGO pieces, particularly on regards whether the missing pieces might be due to the nature of some LEGO categories that do not have pieces (e.g., books?) or the piece of information is just missing.

**Queries 3-5**

```
-- Percentage of null pieces by category and over time
SELECT 
    Year, 
    category,
    count(set_id) as Launched_sets,
    FORMAT(SUM(CASE WHEN pieces IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(set_id), '0%') as Perc_null_pieces,
    FORMAT(SUM(SUM(CASE WHEN pieces IS NULL THEN 1 ELSE 0 END)) OVER (PARTITION BY category) * 1.0 / SUM(COUNT(set_id)) OVER (PARTITION BY category), '0%') as Total_Perc_null_pieces_for_category
FROM lego_sets
GROUP BY Year, category
ORDER BY Year;

-- Category-theme analysis
SELECT
category,
SUM(count(*)) OVER (PARTITION BY category) as Launched_sets_by_themeGroup,
FORMAT(SUM(SUM(CASE WHEN pieces IS NULL THEN 1 ELSE 0 END)) OVER (PARTITION BY category) * 1.0 / SUM(COUNT(*)) OVER (PARTITION BY category), '0%') as Perc_null_pieces_by_category,
theme,
COUNT(*) as Launched_sets_by_theme,
FORMAT(SUM(CASE WHEN pieces is null THEN 1 ELSE 0 END) *1.0 / count(set_id), '0%') as Perc_null_pieces_by_theme
FROM lego_sets
GROUP BY category, theme
ORDER BY 
SUM(SUM(CASE WHEN pieces IS NULL THEN 1 ELSE 0 END)) OVER (PARTITION BY category) * 1.0 / SUM(COUNT(*)) OVER (PARTITION BY category) DESC , 
SUM(CASE WHEN pieces is null THEN 1 ELSE 0 END) *1.0 / COUNT(*) DESC;

-- ThemeGroup-Theme analysis
SELECT
themeGroup,
SUM(COUNT(*)) OVER (PARTITION BY themeGroup) as Launched_sets_by_themeGroup,
FORMAT(SUM(SUM(CASE WHEN pieces IS NULL THEN 1 ELSE 0 END)) OVER (PARTITION BY themeGroup) * 1.0 / SUM(COUNT(*)) OVER (PARTITION BY themeGroup), '0%') as Perc_null_pieces_by_themeGroup,
theme,
count(*) as Launched_sets_by_theme,
Format(sum(CASE WHEN pieces is null THEN 1 ELSE 0 END) *1.0 / COUNT(*), '0%') as Perc_null_pieces_by_theme
FROM lego_sets
GROUP BY themeGroup, theme
ORDER BY 
SUM(SUM(CASE WHEN pieces IS NULL THEN 1 ELSE 0 END)) OVER (PARTITION BY themeGroup) * 1.0 / SUM(COUNT(*)) OVER (PARTITION BY themeGroup) DESC , 
SUM(CASE WHEN pieces is null THEN 1 ELSE 0 END) *1.0 / COUNT(*) DESC;
```

**Queries 3-5 results:**

Throughout the whole time span 1970-2022, there are books and gears categories that have respectively 89% and 100% of avg. percentage of null values when it comes to LEGO pieces.

This is mostly for the nature of the products, but, there are also other categories and themes that could have this piece of information, that is however missing.

For instance, “extended“ and "collection", categories show a growing trend of their percentage of null pieces especially in the past 10-20 years of releases. These two categories have respectively 12% and 10% of null values from 1970 to 2022.

Finally, looking at the results of the Query 4 and 5 in particular,  it is really visible how there are more than 30 themes, across all the categories, with more than 10% of missing values on LEGO pieces and some of them are referring to LEGO sets that actually contain pieces (easy to verify with a simple desktop research for some samples), therefore, in that circumstance the null values are not due to the nature of the product (e.g., the book) but to the missed retrieval of the piece of information.

**Conclusion on how to use LEGO pieces, mini figures, age range and price columns:**

- The number of LEGO pieces, the number of mini figures, the minimum age for using the LEGO set, the launch price in USD provide facts/useful pieces of information.
- However, these pieces of information are not always available for each LEGO Set (actually most of the sets have at least 1 or 2 missing among these 4 analysed elements), and the quality of this data vary across time and categories. Sometimes there is a null value because the data bit does not exists, sometimes it is due to missed retrieval of the data piece. 
- It would not be fair providing users with insights like "the LEGO set with more pieces is..." or "the top 5 LEGO sets with more mini figures are..." etc... because simply it is not possible to say that with certainty, as a missing value could be in reality the one that would have more pieces or would be among the top 5. 
- On top of that, a LEGO enthusiast is not necessarily a data person and therefore he/she might be totally not interested in statistics or trends.
- On the other hand, it is still nice to provide the users with the information on these elements, when available, after they narrow down their selection and they explore their results (e.g., if a user narrows down the selection to Duplo sets and wants more details on Duplo Train Starter Set, it would be nice for him/her to see that this set comes with 65 pieces, 2 mini figures and it was launched wit a price of 49.99$) .
- For this reason, considering the explorative goal of the tool/dashboard,  it is advisable not to use these 4 columns for anything, but to provide additional facts on a specific LEGO set, after the users make their selection on what LEGO set they want to see in detail.

**- How to deliver value to the "LEGO enthusiasts, based on the available data -**

After understanding the dataset, it was decided  what could and should be delivered to LEGO enthusiasts in detail, and was drafted a list of items and functionalities that should be included in the tool, to maximize the users' experience:

- A first landing  welcome page with Logo, tool name, a nice background and a sentence of what is this tool about. Here the users are free to type in what they are looking for, narrowing down their search by year of release. 
- In the welcome page there must be also a button that the users can click for having additional filter possibilities such as: LEGO category, LEGO theme group, LEGO theme, LEGO sub-theme, and a filter that includes Lego Set Name and Year of Release together sorted by year of release. 
- Finally through a "Go" button, to be clicked after the users are happy with their selections, the users can land to page 2, the results page.
- The results page needs to be simple with, on the left, a small visual preview (card slicer) of the LEGO Sets in the current selection (image, name and released year), that is scrollable and, allows the users to select one LEGO Set they want to see in detail. On the right, there should be the selected LEGO Set's details, with a bigger image, the release date, the LEGO Set name ad from what LEGO theme it comes from and, the number of pieces, the number of mini figures, the price at launch in $ the minimum age recommended and the link to brikset.com page of that selected LEGO Set, where the users can eventually purchase the set.
- Also, in the results page there must be the option to open and close a filters pane where the users can adjust their selection, as well as a button that lets them reset all the filters.

## **Third step: data clean-up and transformation to make it suitable for developing the tool as designed.**

For making data clean-up and transformations, it has been chosen Power Query, and, below it is provided the M-Code, with comments for each step, that is fully explanatory of the extensive data transformation that has been done at this stage.

It's worthy to spend couple of words on some specific steps (from step 30 to step 38) for couple of reasons: first, these steps allow the tool to work properly, so that the users can visualize in a user-friendly way all the available LEGO Sets, second, they are the most complex transformations in this long code.

As the tool will need to have a card slicer in the Results Page, where users will pick the LEGO Set they want to see in detail, the idea is that the LEGO Sets displayed in this slicer are sorted by their release date and those with the same date, in alphabetical order.

In the steps 30-31, the code achieves a sorting index column to be put in the tooltip of the slicer and used to sort. This sorting index is created sorting by year of release first, then, by alphabetical order, so that within the same year of release all the LEGO Sets names are alphabetically sorted, and finally, by LEGO Sets IDs.

**Why this last step?** Because there are some LEGO Sets that have the same name, exactly the same name, but different IDs, and they can appear multiple times within the same year or in different years. Therefore it is necessary to also discover these sets' editions. For achieving this, it is possible to sort them by ID, so those with the "lower ID" would be those belonging to an earlier edition.

The steps from 32 to 38 achieve the identification of the edition and its inclusion in the LEGO Set names, creating a new column called "LEGO Set Name and Edition" column to be used as field in the card slicer. This is crucial to make the tool working properly. In fact, if only the "LEGO Set Name" column is used in the card slicer field, and there are 2 or more sets with the same exact name, only the first one can be visualised in detail, not allowing the user to potentially view all the 18,457 LEGO Sets.

*This M-Code can be directly copied and pasted in the Advanced Editor and be used to perform transformations on this dataset. Please note that using this code it is necessary to change the "Source" of the dataset.*

/* **Initial Power Query "let" line** */

```
let
```

/* **STEP 1:** Import the file */

```
  Source = Sql.Database("STEFANO_PC", "Lego_DB", [Query= "SELECT * FROM lego_sets"]), 
```

/* **STEP 2:** Converted everything to text, to start then working on transformations */

```
#"Converted everyting to text" = Table.TransformColumnTypes(
  Source, 
  {
    {"set_id", type text}, 
    {"name", type text}, 
    {"year", type text}, 
    {"theme", type text}, 
    {"subtheme", type text}, 
    {"themeGroup", type text}, 
    {"category", type text}, 
    {"pieces", type text}, 
    {"minifigs", type text}, 
    {"agerange_min", type text}, 
    {"US_retailPrice", type text}, 
    {"bricksetURL", type text}, 
    {"thumbnailURL", type text}, 
    {"imageURL", type text}
  }
),
```

/* **STEP 3:** Trim text to be sure in every column and in every row the text starts homogenously */

```
#"Trimmed Text" = Table.TransformColumns(
    #"Promoted Headers", 
    {
      {"set_id", Text.Trim, type text}, 
      {"name", Text.Trim, type text}, 
      {"year", Text.Trim, type text}, 
      {"theme", Text.Trim, type text}, 
      {"subtheme", Text.Trim, type text}, 
      {"themeGroup", Text.Trim, type text}, 
      {"category", Text.Trim, type text}, 
      {"pieces", Text.Trim, type text}, 
      {"minifigs", Text.Trim, type text}, 
      {"agerange_min", Text.Trim, type text}, 
      {"US_retailPrice", Text.Trim, type text}, 
      {"bricksetURL", Text.Trim, type text}, 
      {"thumbnailURL", Text.Trim, type text}, 
      {"imageURL", Text.Trim, type text}
    }
  ), 
```

/* **STEP 4:** Changed the data types to work better in the next steps. */

```
#"Changed Type" = Table.TransformColumnTypes(
    #"Trimmed Text", 
    {
      {"pieces", Int64.Type}, 
      {"minifigs", Int64.Type}, 
      {"agerange_min", Text.Type}, 
      {"US_retailPrice", Currency.Type}, 
      {"year", Int64.Type}
    }
  ),
```

/* **STEPS 5-21:** formatting changes in "name" column for better user friendliness and better consistency for further operations (see from step 30) */

```
/* STEPS 5-21: formatting changes in "name" column for better user friendliness and better connsistency for further operations (see from step 30) */
  #"Replaced Value ""{?}"" in name" = Table.ReplaceValue(
    #"Changed Type", 
    "{?}", 
    "Unknown", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""(Unnamed)"" in name" = Table.ReplaceValue(
    #"Replaced Value ""{?}"" in name", 
    "(Unnamed)", 
    "Unknown", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""unknown"" in name" = Table.ReplaceValue(
    #"Replaced Value ""(Unnamed)"" in name", 
    "unknown", 
    "Unknown", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""{"" in name" = Table.ReplaceValue(
    #"Replaced Value ""unknown"" in name", 
    "{", 
    "", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""}"" in name" = Table.ReplaceValue(
    #"Replaced Value ""{"" in name", 
    "}", 
    "", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""-"" in name" = Table.ReplaceValue(
    #"Replaced Value ""}"" in name", 
    "– Page from British Patent Application for LEGO Elements, 1968", 
    "Page from British Patent Application for LEGO Elements, 1968", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""#1"" in name" = Table.ReplaceValue(
    #"Replaced Value ""-"" in name", 
    " #1", 
    "", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""#2"" in name" = Table.ReplaceValue(
    #"Replaced Value ""#1"" in name", 
    " #2", 
    "", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""#3"" in name" = Table.ReplaceValue(
    #"Replaced Value ""#2"" in name", 
    " #3", 
    "", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""#4"" in name" = Table.ReplaceValue(
    #"Replaced Value ""#3"" in name", 
    " #4", 
    "", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""#5"" in name" = Table.ReplaceValue(
    #"Replaced Value ""#4"" in name", 
    " #5", 
    "", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""#6"" in name" = Table.ReplaceValue(
    #"Replaced Value ""#5"" in name", 
    " #6", 
    "", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""#7"" in name" = Table.ReplaceValue(
    #"Replaced Value ""#6"" in name", 
    " #7", 
    "", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""#8"" in name" = Table.ReplaceValue(
    #"Replaced Value ""#7"" in name", 
    " #8", 
    "", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Replaced Value ""#51"" in name" = Table.ReplaceValue(
    #"Replaced Value ""#8"" in name", 
    " #51", 
    "", 
    Replacer.ReplaceText, 
    {"name"}
  ), 
  #"Capitalized Each Word" = Table.TransformColumns(
    #"Replaced Value ""#51"" in name", 
    {{"name", Text.Proper, type text}}
  ), 
```

/* **STEP 22:** For user friendliness, put Random under "Other" in "category column" */

```
#"Replaced Value ""Random"" in Category" = Table.ReplaceValue(
    #"Capitalized Each Word", 
    "Random", 
    "Other", 
    Replacer.ReplaceText, 
    {"category"}
  ),
```

/* **STEP 23:** Put every blank value in "themeGroup" column, into the Miscellaneous theme group */

```
#"Replaced null values in themeGroup" = Table.ReplaceValue(
    #"Replaced Value ""Random"" in Category", 
    null, 
    "Miscellaneous", 
    Replacer.ReplaceValue, 
    {"themeGroup"}
  ),
```

/* **STEP 24-25:** in the column "subtheme" it has been implemented the logic that if in subtheme there is an empty value, then the subtheme is equal to the theme, otherwise the subtheme is kept. This way the users don't have any empty subtheme in their search options, then, proper data type set for column subtheme, after the conditional replacement  */

```
#"Replaced null  values in subtheme" = Table.ReplaceValue(
    #"Replaced null values in themeGroup", 
    each [subtheme], 
    each if [subtheme] = null then [theme] else [subtheme], 
    Replacer.ReplaceValue, 
    {"subtheme"}
  ), 
  #"Changed Type in legotheme" = Table.TransformColumnTypes(
    #"Replaced null  values in subtheme", 
    {{"subtheme", type text}}
  ), 
```

/* **STEP 26-27:** Made Age column more user-friendly setting it up so that the age is displayed as people are used to see it (e.g., 3+, 12+, 18+ etc...), then, proper data type set for column agerange_min, after the conditional replacement */

```
#"Replaced Values null and ""+"" in agerange" = Table.ReplaceValue(
    #"Changed Type in legotheme", 
    each [agerange_min], 
    each if [agerange_min] <> null then [agerange_min] & "+" else [agerange_min], 
    Replacer.ReplaceValue, 
    {"agerange_min"}
  ), 
  #"Changed Type in agerange_min" = Table.TransformColumnTypes(
    #"Replaced Values null and ""+"" in agerange", 
    {{"agerange_min", type text}}
  ), 
```

/* **STEP 28:** Added an image that display a "No Image Available" when the URL field is empty */

```
#"Added image for no images" = Table.ReplaceValue(
    #"Changed Type in agerange_min", 
    null, 
    "https://upload.wikimedia.org/wikipedia/commons/6/65/No-Image-Placeholder.svg", 
    Replacer.ReplaceValue, 
    {"imageURL"}
  ), 
```

/* **STEP 29:** Renamed most of the columns to make them more user friendly/easier to use later in report dev */

```
#"Renamed Columns" = Table.RenameColumns(
    #"Added image for no images", 
    {
      {"name", "LEGO Set Name"}, 
      {"year", "Release Year"}, 
      {"theme", "LEGO Theme"}, 
      {"subtheme", "LEGO Sub-Theme"}, 
      {"themeGroup", "LEGO Theme Group"}, 
      {"category", "LEGO Category"}, 
      {"pieces", "Number of Pieces"}, 
      {"minifigs", "Number of Mini Figures"}, 
      {"agerange_min", "Min. Age"}, 
      {"US_retailPrice", "Price at Launch ($)"}
    }
  ),
```

/* **STEPS 30-31:** creation of the Sorting Index column, for having sorted the "cards slicer" and the dataset in a chronological-alphabetical order */

/* **A:** the logic is to first sorting by year so older Sets are on top, then, sorting by name so that they are sorted by alphabetical order in each year and finally sorting by Sets ID, so that the older ID is on top and, for instance 2 Sets that are from 1970 with both named Set_Example, and with IDs respectively a1 and a2, will be sorted so that the Set_Example with ID = a1 comes before than the one with ID = a2. This can make easier to identify the editions for Lego Sets with the same name (id = a1 should be the edition 1 in this case */

```
#"Sorted Rows" = Table.Sort(
    #"Renamed Columns", 
    {
      {"Release Year", Order.Ascending}, 
      {"LEGO Set Name", Order.Ascending}, 
      {"set_id", Order.Ascending}
    }
  ),     
```

/* **B:** added the Sorting Index column, based on the sorting of previous step, that, from now on, can be used in visuals an/or to sort the dataset */

```
#"Added Sorting Index" = Table.AddIndexColumn(#"Sorted Rows", "Sorting Index", 1, 1, Int64.Type),
```

/* **STEPS 32-37:** creation of the "Lego Set Edition" column: after identifying the correct sorting order and the potential Editions for Lego Sets with the same name, we need to create a "Lego Set Edition" column, that will ultimately allow to have a "Lego Set Name and Edition" column. This column will be essential as it will be a distinct list of names to be used in the cards slicer. Without this list the column "Lego Set Name" does not allow proper filtering as it is not distinct. Another possible solution would have been to concatenate name and ID, but it is not as elegant as providing the edition number, which is also an interesting piece for the users. The users will have a chronologically sorted slicer with all the Lego Sets names and the editions, for every available set in the database. In order to achieve it, we need to replicate the Rank window function of SQL logic over the "Lego Set Name column", so that 2 sets with the same name would be ranked 1 and 2 (the editions) based on their location within the dataset after the table is sorted by the Sorting Index - ascending */

/* **A:** group the whole table by "LEGO Set Name" so that the output is a column with a distinct list of Lego sets names and another column "UniqueNames" that in each row for each name contains a table with all the other elements of the main dataset and all the rows where a duplicated Lego set name appears. So if for instance we take the Set_Example, in the table in the column "UniqueNames", we would have 2 rows, one with ID a1 and one with ID a2. In these rows there are also all other elements of the Main Table (e.g., Lego Theme, Lego Sub-Theme, etc..) */

```
#"Grouped Rows" = Table.Group(
    #"Added Sorting Index", 
    {"LEGO Set Name"}, 
    {
      {
        "UniqueNames", 
        each _, 
        type table [
          set_id = text, 
          LEGO Set Name = nullable text, 
          Release Year = nullable number, 
          LEGO Theme = text, 
          #"LEGO Sub-Theme" = nullable text, 
          LEGO Theme Group = text, 
          LEGO Category = text, 
          Number of Pieces = nullable number, 
          Number of Mini Figures = nullable number, 
          Min. Age = nullable text, 
          #"Price at Launch ($)" = nullable number, 
          bricksetURL = text, 
          thumbnailURL = text, 
          imageURL = text, 
          Sorting Index = number
        ]
      }
    }
  ),     
```

/* **B:** we need to create another column: the "Column_to_expand" later, in which we rank every dataset name within the virtual tables of the column "UniqueNames". Here is where Rank is used as window function and it is ranking the Lego Set names over the "Lego Set Name" column, here Set_Example with ID = a1 gets "Lego Set Edition" = 1 and Set_Example with ID = a2 gets Lego Set Edition" = 2 */

```
#"Creation of the Lego set Edition" = Table.AddColumn(
    #"Grouped Rows", 
    "Column_to_expand", 
    each Table.AddIndexColumn([UniqueNames], "LEGO Set Edition", 1, 1, Int64.Type)
  ),     
```

/* **C:** let's remove the Lego Set Name and UniqueNames columns as everything we need is now in the "Column to Expand" */

```
#"Removed Columns" = Table.RemoveColumns(
    #"Creation of the Lego set Edition", 
    {"LEGO Set Name", "UniqueNames"}
  ),     
```

/* **D:** let's expand it and we will have now our old table, with added the Lego Set Edition
column */

```
#"Expanded {0}" = Table.ExpandTableColumn(
    #"Removed Columns", 
    "Column_to_expand", 
    {
      "set_id", 
      "LEGO Set Name", 
      "Release Year", 
      "LEGO Theme", 
      "LEGO Sub-Theme", 
      "LEGO Theme Group", 
      "LEGO Category", 
      "Number of Pieces", 
      "Number of Mini Figures", 
      "Min. Age", 
      "Price at Launch ($)", 
      "bricksetURL", 
      "thumbnailURL", 
      "imageURL", 
      "Sorting Index", 
      "LEGO Set Edition"
    }, 
    {
      "set_id", 
      "LEGO Set Name", 
      "Release Year", 
      "LEGO Theme", 
      "LEGO Sub-Theme", 
      "LEGO Theme Group", 
      "LEGO Category", 
      "Number of Pieces", 
      "Number of Mini Figures", 
      "Min. Age", 
      "Price at Launch ($)", 
      "bricksetURL", 
      "thumbnailURL", 
      "imageURL", 
      "Sorting Index", 
      "LEGO Set Edition"
    }
  ),     
```

/* **E:** we need to re-set all the data type for each column as all this work set every data type to "any" */

```
#"Changed Type 2" = Table.TransformColumnTypes(
    #"Expanded {0}", 
    {
      {"set_id", type text}, 
      {"LEGO Set Name", type text}, 
      {"Release Year", Int64.Type}, 
      {"LEGO Theme", type text}, 
      {"LEGO Sub-Theme", type text}, 
      {"LEGO Theme Group", type text}, 
      {"LEGO Category", type text}, 
      {"Number of Pieces", Int64.Type}, 
      {"Number of Mini Figures", Int64.Type}, 
      {"Min. Age", type text}, 
      {"Price at Launch ($)", Currency.Type}, 
      {"bricksetURL", type text}, 
      {"thumbnailURL", type text}, 
      {"imageURL", type text}, 
      {"Sorting Index", Int64.Type}, 
      {"LEGO Set Edition", Int64.Type}
    }
  ),    
```

/* **F:** let's re-sort the table by sorting index - ascending */

```
#"Sorted by sorting index" = Table.Sort(#"Changed Type 2", {{"Sorting Index",Order.Ascending}}),
```

/* **STEP 38:** now we use the new Lego Set Edition column to create the conditional Lego Set and Edition column, where we add to the Lego Set Name the [Ed. n.] for Lego Sets Names that are from the second edition. So, coming back to the Set_Example, we will have one row where there will be in column Lego Set Name and  Edition "Set_Example" and one row where there will be Set_Example - [Ed. 2]" */

```
#"Added Set Name & Edition Col" = Table.AddColumn(
    #"Sorted by sorting index", 
    "LEGO Set Name and Edition", 
    each 
      if [LEGO Set Edition] > 1 then
        [LEGO Set Name] & " [Ed. " & Number.ToText([LEGO Set Edition]) & "]"
      else if [LEGO Set Name] = "Unknown" then
        [LEGO Set Name] & " - " & Number.ToText([LEGO Set Edition])
      else
        [LEGO Set Name]
  ),    
```

/* **STEP 39:** added a column with key words for free search visual */

```
#"Added Free Search Column" = Table.AddColumn(
    #"Added Set Name & Edition Col", 
    "Free Search Column ", 
    each [LEGO Set Name and Edition]
      & ", "
      & [#"LEGO Sub-Theme"]
      & ", "
      & [LEGO Theme]
      & ", "
      & [LEGO Theme Group]
      & ", "
      & [LEGO Category]
      & ", "
      & Number.ToText([Release Year])
  ),   
```

/* **STEP 40:** changed type of the last 2 added columns */

```
#"Changed Type 3" = Table.TransformColumnTypes(
    #"Added Free Search Column", 
    {{"LEGO Set Name and Edition", type text}, {"Free Search Column ", type text}}
  ),
```

/* **STEP 41-42:** added a column with Full Name and Year to offer slicer possibility of filtering both by name and by year of release, Also changed the type to text. */

```
 #"Added Full Name and Year" = Table.AddColumn(
    #"Changed Type 3", 
    "Name_Edition_Year - slicer col", 
    each Number.ToText([Release Year]) & " - " & [LEGO Set Name and Edition]
  ), 
  #"Changed Type 4" = Table.TransformColumnTypes(
    #"Added Full Name and Year", 
    {{"Name_Edition_Year - slicer col", type text}}
  ),    
```

/* **STEP 43:** replaced empty values to null values, in Min. Age column, to have later "blanks" values once the data is loaded  */

```
#"Replaced Value" = Table.ReplaceValue(
    #"Changed Type 4", 
    "", 
    null, 
    Replacer.ReplaceValue, 
    {"Min. Age"}
  ), 
```

/***STEPS 44-72:** Final operations for better User Experience */

```
 #"Capitalized Each Word final UX" = Table.TransformColumns(
    #"Replaced Value", 
    {
      {"LEGO Theme", Text.Proper, type text}, 
      {"LEGO Sub-Theme", Text.Proper, type text}, 
      {"LEGO Theme Group", Text.Proper, type text}, 
      {"LEGO Category", Text.Proper, type text}
    }
  ), 
  #"Replaced Value final UX" = Table.ReplaceValue(
    #"Capitalized Each Word final UX", 
    "'S", 
    "'s", 
    Replacer.ReplaceText, 
    {
      "LEGO Set Name", 
      "LEGO Theme", 
      "LEGO Sub-Theme", 
      "LEGO Theme Group", 
      "LEGO Set Name and Edition"
    }
  ), 
  #"Replaced Value final UX 2" = Table.ReplaceValue(
    #"Replaced Value final UX", 
    " And ", 
    " and ", 
    Replacer.ReplaceText, 
    {
      "LEGO Set Name", 
      "LEGO Theme", 
      "LEGO Sub-Theme", 
      "LEGO Theme Group", 
      "LEGO Set Name and Edition"
    }
  ), 
  #"Replaced Value final UX 3" = Table.ReplaceValue(
    #"Replaced Value final UX 2", 
    " Is ", 
    " is ", 
    Replacer.ReplaceText, 
    {
      "LEGO Set Name", 
      "LEGO Theme", 
      "LEGO Sub-Theme", 
      "LEGO Theme Group", 
      "LEGO Set Name and Edition"
    }
  ), 
  #"Replaced Value final UX 4" = Table.ReplaceValue(
    #"Replaced Value final UX 3", 
    " Of ", 
    " of ", 
    Replacer.ReplaceText, 
    {
      "LEGO Set Name", 
      "LEGO Theme", 
      "LEGO Sub-Theme", 
      "LEGO Theme Group", 
      "LEGO Set Name and Edition"
    }
  ), 
  #"Replaced Value final UX 5" = Table.ReplaceValue(
    #"Replaced Value final UX 4", 
    " With ", 
    " with ", 
    Replacer.ReplaceText, 
    {
      "LEGO Set Name", 
      "LEGO Theme", 
      "LEGO Sub-Theme", 
      "LEGO Theme Group", 
      "LEGO Set Name and Edition"
    }
  ), 
  #"Replaced Value final UX 6" = Table.ReplaceValue(
    #"Replaced Value final UX 5", 
    " On ", 
    " on ", 
    Replacer.ReplaceText, 
    {
      "LEGO Set Name", 
      "LEGO Theme", 
      "LEGO Sub-Theme", 
      "LEGO Theme Group", 
      "LEGO Set Name and Edition"
    }
  ), 
  #"Replaced Value final UX 7" = Table.ReplaceValue(
    #"Replaced Value final UX 6", 
    "Wwf", 
    "WWF", 
    Replacer.ReplaceText, 
    {
      "LEGO Set Name", 
      "LEGO Theme", 
      "LEGO Sub-Theme", 
      "LEGO Theme Group", 
      "LEGO Set Name and Edition"
    }
  ), 
  #"Replaced Value final UX 8" = Table.ReplaceValue(
    #"Replaced Value final UX 7", 
    " The ", 
    " the ", 
    Replacer.ReplaceText, 
    {
      "LEGO Set Name", 
      "LEGO Theme", 
      "LEGO Sub-Theme", 
      "LEGO Theme Group", 
      "LEGO Set Name and Edition"
    }
  ), 
  #"Replaced Value final UX 9" = Table.ReplaceValue(
    #"Replaced Value final UX 8", 
    " In ", 
    " in ", 
    Replacer.ReplaceText, 
    {
      "LEGO Set Name", 
      "LEGO Theme", 
      "LEGO Sub-Theme", 
      "LEGO Theme Group", 
      "LEGO Set Name and Edition"
    }
  ), 
  #"Replaced Value final UX 10" = Table.ReplaceValue(
    #"Replaced Value final UX 9", 
    "Vs.", 
    "vs.", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 11" = Table.ReplaceValue(
    #"Replaced Value final UX 10", 
    "Ii", 
    "II", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 12" = Table.ReplaceValue(
    #"Replaced Value final UX 11", 
    "Iii", 
    "III", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 13" = Table.ReplaceValue(
    #"Replaced Value final UX 12", 
    "IIi", 
    "III", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 14" = Table.ReplaceValue(
    #"Replaced Value final UX 13", 
    "Episode Vi", 
    "Episode VI", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 15" = Table.ReplaceValue(
    #"Replaced Value final UX 14", 
    "Episode Vii", 
    "Episode VII", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 16" = Table.ReplaceValue(
    #"Replaced Value final UX 15", 
    "Episode VIi", 
    "Episode VII", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 17" = Table.ReplaceValue(
    #"Replaced Value final UX 16", 
    "Episode Iv", 
    "Episode IV", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 18" = Table.ReplaceValue(
    #"Replaced Value final UX 17", 
    "Us ", 
    "US ", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 19" = Table.ReplaceValue(
    #"Replaced Value final UX 18", 
    "Uk", 
    "UK", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 20" = Table.ReplaceValue(
    #"Replaced Value final UX 19", 
    " Usa", 
    " USA", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 21" = Table.ReplaceValue(
    #"Replaced Value final UX 20", 
    "Usb", 
    "USB", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 22" = Table.ReplaceValue(
    #"Replaced Value final UX 21", 
    " For ", 
    " for ", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 23" = Table.ReplaceValue(
    #"Replaced Value final UX 22", 
    " Use ", 
    " use ", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 24" = Table.ReplaceValue(
    #"Replaced Value final UX 23", 
    "Housing.", 
    "Housing", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 25" = Table.ReplaceValue(
    #"Replaced Value final UX 24", 
    " Hq", 
    " HQ", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 26" = Table.ReplaceValue(
    #"Replaced Value final UX 25", 
    "-In-", 
    "-in-", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  ), 
  #"Replaced Value final UX 27" = Table.ReplaceValue(
    #"Replaced Value final UX 26", 
    " Vii", 
    " VII", 
    Replacer.ReplaceText, 
    {"LEGO Set Name", "LEGO Sub-Theme", "LEGO Set Name and Edition"}
  )
```

/* **Final Power Query "in" line** */

```
in
#"Replaced Value final UX 27"
```

## **Fourth step - creation of a first working tool prototype (back-end development).**

After the data has been cleaned and transformed, it is now ready to be used for developing the tool in accordance with the list of items and functionalities established in the second step.

For developing the first prototype it has been used Power BI desktop and, all the elements and functionalities of the list have been included.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/041c6cd9-2a2e-4de5-ac71-59f1add6c008.gif)*Working prototype of the LEGO Sets Explorer tool, all the items and functionalities that were planned are here and are working*

## **Fifth step: UX/UI optimization using Figma**

Once the prototype of the tool was available, the goal was to enhance the user experience, providing a clear to use and good looking product. In order to do it, starting from the prototype, it has been used Figma for designing its user interface.

**- Creation of the wireframes using Figma -**

This is when the final design of the tool is actually brought to life. The high fidelity wireframes need to show each element as they ideally would be in the tool that the users will interact with.

As the theme of this tool is LEGO, it has been chosen a LEGO context, but avoiding to overwhelm the user with too many colours or icons.

The style is raw and rather simple in order to compensate the colourful images and the buttons are more text-based, rather than icon-based, and they recall the LEGO colours. The colour mining is: green for going somewhere and red for clearing something.

The only icons used are those representing the LEGO pieces, the mini figures, the minimum advised age and the price at launch. Even in this case, the style of the icons is on purpose minimal.

The tool functionalities need to be website-like, therefore for the filter pane in the Search Results Page it has been chosen to have it pop-up.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/0ffdda30-a405-450d-be59-d96715b57de0.png)*Landing Page, high-fidelity wireframe.*

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/b6db388c-e1fc-4121-a899-3ba460715c00.png)*Landing Page, high-fidelity wireframe, with "more filters" option on.*

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/d266d405-f6b7-46a7-abfa-7400791e5ce3.png)*Search Results Page, high-fidelity wireframe.*

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/9b381104-9a47-442c-a14f-3b5c91df5b33.png)*Search Results Page, high-fidelity wireframe, with "more filters" option on.*

## **Sixth step: incorporation of the UX/UI directions into Power BI** **(front-end development)**

Starting from the designed user interface in Figma, the challenge was to "program" Power BI so that everything, or almost everything, that was designed in Figma was translated into the LEGO Sets Explorer tool, guaranteeing the users an optimal experience.

To note, starting from the initial design, some elements have been created a bit differently (e.g. filters and filter pane, location of some items in the canvas, etc...) this due to impossibility to replicate the original design with a 100% match, or simply because during the development it turned out they would offer better user experience by slightly changing the original plan.

Aleksandra and Stefano collaborated closely and engaged in brainstorming to accomplish this part.

**Here the most interesting techniques used and challenges that were faced during Power BI development:**

**- The main challenge: new card slicer bug -**

The main challenge during the tool's development has been a "bug" that the new card slicer has, once the report is published on Power BI Service.

Basically, when a selection is made in the Landing Page or through the filter pane, and then the user goes back to the Results Page, although the card slicer is set to force the one single selection, it retains the previous item in its selection and does not select the first item that appears now among its cards.

This caused the main visualization on the right side of the Results Page to be blank. At first this has been handled with a message to select one item, but, it was not the best way as it was ruining user experience, and the original design was not intended to work like that.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/3b99b84c-1faa-4771-ab6b-6056969185f6.png)*Example of the cards slicer bug,  where nothing is selected despite the "force selection" being active on the card slicer, and how it was originally handled.* 

Waiting for Microsoft Team to fix this, the only viable solution so far, is to create a data model, where the lego_sets table is duplicated (using "reference" in Power Query so that the ETL runs once in the original table) several times to create its duplication and all the lookup tables that represent all the filtering options of the tool.

In this data model, the table (cards slicer lookup) with the field for the card slicer (lego_sets[LEGO Set Name and Edition]) is connected only to the original data table. This is crucial, as in this way there are now 2 different filter contexts: one with the card slicer and one without.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/65e13cb7-124f-457e-a38d-2ac9c9f49cf0.png)*Data model as workaround for the card slicer bug*

Keeping in mind the model, the below measure for each item to display in the visuals has been made - Here this below measure is only for the number of pieces, but the logic is the same for all the other measures.

```
Number Pieces = 
Var ValueSelected_firstTable =
SELECTEDVALUE(lego_sets[LEGO Set Name and Edition])

VAR code_if_1 =
CALCULATE(
    SELECTEDVALUE(lego_sets2[Number of Pieces]),
    lego_sets2[Sorting Index] = MIN(lego_sets2[Sorting Index])
)

VAR code_if_0 =
SELECTEDVALUE(lego_sets[Number of Pieces])

Var Verification =
IF( ISBLANK(ValueSelected_firstTable) = TRUE(), 
1, 0)

Return
IF(
    Verification = 1, 
    code_if_1, code_if_0
)
```

This measure starts checking whether the card slicer's forced selection is on an item that is actually within the filter context of all the other slicers. If the check returns blank, it means the card slicer is still forcing the selection on an item that is now out of the filter context, and thus, the bug is happening.

Therefore, two different codes for the desired output are created: one in case of bug, and one in case of no bug.

The one in case of no bug is straight forward: a selected value, in this case of LEGO set pieces, for the LEGO set that is selected by the card slicer.

The one in case of bug instead, needs to return the selected value, for the first item displayed by the card visual, that is however not selected by the card visual. This is when the second data table and the filter context without card slicer kick in.  This code, returns the selected value, in this case of LEGO set pieces, for the LEGO set with the smaller sorting index, from the duplicated lego_sets data table - the LEGO set that is on top of the card slicer now -

For all the visuals, this logic uses either one or the other code, depending on whether the bug is or isn't happening.

In the image below it is shown how it looks like when the bug is happening, and the card slicer is not selecting anything that is now available with the current filter context, but the workaround allows the users to have a decent experience anyways, displaying all the details of the first item on top of the card slicer.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/bceb97d0-82f4-4015-9ed2-937fa8cd6a97.png)*Example of the cards slicer bug,  where the workaround kicks in, displaying the first item of the card slicer as it was selected.*

**- The most interesting solution: HTML button that is exactly the same as those of Power BI and HTML Image Box -**

HTML has been extensively used in the Results Page to create the main visualizations of the selected LEGO sets.

Particularly, HTML came handy to create the text and the "Visit Brickset Website " button, so that the button
could always be "attached" to the text and when the text is longer or shorter the button goes up and down too.

The challenge was to recreate a button that is exactly the same as those offered by Power BI, but after some work the code for it has been successfully created. Code available below.

Another interesting element created in HTML is the image box with the image inside. The reason why it hasn't been used a native new card, for instance, is that it has a big limitation when the image comes from a link: no possibility to select "fit". Therefore this image box acts as a custom visual for image display. Also for this image box the code is available below.

*HTML Button Code - button that is exactly the same as those of Power BI*

```
Button HTML = 
"<!DOCTYPE html>
<html>
<head>
    <style>
        .myButton {
            background-color: white; /* White */
            border: 2pt solid #009F28;
            color: #009F28;
            padding: 5px 9px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 12px;
            margin: 4px 2px;
            cursor: pointer;
            box-shadow: -3px 4px 1px rgba(0, 0, 0, 0.85);
            width: 170px; /* Specify the width */
            height: 24px; /* Specify the height */
            line-height: 10px; /* Add this line */
        }
        .myButton:hover {
            border-width: 1pt;
            box-shadow: none;
            font-size: 13px;
            box-shadow: none;
            line-height: 12px;
        }
    </style>
</head>
<body>
<a href="""&[Brickset URL]&""" class=""myButton""><b>Visit Brickset Website</b></a>
</body>
</html>"
```

*HTML Image Box Code*

```
HTML Image Box = 
"<!DOCTYPE html>
<html>
<head>
    <style>
        .box {
            width: 402px;
            height: 215px;
            background-color: white;
            border: 1pt solid #B3B3B3;
            border-radius: 2pt;
            padding: 12px 12px 12px 12px;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }
        .box img {
            max-width: 100%;
            max-height: 100%;
            width: auto;
            height: auto;
            object-fit: contain;
        }
    </style>
</head>
<body>
<div class='box'>
    <img src='"&[Image URL]&"' alt='Image'>
</div>
</body>
</html>
"
```

**- No results handling -**

To handle an empty user's selection, guaranteeing an elegant solution, it has been set a conditional page navigation, from the landing page and from the filter pane's page, with the DAX codes below.

```
// Conditional Navigation Code from Landing Page to either Results Page (Page 2) or the pop-up page
IF(
    COUNTA(
    lego_sets[set_id]
    ) = 0, "Page 4", "Page 2")

// Conditional Navigation Code from Filters Pane Page to either Results Page (Page 2) or the pop-up page
IF(
    COUNTA(lego_sets[set_id]
    ) = 0, "Page 5", "Page 2")
```

These codes send the user to another page where a pop- up alerts him/her that nothing has been selected and that he/she needs to go back for selection.

Closing the pop-up the user is sent to the page where he/she clicked "Go".

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/8a8ce288-4c4e-4aa1-991d-aef3a97dcb2b.png)![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/1cc53e92-d784-4faf-97ca-458ab7096006.png)*Example of what happens when someone makes a zero results selection and click on "Go", from the Landing Page, and how this is handled.*

**- Other interesting topics -**

The pop-up effects used throughout the tool have been realised using a shape with a certain level of transparency, and activating the page navigation to the shape, so that when clicked, it closes the pop-up (as it happens in a website).

In order to create the shadow below the years filter options and below search engine box it has been used a blank button with shadow on, and it has been located a layer behind the slicers, this way the effect is consistent between buttons and slicers.

Finally, for the pages background, it has been exported a svg image of the background without the items that need to be interactive,  from Figma, and it has been uploaded as canvas background in Power BI Desktop.

## **Seventh and last step: final overall testing and changes and publishing of the LEGO Sets Explore Tool.**

After the tool was programmed as intended in Power BI Desktop, it was extensively tested in Power BI Service, that resulted crucial to find some bugs and inconsistencies to correct.

There has been several back-and-forth between the two environments and finally the tool was completed.

Before the final publishing however, Aleksandra and Stefano let few days pass and re-looked at the design to make some final adjustments vs. the first design.

Once everything was considered final, the tool was uploaded one last time in the Power BI Service environment and was published to web, so that everyone could use it.
