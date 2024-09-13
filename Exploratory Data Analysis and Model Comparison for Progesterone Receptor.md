# **Computational Drug Discovery: Evaluating Descriptor Performance and Machine Learning Model Assessment for Progesterone Receptor**

## **Aim of the project:** 

Exploratory data analysis to analyze descriptors for building a Quantitative Structure-Activity Relationship (QSAR) model for human progesterone receptor. Comparing and ranking various regression models based on their performance to generate an optimal model with close predicted and experimental values for progesterone receptor.

## Progesterone receptor:

Progesterone Receptor (PR) belongs to the family of nuclear/steroid hormone receptor. PR plays an important role in cell division and differentiation during female developmental and reproductive cycle through gene expression networks that control development, differentiation and proliferation of target tissues. Mutations in PR are a cause of endocrine dependent breast cancer where changes in the control of pathological processes leads to dysfunction in cells. (Sandra L. Grimm, 2016)

## Quantitative Structure-Activity Relationship (QSAR):

Quantitative Structure-Activity Relationship (QSAR) model is a computational and mathematical approach that develops a relationship between a molecule and its activity numerically. Learning from this relationship, the model predicts the activity of new structures against a target protein. These new structures, after experimental testing, could become potent drugs against the protein. (Cherkasov, 2014)

Various machine learning QSAR models have been employed for progesterone receptors, such as random forest, multiple linear regression, support vector regression, and extreme gradient boost in a study conducted by (Du, 2023). 

Our study aims to classify 40 machine learning models based on their performance for progesterone receptors. Our performance visualization is based on bar plots of R-squared values, RMSE and calculation time.

## Methodology:

### 1. Bioactivity Data Retrieval:

In the first step we retrieved bioactivity data for our target, ‘Progesterone Receptor’, through ChEMBL database. 

### 2. Data Preprocessing:

Out of 15 entries, we chose our target on the basis of target organism, Homo sapiens, and target type, single protein. The choice of single protein is important to make the model more specific towards a single target, which enhances the predictive power by predicting the binding and affinity of compounds to one target. In many cases researchers only want compounds to narrow down their binding to one protein, for that a single protein machine learning model is required.

The potency of a compound against its target is represented by certain units, such as, IC50 value. The IC50 value is the pharmacologically active amount of a drug that inhibits the activity of a protein or a molecule by 50% (M., 2021).

Our dataset was filtered to only contain those compounds that have their bioactivity expressed as IC50. Compounds with missing data for canonical smiles and IC50 values were dropped. The data was classified into active (IC50 < 1000nM), inactive (IC50 > 10,000nM) and intermediate (IC50 = 1000 – 10,000nM). Our final pre-processed dataset contained 4 columns, molecule ChEMBL ID, canonical smiles, standard value (IC50 value), and class (active, intermediate, inactive).

### 3. Lipinski Descriptor Calculation:

Lipinski’s Rule of 5 (RO5) describes the ideal properties of a compound to cause an affect when it acts as an orally administered drug. Proposed by (Lipinski, 2001), these properties represent the drug-likeness of a compound. According to RO5, the potential drug candidate should have a molecular weight (MW) < 500g/mol, hydrophobicity represented by logP < 5, hydrogen bond donors < 5, Hydrogen bond acceptors < 10. In drug discovery, this rule allows the initial screening of drugs to filter out only those candidates that fit to these requirements (Chen, 2020).

The Lipinski descriptor for our compounds were calculated based on the input information of the smiles that represents the structural information of the compounds. These descriptors include, molecular weight (MW), octanol-water partition coefficient (LogP), number of hydrogen bond donors (NumHDonors), and number of hydrogen bond acceptors (NumHAcceptors).

### 4. IC50 to pIC50 Conversion:

To normalize our dataset, we apply the ‘pIC50 ()’ function that converts the IC50 values to -log10 (IC50). To prevent negative logarithmic values to become negative, we use the normalization value function that caps the values greater than 100,000,000 to 100,000,000. This handles large variations in IC50 values and improves comparability.

### 5. Removing Intermediate Bioactivity Class:

Compounds with intermediate bioactivity class (IC50 = 1000 – 10,000nM) were removed to perform exploratory data analysis as a comparison between active and inactive class.

### 6. Exploratory Data Analysis (EDA):

EDA is used by data scientists to extract insights from data through various data visualization techniques. These data visualization techniques have advanced rapidly since the onset of machine learning and artificial intelligence. 

