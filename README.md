# Deep Similarity Using EHR
This is an attempt to reproduce the paper ["A Novel Deep Similarity Learning Approach to Electronic Health Records Data"](https://ieeexplore.ieee.org/document/9257424)

    V. Gupta, S. Sachdeva and S. Bhalla, ”A Novel Deep Similarity Learning Approach to Electronic Health Records Data,” in IEEEAccess, vol. 8, pp. 209278-209295, 2020, doi:10.1109/ACCESS.2020.3037710


# Setup
All the code is in Jupyter notebooks. It is important to setup a virtal environement and jupyter kernel 

## 1. Create a Virtual Environemnt
This project requires Python 3.7+

Setup a virtual environment using the following commands 

`python3 -m venv ~/.venv` 

## 2. Active virtual environment
`source ~/.venv/bin/activate`
`pip install --upgrade pip`

## 2. Install dependencies
All the dependencies are specified in `requirements.txt` file. Install dependencies using

`pip install -r requirements.txt`

## 3. Create Jupyter kernel to use with jupyter notebooks

`python -m ipykernel install --name=my-kernel`

## 4. Start jupyter notebook

`jupyter notebook`

Open the notebooks and choose kernel "my-kernel" to run the notebooks. 


# Files

# Data
## Raw Data 
The paper uses OpenEHR data which is a benchmark data available at http://www.lampada.uerj.br/en/orbda/

Data can be imported to PostgreSQL database using the scripts provided on the webiste. For this work,  nephrology data was downloaded from the database and saved to csv. A sample of saved data is available under 'raw-data' folder. 
* data-nephrology-s.csv.gz - Sample of ORBDA Nephrology data. This is just a 25% of data. Full data file is too big to store here. Complete data can be downloaded from orbda website. For this work, pre-processed data is available under "processed-data".   

## Preprocessed Data
* For reusing data with different models, preprocessed data is saved under `prcocessed-data` folder. 
    * data-interpolated.csv.gz - Pre-processed and interpolated data
    * data-normlized.csv.gz - Normalized data 
    * p_x.pt.gz - Patient data with multiple visits grouped. Size (18432, 40, 42). Visits are clipped to 40 with 42 features. Uncompress this before using with the model 
    * p_y.pt.gz - Target label for the patient. Visits are sorted and last label is used. Patient can have more than one label across all the visits. The dataset is specifically for chronic kidney failures. So, visits are sorted by date and picking the last one to represent the patient. Uncompress this before using with the model
    * p_y_oh.pt.gz - One hot encoded data for the target lables. Uncompress this before using with the model

# Baseline models
* baseline-ml.ipynb - Baseline ml models using the following 
    * Logistic Regression
    * Naive Bayes 
    * KNN
    * Decision Trees
* baseline-cnn.ipynb - Baseline CNN model for muti-class classification
* baseline-rnn.ipynb - Baseline RNN model for muti-class classification
* baseline-mlp.ipynb - Baseline MLP model for muti-class classification
* baseline-cnn-cosine-euclidean-manhattan.ipynb - Baseline cnn model for patient similarity using cosine similiarty, euclidean and manhattan distance. 

All the baseline CNN models are using 2d convolution

# CNN Softmax
* cnn-softmax.ipynb - Patient similarity using CNN softmax. 
* cnn-softmax-balanced-data.ipynb - Patient similarity using CNN softmax using a balanced dataset. 


# Results 
## Baseline ML Models
| Method              | Accuracy | Recall | Precision | F1 Score |
| :------------------ | :------- | :----- | :-------- | :------- |
| Logsitic Regression | 0.9825   | 0.9825 | 0.9653    | 0.9738   |
| Naïve Bayes         | 0.9825   | 0.9825 | 0.9653    | 0.9738   |
| KNN                 | 0.9817   | 0.9817 | 0.9696    | 0.9742   |
| Decision Trees      | 0.9762   | 0.9762 | 0.9717    | 0.9737   |

## Baseline DNN Models
| Method | Accuracy | Loss   |
| :----- | :------- | :----- |
| RNN    | 0.9000   | 1.9711 |
| CNN    | 0.9937   | 1.4911 |
| MLP    | 0.9829   | 1.4788 |

## Baseline deep similairy Models
| Method        | Accuracy | Recall | Precision | F1 Score |
| :------------ | :------- | :----- | :-------- | :------- |
| CNN Manhattan | 0.9533   | 0.4993 | 0.4773    | 0.4881   |
| CNN Euclidean | 0.9533   | 0.5013 | 0.5305    | 0.4921   |
| CNN Cosine    | 0.2833   | 0.5750 | 0.5174    | 0.2527   |

## CNN Softmax 
| Method      | Accuracy | Recall | Precision | F1 Score |
| :---------- | :------- | :----- | :-------- | :------- |
| CNN Softmax | 0.9945   | 0.9942 | 0.9750    | 0.9844   |

## CNN Softmax using balanced data 
| Method      | Accuracy | Recall | Precision | F1 Score |
| :---------- | :------- | :----- | :-------- | :------- |
| CNN Softmax | 0.9361   | 0.7337 | 0.9658    | 0.7830   |