
# 1. Linear Regression Example in 2D

Now we use **linear regression** instead of logistic regression.

We want to predict a continuous value using one feature.

Suppose:

- `x` = study hours
- `y` = exam score

We use 6 data points.

---

# 2. Dataset

| Student | Study Hours x | Exam Score y |
|---|---:|---:|
| A | 1 | 52 |
| B | 2 | 57 |
| C | 3 | 63 |
| D | 4 | 68 |
| E | 5 | 74 |
| F | 6 | 79 |

---

# 3. Linear Regression Model

The model is:

```math
y = \theta_0 + \theta_1 x
```

where:

- `theta0` = bias/intercept
- `theta1` = slope

---

# 4. Mean Squared Error

We use the Mean Squared Error (MSE) loss:

```math
J(\theta_0, \theta_1)
=
\frac{1}{m}
\sum_{i=1}^{m}
(\hat{y}^{(i)} - y^{(i)})^2
```

---

# 5. Gradients

## Gradient for theta0

```math
\frac{\partial J}{\partial \theta_0}
=
\frac{2}{m}
\sum_{i=1}^{m}
(\hat{y}^{(i)} - y^{(i)})
```

## Gradient for theta1

```math
\frac{\partial J}{\partial \theta_1}
=
\frac{2}{m}
\sum_{i=1}^{m}
(\hat{y}^{(i)} - y^{(i)})x^{(i)}
```

---

# 6. Update Rule

Gradient descent updates the parameters using:

```math
\theta_j := \theta_j - \alpha \frac{\partial J}{\partial \theta_j}
```

where:

- `alpha` = learning rate

---

# 7. Complete Python Code

```python
import numpy as np
import matplotlib.pyplot as plt

# -----------------------------------
# Dataset
# -----------------------------------

x = np.array([1, 2, 3, 4, 5, 6], dtype=float)
y = np.array([52, 57, 63, 68, 74, 79], dtype=float)

m = len(x)

# -----------------------------------
# Parameters
# -----------------------------------

theta0 = 0.0
theta1 = 0.0

# -----------------------------------
# Hyperparameters
# -----------------------------------

learning_rate = 0.01
epsilon = 1e-6
max_iterations = 10000

# -----------------------------------
# Gradient Descent
# -----------------------------------

for iteration in range(max_iterations):

    # Predictions
    y_hat = theta0 + theta1 * x

    # Gradients
    d_theta0 = (2/m) * np.sum(y_hat - y)
    d_theta1 = (2/m) * np.sum((y_hat - y) * x)

    # Save old parameters
    old_theta0 = theta0
    old_theta1 = theta1

    # Update parameters
    theta0 = theta0 - learning_rate * d_theta0
    theta1 = theta1 - learning_rate * d_theta1

    # Convergence check
    if (abs(theta0 - old_theta0) < epsilon and
        abs(theta1 - old_theta1) < epsilon):
        print(f"Converged after {iteration} iterations")
        break

# -----------------------------------
# Final parameters
# -----------------------------------

print("theta0 =", theta0)
print("theta1 =", theta1)

# -----------------------------------
# Plot
# -----------------------------------

plt.scatter(x, y, label="Training Data")

# Regression line

y_line = theta0 + theta1 * x

plt.plot(x, y_line, label="Regression Line")

plt.xlabel("Study Hours")
plt.ylabel("Exam Score")
plt.title("Linear Regression with Gradient Descent")
plt.legend()

plt.show()
```

---

# 8. Interpretation

After training:

- `theta0` is the intercept of the line
- `theta1` is the slope of the line

The regression line:

```math
y = \theta_0 + \theta_1 x
```

fits the data by minimizing the Mean Squared Error.

Gradient descent slowly moves the line until the prediction error becomes as small as possible.

