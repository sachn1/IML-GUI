# Study and Visualization of Model Agnostic Interpretable Machine Learning Approaches

The main task was to implement few Interpretable Machine Learning models that could help answer some questions of the underlying classification black-box model. The main aim here is to help make the end-user trust the model and help one understand why it predicted the way it did. This could help the user make better decisions. 
  
## Dataset

The dataset considered for this particular task was the morphological and demographic features of 100 intracranial aneurysms corresponding to 93 patients recorded at the university hospital of Magdeburg, Germany. There were few missing values in the otherwise clean data. Due to the issues of data privacy, the dataset is not uploaded in the repository along with the source code and other resources. 

## Model Design

Even though the intent of the task was not to come up with the best model, two algorithms were chosen based on the motivation from http://wwwisg.cs.uni-magdeburg.de/visualisierung/wiki/data/media/files/misc/niemann_2018_cbms.pdf where the comparison between different models was performed on the same dataset - Gradient Boosting Trees (also XGBoost) and Support Vector Machines. The best model was chosen based on nested cross-validation of all three algorithms fit on three variations of the dataset - one where a standard normalization was applied, one where the dataset was transformed into Z-scale and one where no transformations were applied. Based on grid search hyperparameter tuning, XGBoost and SVM with a generalization performance of 65 and 64 per cent accuracy on the z-score transformed dataset were chosen for further study.

![alt text](img/performance_scores.PNG)
![alt text](img/Model_details.JPG)

## Questions
  1. Which are the important features that contributed towards the prediction of the aneurysm classes?
  2. Does change in a feature value affect prediction of an instance?
  3. What is the possible range of values a feature of an instance could take for a particular aneurysm class? Or, at which value of the feature does the classification label changes?
  4. Which minimal set of features played a significant role in the prediction of aneurysm classes and how?

## Interpretable Machine Learning Models

### Model Reliance
  ![alt text](img/PermutationFeatureImportanceAlgm.png)
  ![alt text](img/Model_reliance.PNG)
### Individual Conditional Expectation
  ![alt text](img/iceplot_gbt_ei_1.jpg)
### Counterfactuals
  ![alt text](img/counterfactual_gbt_52_beta.jpg)
### Decision Ruleset
![alt text](img/decision_ruleset.png)
## Graphical User Interface for the Visualization 
  ![alt text](img/GUI_sample.PNG)
