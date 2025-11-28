# The Challenge

"... The intent of the HCAHPS initiative is to provide a standardized survey instrument for measuring patients’ perspectives on hospital care, and one of its 3 main goals is to "create incentives for hospitals to improve their quality of care".

Your task is to **evaluate whether the HCAHPS survey has been successful in accomplishing this goal** by answering questions like these:

- Have hospitals' HCAHPS scores improved over the past 9 years?
- Are there any specific areas where hospitals have made more progress than others?
- Are there any major areas of opportunity remaining?
- What recommendations can you make to hospitals to help them further improve the patient experience?**"**

[Full Challenge Link](https://mavenanalytics.io/challenges/maven-healthcare-challenge)

# Maven Healthcare Challenge - HCAHPS Survey Evaluation Report (2015 - 2023)

**Full One Page Report**

<img width="2911" height="3429" alt="image" src="https://github.com/user-attachments/assets/8890de82-4de2-45f7-aeb8-875641faa97e" />

Examination of the HCAHPS survey spanning 2015 to 2023, with the intent of furnishing guidelines to enhance the holistic patient experience within USA's healthcare institutions.

# Approach Employed

The survey evaluation commenced with a meticulous comprehension of the survey's investigative scope, leading to a dichotomy of measures into two pivotal categories: Hospital Attributes and Hospital Evaluation/Promotion.

<img width="701" height="265" alt="image" src="https://github.com/user-attachments/assets/00cfa193-d48d-4624-8948-dd25cc466e0f" />

Within the Hospital Attributes category, specific attributes reflecting distinct facets of the hospital's service were scrutinized. These encompassed elements such as personnel communication, premises cleanliness, and environmental tranquility. Concomitantly, the Hospital Evaluation/Promotion category encapsulated overarching assessments—namely, the Overall Hospital Rating and the Willingness to Recommend the Hospital. These latter measures were deemed comprehensive reflections of the aggregate impact of the hospital attributes.

Subsequently, the analysis proceeded with a nationwide assessment to ascertain if the percentage of Top-Box responses for the Overall Hospital Rating and the Recommendation Score exhibited a progression from the first HCAHPS report release in 2015 to the latest iteration in 2023. The findings revealed a consistent or marginally diminished level in 2023 compared to 2015.

<img width="2591" height="638" alt="image" src="https://github.com/user-attachments/assets/795266e0-bdbf-4116-963a-d537451e0c60" />

The subsequent phase of the study was dedicated to discerning if this trend was uniformly manifested across all 51 states within the United States. The objective was to determine whether specific states had effectively executed policies aimed at ameliorating the patient experience between 2015 and 2023.

<img width="1066" height="1125" alt="image" src="https://github.com/user-attachments/assets/87088634-ed03-47f8-bbc6-1a6f44d0d462" />

The matrix analysis, elucidates the percentage variation of Top-Box responses, from 2015 percentages to 2023 percentages, for each measure across states. The matrix underscored discernible instances wherein certain states achieved enhancements in the Overall Hospital Rating and select Hospital Attributes, while others exhibited stagnation or deterioration.

Leveraging these variations, the study delved into the exploration of interrelationships among measures' variations where the primary hypothesis to be validated was the substantial correlation between the Overall Hospital Rating's variation and the Willingness to Recommend the Hospital's variation.

<img width="2000" height="421" alt="image" src="https://github.com/user-attachments/assets/e9dcc111-cfba-4e91-8f6d-83a33f12a3b1" />

As evidenced by the correlation matrix, the two measures exhibited a robust correlation coefficient of 0.876.

In the light of that is safe to assume that when hospitals obtain improvements in their Overall Hospital Rating, patients are also more willing to recommend these hospitals.

Following this validation, the study transitioned into a pivotal phase involving the construction of a robust model. This model aimed to elucidate the feature-related attributes exerting the most profound influence on the Overall Hospital Rating. To this end, a multiple linear regression framework was employed. The Overall Hospital Rating variation was designated as the dependent variable, with all other measures' variations considered as independent variables—excluding the Recommendation Score's variation. Exclusion of the Recommendation Score's variation was warranted due to its intrinsic linkage with the Overall Hospital Rating's variation and its potential to introduce bias into the model.

<img width="2000" height="1084" alt="image" src="https://github.com/user-attachments/assets/b2ebe004-a112-4b06-b302-3900b14f3f32" />

The ensuing regression model yielded unequivocal outcomes. It elucidated a substantial portion of the observed variability, with R-squared (R2) and Adjusted R-squared values of 0.81 and 0.77 respectively. The model's robustness was underscored by a p-value nearing 0.00.

In synthesis, this investigation culminated identifying areas warranting improvement to heighten the overall patient experience. The empirical findings underscored that elements such as hospital environment quietness, care transition efficacy, and communication between patients and medical/paramedical staff hold significant potential to elevate the Overall Hospital Rating, thus enhancing the holistic patient experience, and increasing the likelihood that patients will recommend the hospital.

# Dataset

National & state-level results from 2013 to 2022 for the Hospital Consumer Assessment of Healthcare Providers and Systems (HCAHPS) survey, including hospital-level completions and response rates.
