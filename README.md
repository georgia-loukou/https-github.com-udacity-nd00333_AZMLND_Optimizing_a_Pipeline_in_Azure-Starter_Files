
This project is part of the Udacity Azure ML Nanodegree and the aim is to build and optimize an Azure ML pipeline using the Python SDK, compared to an Azure AutoML run.

Summary
This dataset contains data about peaple applying for bank loans. The task is to predict whether they will subscribe to a service.

Scikit-learn Pipeline
Steps taken:

Removing NAs from the dataset.
One-hot encoding job titles, contact, and education variables.
Encoding a number of other categorical variables.
Encoding months of the year.
Encoding the target variable.
Once the data has been prepared it is split into a training and test set. A test set size of 20% of total entries was selected as a compromise between ensuring adequate representation in the test data and providing sufficient data for model training.

Logistic regression was used as the classification method. The parameters available within the training script are learning rate, batch size and keep propability .

Azure's Hyperdrive service was used for hyperparameter tuning with the following key elements:


I decided to use random parameter sampling. I does not require pre-specified values (like grid search) and can make full use of all available nodes (unlike bayesian parameter sampling). Random parameter searching is preferable to gridsearch also in that it is highly unlikely that the specified values in gridsearch are optimal, while there is a chance that the ones obtained randomly are closer to ideal values. Combined with the fact that I have very little background in using logistic regression random parameter sampling enables a much more broad search over the parameter space and with it's capabilities to make use of concurrency would outperform bayesian sampling for large jobs.

I selected the BanditPolicy stopping policy because it allows one to select a cut-off at which models reporting metrics worse than the current best model are terminated. This allows a relatively intuitive method to screen models, only retaining those with similar or better performance. This policy offers a little more flexibility than truncation and median stopping.

I was getting an unexpected bad request error. 

AutoML
The autoML pipeline is very similar to the Scikit-learn pipeline described above with several notable differences:

Data retrival from the provided URL.
Data cleaning using the same process as described above.
The joined dataset is used as input in the autoML configuration and the autoML run is processed locally.
The best model selected by autoML was a MaxAbsScaler XGBoostClassifier   (~91.53% accurate). 
