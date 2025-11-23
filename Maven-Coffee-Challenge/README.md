## **Challenge Objective**

For the Maven Coffee Challenge, you'll play the role of an Analytics Consultant hired by a group of investors looking to break into the US coffee market. They would like to leverage insights from "The Great American Coffee Taste Test", but lack the analytical skills to do so. That's where you come in!

You've been asked to share an explanatory report providing a data-driven strategy for opening their first coffee shop. The investors expressed interest in the following areas, but are open to any additional insights and recommendations you can provide:

**Target audience:** What type of customer should we target, and what are their preferences?

**Product offering:** What types of coffee beans and drinks should we offer?

[Full Challenge Link](https://mavenanalytics.io/challenges/maven-coffee-challenge)

**Pricing strategy:** How can we align prices with customer value perception?

## Maven Coffee Challenge [🏆 Winner]

***How to successfully break into US coffee shops market?***

https://github.com/user-attachments/assets/5b2f1e10-7781-467d-8878-988e18b03302 <br>

Data Driven Strategy for breaking into US Coffee Shops Market

[📊 Live Report](https://app.powerbi.com/view?r=eyJrIjoiOGYyMjU2YzgtNjcwMi00M2Y0LTg5YTUtMGEyYzUzYzQ5N2VlIiwidCI6IjhhNDk1ZGQwLThkNDEtNDcyYy1iMTljLTFhMzQzZjYxYmFhMSIsImMiOjl9)

# **Approach to the Challenge**

I tackled this challenge dividing the workflow in two distinct parts: (a) data exploration and deep analysis in Fabric, to create the full report, (b) creation of a simplified and visually appealing version of the report using Power BI.

(a.1) I began by importing the data into a “Landing” Lakehouse, where I conducted initial explorations to identify target groups for the new coffee shop.

(a.2) Then, I transformed the survey data using Dataflow Gen-2 and loaded it into a “Final-Model” Lakehouse. More details on data transformations in its dedicated section.

(a.3) In a Spark Notebook, I made the explanatory analysis and visualizations using Spark SQL and PySpark and, I started to write the report draft, always within the Notebook, using the Markdown options. I first made a tabulation work to aggregate the survey results into summary tables using Spark SQL. After these tables were created, I converted them into pySpark data frames and I visualized them leveraging Python libraries. I finally used the Markdowns to comment the results and to provide recommendations, starting to basically create the final report draft.

Link to the Notebook with all the SQL and Python codes for the visualizations  ->  [here](https://github.com/SteCiu01/Power-BI-Reports/blob/main/Maven-Coffee-Challenge/Maven%20Coffee%20Challenge%20-%20Fabric_Notebook.ipynb)

(b) The second part of the challenge involved summarizing the key insights and recommendations, in a visually appealing manner. To achieve this, I utilized Power BI to create a simplified and visually appealing version of the report (particularly focusing on the recommended actions for the stakeholders).

By following this approach, I ensured that stakeholders received both a detailed analysis and a concise summary of the findings and the advised actions to take.

# **Data Transformation**

To recap, the data was ingested into the Maven Coffee Challenge Landing Lakehouse, where the target groups of the analysis were defined.

Following the definition of the target audiences, a data transformation was performed. The objective was to convert the raw survey data into a format that facilitated analysis and insight extraction.

In order to perform it, it was decided to use a Transformation Dataflow Gen-2, so that the transformed and ready for analysis data was stored in a second Lakehouse called Maven Coffee Challenge Model.

The transformation itself occurred as follows within the dataflow:

**Step 1: dim_questions**

Imported the questions table from the Maven Coffee Challenge Landing Lakehouse into the transformation dataflow.  This table was just trimmed with Power Query and made ready to be used in the next steps. Its final name is now: *dim_questions_transformed*

**Step 2: facts_questionnaire_results - Power Query Work**

Imported the questionnaire results table from the Maven Coffee Challenge Landing Lakehouse into the transformation dataflow.

At first, some general transformations have been applied to obtain the *facts_questionnaire_results_PQWork_1_General_ETL.*

M-Code [here](https://github.com/SteCiu01/Power-BI-Reports/blob/main/Maven-Coffee-Challenge/facts_questionnaire_results_PQWork_1_General_ETL_.txt)

After this initial operations, this table has been referenced 6 times in order to work separately on each question type (multiple choice, single choice, Y/N, etc...).

Multiple Choices Questions

First it was necessary to isolate all those questions where the respondents did not select any multiple answers' option that however were displaying "false" for all the multiple options instead of blank.

On this purpose the *facts_questionnaire_results_PQWork_MultiAnswer_noResponse* table was created

M-Code [here](https://github.com/SteCiu01/Power-BI-Reports/blob/main/Maven-Coffee-Challenge/facts_questionnaire_results_PQWork_MultiAnswer_noResponse.txt)

After that, in a second referenced table, where the *facts_questionnaire_results_PQWork_MultiAnswer_noResponse* table was merged, the multiple choice questions were made "ready" for analysis.

The resulting table was called *facts_questionnaire_results_PQWork_MultiAnswer.*

M-Code [here](https://github.com/SteCiu01/Power-BI-Reports/blob/main/Maven-Coffee-Challenge/facts_questionnaire_results_PQWork_MultiAnswer.txt)

Single Choice Questions, Attributes, Y/N Questions and Verbatims

For all these question types , specific transformations were applied in separate tables as follows.

M-Code for *facts_questionnaire_results_PQWork_SingleSelect* table [here](https://github.com/SteCiu01/Power-BI-Reports/blob/main/Maven-Coffee-Challenge/facts_questionnaire_results_PQWork_SingleSelect.txt)

M-Code for *facts_questionnaire_results_PQWork_Attributes* table [here](https://github.com/SteCiu01/Power-BI-Reports/blob/main/Maven-Coffee-Challenge/facts_questionnaire_results_PQWork_Attributes.txt)

M-Code for *facts_questionnaire_results_PQWork_YN_AdditionalQs*  table [here](https://github.com/SteCiu01/Power-BI-Reports/blob/main/Maven-Coffee-Challenge/facts_questionnaire_results_PQWork_YN_AdditionalQs.txt)

M-Code for *facts_questionnaire_results_PQWork_Verbatims* table [here](https://github.com/SteCiu01/Power-BI-Reports/blob/main/Maven-Coffee-Challenge/facts_questionnaire_results_PQWork_Verbatims.txt)

The overall goal of all these transformations was to move from a table that had one row per respondent and each column per question, or parts of the same question (for multiple choice questions), to a table that has several rows for each question to each respondent, each row has one possible answer (answer choice) for that question, and the actual answer in the form of 1/0. Where 1 is an option selected by the respondent (true) and 0 is an option not selected by the respondent (false).

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/831b2bf6-28c1-4e78-a771-b61271121fa6.png)*Initial facts_questionnaire_results_landing before any transformation*

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/ef81a8f5-9531-43af-a604-f824dac7bd1b.png)*Final facts_questionnaire_results_PQWork_SingleSelect table after all the transformations*

**Step 3: facts_questionnaire_results - Ready to use**

The third step in the transformation dataflow was to append together all the tables that contained the answers as 0/1:

```
Table.Combine({facts_questionnaire_results_PQWork_YN_AdditionalQs, facts_questionnaire_results_PQWork_MultiAnswer, facts_questionnaire_results_PQWork_SingleSelect, facts_questionnaire_results_PQWork_Attributes})
```

This way it was provided for data analysis a single table with well structured data, in a format that is easy and efficient to use for analysis.

The verbatims table was left as stand alone table as it requires different type of analysis (qualitative) vs. quantitative data.

For reference, here below the transformation dataflow structure. Only the *Facts_Questionnaire_FullTable_NoVerbat* and the *facts_questionnaire_results_Verbatims* tables were assigned the Maven Coffee Challenge Model Lakehouse.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/4c0ba8ef-f7c6-47bc-8d4f-632b8326d067.png)*Transformation dataflow structure*

# **Full Report: Data Driven Strategy for breaking into US Coffee Shops Market**

## **Target Audience Definition**

**Stakeholders question: "what type of customer should we target?"**

The goal is the successful establishment of a coffee shop. For this to be achieved, offerings and pricing must be tailored to the preferences of potential customers.

But who are these potential customers?

Before proceeding, it’s needed to take a step back to fully comprehend the nature of this business.

**What is a coffee shop?** *“A coffeehouse, coffee shop, or café is an establishment that primarily serves various types of coffee, espresso, latte, and cappuccino. Some coffeehouses may serve cold drinks, such as iced coffee and iced tea, as well as other non-caffeinated beverages. A coffeehouse may also serve food, such as light snacks, sandwiches, muffins, fruit, or pastries.”* - Source: [Wikipedia](https://en.wikipedia.org/wiki/Coffeehouse#:~:text=A%20coffeehouse%2C%20coffee%20shop%2C%20or,as%20other%20non%2Dcaffeinated%20beverages)

Given this definition, potential customers can be all the people that have interest in a coffee shop offering. Therefore, leveraging the information available in the survey, two potential target groups can be defined as:

**Daily Out-of-Home Coffee Drinkers:** individuals who typically drink at least one coffee per day, and that claim to drink coffees at a café or at the office or on the go, among the places where they typically drink coffees.

**Why this group?** This group forms a solid customer base because these individuals have no problem getting a coffee outside of their home and can be easily targeted: those that drink coffees in a café are natural customers, those that drink it on the go can be targeted by establishing an on-the-go/take away format in the café, and those at the office can be targeted with online delivery services to bring coffees and snacks to workplaces surrounding the coffee shop. These are all potential customers of a newly opened coffee shop.

**Daily Cafe Coffee Drinkers:** individuals who typically drink at least one coffee per day, and claim to drink coffees at cafes, among the places where they typically drink coffees.

**Why this group?** This group is the natural target group of the coffee shop business as these individuals always include cafes among the places where they typically get a coffee.

**An important consideration:** it is crucial to create target groups that, on the one hand, align with the ‘ideal customer’ persona, but on the other, are large enough and don't exclude any potential customer.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/e71ebe01-348b-406c-b209-7c5baba4cf6a.png)*Target Groups incidence analysis*

The analysis indicates that ‘Daily Out-of-Home Coffee Drinkers’ make up approximately 56% of the surveyed population, while ‘Daily Cafe Coffee Drinkers’ represent around 25%. These proportions validate the chosen target groups as they have sufficient base size.

Furthermore, no additional restrictions are imposed when identifying target customers. It is preferred to maintain broader target groups, as it’s more realistic in the current scenario. This approach ensures that no potential customers are overlooked.

**The next analysis will focus on determining optimal offering and pricing for the new coffee shop to:**

- **ensure satisfaction of the ‘Daily Cafe Coffee Drinkers’:** this group forms the core customer base, and their satisfaction is critical to success.

- **attract the wider ‘Daily Out-of-Home Coffee Drinkers’:** engaging this larger customer segment could significantly expand the customer base.

## **Target Audience Preferences**

**Stakeholders’ questions: "what are their (Target Audiences) preferences?" - "What types of coffee beans and drinks should we offer?**

At first, it has been investigating **why** the Daily Out-of-Home Coffee Drinkers and the Daily Cafe Coffee Drinkers **drink coffee**.

Then it has been proceeded to understand **what coffee drinks** they prefer and **how** **they drink their coffees**.

Finally, a deep dive into the type of coffee beans has been conducted to have clear **what coffee beans** the Coffee Drinkers wish to have for their coffee drinks.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/08687e5f-3579-45e5-886d-c4304bcb1aaf.png)*Reasons for drinking coffee*

**WHY THEY DRINK COFFEE**

Taste, ritual and need of caffeine are the main reasons for coffee consumption among both target groups.

Particularly, taste plays a crucial role as almost the entirety of the surveyed people points this out as reason for drinking coffee.

The first important insight here for opening a coffee shop in the US market would be to set as primary goal to offer coffee drinks that Coffee Drinkers perceive having a good taste, both in terms of good coffee itself (type of coffee grains that are offered for the drinks in the menu) and in terms of coffee drinks (variety of drinks offered in the menu).

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/89f6a712-4656-4c5b-a63e-1ecf91f2e1c4.png)*Coffee drinks, Pareto Analysis*

**WHAT COFFEE DRINKS**

Pourover and Latte are the preferred coffee drinks among both target groups.

By including other 6 specific SKUs on the menu - Regular Drip Coffee, Cortado, Espresso, Cappuccino, Americano, and Iced Coffee - for a total of 8 SKUs, it is possible to cater to over 90% of the coffee preferences of both Daily Out-of-Home Coffee Drinkers and Daily Cafe Coffee Drinkers.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/0cf3ac15-23ca-45de-986e-b40ef06c5856.png)![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/b841bfde-73b3-4cd7-8eaf-cfd1aba40970.png)![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/b2f7e772-1ed3-45af-9ae4-7810f1c6b022.png)*Coffee additives analysis*

**HOW THEY DRINK THEIR COFFEES**

Most coffee drinkers (especially the Daily Cafe Coffee Drinkers) prefer their coffee black.

The most common additive is milk or a dairy product, particularly whole milk, half-and-half, and oat milk as vegan substitute to dairy. These should always be in the menu.

Also, consider having granulated sugar, brown sugar, raw sugar turbinado, honey and artificial sweeteners available.

Flavour syrups are the least used additives, they could be included in the offer, but aren't a must to have, however due to some survey technical issues there isn't additional information on what flavouring respondents would add.

**WHAT COFFEE BEANS**

Following the understanding of both Daily Cafe Coffee Drinkers and Daily Out-of-Home Coffee Drinkers in terms of motivations for drinking coffee, and their favourite drinks and additives, let’s delve into **their preferences in terms of coffee itself**, including **the types of coffee, coffee strength and roasting level**.

Respondents were made to try 4 different types of coffee, and, among several after-use questions, they were asked to openly write down few notes on each variety.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/8f5fea70-5f04-43a0-af03-b1fc881de132.png)![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/026d9117-52cb-4f72-86af-ceb689dc3f7c.png)![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/3205bf47-2271-4d74-8654-a4eeb7d5d5e0.png)![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/1984b646-8bcc-4465-befa-3b0a22311361.png)*WordClouds based on the verbatims for each coffee variety the respondents tried*

What both Daily Out-of-Home Coffee Drinkers and Daily Cafe Coffee Drinkers highlighted in their comments can be summarized as follows:

- Coffee A: fruity coffee, with some caramel and/or apple taste, that is somehow sweet.
- Coffee B: nutty and chocolatey coffee that is somehow bitter.
- Coffee C: earthy and chocolatey coffee that is overall balanced.
- Coffee D: fruity coffee with some blueberry taste that somehow fermented/sour.

After having cleared the distinctions between the various types of coffee sampled by the participants of the survey, the analysis needed to comprehend their preferred blends. The objective is to provide coffee beans in the coffee shop that align with the preferences of the target demographic.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/f839fb81-2545-409a-8d62-b8c5ce016008.png)*Favourite coffee beans - direct prompt*

When respondents are directly prompted to choose their favourite variety, coffee D is the most praised, followed by coffee A (the two fruity coffees). This is a particularly strong tendency among Daily Cafe Coffee Drinkers.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/78b63141-2415-483e-b0c3-e982a6fe6aa0.png)*Personal preference for different coffee types - indirect prompt*

When respondents are asked to rate their personal preference for each coffee separately, on a scale from 1 to 5, the median answer for coffees A and C is 4 while is 3 for coffees B and C.

This highlights how both fruity coffee beans gather indeed higher appreciation, although coffee D is skewed towards even higher appreciation than coffee A, with respondents that are more inclined to give the highest appreciation to coffee D vs. coffee A.

This gives the indication that it should be considered to have in the coffee shop a fruity variety, and possibly, one that has a slight blueberry retro-taste and is somehow sour, similarly to coffee D.

Next it was important to understand which kind of coffee varieties coffee drinkers claim to like overall, when specifically prompted, to have in the coffee shop a broader offer that goes behind fruity coffees.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/62ff5b8e-23db-44ce-9aeb-5744be48e2d8.png)*Coffee varieties, Pareto Analysis*

More than 60% of both Daily Out-of-Home Coffee Drinkers and Daily Cafe Coffee Drinkers claim they liked Fruity (that also in this case emerges to be on top of the list), Chocolatey, Full-bodied, and Bright types of coffees.

Including these 4 coffee SKUs in the coffee shop offer should be a must.

On top of that, depending on the target group, there are other types of coffees that, if included, would cover a wider range of Coffee Drinkers’ preferences.

As the coffee shop is just opening, and the ‘Daily Cafe Coffee Drinkers’ form the core customer base, their satisfaction is critical to success. Including Juicy, Nutty, Floral, and Sweet coffee beans in the menu would cover more than 91% of their preferences. If there are resources to do so, including Caramelized coffee would guarantee more than 95% of coverage of coffee tastes for both targeted audiences.

Finally, let's delve into coffee strength and roasting level of the coffee beans.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/1a66d3c9-8616-43f1-834c-58bb543da0f2.png)![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/168069bc-bb6f-4f4e-89c5-b06b99683ab4.png)*Coffee strength and roasting level analysis*

Most of of the respondents belonging to both target audiences prefer a medium-strong coffee with light-medium roasted coffee grains, therefore it is advised to prioritize coffee grains with these features.

If resources are available, it is also recommended to include in the menu very strong coffee and dark roasted grains options, as these types are appreciated by around 7%-11% of respondents, across target groups.

**To conclude this section on Target Audience Preferences**, in terms of coffee drinks and additives, coffee beans type, strength and roasting, here below a **summarization matrix**, as guidance for the coffee shop opening.

## ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/921c33c2-a666-4994-8d84-77282d7fc07c.png)**Pricing Strategy** 

**Stakeholders question: "How can we align prices with customer value perception?"**

To provide the stakeholders with actionable insights on prices it has decided to use the survey question *"What is the most you'd ever be willing to pay for a cup of coffee? Single-Choice"* as it represents what respondents, in their ideal world, would consider as their maximum price for a cup of coffee.

On top of that, respondents have their favourite coffee drinks, and here it was done an assumption: if a respondent has a favourite coffee drink (e.g., espresso), it is likely that respondent would consider his/her maximum price to pay thinking about his favourite coffee drink (espresso).

This assumption was made to split the answers of the willingness to pay question by favourite drinks, to provide a somewhat more accurate price direction for each of the 8 coffee drinks that were labelled as "must have" on the menu.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/547e59f5-035a-45dc-940d-6f188cc000b4.png)![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/3445a6d2-a3d9-4a69-bb11-1e4390fff732.png)![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/4af184db-a5af-4f83-9815-76dc71198d00.png)*Willingness to pay distribution, across price ranges, and favourite types of coffee drink*