For EDA analysis of progesterone receptor, we created a box plot that represented the frequency of bioactivity classes, active and inactive. We then created a scatter plot that showed the distribution of active and inactive class between logP on y-axis and molecular weight on x-axis. 

### 7. Statistical Analysis:

Mann-Whitney U Test is a non-parametric test used in statistical analysis. It compares two independent groups and uses ranks to classify them (Nachar, 2008) (Patrick E. McKnight, 2010). For statistical analysis, we created box plots and conducted Mann-Whitney U Test for our descriptors (MW, LogP, NumHDonors, NumHAcceptors) to identify which of them are significant in building the model. 

### 8. Molecular Fingerprint generation:

Molecular fingerprints quantitatively define the physical and chemical properties of compounds in terms of bit (Adrià Cereto-Massagué, 2015). The PubChem fingerprints of all the compounds were generated using PaDEL descriptor. Our dataset now contains fingerprints for all 1730 compounds and their respective pIC50 values. 

### 9. Building a Random Forest Regression Model:

To build our random forest model, the input variable X is the fingerprints and output variable Y is the pIC50 value. By removing low variance features, the fingerprints were reduced to 140 from 881. 

|**Input (X)**|**Output (Y)**|
| :-: | :-: |
|Train (1384, 140)|Train (1384)|
|Test (347, 140)|Test (347)|

Random forest was built with a seed number of 100, so that the data is not randomized. The model score was 0.543.

The scatter plot of experimental vs. predicted pIC50 values was obtained. 

### 10. Comparing Regression Models:

We compared 40 different machine learning regressor models to test their performance for our dataset. The results were analysed through visualization. Models were judged based on their performance through RMSE and R<sup>2</sup> values. 

## **Results and Analysis:**
### 1. **Descriptor performance:**

#### **pIC50 values for active and inactive compounds:**

According to the box plot, there is a statistically significant difference between the distributions of pIC50 values for the active and inactive compounds in our dataset. For Mann-Whitney U test, we can reject the null hypothesis H<sub>0</sub>, which says that there is no significant difference between pIC50 values of active and inactive molecules; low p-value (1.0089×10<sup>-72</sup>) rejects this hypothesis because it is lower than 0.05 (Patrick E. McKnight, 2010). 

#### **Molecular weight assessment for bioactivity class:**

For molecular weight (MW) descriptor we fail to reject the null hypothesis because p-value = 0.505, which means that our values are not statistically different. The box plot further confirms this as the values lie relatively in the same range.

#### **LogP assessment for bioactivity class:**

LogP is a significant descriptor for our model as p-value for LogP is 0.000306, that is smaller than the alpha level (0.05).

#### **Number of Hydrogen bond Acceptors:**

NumHAcceptors is a significant descriptor for our model as p-value for NumHAcceptors is 0.000879, that is smaller than the alpha level (0.05).

#### **Number of Hydrogen bond Donors:**

NumHDonors is a not a significant descriptor for our model as p-value for NumHDonors is 0.181724, that is higher than the alpha level (0.05).

#### **Scatter plot of predicted and experimental pIC50 values using Random Forest:**

The scatter plot indicates close clustering of molecules around the diagonal line indicating that the model is a good fit. There are no significant differences between experimental and predicted pIC50 values.

### 2. **Model Evaluation:**
Model evaluation was done based on:

**Root Mean Square Error (RMSE)** is the magnitude of errors between predicted and experimental value. A model performs better when its RMSE is lower. 

<b>R-squared (R<sup>2</sup>)</b> value is the measure of data variance in the dependent variable by that is shown by the input variable. A model performs better with a higher R<sup>2</sup> value.

#### **Performance Table for the Train Set Predictions:**

Train set was prepared from 80% subset of the data. Decision tree regressor showed the best performance out of all the models for trainset prediction.

