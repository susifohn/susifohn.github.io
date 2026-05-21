# Logistic Regression Example: Snake vs. Not Snake

## Goal

We want to classify animals into:

- `1` = snake
- `0` = not snake

using a single feature:

- the **length/width ratio**

A snake is usually long and thin, so it tends to have a larger length/width ratio.

---

# 1. Training Dataset

| Animal | Length/Width Ratio x | Label y |
|---|---:|---:|
| Worm-like snake | 9.0 | 1 |
| Cobra | 8.0 | 1 |
| Python | 7.0 | 1 |
| Lizard | 3.0 | 0 |
| Turtle | 1.5 | 0 |

---

# 2. Logistic Regression Model

The model computes:

```math
z = \theta_0 + \theta_1 x
```

where:

- `theta0` = bias parameter
- `theta1` = slope parameter
- `x` = input feature (length/width ratio)

Then we apply the sigmoid function:

```math
\hat{y} = \sigma(z) = \frac{1}{1 + e^{-z}}
```

The output `y_hat` is a probability between 0 and 1.

- if `y_hat > 0.5` → classify as snake
- otherwise → classify as not snake

---

# 3. Gradient Descent

We use gradient descent to learn the parameters `theta0` and `theta1`.

The update rule is:

```math
\theta_j := \theta_j - \alpha \frac{\partial J}{\partial \theta_j}
```

where:

- `alpha` = learning rate
- `J` = loss function

The gradients are:

## Gradient for theta0

```math
\frac{\partial J}{\partial \theta_0}
=
\frac{1}{m}
\sum_{i=1}^{m}
(\hat{y}^{(i)} - y^{(i)})
```

## Gradient for theta1

```math
\frac{\partial J}{\partial \theta_1}
=
\frac{1}{m}
\sum_{i=1}^{m}
(\hat{y}^{(i)} - y^{(i)}) x^{(i)}
```

---

# 4. Hyperparameters

We choose:

```python
learning_rate = 0.1
epsilon = 1e-6
max_iterations = 10000
```

## Meaning

- `learning_rate` controls the step size during learning
- `epsilon` is the stopping threshold
- `max_iterations` prevents infinite loops

---

# 5. Complete Python Code

```python
import numpy as np

# -----------------------------------
# Training data
# x = length/width ratio
# y = 1 => snake
# y = 0 => not snake
# -----------------------------------

x = np.array([9.0, 8.0, 7.0, 3.0, 1.5])
y = np.array([1, 1, 1, 0, 0])

m = len(x)

# -----------------------------------
# Model parameters
# theta0 = bias
# theta1 = slope
# -----------------------------------

theta0 = 0.0
theta1 = 0.0

# -----------------------------------
# Hyperparameters
# -----------------------------------

learning_rate = 0.1
epsilon = 1e-6
max_iterations = 10000

# -----------------------------------
# Sigmoid function
# -----------------------------------

def sigmoid(z):
    return 1 / (1 + np.exp(-z))

# -----------------------------------
# Gradient Descent
# -----------------------------------

for iteration in range(max_iterations):

    # Linear model
    z = theta0 + theta1 * x

    # Predictions
    y_hat = sigmoid(z)

    # Gradients
    d_theta0 = (1/m) * np.sum(y_hat - y)
    d_theta1 = (1/m) * np.sum((y_hat - y) * x)

    # Save old values for convergence check
    old_theta0 = theta0
    old_theta1 = theta1

    # Update rule
    theta0 = theta0 - learning_rate * d_theta0
    theta1 = theta1 - learning_rate * d_theta1

    # Abort if parameters barely change
    if (abs(theta0 - old_theta0) < epsilon and
        abs(theta1 - old_theta1) < epsilon):
        print(f"Converged after {iteration} iterations")
        break

# -----------------------------------
# Final model
# -----------------------------------

print("theta0 =", theta0)
print("theta1 =", theta1)

# -----------------------------------
# Predictions
# -----------------------------------

for value in x:
    probability = sigmoid(theta0 + theta1 * value)
    prediction = 1 if probability > 0.5 else 0

    print(
        f"x={value:.1f}, "
        f"probability={probability:.3f}, "
        f"class={prediction}"
    )
```

---

# 6. Interpretation

After training:

- `theta0` shifts the decision boundary
- `theta1` controls how strongly the feature influences the prediction

The decision boundary occurs when:

```math
\theta_0 + \theta_1 x = 0
```

Solving for `x`:

```math
x = -\frac{\theta_0}{\theta_1}
```

This value separates:

- snake
- not snake

---

# 7. Important Learning Idea

Gradient descent repeatedly:

1. predicts outputs
2. measures the error
3. computes gradients
4. updates parameters
5. slowly improves the model

The gradients tell us:

- in which direction the parameters should move
- and by how much

