# The Challenge 

"...your task is to **create an impactful visual** for an article on parental leave policies across the business world"

[Full Challenge Link](https://mavenanalytics.io/challenges/maven-family-leave-challenge)

# Maven Family Leave Challenge

<img width="1068" height="599" alt="image" src="https://github.com/user-attachments/assets/c12be498-703f-42ee-9e36-fa57d93cfa4e" />
<br><br>

Parental Leave Policies across the USA's Business World: impactful visual and related article for the Women's History Month in the United States

[📊 Live Report](https://app.powerbi.com/view?r=eyJrIjoiNTI5Mzc2ODctZjA2ZC00NGFmLWJiZTctZjRjNGE0MmFlYzVjIiwidCI6IjhhNDk1ZGQwLThkNDEtNDcyYy1iMTljLTFhMzQzZjYxYmFhMSIsImMiOjl9)

# The idea

For this project I went a bit beyond the task. I decided to team-work with [Aleksandra Zaczek](https://www.linkedin.com/in/aleksandra-%C5%BCaczek-1875b4108/), an aspiring UX/UI designer, and create a porotype of an article on the Parental Leave Policies in the USA on a hypothetical news website. 

It is worthy to mention that for Aleksandra this is her first published project and first step into creating her own portfolio as well. Her choice of style is rather minimal and clear since she believes it is good not only for accessibility but also it is a good practise for a beginner.

# Before starting

Stefano and Aleksandra had some ideas on what they wanted to create: a visual with the key highlights on parental leave policies and a full article within a prototype of an online journal.

Starting from this, the work was planned ahead as follows:

Identification of the potential readers of the article and targeting it to them. The article is related to Women's History Month in the United States, so the chosen target audience was people expecting a baby.

Then, the work organization was planned: journal prototype in Figma, data analysis in Excel, production of the visuals in Excel and Power Point.

Finally, the plan included to make the whole work "alive" and shareable in Power BI.

# The actual work

**First step**

**Stefano:** data clean-up, data analysis, creation of the story for the article, writing of the article itself, based on what emerged from the data analysis, and, creation of the visuals within the article.

In this phase Excel, Power Point and Word have been the "go-to" tools. Their combination allowed flexibility in data analysis but also in quickly creating several drafts for each visual. Word was quite useful to write the article and then see where each visual could be located within the text.

**Aleksandra:** preparing the design of the hypothetical Journal website, its logo, branding, style layout and interface design (wireframes) in Figma.

**Second step**

In this phase, Stefano's and Aleksandra's works needed to be joined: this was done in Figma where Aleksandra created the prototype of the Journal website with actual text and graphs. 

**Third step**

Creation of the impactful visual: Stefano and Aleksandra worked together on creating a one-page teaser for the article that included only the key findings. The visual is aimed to push a potential reader to click the button "read the full article". This visual was done in Figma, Aleksandra made sure the whole style was consistent with the article's and Stefano directed the analytical content to include.

**Fourth and final step**

The Figma prototype was used by Stefano to create the more interactive version in Power BI where the reader sees first the impactful visual and, can decide to go further and read the whole article.

**Thanks to the functionality "publish to web" the article can now be shared through a link or be embedded in other websites (using iframe) as a real article would. The impactful first visual should now work as "click-bait" in triggering potential readers to go further and read.**

# About the dataset

The dataset contains a **csv table** with 1,601 records, one for each company.

Each record contains the company's name & industry, as well as crowdsourced information on the paid & unpaid weeks off they offer as part of both their maternity & paternity leave policies (when available)

# Data clean-up and manipulation

Before using the Data Table it was necessary to perform data cleaning and manipulation, in order to make the whole dataset ready to be visualized. Particularly, what was done can be summarized as follows:

1 - Added column "Industry (New)", separating the industry before ":" from the sub-industry after ":"

Some entries did not have the sub-industry (e.g. Aerospace) and in this case the whole industry was kept in the new "Industry" column.

2 - Needed to "trim" all the industries as some of them were having an initial space before the text.

**In order to achieve points 1 and 2 in one step, this formula has been used:**

**=IFERROR(TRIM(LEFT(H2, FIND(":",H2)-1)),H2)**

<img width="1451" height="406" alt="image" src="https://github.com/user-attachments/assets/1541813b-f569-4568-8cb9-27f79b5d5e16" />

3 - Removed one of the 2 duplicate entries: "Collins Aerospace" company

<img width="4036" height="406" alt="image" src="https://github.com/user-attachments/assets/3e590490-ed5f-4f32-8a6f-c565dec97bf5" />

**Methodology of removal:** as stated in the data dictionary, this is user-reported data, where users report conflicting information, consensus numbers (if any) or the median are shown. In this specific case, the median of 2 numbers is the number that is halfway (their average).

Therefore this is what has been left in the dataset for the Collins Aerospace company:

<img width="1722" height="92" alt="image" src="https://github.com/user-attachments/assets/116580de-0d72-4899-b207-cb74b48c5c9e" />

4 - To avoid problems and to better handle the analysis it has been substituted the "N/A" with "blank" in the 4 columns (Paid Maternity Leave, Unpaid Maternity Leave, Paid Paternity Leave and Unpaid Paternity Leave) that provide the number of weeks of paid/unpaid leave granted by each company in the dataset.

This way, in each column only values for those companies where information has been reported remains. 