|**Model**|**Adjusted R-squared**|**R-squared**|**RMSE**|**Time Taken**|
| :-: | :-: | :-: | :-: | :-: |
|DecisionTreeRegressor|0\.91|0\.92|0\.31|0\.19|
|ExtraTreeRegressor|0\.91|0\.92|0\.31|0\.11|
|ExtraTreesRegressor|0\.91|0\.92|0\.31|3\.34|
|GaussianProcessRegressor|0\.91|0\.92|0\.31|1\.18|
|XGBRegressor|0\.90|0\.91|0\.33|0\.27|
|RandomForestRegressor|0\.86|0\.88|0\.39|2\.02|
|MLPRegressor|0\.86|0\.88|0\.39|2\.58|
|BaggingRegressor|0\.85|0\.86|0\.41|0\.39|
|HistGradientBoostingRegressor|0\.79|0\.81|0\.49|1\.02|
|LGBMRegressor|0\.75|0\.78|0\.53|0\.25|
|SVR|0\.65|0\.69|0\.62|0\.62|
|NuSVR|0\.65|0\.68|0\.63|0\.49|
|KNeighborsRegressor|0\.63|0\.67|0\.64|0\.07|
|GradientBoostingRegressor|0\.62|0\.66|0\.65|0\.88|
|LinearRegression|0\.46|0\.52|0\.77|0\.09|
|TransformedTargetRegressor|0\.46|0\.52|0\.77|0\.04|
|Ridge|0\.46|0\.52|0\.78|0\.03|
|RidgeCV|0\.45|0\.51|0\.78|0\.10|
|SGDRegressor|0\.43|0\.49|0\.79|0\.11|
|HuberRegressor|0\.43|0\.49|0\.79|0\.14|
|BayesianRidge|0\.43|0\.49|0\.80|0\.28|
|ElasticNetCV|0\.43|0\.48|0\.80|8\.18|
|LassoCV|0\.43|0\.48|0\.80|3\.75|
|LinearSVR|0\.41|0\.47|0\.81|0\.37|
|PoissonRegressor|0\.38|0\.44|0\.83|0\.68|
|OrthogonalMatchingPursuit|0\.32|0\.39|0\.87|0\.03|
|OrthogonalMatchingPursuitCV|0\.32|0\.39|0\.87|0\.06|
|TweedieRegressor|0\.32|0\.39|0\.87|0\.13|
|AdaBoostRegressor|0\.30|0\.37|0\.88|0\.58|
|LassoLarsIC|0\.29|0\.36|0\.89|0\.09|
|LassoLarsCV|0\.29|0\.36|0\.89|0\.14|
|LarsCV|0\.24|0\.32|0\.92|0\.32|
|PassiveAggressiveRegressor|-0.01|0\.10|1\.06|0\.04|
|LassoLars|-0.11|0\.00|1\.11|0\.03|
|Lasso|-0.11|0\.00|1\.11|0\.04|
|ElasticNet|-0.11|0\.00|1\.11|0\.16|
|DummyRegressor|-0.11|0\.00|1\.11|0\.11|
|KernelRidge|-5.11|-4.49|2\.61|0\.21|
|Lars|-2898728042565506560.00|-2605292087425108480.00|1798279628.07|0\.11|
|RANSACRegressor|-4059618571555627285348352.00|-3648666583111818362748928.00|2128119953373.86|2\.50|

#### **Performance Table for the Test Set Predictions:**

Test set was prepared from 20% subset of the data. KNeighborsRegressor showed the best performance out of all the models for test set prediction.

