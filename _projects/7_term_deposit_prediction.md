---
layout: page
title: Predicting term-deposit sign-ups
description: An end-to-end ML case study on 45,000 banking records, from the Finlatics data science internship
importance: 1
category: machine learning
---

A case study from my data science internship with Finlatics (remote, October 2025). The data came from a Portuguese bank's direct telephone marketing campaigns: about 45,000 customer records, each labelled by whether the customer subscribed to a term deposit. The task was to predict who would.

The catch is that only about **12%** of customers said yes. A model can be 88% accurate by predicting "no" for everyone, so accuracy alone is misleading. What matters to a marketing team is how many of the real subscribers the model finds (recall) and how many of its "yes" calls are right (precision).

## Preprocessing

A scikit-learn pipeline that drops duplicate columns, imputes missing and "unknown" values with the most frequent category, converts yes/no columns to binary, one-hot encodes the categorical variables and standardises the numeric ones. An 80/20 train/test split leaves 9,044 test records.

## Results

| Model                          | Accuracy | Subscribers found (recall) | Precision on "yes" | F1 on "yes" |
| ------------------------------ | -------- | -------------------------- | ------------------ | ----------- |
| Random Forest                  | **0.90** | 0.37                       | **0.64**           | 0.47        |
| Logistic Regression (balanced) | 0.84     | 0.81                       | 0.41               | 0.54        |
| SVM (balanced)                 | 0.84     | **0.86**                   | 0.41               | **0.56**    |

Random Forest has the best accuracy but finds only about a third of the actual subscribers. The two class-balanced models give up some accuracy to find more than 80% of them. That is the better trade for a calling campaign, where a missed subscriber costs more than an extra call. Logistic Regression gets close to the SVM's F1 score at a fraction of the training time.

The internship ended with a certificate of completion and a letter of recommendation.

_Python, scikit-learn, NumPy, Pandas, Matplotlib._
