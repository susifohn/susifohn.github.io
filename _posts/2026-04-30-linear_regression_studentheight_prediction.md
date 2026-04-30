---
title: 3c. Predict Student height using Linear Regerssion
categories: [Machine Learning, linear regression ]
tags: [gibb, tsb, exercise]     # TAG names should always be lowercase
math: true
---

# Class Exercise: Linear Regression
### Machine Learning Basics — Group Activity

---

## Introduction

Linear regression is one of the most fundamental machine learning techniques. It models the relationship between an **input feature** (X) and a **target variable** (Y) by fitting a straight line through the data.

The equation of a linear regression model is:

```
ŷ = β₀ + β₁ · x
```

Where:
- **ŷ** = predicted value (height in cm)
- **β₀** = intercept (value of ŷ when x = 0)
- **β₁** = slope (how much ŷ changes per unit of x)
- **x** = input feature

---

## Step 1 — Data Collection

Measure or collect the following data from each student in the class and fill in the table below.

| # | Name | Age | Shoe Size (EU) | Index Finger Length (cm) | Name Length (# letters) | Height (cm) |
|---|------|-----|---------------|--------------------------|-------------------------|-------------|
| 1 |      |     |               |                          |                         |             |
| 2 |      |     |               |                          |                         |             |
| 3 |      |     |               |                          |                         |             |
| 4 |      |     |               |                          |                         |             |
| 5 |      |     |               |                          |                         |             |
| 6 |      |     |               |                          |                         |             |
| 7 |      |     |               |                          |                         |             |
| 8 |      |     |               |                          |                         |             |
| 9 |      |     |               |                          |                         |             |
| 10 |     |     |               |                          |                         |             |
| 11 |     |     |               |                          |                         |             |
| 12 |     |     |               |                          |                         |             |
| 13 |     |     |               |                          |                         |             |
| 14 |     |     |               |                          |                         |             |
| 15 |     |     |               |                          |                         |             |
| 16 |     |     |               |                          |                         |             |

---

## Step 2 — Train / Test Split

Before fitting your model, split your data into a **training set** and a **test set**.

- Use the **first 12 students** (rows 1–12) as your **training data**
- Use the **last 4 students** (rows 13–16) as your **test data**

> **Why do we split the data?** Write your answer here:
>
> ___________________________________________________________________________
>
> ___________________________________________________________________________

---

## Step 3 — Group Tasks

Each group uses the **same dataset** but a different feature (X) to predict **height (Y)**.

| Group | Feature X | Target Y |
|-------|-----------|----------|
| **Group A** | Age | Height (cm) |
| **Group B** | Shoe Size (EU) | Height (cm) |
| **Group C** | Index Finger Length (cm) | Height (cm) |
| **Group D** | Name Length (# letters) | Height (cm) |

---

## Step 4 — Plot Your Data (Training Set Only)

On the axes below, draw a **scatter plot** of your feature (X) on the horizontal axis and height (Y) on the vertical axis. Use only the 12 training students.

```
Height
(cm)
 195 |
     |
 185 |
     |
 175 |
     |
 165 |
     |
 155 |___________________________________________
                                            X (your feature)
```

Label your X axis with your feature name and appropriate values.

---

## Step 5 — Fit the Regression Line

Use the formulas below to calculate the slope (β₁) and intercept (β₀) from your **training data**.

**Mean of X:**
```
x̄ = (x₁ + x₂ + ... + x₁₂) / 12  =  ________
```

**Mean of Y (height):**
```
ȳ = (y₁ + y₂ + ... + y₁₂) / 12  =  ________
```

**Slope:**
```
β₁ = Σ(xᵢ - x̄)(yᵢ - ȳ) / Σ(xᵢ - x̄)²  =  ________
```

**Intercept:**
```
β₀ = ȳ - β₁ · x̄  =  ________
```

**Your regression equation:**
```
ŷ = ________ + ________ · x
```

Draw this line on your scatter plot above.

---

## Step 6 — Evaluate on Training Data

For each training student, calculate the **predicted height** using your equation, then compute the error.

| # | Actual Height (cm) | Predicted Height (cm) | Error = Actual − Predicted |
|---|-------------------|----------------------|---------------------------|
| 1  |                   |                      |                           |
| 2  |                   |                      |                           |
| 3  |                   |                      |                           |
| 4  |                   |                      |                           |
| 5  |                   |                      |                           |
| 6  |                   |                      |                           |
| 7  |                   |                      |                           |
| 8  |                   |                      |                           |
| 9  |                   |                      |                           |
| 10 |                   |                      |                           |
| 11 |                   |                      |                           |
| 12 |                   |                      |                           |

**Mean Squared Error (MSE) on training data:**
```
MSE_train = (error₁² + error₂² + ... + error₁₂²) / 12  =  ________
```

---

## Step 7 — Evaluate on Test Data

Now apply your model to the **4 test students** (rows 13–16) that the model has **never seen**.

| # | Actual Height (cm) | Predicted Height (cm) | Error = Actual − Predicted |
|---|-------------------|----------------------|---------------------------|
| 13 |                   |                      |                           |
| 14 |                   |                      |                           |
| 15 |                   |                      |                           |
| 16 |                   |                      |                           |

**Mean Squared Error (MSE) on test data:**
```
MSE_test = (error₁³² + error₁₄² + error₁₅² + error₁₆²) / 4  =  ________
```

---

## Step 8 — Group Comparison & Discussion

Once all groups are done, fill in the summary table together as a class.

| Group | Feature | MSE (Train) | MSE (Test) | Test / Train ratio |
|-------|---------|------------|-----------|-------------------|
| A | Age | | | |
| B | Shoe Size | | | |
| C | Index Finger Length | | | |
| D | Name Length | | | |

**Discussion questions — answer together:**

**1.** Which group had the lowest test MSE? What does that tell you about that feature?

___________________________________________________________________________

___________________________________________________________________________

**2.** Which group had the highest test MSE? Why do you think that is?

___________________________________________________________________________

___________________________________________________________________________

**3.** For each group: is the test MSE much higher than the train MSE? What would that indicate?

___________________________________________________________________________

___________________________________________________________________________

**4.** Group D used name length to predict height. What do you expect their result to look like, and why?

___________________________________________________________________________

___________________________________________________________________________

**5.** If the test MSE is much larger than the train MSE, what is this phenomenon called in machine learning?

___________________________________________________________________________

---

## Bonus Task — Multivariate Regression

Can you do better by combining multiple features at once?

Try predicting height using **shoe size AND index finger length** together:

```
ŷ = β₀ + β₁ · shoe_size + β₂ · finger_length
```

To fit this model you need to solve the **normal equations**:

```
β = (XᵀX)⁻¹ Xᵀy
```

Where X is the matrix of your two features (with a column of 1s for the intercept).

Does the combined model perform better than any single-feature model? Record your MSE values:

```
MSE_train (combined) = ________

MSE_test  (combined) = ________
```

---

*Exercise prepared for: Machine Learning Basics — Linear Regression*
