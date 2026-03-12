# NLP CWK
## Overview
Given a text extracted from a news article, determine whether it contains patronizing or condescending language (PCLL) toward vulnerable communities.
This project aims to improve on the RoBERTa-base baseline model for classifying text into PCL (patronising and condescending language) vs non-PCL, which achieved an F1 score of 0.48. 

Dataset used: https://github.com/Perez-AlmendrosC/dontpatronizeme 
Contains more than 10,000 paragraphs extracted from English-language news stories spanning 20 different countries, annotated to indicate the presence of PCL.


## Model architecture and training

Training used the AdamW optimizer with a cosine annealing learning rate scheduler, which helps stabilize training on smaller datasets by smoothly decreasing the learning rate over time. The model used a two-head architecture where both classification heads shared the [CLS] representation from the RoBERTa encoder: one head performed the primary binary classification task, while the second head performed an auxiliary multilabel classification task on the categories of PCL present within a given text. The parameter λ controls the contribution of the auxiliary task to the overall loss, weighting the multilabel loss relative to the binary classification loss during training.

| Hyperparameter              | Value        |
|-----------------------------|--------------|
| Base Model                  | RoBERTa-base |
| Max Epochs                  | 10           |
| Batch Size                  | 16           |
| Weight Decay                | 0.01         |
| Learning Rate               | 2e-5         |
| Best Lambda Found           | 0.5          |
| Early Stopping Patience     | 3            |



## Dev set performance 

```
              precision    recall  f1-score   support

     Non-PCL       0.95      0.97      0.96      1895
         PCL       0.66      0.48      0.56       199

    accuracy                           0.93      2094
   macro avg       0.80      0.73      0.76      2094
weighted avg       0.92      0.93      0.92      2094

```
