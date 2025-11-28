# The Challenge
"All you need to do is share a single page data visualization that captures the awe of outer space through our history with space travel"

[Full Challenge Link](https://mavenanalytics.io/challenges/maven-space-challenge)

# Data Challenge - The History of the Space Race [Finalist]

## Full One Pager

<img width="1635" height="963" alt="image" src="https://github.com/user-attachments/assets/cebb8afa-ded7-4e01-9535-673698af65be" />
<br><br>

I tried to tell the story of the Space Race, using a dataset containing data for all the space missions from 1957 to August 2022.

# Dataset
All space missions from 1957 to August 2022, including details on the location, date, and result of the launch, the company responsible, and the name, price, and status of the rocket used for the mission.

# Approach Used:
The first step was to deep dive in the context to really understand the dataset - this has been key to provide the full picture, in fact:

The launch sites from the dataset can be misleading if not properly handled: there could be one launch site, but different missions were launched from 2, 3, 4, etc.. different launching pods in the same site, it does not mean there are multiple sites, the launch site is still one.

The launch sites do not match with launches made by the countries, in fact, some countries launched from sites outside their borders. (e.g. European countries launched from outside Europe and Kazakhstan was used as launch site by USSR first, then Russia but despite being a former Soviet country, also companies from other "Western" countries launched from there - who? French and American companies)

In order to really understand patterns of this Space Race at country level, it was necessary a desktop research, to create a lookup table for the companies, where each company was assigned with its country (Headquarters location).

Subsequently it was visualized the data providing a general piece on what mankind achieved in the space race and from where the missions were sent.

Finally, it was made a deep dive at country level showing what countries mostly contributed and when.