The distribution of willingness to pay across price ranges reveals that most respondents would be willing to pay a maximum of $6 to $15 for their favourite cup of coffee. Interestingly, there’s a spike in willingness to pay more than $20, particularly for Espresso, Pourover, and Cortado, and to some extent, for Iced Coffee and Cappuccino. This tendency is generally stronger among the Daily Cafe Coffee Drinkers.

It is possible to hypothesize that some respondents might consider the $20+ price range as a combination of a coffee cup and some light food typically sold in a coffee shop. However, this hypothesis can’t be validated with the available survey questions.

Given that more than 20% of respondents across target groups seem willing to spend $20+, it’s recommended to further investigate this coffee-light food combination hypothesis. In conjunction with validating these hypotheses, stakeholders could also investigate potential food items to include in the menu.

A potential validation method could be to conduct another survey, maintaining the same demographic balance as the current survey. The survey could ask respondents if they typically consume food with their coffee. If the response is affirmative, further investigation into the type of food could be conducted (a single-choice question with various options, plus an ‘other’ option, could suffice). Additionally, the survey could inquire about the price respondents are willing to pay for a combination of coffee and food.

> ![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/180964d5-b7fa-4743-a339-e7e8a070a300.png)![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/81a5bf4d-4a4a-4320-8468-a1d2fec64f80.png)![undefined](https://mavenanalyticsio-upload-bucket-prod.s3.us-west-2.amazonaws.com/65253099/projects/cfefe670-e05a-489d-a346-5abb26bcb27e.png)*Decreasing curves of willingness to pay across favourite types of coffee drink*

In investigating the optimal price for each coffee drink, it was conducted an additional analysis: it was created the curve of willingness to pay more than a specified price range, across coffee drinks preferences.

The analysis shows that when considering a price range of $4-$6 for a cup of coffee, from 85% to 97% of respondents, across all the favourite drinks, would be willing to pay more than that $4-$6 specified price range.

However, when we move to the next price range of $6-$8, the willingness to pay more than that drops significantly to from 64% to 85%, depending on the favourite drink.

The drop is even sharper beyond $8 per drink. Therefore, it is advisable to keep the price range from $4 to $8 per coffee drink. Looking at the individual coffee drinks preferences and their decreasing curves of willingness to pay, we can summarize the prices as follows:

- Pourover: $6-$8 per cup
- Latte: $4-$6 per cup
- Cortado: $6-$8 per cup
- Espresso: $6-$8 per cup
- Cappuccino: $4-$6 per cup
- Regular drip coffee: $4-$6 per cup
- Americano: $4-$6 per cup
- Iced coffee: $4-$6 per cup

## **Final Recommendations**

**Advised target audience**

- Daily Cafe Coffee Drinkers: individuals who typically drink at least one coffee per day, and that claim to drink coffees at cafes, among the places where they typically drink coffees - This is the natural target group of a newly opened coffee shop.
- Daily Out-of-Home Coffee Drinkers: individuals who typically drink at least one coffee per day, and that claim to drink coffees at a café or at the office or on the go, among the places where they typically drink coffees - This target group offers opportunities to expand the base of potential customers, targeting those that drink coffees on-the-go or at the office with ad-hoc strategies (offering take away and/or coffee and food delivery to offices).

Given that both target groups share similar preferences and a willingness to pay for a cup of coffee, the recommended items for the initial menu have been carefully curated to cater to the entire audience.

**Advised Initial Menu Prioritization**

**Coffee Drinks and Price**

- Pourover: $6-$8 per cup
- Latte: $4-$6 per cup
- Cortado: $6-$8 per cup
- Espresso: $6-$8 per cup
- Cappuccino: $4-$6 per cup
- Regular drip coffee: $4-$6 per cup
- Americano: $4-$6 per cup
- Iced coffee: $4-$6 per cup

**Coffee Additives - Diary**

- Whole milk
- Half and half
- Oat milk

**Coffee Additives - Sugar**

- Granulated sugar
- Brown sugar
- Raw Sugar Turbinado
- Artificial sweeteners
- Honey

**Coffee Bean Types**

- Fruity (if possible, with blueberries retro-taste and fermented/sour)
- Chocolatey
- Full Bodied
- Bright

**Coffee Strength & Roasting Level**

- Medium-strong coffee
- Light-medium roasting

**Advised Next Steps**

Consider further market research investigations on the possible incidence, among the targeted audience, of potential customers that would purchase food together with their cup of coffee. Also consider investigating what foods should be introduced on the menu and for how much.
