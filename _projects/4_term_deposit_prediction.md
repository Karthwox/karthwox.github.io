---
layout: page
title: Predicting term-deposit sign-ups
description: An end-to-end ML case study on 45,000 banking records, from the Finlatics data science internship
importance: 1
category: machine learning
---

A case study from my data science internship with Finlatics (remote, October 2025). The data came from a Portuguese bank's direct telephone marketing campaigns, about 45,000 customer records, and the task was to predict which customers would subscribe to a term deposit.

**Preprocessing.** I built a full pipeline in scikit-learn: imputing missing values, one-hot encoding the categorical variables, and normalising features with `StandardScaler`.

**Models.** I trained and compared three classifiers, each with a full classification report:

- **Random Forest,** the most accurate overall
- **Logistic Regression,** the best balance of F1 score and speed
- **SVM,** the highest F1 score

The internship ended with a certificate of completion and a letter of recommendation.

_Python, scikit-learn, NumPy, Pandas, Matplotlib._
