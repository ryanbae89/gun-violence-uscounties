# gun-violence-uscounties

## Introduction

[Gun violence in the US](https://en.wikipedia.org/wiki/Gun_violence_in_the_United_States) has been a contentious subject of debate for decades. Political commentaries aside, there have been lots of research done by various academic groups and think tanks trying to explain this unfortunate phenomenon. `gun-violence-uscounties` project is my small contribution to this long history, primarily fueled by my intellectual curiosity on the topic.

More specifically, **this study aims to look at rates of gun violence in the US at the county level**. While country and state level analyses have been done many times, often they are not granular enough to capture more nuanced factors, as states and countries are large, heterogeneous entities. In addition, **this study aims to fit a statistical model predicting gun violence deaths** in a given year in given county. This is done to **comprehensively capture effects from multiple variables, ranging from demographic, economic, social, and political dimensions**. The models are designed to answer the question, **what factors most contributes to gun deaths in the US and how have they changed over time?**

## Methodology

### Factors Brainstorm

Initially, there were many different features I wanted to include in the model. The various proposed features spanned 6 different dimensions (Economic, Social, Demographic, Cultural, Political, and Geographic). Each dimension had several proposed metric under them as a starting point for the project.

1. Economic
	* Gini Coefficient - measure of economic inequality
	* Mean/median income
	* Unemployment rate
	* Poverty rate
2. Social
	* Crime
		* Murder and violent crime rate per capita
	* Presence of individuals with mental health issues per capita
		* Suicide/depression rate per capita
		* Depression rate per capita
3. Demographic
	* % of population based on age groups
	* % of population based on race
	* Male/female sex ratio
	* % of households married
4. Cultural
	*  Gun culture
		* Number of guns per capita
		* % of households with guns
	* Presence of gang or drug activity
5. Political
	* Political lean of a county
	* Measure of gun laws
6. Geographic
	* Latitude - to study effect of lack of sunlight on mental health
	* Local climate

However, as I began to look further into each proposed factors, I ran into several problems:

* Many metrics were not available at the county level
* Some metrics were highly correlated with each other

Due to these problems, I had to narrow down the available features to a more limited subset. The final features used and their data source are described in the next section.

### Feature Engineering and Data Sources

Below are the features and their data sources used for this analysis.

|Dimension   | Feature   | Data Source   |   
|---|---|---|
|Economic   | [Unemployment rate](https://en.wikipedia.org/wiki/Unemployment_in_the_United_States)  | [US Bureau of Labor Statistics](https://www.bls.gov/lau/#cntyaa) |   
|Economic   | [Poverty rate](https://en.wikipedia.org/wiki/Poverty_threshold#United_States)  | [US Department of Agriculture Economic Research Service](https://www.ers.usda.gov/data-products/county-level-data-sets/)   |   
|Demographic   | % of population by race  | [US Census Data](https://www2.census.gov/programs-surveys/popest/datasets/2010-2018/counties/asrh/)  |   
|Education   | % of population without high school degree  | [US Department of Agriculture Economic Research Service](https://data.ers.usda.gov/reports.aspx?ID=17829)  |   
|Political   | [Partisan Voter Index](https://en.wikipedia.org/wiki/Cook_Partisan_Voting_Index)   | [Cook Partisan Voting Index](https://www.cookpolitical.com/pvi-map-and-district-list)   |  

All features were converted into percentages, with the exception of the PVI. However, PVI itself is the % Democratic Party lean in a county, making it comparable to the other features. This simplified the model interpretation.

The actual gun violence data came from [Kaggle](https://www.kaggle.com/datasets/jameslko/gun-violence-data), and contain over 260k gun violence incidents from 2013 - 2018.

### Modeling

The modeling approach used was to try to predict the number of people killed in each county using a simple linear regression. The gun violence dataset was aggregated to calculate the number of people killed in each county. Also, to account for the population differences in each county, each data point (county) was weighted by its population.

*Equation here*

$$
\beta
$$

In order to analyze how the importance of the above features changed over time (and to account for any seasonality in the data), the modeling was blocked by each year. There were 4 years where complete data was available, from 2014 to 2017.

The resulting model coefficients and their p-values were plotted over time, year to year.

### Limitations

This study, like all others, has limitations:

* Limits due to lack of data - Several dimensions listed above were not modeled in this study due to their lack of data. They include fairly important factors such as pravelence of guns, mental health issues, and geograpic.

* Limits of methodology - The GLM proposed here have modeling limitations, such as assumption of linear relationship between the independent and dependent variables. Also, interaction effects were not modeled in this study to keep the models as simple as possible for interpretation of results.

The approach taken in this study is largely done in the spirit of aphorism often quoted in statistics: *all models are wrong, but some are useful.*

## Results

### 1. Demographic Features

The demographic features were % of population in each county by race. The 4 features considered are the following:

| Plot Label  | Feature Description  |
|---|---|
| perc_NHWA  | Percent Non-Hispanic White American |
| perc_NHWA  | Percent Non-Hispanic Native American |
| perc_NHWA  | Percent Non-Hispanic Black American |
| perc_NHWA  | Percent Hispanic |

![Plot 1](pictures/demographic_features.png)

Regression coefficients show a clear statistical significance of one variable over time, **perc_NHBA**. One item to note here is that the perc_NHNA variable; the very large confidence interval of this coefficient is due to negligible Native-American population in most counties.

### 2. Economic Features  

| Plot Label  | Feature Description  |
|---|---|
| Econ_perc_poverty  | Percent Below the Poverty Threshold |
| Unemployment_rate  | Unemployment Rate |

![Plot 1](pictures/economic_features.png)

Of the two economic features considered, only the **Econ_perc_poverty** variable had statistically significant positive coefficient value. This is largely in line with

### 3. Education Features  

| Plot Label  | Feature Description  |
|---|---|
| Edu_perc_NoHS  | Percent with No High School Education |

![Plot 1](pictures/education_features.png)

Perhaps surprisingly, **Edu_perc_NoHS** variable had statistically significant **negative** coefficient each year. While common sense would

### 4. Political Features  

| Plot Label  | Feature Description  |
|---|---|
| PVI_2016  | Partisan Voting Index (% Democratic Party Lean) |

![Plot 1](pictures/political_features.png)

The models found no positive correlations between political inclination of a county and the number of deaths due to gun violence.

### Conclusions

The following conclusions can be drawn from the above results.

**1. Gun violence in the US is largely a problem that affects African American community. This at least in part related to poverty, which is correlated with African American community presence.**

The two most statistically significant variables with positive coefficient values from the generalized linear models were percentage of population non-hispanic black and percentage of population living below poverty. The two variable are also correlated, as poverty is much more pravelent among African American community.

The underlying reasons for this are beyond the scope of this study, but one can reason based on several commonly known issues that specifically affect the African American community, including but not limited to:

* Presence of gang violence
* High prevalence of single motherhood
* Economic inequality
* Etc.

**2. There is no evidence that political leanings play any role on gun violence.**

Many political actors make claims of partisan bias contributing to gun violence. However, this analysis found no relationship between a county's partisan lean (Democratic lean in this case) and the number of deaths from gun violence. While the regression coefficients were positive year to year, none were statistically significant.

**3. There is also little evidence that unemployment rate or lack of education has an impact on gun violence.**

While poverty had a clear impact on gun violence, other economic and education factors did not  Curiously, there seems to be negative correlation between lack of eduction (high school degree) and deaths from gun violence in a county. The reason for this is unexplored in this study, although one can speculate it could be due to correlations with other variables.

**5. These findings, at least in period between 2014 - 2017, have remained largely consistant.**

All variables that were found to be statistically significant remained so in the 4 years modeled in this study. This suggests that factors contributing to gun violence in the US are consistent, with any underlying changes happening slowly, on time period much longer than this analysis.

## Final Thoughts

Finally, I would like to say a few words about this subject and myself. I understand that gun violence in the US is a contentious subject, with strong opinions on all sides of the political spectrum.

I also acknowledge that it's near impossible to completely separate one's biases when studying an issue like this. I can, however, at least be open and transparent about my biases and let the readers make their own decisions based on the quality of the content presented here. I am someone who believes fairly strongly in the 2nd amendment while also acknowledging that deaths from gun violence in the US is far to high for society to accept.

### Acknowledgements

I would like to thank the following people for giving their thoughts and feedback on this project:



## References
