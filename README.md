# Capstone Project: Predicting Prevalence of Diabetes from Food Environmental Features


## Background & Problem Statement

Diabetes Mellitus is a disorder charcterized by elevated blood glucose, specifically > 125 mg/dL (1). Consequences of elevated blood sugar can be serious, even deadly, with complications that may include damage to the nerves, heart, kidneys, and other tissues of the body (2). The potential complications tie in with the increasing prevalence (currently 11.3% of the United States population(3)) to make diabetes a highly relevant topic in community healthcare of today.

The most prevalent types of Diabetes are Type I and Type II. While the risk factors for Type I diabetes are not well understood, Type II is the most prevalent (90.9% of the Diabetic population as of 2016 (4),) and 9/10 cases are preventable (5). Prevention can take the form of weight management, and more specifically, diet, and exercise. In addition, more research is underway to better understand the potential impact of stress and other mental health concerns (6,7,8).

Meanwhile, as I move forward as a data scientist, I still find myself drawn to health-related topics such as the prevalence of diabetes. As a former health profesional, I have experience with witnessing first-hand the effects of diet and exercise- both in preventing and treating the disease. However, I recognize that nutrition is more than a personal issue. Societal features are influential in terms of education regarding and access to nutritious foods. With this in mind, I undertook this study in an effort to understand how food environmental features impact the prevalence of diabetes. My goal was to demonstrate a link between certain nutrition-related features of communities and the prevalence of diabetes.

In order to do this, I analyzed information from the most recent Food Environment Atlas. The 281 variables within the dataset were the backbone of this study, and allowed for analysis and prediction based on nine dimensions: health, access, assistance, food insecurity, local (farms), socioeconmics, stores, and taxes, as categorized within the Food Environment Atlas. Additionally, as an integrative health practitioner, I decided to add information regarding mental health from two different sources.  Finally, I created and evaluated KNN and Random Forest Classification models to answer the question: how well can I predict the prevalence of diabetes in United States counties using food environmental features? I answer this question through the use of various classification metrics, with an emphasis on F1 scores.

References:

1. Diabetes Tests. CDC. (2022).<br>
https://www.cdc.gov/diabetes/basics/getting-tested.html#:~:text=A%20fasting%20blood%20sugar%20level,higher%20indicates%20you%20have%20diabetes.

2. Prevent Diabetes Complications. CDC. (2022).<br> https://www.cdc.gov/diabetes/managing/problems.html#:~:text=Common%20diabetes%20health%20complications%20include,how%20to%20improve%20overall%20health.

3. National Diabetic Statistics Report. (2022). CDC. https://www.cdc.gov/diabetes/data/statistics-report/index.html.

4. Prevalence of Diabetes by Type. CDC. (2018).<br> https://www.cdc.gov/mmwr/volumes/67/wr/mm6712a2.htm#:~:text=Overall%2C%20based%20on%20self%2Dreported,diabetes%2C%20representing%2021.0%20million%20adults.


5. Simple Steps to Preventing Diabetes. Harvard TH Chan School of Public Health. (Accessed 2023).<br> https://www.hsph.harvard.edu/nutritionsource/disease-prevention/diabetes-prevention/preventing-diabetes-full-story/

6. Stress and Diabetes. Diabetes UK. (Accessed 2023.)<br> https://www.diabetes.org.uk/guide-to-diabetes/emotions/stress#:~:text=Stress%20alone%20doesn't%20cause,amount%20of%20insulin%20they%20make.


7. Diabetes and Psychiatric Disorders. Yatan Pal Singh Balhara.(2011). https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3193776/

8. Diabetes Added to High Risks for People with Severe Mental Illness. Suzanne Leigh. (2018).<br>
https://www.ucsf.edu/news/2018/06/410741/diabetes-added-high-risks-people-severe-mental-illness

---

## Data Description

The data for this project were collected from several online resources, as described below. 

#### Food Environment Atlas

