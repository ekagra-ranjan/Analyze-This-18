# Analyze-This-18 
## (American Express Flagship Data Science Competition)

## Final Result
National Rank **22**  
<br>
Team Name: Flabbergasted
<br>

![picture alt](https://github.com/ekagra-ranjan/Analyze-This-18/blob/master/Screenshot%20from%202018-10-04%2000-12-56.png
 "Final Results")


The repo contains kernels for the following algorithms and techniques:
1) Random Forest
2) Gradient Boosting Machine
3) XGBoost - from CoreLib and sklearn API [ They do give different accuracies so better to try them both :) ]
4) SVM Regressor
5) Categorical Variable Analysis
6) Data Exploration
7) Feature Synthesis
8) Data Normalization
9) Ensembling of the best models
10) K-fold Cross-Validation



## Background
The students of Ivyleague University have decided to embark on a start-up journey. They decided to establish and operate a departmental store named Tabz Departmental Store. This store offers a wide variety of products and services that caters to the daily needs of all the people staying in the city in which the university is located. Apart from Tabz, there are a couple of other departmental stores in the city. In order to tackle the intense competition, Tabz decided to offer its products and services on credit i.e., customers can consume products and services without paying for it at the time of consumption. Payment for the products and services consumed from Tabz during the month can be made at the end of the month. This is a revolutionary idea when compared to the traditional mode of cash payment and is expected to simplify the lives of people in the city while giving competitive advantage to Tabz. So, the founders decided to launch a co-branded credit card in association with a local bank, Banco. Tabz has decided to offer two types of cards:

* Charge card: The balance is required to be paid in full each month
* Lending card: Lending cards allow the customer to pay the balance over a period of time subject to interest being charged

An individual can apply for only one of the two types of credit card on offer. In order to extend the credit card to the individuals, Banco must first underwrite the applicant. Underwriting is the process by which the lender decides whether an applicant is creditworthy and should receive a credit line. Given the innovative business model of Tabz and the sound reputation of its founders, thousands of residents in the city submitted their application forms for the co-branded credit card from Tabz. Along with the data present in application forms, Banco also has access to the consumer bureau. Bureau is an agency that aggregates consumer borrowing and payment information for the purpose of assessing credit-worthiness of an individual and setting a limit on the cumulative credit that can be extended to an individual by lenders.
Banco has hired you to help underwrite each applicant and predict the credit worthiness of an individual. Banco has provided you with the customer application and bureau data with the default tagging i.e., if a customer has missed cumulative of 3 payments across all open trades, his default indicator is 1 else 0. Data consists of independent variables at the time T0 and the actual performance of the individual (Default/ Non Default) after 12 months i.e., at time T12. Banco’s expectation from you is to predict if an applicant will go default in next 12 months from the time of application submission.

## Problem Statement
You have to create a list of applications in the order in which Banco should process them. With an objective to maintain healthy financials, Banco would like to process least risky applications first. Against each application, you also have to provide your prediction of default - 1 or 0, where 1 indicates a default and 0 indicates no default.

Assume:
* A resident of the city can submit only a single application form
* None of the applications submitted are fraudulent
* State any other assumptions in your final submission

## Data for Analysis
Following files can be downloaded for your analysis.

* Training_dataset.csv: This dataset contains:
    * Applicant level historic credit history
    * Performance in terms of default tagging i.e. 1 for default and 0 for no default
    * Application and bureau data
    
* Leaderboard_dataset.csv: This data has historical applicant level data along with all the variables in the training dataset. The actual performance i.e. default tagging is not present in this data.

* Evaluation_dataset.csv: This data has applicant level data along with all the variables in the training dataset. The actual performance i.e. default tagging is not present in this data.

* Data_Dictionary.xlsx: This sheet will give you the description of all the variables contained in the 3 datasets above. 
Please note that you can make multiple submissions corresponding to the Leaderboard Dataset. However, for the Final dataset you can submit only one solution.

# Solution
## Estimation Technique Used
We used a Gradient Boosting Machine – implementing the xgboost Python library.
 
A Gradient Boosting Machine iteratively trains decision trees, and ensembles them (essentially combining their predictions), all while minimizing the error function (in our case, the ‘softmax’ function) at each step. 
The xgboost library provides an out-of-the-box implementation of a GBM.It is a form of extreme Gradient Boosting.

We then used an ensemble model of Linear Support Vector Machine, Extreme Gradient Boost, Multi Layer Perceptron and Random Forest. Each of these models are alone powerful models and combining them increases our predictive power.

Ensemble methods helps improve machine learning results by combining multiple models. Using ensemble methods allows to produce better predictions compared to a single model.

We used transformation (log or cuberoot) to make the distribution Gaussian as well as normalized 21 feature columns, whiich we got  after removing, and combining some of the input data – explained later.

## Visualizations
### Feature Importance using Random Forest Feature Sampling Technique
![picture alt](https://github.com/ekagra-ranjan/Analyze-This-18/blob/master/var_img.jpg "Feature Importance using Random Forest Feature Sampling Technique")

### Correlation Plots between Features
![picture alt](https://github.com/ekagra-ranjan/Analyze-This-18/blob/master/corrleation.png "Correlation Plots between Features to remove the redundant ones")

## Details of each Variable used in the logic/model/strategy [Feature Synthesis]
	
Nearly all the variables had missing values. We analysed all the variables and decided that the  mean of the features was the best alternative for the missing values.

The variables *mvar6* was found to be inversely related to the *default rate* whereas *mvar7* and *mvar8* were directly related to it. So we used a new feature *(mvar7 + mvar8)/mvar6*.

Similar observations led us to include new features like *(1+mvar3)* * *(1+mvar4)* * *(1+mvar5)* and *(mvar36)* * *mvar(37)*/*mvar38*. These synthesised features were helpful in increasing the performance which can be seen from the feature importance graph in above section.

To reduce the number features we removed highly correlated features using the heatmap from correlation plot (on above section). For eg: we removed *mvar16* and *mvar17* as they were highly correlated with *mvar18*. These reductions made our model faster and further increased the performance.

## Reasons for Technique(s) Used
We feel that a single estimator is prone to overfit the data and hence might not perform well on the test data.

Hence we used ensemble (averaging over different estimators) of models that are different in nature to reduce overfitting and give us a robust estimator.
