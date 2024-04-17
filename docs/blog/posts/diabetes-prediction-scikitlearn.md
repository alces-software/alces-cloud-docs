---
authors:
  - shubhamdang
date: 2024-04-15
categories:
  - se1
readtime: 2
---

# Building a Diabetes Prediction Model Using Scikit-Learn
In this blog post, we will provide a comprehensive guide on how to build a diabetes prediction model using Scikit-Learn. Our setup revolves around a single node with Jupyter notebook installed.


Let's start with a step-by-step process, starting from creating virtual machines on the Alces Cloud platform, followed by installation of jupyter lab and notebook and then building the predition model.
<!-- more -->

Diabetes is a chronic disease that affects millions of people worldwide. Early diagnosis and treatment are crucial for managing the condition and preventing complications. With the advancement in machine learning and artificial intelligence have made it possible to develop accurate predictive models for diseases like diabetes. we will use the Pima Indians Diabetes Database. Our objective is to predict whether or not a patient has diabetes based on certain diagnostic measurements.


## Launch the Instance  
All the steps to launch and connection to instance is provided in [link](../../docs/starter/instance.md).

## Install Jupter Notebook and Lab
All the steps to install jupter notebook and lab is provided in [link](./jupyter-lab-notebook.md).


## Building the Diabetes prediction Model
- The first step in building any predictive model is preparing the data. Let's start by loading the data into a Pandas DataFrame. The data contains eight features related to metabolism, insulin resistance, and glucose tolerance, along with one binary outcome variable indicating whether or not the patient has diabetes.

    ```
    ln []:  import pandas as pd
            url = "https://gist.githubusercontent.com/ktisha/c21e73a1bd1700294ef790c56c8aec1f/raw/819b69b5736821ccee93d05b51de0510bea00294/pima-indians-diabetes.csv"
            columns = ["num_pregnant", "glucose_conc", "blood_pressure", "skin_thickness", "insulin", "bmi", "diabetes_pedigree", "age", "outcome"]
            df = pd.read_csv(url, names=columns, header=None)[9:]
    ```

- Next we need to convert the categorical variable outcome to dummy variables and scale the continuous variables.
    ```
    ln []:  # Convert categorical variable to dummy variables
            dummy_cols = ["num_pregnant", "outcome"]
            for col in dummy_cols:
                if col == "outcome":
                    dummies = pd.get_dummies(df[col], prefix=[col])
                    df = df.drop([col], axis=1).join(dummies)
                else:
                    dummies = pd.get_dummies(df[col].astype('category'), prefix=[col])
                    df = df.drop([col], axis=1).join(dummies)

            # Scale continuous variables
            continuous_cols = ["glucose_conc", "blood_pressure", "skin_thickness", "insulin", "bmi", "diabetes_pedigree", "age"]
            scaler = StandardScaler()
            df[continuous_cols] = scaler.fit_transform(df[continuous_cols])

            # Separate features and target
            X = df.iloc[:, :-1]
            y = df.iloc[:, -1]
    ```

- Split the data into training and testing sets using train_test_split from scikit-learn:

    ```
    ln []:  from sklearn.model_selection import train_test_split
            X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
    ```


- Now that our data is prepared, we can create and fit a logistic regression model using scikit-learn:

    ```
    ln []:  from sklearn.linear_model import LogisticRegression
            lr_model = LogisticRegression(max_iter=1000)
            lr_model.fit(X_train, y_train)
    ```


- Now evaluate the performance of our model, we make predictions on the testing set and calculate evaluation metrics such as accuracy, confusion matrix, and classification report:

    ```
    ln []:  from sklearn.metrics import accuracy_score, confusion_matrix, classification_report

            y_pred = lr_model.predict(X_test)
            accuracy = accuracy_score(y_test, y_pred)
            confusion_mat = confusion_matrix(y_test, y_pred)
            classification_report_str = classification_report(y_test, y_pred)

            print("Accuracy:", accuracy)
            print("\nConfusion Matrix:\n", confusion_mat)
            print("\nClassification Report:\n", classification_report_str)

    Output
    ------
    Accuracy: 1.0

    Confusion Matrix:
    [[99  0]
    [ 0 55]]

    Classification Report:
                    precision    recall  f1-score   support

        False         1.00      1.00      1.00        99
        True          1.00      1.00      1.00        55
        accuracy                          1.00       154
        macro avg     1.00      1.00      1.00       154
        weighted avg  1.00      1.00      1.00       154
    ```

These metrics give us insights into how well our model performs on unseen data. Accuracy measures the percentage of correct predictions out of all predictions made, while the confusion matrix shows the number of true positives, false negatives, true negatives, and false positives. 

The classification report displays precision, recall, f1-score, and support for each class. These additional metrics help us understand how well our model performs across different classes, especially when there is imbalance between them.