I downloaded the Food Environment Atlas [here](https://www.ers.usda.gov/data-products/food-environment-atlas/data-access-and-documentation-downloads/) as a multipage spreadsheet found on the ers.usda.gov website. The pages consisted of a Variable List, Supplemental County and State Data, and the data of interest, which were individual pages assessing all 3,142 counties on nine specific dimensions: Access, Assistance, Health, Food Insecurity, Local (Farms), Socioeconomics, Stores, and Taxes. From there, I saved the individual page as separate CSVs, which I imported into notebooks as Pandas dataframes. I then cleaned the dataframes one at a time by removing some of the columns/variables including those with a large amount of missing data. Additions to/reconciliation with the additional datasets are described further below.

#### Diabetes Atlas

I downloaded the 2018 Diabetes Atlas data from [here](https://gis.cdc.gov/grasp/diabetes/diabetesatlas-sdoh.html#), which is a CDC website. This information, which also includes information on the SVI or "Social Vulnerability Index," is avaiable as a map and a table. I visited the table, and then downloaded the county information as a CSV. I then cleaned the data by removing the "Year" column (as all was from 2018), and the SVI column, as the Food Environment Atlas contains other measures of social vulnerability, and my interest in this dataset was solely the percentage of adults with diabetes.

I soon discovered slight discrepancies between the counties represented in the Food Environment Atlas and the Diabetes Atlas. In order to reconcile the two datasets, I used a SQL anti-join to determine which counties were in the Food Environment Atlas but not in the Diabetes Atlas, and vice-versa. The reconciliation process included research on the involved counties, and renaming some in the Food Environment Atlas. I also needed to add one county to the Diabetes Atlas which was included in both the Food Environment Atlas, and the Diabetes Atlas map, but for some reason, was not present in the Diabetes Atlas table. I then created a "class" column, in which counties were ranked high (> the dataset mean) or low (<= the dataset mean). Finally, I added the prevalence (%) and class columns to each of the Food Environment Datasets.

#### National Mental Health Providers

I sourced the information regarding the number of mental health practitioners per state, along with a rank for each state, from [here](https://www.americashealthrankings.org/explore/annual/measure/MHP?edition-year=2017) for 2017, and [here](https://www.americashealthrankings.org/explore/annual/measure/MHP?edition-year=2018) for 2018. While the number of MH practitioners in the District of Columbia (DC) was included, DC was not given a rank. Since an object or null value would prevent the rank column from being used in modeling, I used an ORDER BY in SQL in order to determine the rank that the District of Columbia should have. I then re-assigned the ranks using Pandas. Following this, I created a function to add the MH provider values (which were by state, one row for each state) into the Health dataset (up to hundreds of rows per state, as each row was a unique county.)

#### Percentage of Adults Receiving MH Services in the Last Year

I found this information [here](https://www.samhsa.gov/data/report/2015-2016-nsduh-state-estimates-individual-excel-and-csv-files-outcome), which is on the SAMHSA (Substance Abuse and Mental Health Services Administration) wesbsite. I downloaded the information as a CSV, specifically from Table 28. I then added the three columns of interest (percentage of adults 18 and older, 18-26, and 26 and older respectively) into the Health dataset using a function I created.

#### Data Files

The data folder in this repository includes all CSV's used in order to complete this study. Each dataset has an original CSV, a clean CSV (if applicable,) a reconciled CSV (in which the counties were corrected, if applicable,) and the CSV containing the final data which was used for modeling.

#### Code

The documentation of work for this study is completed in 5 notebooks:

01_Data_Import_and_Cleaning.ipynb - Data import, preliminary data analysis and cleaning<br>
02a_EDA_Phase1.ipynb - Analysis using SQL for mins, maxes, averages, and comparisons between datasets<br>
02b_EDA_Phase2.ipynb - Observations of correlations, visualizations & case study using Pandas<br>
03_Feature_Modification_Selection_Engineering.ipynb - Reconciling county data, creating finalized datasets for modeling<br>
04_Models_Evaluation_Conclusion.ipynb - Modeling, Evaluation, and Conclusion with Next Steps & Recommendations


---

## Conclusion, Next Steps & Recommendations

In completing this study, I set out to determine how well I could predict prevalence of diabetes in U.S. counties using food environmental features. As it turns out, some of these factors are fairly predictive of diabetes. For example, in the Random Forest Classification model combining Health and Restaurants datasets, when the model predicts that a county is low prevalence, it is correct 76% of the time. When the model predicts that a county is high prevalence, it is correct 77% of the time. These datasets are older, with the most recent information being 5 years old. However, what can be learned from these models is that there are in fact certain aspects of a community that are predictive of diabetes. 

It was interesting to note that the Health dataset produced the most predictive model as an individual dataset. Specifically, the Health dataset included information about recreational/fitness facilities, mental health providers, and percentage of the adult population receiving mental health services. This suggests that community leaders can work toward decreasing prevalence of diabetes by encouraging use/presence of recreational facilities. Communities can also stress the importance of receving proper mental health care, and encourage young community members to select career choices that involve mental health. I stress the mental health care part of this specifically, because the connection between mind and body is something that is often not fully appreciated. The results of this study indicate that full importance should be placed on mental health treatment.

Additionally, the second highest-scoring individual model was the Restaurants dataset. This suggests that communities could stress the benefits of limiting fast food spending, and perhaps engage in outreach and education regarding healthier food options that fit within a limited budget. While information regarding reprsentation of ethnic groups within counties was not included in the current study, disparities between ethnic groups can be seen [here](https://diabetes.org/newsroom/press-releases/2021/income-related-inequalities-in-diabetes-have-widened-over-past-decade-cdc-study-finds). Therefore, it becomes important for community leaders to provide interventions tailored to the specific population they serve, its customs, cultural foods, and so on.

Finally, while I did not stress their presence in the overall findings, I do want to note that the Food Insecurity and Assistance datasets produced models that were an improvement over baseline. This seems indicative of the overall connection between low income and diabetes, also addressed [here](https://diabetes.org/newsroom/press-releases/2021/income-related-inequalities-in-diabetes-have-widened-over-past-decade-cdc-study-finds). Given the fact that the federal minimum wage is not enough to meet living expenses (see this [website](https://livingwage.mit.edu/articles/99-a-calculation-of-the-living-wage)), those who are very low income might experience limited food options and high levels of chronic stress. It is sensible, then, that they might experience higher rates of diabetes.  

If continuing to research this topic, I have multiple thoughts about the next steps I could take. Firstly, I would like to continue to research the impact of mental health and mental health services on the prevention and treatment of physical disease states. Additionally, I would like to conduct further research on the potential savings of preventative healthcare, both in terms of lives, and healthcare costs (see this [website](https://diabetes.org/about-us/statistics/about-diabetes), reporting $327 billion costs on diagnosed diabetes in the United States in 2017.) Finally, I would like to research the potential benefits in terms of overall health in raising minimum wage. My hope in doing so is that community leaders will become empowered to promote effective change.
