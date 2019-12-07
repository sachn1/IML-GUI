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

Model-agnostic interpretation methods provide great deal of flexibility. This means, various interpretation methods can anyway be applied irrespective of which machine learning model is used. On the other hand, Model-specific interpretation methods restrict  the  explanation to that model alone. For example, algorithms like RandomForest and XGBoost automatically calculates important features on a trained predictive model. The second distinction is between global and local feature importance. Local feature importance evaluates the importance of features in case of prediction of a sub-problem. Whereas, global feature importance considers the overall predictions into account. Thus, to define the most important global features irrespective of the model used, we went for a model agnostic approach called model reliance (MR). MR is a permutation feature importance algorithm. The whole process of permutation feature importance is as explained in the figure given below. 
  ![alt text](img/PermutationFeatureImportanceAlgm.png)
  
We implemented model reliance for XGBoost and SVM models and have provided the results in terms of a horizontal bar chart. The MR measure for each instance was the subtraction between the two errors. This could possibly be a workaround when the original estimated error is zero.  
  ![alt text](img/Model_reliance.PNG)
### Individual Conditional Expectation

A black-box model can have complicated parameters which can range from a few thousand to millions for a real-world dataset. A key question about model interpretation is 'How does the model inputs work?' or to rephrase 'How does change in a particular variable affect the model's prediction?'. One of the ways as discussed in the previous section, feature importance shows the strength of the relationship between a variable and model's prediction. But it lacks in providing any functional relationship between model inputs and predictions.  

Individual conditional expectations (ICE) is a disintegrated form of Partial Dependence Plot (PDP) from a visual perspective. It plots one individual line graph for every instance which shows the output change when values of feature changes. Each line establishes a homogeneous (if any) relationship between an observation with different values of the feature in focus thereby giving end-user several inferences of conditional relationships modeled by the black-box algorithm.   
  ![alt text](img/cen_ice_plot_feat_ch_volume.jpg)
  The above ICE plot shows that of the feature 'ch_volume' where x-axis shows range of feature value and y-axis shows values ranging from 0 to 1 showing probability of rupture classification. Each line also shows rupture status and a PDP shows overall trend.
  
### Density plots to explain Counterfactuals

Counterfactual explanations are in form of 'if x had not occurred, y would not have occurred'. Counterfactual examples are hypothetical instances which flips the prediction of the original instance. Due to the large number of instances, it is usually challenging to represent the higher number of counterfactuals in a more meaningful format. So for this task, the nearest counterfactual for every feature is considered and hence shown using a density plot. 

We select one instance from the data in order to explain the learned model. We use range of each feature (min value of the feature in the dataset and max value of the feature in the dataset) to create example counterfactual value. For each instance, we change only one feature value at a time and replace it with the calculated feature value to create our example counterfactual instance. We create 100 such example counterfactual instances. The reason for choosing 100 was that, each time the number of counterfactual instances for an instance would have exactly 99 (since the original dataset had 100) so that perturbing feature values within a range for a good number of instances would be a good justification rather than randomly choosing a number. We now look for counterfactual instances that can flip the target value from 'ruptured' to 'unruptured' or vice versa. We then predict the outcome of example instances using our model. We then return the first example instance that changes the target value of the instance as our counterfactual instance. 
  ![alt text](img/CF_plot_inst_77_feat_ch_volume.jpg)
The above counterfactual represents the density plot of the feature 'ch_volume' for the instance number 77. The original value of the instance is represented by orange dashed line and the counterfactual value is represented by blue dashed line. As visible from the plot, the target value of the instance 77 changes from ruptured to non ruptured if the value of ch\_volume changes from 48.07 to 52.83. 

### Decision Ruleset
![alt text](img/decision_ruleset.png)
## Graphical User Interface for the Visualization 

The objective of the whole task is to design and implement a prototypical GUI that explains the above mentioned four model agnostic approaches. Hence, we came up with a GUI based solution called G-MARC (GUI for Model Agnostic explanations for Rupture status Classification).  It is implemented using the PyQt software which is a FOSS widget toolkit for creating GUIs. The GUI is divided into 4 tabs for each of the model agnostic techniques. Each tab provides the option for the user to select the model for which he/she wants the result.  In addition to that, various other options such as selecting the features, type of plot, choosing instances and selecting a particular aneurysm class are provided based on the requirement of the technique.  The GUI is also made user-friendly by providing a toolbar for the plots where the user can stretch or adjust the plot, zoom values, modify the layout and axes and also save each plot as an image to his/her local system
  ![alt text](img/GUI_sample.PNG)
  
### Add-on Feature - Report Generation and Download as PDF

In addition to the tasks, I have included an option for the user to download the report with the necessary information and plots of each task. Saving and writing the plots (as images) to the report is quite tedious in terms of time complexity. So, this process has been implemented as a separate thread to make sure that the GUI does not become non-responsive.  Also a Help option is provided in the menu bar which is moreover a guide for the user regarding the GUI.

## Product Release

The  whole  application  has  been  converted  into  a  single  standalone  exe-cutable.  The GUI with all the features are compiled into a Windows (platform-specific) executable file with the help of cxfreeze package of python.  This helps the  user  to  install  the  application  without  manual  installation  of  python  or any of its dependencies by running the msi file by following the INSTALL.txt provided along with it. But sometimes there might be some dll dependencies that aren’t sorted out automatically by cxfreeze.