|**Model**|**Adjusted R-Squared**|**R-Squared**|**RMSE**|**Time Taken**|
| :-: | :-: | :-: | :-: | :-: |
|KNeighborsRegressor|0\.24|0\.55|0\.70|0\.04|
|NuSVR|0\.23|0\.54|0\.70|0\.30|
|SVR|0\.22|0\.54|0\.71|0\.35|
|HistGradientBoostingRegressor|0\.19|0\.52|0\.72|2\.15|
|RandomForestRegressor|0\.19|0\.52|0\.72|2\.59|
|LGBMRegressor|0\.18|0\.51|0\.73|0\.19|
|GradientBoostingRegressor|0\.17|0\.51|0\.73|1\.00|
|BaggingRegressor|0\.13|0\.48|0\.75|0\.20|
|XGBRegressor|0\.12|0\.48|0\.75|0\.21|
|BayesianRidge|0\.04|0\.43|0\.78|0\.08|
|ElasticNetCV|0\.03|0\.42|0\.79|4\.60|
|RidgeCV|0\.03|0\.42|0\.79|0\.11|
|MLPRegressor|0\.03|0\.42|0\.79|2\.62|
|LassoCV|0\.03|0\.42|0\.79|3\.79|
|SGDRegressor|0\.02|0\.42|0\.79|0\.11|
|Ridge|0\.01|0\.41|0\.80|0\.04|
|PoissonRegressor|0\.00|0\.41|0\.80|0\.30|
|HuberRegressor|-0.00|0\.40|0\.80|0\.14|
|LinearSVR|-0.02|0\.39|0\.81|0\.40|
|TweedieRegressor|-0.04|0\.38|0\.81|0\.25|
|OrthogonalMatchingPursuitCV|-0.10|0\.35|0\.84|0\.06|
|OrthogonalMatchingPursuit|-0.10|0\.35|0\.84|0\.03|
|LassoLarsCV|-0.10|0\.34|0\.84|0\.13|
|LassoLarsIC|-0.10|0\.34|0\.84|0\.06|
|ExtraTreesRegressor|-0.11|0\.34|0\.85|2\.39|
|ExtraTreeRegressor|-0.12|0\.34|0\.85|0\.06|
|DecisionTreeRegressor|-0.14|0\.32|0\.85|0\.06|
|LarsCV|-0.15|0\.31|0\.86|0\.32|
|AdaBoostRegressor|-0.17|0\.31|0\.86|0\.29|
|PassiveAggressiveRegressor|-0.53|0\.09|0\.99|0\.04|
|ElasticNet|-0.68|-0.00|1\.04|0\.03|
|LassoLars|-0.68|-0.00|1\.04|0\.06|
|Lasso|-0.68|-0.00|1\.04|0\.03|
|DummyRegressor|-0.68|-0.00|1\.04|0\.03|
|GaussianProcessRegressor|-5.24|-2.71|2\.00|0\.84|
|KernelRidge|-9.50|-5.25|2\.59|0\.20|
|TransformedTargetRegressor|-3631116534034068406272.00|-2161878630089647718400.00|48254656737.02|0\.04|
|LinearRegression|-3631116534034068406272.00|-2161878630089647718400.00|48254656737.02|0\.04|
|RANSACRegressor|-1406450438909158473334784.00|-837366446286955691900928.00|949688881519.97|1\.96|
|Lars|-2044047148197643972710428093969380912660480.00|-1216976047770851657639056845675146923474944.00|1144892186851303096320.00|0\.12|

#### **Data visualization of Model performance:**

## **Conclusions:**

- Random Forest shows close correlation between experimental and predicted pIC50 values.
- The pIC50 values of bioactivity class (active and inactive) were extremely different.
- LogP and NumHAcceptors are significant descriptors for our model, since they are able to differentiate between active and inactive class. MW and NumHDonors, however, are not significant, since they are not able to distinguish two classes. 
- Based on R<sup>2</sup> and RMSE values, KNeighborsRegressor was the best performer for test set, while Decision Tree Regressor was the best regressor for train set predictions.
## **References**
1. Adrià Cereto-Massagué, M. J.-V. (2015). Molecular fingerprint similarity search in virtual screening. *Methods*. doi:https://doi.org/10.1016/j.ymeth.2014.08.005.
1. Chen, X. (2020). Analysis of the Physicochemical Properties of Acaricides Based on Lipinski's Rule of Five. *Journal of Computational Biology*. doi:https://www.liebertpub.com/doi/abs/10.1089/cmb.2019.0323
1. Cherkasov, A. e. (2014). QSAR Modeling: Where Have You Been? Where Are You Going To? *Journal of Medicinal Chemistry*. doi:10.1021/jm4004285
1. Du, Y. X. (2023). Integrated Learning Activity Prediction Model of BHO-AdaBoosting Anti-Breast Cancer ERα Inhibitor Based on Improved Random Forest. *Preprints*. doi:https://doi.org/10.20944/preprints202308.1209.v1
1. Lipinski, C. A. (2001). Experimental and computational approaches to estimate solubility and permeability in drug discovery and development settings. *Advanced drug delivery reviews*. doi:https://doi.org/10.1016/s0169-409x(00)00129-0
1. M., A.-T. (2021). Considerations to Be Taken When Carrying Out Medicinal Plant Research—What We Learn from an Insight into the IC50 Values, Bioavailability and Clinical Efficacy of Exemplary Anti-Inflammatory Herbal Components. *Pharmaceuticals*. doi:https://doi.org/10.3390/ph14050437
1. Nachar, N. (2008). The Mann-Whitney U: A Test for Assessing Whether Two Independent Samples Come from the Same Distribution. *Tutorials in Quantitative Methods for Psychology*. doi:10.20982/tqmp.04.1.p013
1. Patrick E. McKnight, J. N. (2010). Mann-Whitney U Test. doi: https://doi.org/10.1002/9780470479216.corpsy0524
1. Sandra L. Grimm, S. M. (2016). Progesterone Receptor Signaling Mechanisms. *Journal of Molecular Biology*. doi:https://doi.org/10.1016/j.jmb.2016.06.020.





