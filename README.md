# Pneumonia-Udacity
This is the first project in fulfilment of the **AI for Healthcare** *Udacity Nanodegree*. The project uses chest X-ray images and includes data processing and EDA, building a diagnostic model in keras, and inspecting FDA proposal requirements.   

## Dataset
Chest X-ray images are provided by [Kaggle](https://www.kaggle.com/nih-chest-xrays/data) which contains more than 112k files for over 30k patients. The labels have been produced by NLP with more than 90% accuracy and include diseases such as Pneumonia, Infiltration, Edema, Mass, Cardiomegaly, etc and their comorbid.

## Project Phases
### Phase 1: EDA
[Data analysis](EDA.ipynb) is performed in jupyter notebook using mainly *pandas* package to address the following points:
-	The patient demographic data such as gender, age, etc.
-	The x-ray views or view position.
-	Sorting cases based on pneumonia and non- pneumonia
-	The distribution of other diseases that are comorbid with pneumonia
-	Number of diseases per patient
-	Histogram analysis of the imaging data for healthy & disease states of interest and compare distributions across diseases.


### Phase 2: Building and Training the Model
#### Data processing
This starts with curating data according to EDA performed. First, each case is assigned with a pneumonia status label (binary value indicating whether pneumonia is diagnosed in the image). 
Train and test dataset are created by taking into account the data demographic distribution in either dataset. The train dataset is composed on 50% positive pneumonia cases while the test dataset consists of 20% positive pneumonia cases to reflect real-life pneumonia case rates.

#### Image preprocessing
To improve the training, image augmentation is applied by adding shear, flips, etc. Moreover, a costume function is defined to initially crop the center to focus on the lung region. This allows for better image processing by eliminating tons of redundant information that would be extracted from unnecessary and irrelevant body parts such as arms and abdomen. The image is later normalized and standardized using mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225] which are the recommended values for VGG model.

#### Model definition
Using *transfer learning* technique, VGG16 model is loaded and last layer is tunned to specifically classify pneumonia data. Since this is a binary classification, loss and metric functions are set to *binary cross-entropy* and *binary accuracy*. 

Adam optimizer is chosen with initial learning rate ($l_r$) of 0.001 and $\beta_1=0.3$. the learning rate is set to decrease 10-fold upon 5 unchanged validation loss down to $10^{-6}$. *Early stopping* criteria is set if no change is observed in validation loss after 12 attempts. Training is performed with 100 epochs and batch size of 32.

The hyperparameters values have been set after several trails. Some of the attempts with different values are presented in the notebook file.

