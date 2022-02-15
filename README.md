## Project Summary: Improving Customer Churn Modeling.

Customer churn has been predicted by a combination of [three variables](view/data/baseline.csv). Accuracy is good, but the ROC curve and the confusion matrix show room for improvement. This project uses an [expanded dataset](view/data/churndata.csv) and seeks to get better performance.


![image](raw/latest/results/AUC_ACC_Baseline.png)  ![image](raw/latest/results/ConfMatx_Baseline.png)


## Project Components


### Building the Model
* [DataPrep.ipynb](view/code/DataPrep.ipynb) ingests the new variables mentioned above, cleans and prepares the data. This is be scheduled to run on a weekly basis.
* [GB_ParamSearch.py](view/code/GB_ParamSearch.py) takes that prepared data and runs an experiment to determine the best loss function and # of estimators for the Gradient Boosting algorithm. The following are examples of how to make the call.
    - code/GB_ParamSearch.py exponential 10
    - code/GB_ParamSearch.py exponential 50
    - code/GB_ParamSearch.py deviance 10 
    - code/GB_ParamSearch.py deviance 50
    
### Hyperparameter Optimization
* [HyperParam_Opt.py](view/code/HyperParam_Opt.py) provides batch method to tune a selection of hyperparameters: GBM vs Adaboost; # of estimators; learning rate; and loss function where applicable. Loss function does not have to explicitly called, and will default to exponential.
    - code/HyperParam_Opt.py gb 40 1 exponential
    - code/HyperParam_Opt.py gb 80 1
    - code/HyperParam_Opt.py gb 80 0.5
    - code/HYperParam_Opt.py ada 80 0.5
    
* Additional metrics have been added to these runs for validation purposes: 
    - False Positives
    - False negatives 
    - Precision 
    - Recall
    - F1 score 


### Model Deployment and Model Health Monitoring
* [Modeling.ipynb](view/code/Modeling.ipynb) performs cross validation on a variety of models, identifies the optimal model, and saves that model as a pickle file.
* [predict.py](view/code/predict.py) loads the best model and builds a function used to create a real time API for churn prediction.

* A [model health scheduled run](https://vip.domino.tech/u/joshpoduska/customer-churn/scheduledruns) executes daily and sends an email to project collaborators if any of the model inputs or outputs are "out of bounds".
* A [Customer Churn Shiny App](https://demo.dominodatalab.com/modelproducts/5d4c5b1946e0fb00070eaa84) allows decision makers to review the likelihood to churn for any customer, along with explanations why. The app also displays model health data to track the performance of the model in production.






******


## Example Template - Project Preflight Checklist
- Business case definition 
    - State the business case including any business KPIs we hope to influence
    - ROI math including t-shirt size estimates of value, effort and risk
- Stakeholder mapping
    - Define responsible parties from each group: data science, business, DevOps, application dev, compliance, etc.
    - Treat as cross-functional teams
- Technology needs
    - Consider opportunities to accelerate research
    - Identify dependencies early
- Data availability
    - Leverage existing sources first to build baseline
- Prior art review
    - internal and external review
- Model delivery plan
    - Design multiple mock-ups of different form factors
    - Designate approvers in advance (IT,DS, business)
    - Create process flow to precisely show where model will impact
    - Proceed incrementally to get feedback from real usage
    - Do not engineer beyond the requirements
    - Make a plan for monitoring in production
- Success measures
    - Pre-emptively answer “how will we know if this worked?”
    - Frame in terms of business KPIs not statistical measures
    - Define needs for holdout groups, A/B testing, etc.
    - Define testing infrastructure
- Compliance and regulatory checks
    - Consider consequences of errors (e.g., false positives / negatives)
    - State likely biases in training data
    - Is explainable modeling needed?
    - Track ongoing usage to prevent inappropriate consumers