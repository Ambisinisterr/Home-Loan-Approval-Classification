# General Assembly Project 5: The Effect of Race on Home Loan Approval

#### Barb Allen, Christopher Durso, Vadim Serebrinskiy, Peter Wentzel
---


## Problem Statement
----

In the United States since 2006, home ownership on average has been on the decline.  Census data suggests that minority populations are less likely to be homeowners in comparison to the majority white demographic in the United States.  One aspect of this could be the inability for people that fall in ethnic minorities to acquire a mortgage.  The goal of this study is to determine if loan approval is at all affected by applicant race in major metropolitan areas in the four largest states in the US.

## Executive Summary
----

The data set chosen for this study comes from the <a href="https://www.consumerfinance.gov/">Consumer Financial Protection Bureau</a>, an agency that was established in the wake of the 2007-2008 financial crisis.  The Home Mortgage Disclosure Act that requires many financial institutions to maintain, report and publicly disclose loan-level information about mortgages.  The goal of these data sets is to provide information on lenders so that it is available for public officials to make decisions and if there are discriminatory lending patterns.  The data sets chosen for this project were the California, Florida, New York and Texas data sets from 2014 to 2017.

Initially only 2017 for each state had EDA performed to determine any interesting elements and trends in the data.  It was found that most samples were getting approved loans, however also that the majority class for race was white.  It was decided that loan approval would be defined as originated loans, approved applications even if not accepted, and loan’s purchased by institutions.  Insights that were gained from EDA encompassed geographical and standard of living effects on approval rate as well minority trends vs the majority class.  Metropolitan areas in Florida for example had the lowest mean loan approvals and mean applicant income, while California made up three of the top four in mean loan approvals with all ~80% rates.  Texas made up the largest portion of the samples with ~39% and New York the smallest portion with ~14%.

The <a href="https://www.consumerfinance.gov/data-research/hmda/historic-data/">original .csv</a> files used for this study were, on average, 700MB per year per state when uncompressed. This prevented the team from having easy access to the same data.  To alleviate this, a routine was created where-in the data was compressed to keys stored in a json file and then decompressed in the main file for further EDA and modeling.  This allowed for easier collaboration and usage of the git repository for the merged datasets.

After the initial cleanup was complete, we began modelling by cleaning up some of the redundant columns that had both a numerical identifier along with their text label. Since this is a classification type problem, we felt that giving deference to the categorical dimension was the best route. We initialized a set of pipelines to transform the numerical and categorical columns with their accompanied scalers.  We started off with a Logistic Regression model since its straightforwardness lent room for experimenting with model parameters. Unfortunately, it had trouble converging – prompting us to add a PCA transformation step to the pipeline and use the main explanatory components in the rest of the models, KNN and Random Forrest, and an updated logistic model that focused only on the main PCA components. The updated logistic model showed promise by delivering a 78% accuracy score. The KNN and Random Forrest (RF) performed slightly lower at 77% and 74% respectively. It is worth noting, however, that the PCA tuned logistic and KNN models were run on the full data set, as opposed to training subset used on the RF. 

## Conclusions
Unfortunately, in order to accurately answer our Problem Statement additional data would be needed. The CFPB data has been currated to protect consumer privacy while still granting easy access to a checks and balance system to ensure banks are not granting loans in a haphazard way.

The model failed to perform signifigantly better than baseline. This is because all of the features have very low correlations to approval ratings. This harkens back to the currated nature of the data not providing a lot of private or consumer specific data.

| Feature | Correlation |
| ------- | ----------- |
| minority_population | -0.040487 |
| applicant_income_000s | 0.039222 |
| hud_median_family_income | 0.035330 |
| tract_to_msamd_income | 0.034826 |
| latino | -0.028736 |
| as_of_year | -0.025821 |

----

**Baseline Score:** 77.54%.

**Logistic Regression Model:** 78% Accuracy

**KNN Classification Model:** 77% Accuracy

**Random Forest Classification Model:** 74% Accuracy *(had to be run on a subset of the dataset)*

## Recommendations for Future Work 
----

There are roughly 2.26M rows in our end dataset. The amount of data made modelling incredibly slow and complicated and PCA became an outright requirement. However, PCA does not work well with categorical variables and the models would be better if PCA was applied differently or we used an alternative way to reducce feature complexity.

As noted in our conclusions more data is needed to accurately answer our problem statement. Census Data should be aquired as well as any information on credit scores.


## File Contents
----

### Preprocessing Notebook

01_compress_and_merge_df.ipynb  -  This notebook processess the initial datasets from the CFPB so that they can be merged into one dataset.

### Main Notebook

02_all_states.ipynb  -  Summarized EDA notebook, modeling, and final plots. Contains all 4 states from years 2014-2017.

### Reference Files

02_all_states_plots.ipynb  -  All Plots made with plotly separated into it's own notebook

barb_eda_ca.ipynb  -  Barb's EDA on California (Los Angeles, San Francisco, San Diego)

chris_eda_fl.ipynb  -  Chris' EDA on Florida (Jacksonville, Tampa, Miami, Orlando)

vadim_eda_ny.ipynb  -  Vadim's EDA on New York (New York City, Suffulk County, Weschester County)


## How to:

1. Download datasets
>Pull datasets from <a href="https://www.consumerfinance.gov/data-research/hmda/historic-data/">original .csv</a> and extract into a folder named "cfpb_data".

>In order to exactly follow our dataset the following should be pulled:
 - California from 2014 - 2017
 - Florida from 2014 - 2017
 - New York from 2014 - 2017
 - Texas from 2014 - 2017

>run 01_compress_and_merge_df.ipynb

>alternatively: merged_df.csv.gz is already has the combined dataframe

2. 02_all_states.ipynb

>Shows Combined EDA on all data and creates various models utilizing PCA.

3. 03_plot_notebook.ipynb

>Plotly plots, more concrete visualization then EDA.

4. 04_all_states_model.ipynb

>Modeling notebook, can be run without EDA notebook to model data from merged_df.csv.gz.

5. Consult Reference Files

>These files are not needed for actual execution and are included as reference only. It shows the team's invidual EDA's by state and how the plotly graphs were created.


## Software Requirements

`pandas` `numpy` `matplotlib.pyplot` `seaborn` `plotly` `json` `math` `LogisticRegression` `RandomForestClassifier` `KNeighborsClassifier` `train_test_split` `GridSearchCV` `SimpleImputer` `OneHotEncoder` `StandardScaler` `ColumnTransformer` `Pipeline` `PCA` `KFold` `cross_val_score`

## Data Dictionary

Data summary via the CFPB for the HMDA mortgage datasets - https://files.consumerfinance.gov/hmda-historic-data-dictionaries/lar_record_codes.pdf

## References

https://www.consumerfinance.gov/data-research/hmda/

https://www.census.gov/library/publications/2021/acs/acsbr-010.html

See cited sources sections in notebooks.


