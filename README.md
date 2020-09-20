# Multilabel classification problem solution for Reuters-21578

### AIM OF THE PROJECT
The objetive of this project is to test different appraches to NLP problem of multilabel classification and find optimal model that would properly predict document "subject" labels based on a raw document text. <br>
<br><br>
The multilabel classification is a classification for which:
* Number of targets >1
* Number of targets for each sample is a-priori unknown and can differ for different samples
* Target cardinality is equal to 2 (0 or 1) == the sample belongs to a certain target (category) or not

### DATASET USED
Reuters-21578

The full set is available here:
https://archive.ics.uci.edu/ml/datasets/reuters-21578+text+categorization+collection

For the project needs, the author used dataset available in NLTK library which is already split to train and test. It can be loaded to the memory by a command:
*from nltk.corpus import reuters*

The dataset contains:
* almost 8000 train articles
* over 3000 test articles
* there is 90 unique labels
* In average only 1.23 labels are assigned to each train set article

### SUMMARY OF THE PROJECT

#### DATA CLEANING

Before applying a model - the data was cleaned, tokenized and normlized.

#### DATA PRE-PROCESSING

In order to find a best approach to this problem, three different approaches were taken for the words embedding:
* Tf-Idf
* Doc2Vec (model trained on the train corpus)
* FastText (pre-trained model, weights applied in embedding layer of CNN)

#### MODELLING

1. The target labels were binarized with MultiLabelBinarizer
2. A few different models were fitted using pipeline and GridSearchCV (for different pre-processing methods):
    * Inherently supporting multilabels:
        * k-nearest neighbors
        * Random Forest
    * OneVsRest with Linear SVC and Logistic Regression
    * Label Powerset with Linear SVC
    * Convolutional Neural Network

#### METRICS
As in multilabel  - the sample belongs to few labels out of all set of labels, the absolute accuracy is not a good measure (as it will tag as correct only these samples which have all set of labels correclty predicted, without taking into account partial correctness (e.g. we predict correctly 3 out of 4 labels)).
It is proposed to use hamming loss function which would take into account partial correctness.

Each individual category predictions can be evaluated based on traditional metrics like F1 score, Precision and Recall, however, to evaluate full multilabel model, these metrics for each category shall be averaged. There are 2 approaches to do that: micro and macro.

Finally for this project, it was decided to evaluate the model based on a hamming loss and micro & macro f1-score.

Find explanation of micro and macro averaging here:
https://datascience.stackexchange.com/questions/15989/micro-average-vs-macro-average-performance-in-a-multiclass-classification-settin