# cm2metrics

A lightweight package that analyzes multiple metrics directly from
confusion matrix.

## Features
- Get all classes parsing results in native _Dataframe_ format
- Print all classes or specified class parsing summary in friendly format
- Only requires _numpy_ and _pandas_, without relying on other machine learning packages.
- Easy to use with a few  APIs
- Supports **16 metrics** for each class:
    * **tp**: true positive
    * **tn**:  true negative
    * **fp**: false positive
    * **fn**: false negative
    * **tpr**: true positive rate
    * **tnr**: true negative rate
    * **fpr**: false positive rate
    * **fnr**: false negative rate
    * **atc**: actual true count
    * **afc**: actual false count
    * **ptc**: predict true count
    * **pfc**: predict false count
    * **accruacy**
    * **precision**
    * **recall**
    * **f1**

## General 
- **Version**: 0.1
- **Dependency**: Python(3.6,3.7.3.8), numpy, pandas

## Install
```
pip install cm2metrics
```

## Use
#### General use

Generate a confusion matrix
<pre>
#  use scikitlearn
from sklearn.metrics import confusion_matrix

#cm is ndarray, convert to dataframe
cm = confusion_matrix(true_target, pred_target)
df_cm = pd.DataFrame(cm, index=class_names, columns=class_names)

# or, use a randomly generated confusion matrix(for test)
# see details in cm_test.py
class_names = {0:"class0", 1:"class1", 2:"class2"}
df_cm = pd.DataFrame([(1,2,3),(4,5,6),(7,8,9)])
df_cm.rename(index=class_names, columns=class_names, inplace=True)
</pre>

Init a confusion matrix parser
<pre>
from cm2metrics.parse_cm import  ConfusionMatrixParser
cm_parser = ConfusionMatrixParser(df_cm)
</pre>

Parse the confusion matrix
<pre>
# parsing result(cm_parsed) is a dataframe
cm_parsed = cm_parser.parse_confusion_matrix()
print(cm_parsed)  

Sample output:

        tp  fp  tn  fn       tpr       fpr       tnr       fnr  atc  afc  ptc  pfc  accuracy  precision    recall        f1
class0   1  28  11   5  0.166667  0.717949  0.282051  0.833333    6   39   12   33  0.644444   0.083333  0.166667  0.111111
class1   5  20  10  10  0.333333  0.666667  0.333333  0.666667   15   30   15   30  0.555556   0.333333  0.333333  0.333333
class2   9  12   9  15  0.375000  0.571429  0.428571  0.625000   24   21   18   27  0.466667   0.500000  0.375000  0.428571


# get class0 true positive
tp = cm_parsed.loc["class0"].at["tp"]
</pre>

#### Print parsing summary in friendly format
<pre>
# print one class summary by name using class_name parameter
cm_parser.print_summary(class_name="class0")

# print one class summary by index in confusion matrix using class_index parameter
cm_parser.print_summary(class_index=0)

# print all classes summary by not specifying parameters
cm_parser.print_summary()

Sample output for class0 summary:
Summary for class0
TP: 1
TN: 28
FP: 11
FN: 5
TPR: 0.167
TNR: 0.718
FPR: 0.282
FNR: 0.833
Actual true count: 6
Actual false count: 39
Predict true count: 12
Predict false count: 33
Accuracy: 0.644
Precision: 0.083
Recall: 0.167
F1: 0.111
</pre>

# License
MIT license.