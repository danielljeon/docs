# Machine Learning

---

<details markdown="1">
  <summary>Table of Contents</summary>

<!-- TOC -->
* [Machine Learning](#machine-learning)
  * [1 Intro and Data](#1-intro-and-data)
  * [2 Optimization](#2-optimization)
    * [3.1 Gradient Descent](#31-gradient-descent)
  * [3 Prediction Models](#3-prediction-models)
    * [3.1 Overview](#31-overview)
      * [3.1.1 Overfitting and Overtraining](#311-overfitting-and-overtraining)
    * [3.2 Linear Regression: Continuous Outcome Prediction](#32-linear-regression-continuous-outcome-prediction)
      * [3.2.1 Gradient Descent Approach](#321-gradient-descent-approach)
      * [3.2.2 Normal Equation Approach](#322-normal-equation-approach)
    * [3.3 Logistic Regression: Probability Based Classification](#33-logistic-regression-probability-based-classification)
      * [3.3.1 Sigmoid](#331-sigmoid)
      * [3.3.2 Decision Boundary](#332-decision-boundary)
      * [3.3.3 Maximum Likelihood](#333-maximum-likelihood)
        * [3.3.3.1 Maximum Log-Likelihood (+ Gradient Decent) Approach](#3331-maximum-log-likelihood--gradient-decent-approach)
<!-- TOC -->

</details>

---

## 1 Intro and Data

---

## 2 Optimization

### 3.1 Gradient Descent

Gradient descent is an iterative optimization algorithm that minimizes an objective function by
moving the current guess down the slope (opposite to the gradient), taking small incremental steps.

$$a_{n+1} = {a}_{n} - \eta_{n} p_{n}$$

where:

- $a_{n+1}$: The next iterate after applying the gradient descent step.
- $a_{n}$: The current iterate at iteration n.
- $\eta_n$: Step size at iteration n.
- $p_{n}$: The gradient of the function at the iteration n.

Heuristic optimization methods often incorporate randomization or other exploratory strategies to
reduce the chance of getting trapped in local minima and to improve the likelihood of finding a
global minimum.

---

## 3 Prediction Models

### 3.1 Overview

#### 3.1.1 Overfitting and Overtraining

Overfitting happens when a prediction model captures noise or random fluctuations in the training
data instead of the underlying patterns that generalize to unseen data. Or in other words, **the
model memorizes the training data instead of learning general rules**.

### 3.2 Linear Regression: Continuous Outcome Prediction

Linear regression models the relationship between input features and a continuous output by fitting
a straight line (or hyperplane) through the data. The model learns weights that minimize the squared
error between predictions and actual values, either by solving a closed-form equation (normal
equation) or using gradient descent to iteratively adjust parameters.

Linear:

$$h(x, \omega) = \omega_0 + \omega_1 x + \omega_2 x^2 + \cdots + \omega_M x^M \\ = \omega_0 + \sum_{j=0}^{M} \omega_j x^j$$

Higher order polynomial:

$$h \left( x_n, \omega \right) = \sum_{j=0}^M \omega_j x_n^j$$

where:

- $h$: Is the hypothesis that predicts the output given input data points $x$.
- $x$: The input variable (aka feature or predictor).
- $\omega$: The set of model parameters (weights).
- $\omega_0$: The intercept (bias term), representing the predicted value when $x = 0$.
- $\omega_j$: The coefficient (weight) for the $j$-th power of $x$.
    - Determines how much influence the term $x^j$ has on the prediction.
- $M$: The degree of the polynomial model. For example:
    - If $M = 1$, the model is simple linear regression (a straight line).
    - If $M > 1$, the model is polynomial regression.

#### 3.2.1 Gradient Descent Approach

The objective function is the mathematical expression that should be minimized in order to find the
best-fitting line (or hyperplane):

$$E(\omega) = \frac{1}{2} \sum_{n=1}^{N} \left\{ f(x_n, \omega) - t_n \right\}^2$$

- $E(\omega)$: The error (loss) function, measures how well the model with parameters $\omega$ fits
  the data.
- $\omega$: The parameter vector (weights) of the model.
    - These are what need to be optimized/solved for.
- $N$: The number of training examples (data points).
- $x_n$: The input feature vector for the $n$-th training example.
- $f \left( x_n, \omega \right)$: The prediction (hypothesis output) of the model for input $x_n$
  using parameters $\omega$.
- $t_n$: The target value (ground truth output) corresponding to input $x_n$.
- $\left( t_n - f(x_n, \omega) \right)^2$: The squared error between the actual value and the
  predicted value for the $n$-th example.
- $\frac{1}{2}$: A scaling factor included for mathematical convenience, since it cancels the
  exponent when differentiating during gradient descent. (No effect on optimization result).

#### 3.2.2 Normal Equation Approach

The normal equation is a a direct algebraic solution for the model parameters (weights) without
needing iterative updates like gradient descent.

$$\omega = (X^T X)^{-1} X^T \hat{y}$$

- $X$: the design matrix (rows = training examples, columns = features).
- $\hat{y}$: the vector of target values.
- $\omega$: the vector of model weights.

### 3.3 Logistic Regression: Probability Based Classification

Logistic regression is a method used to predict the probability of a binary outcome by applying the
sigmoid function to a weighted sum of input features. Instead of fitting the data with the least
squares (like linear regression), it uses maximum likelihood estimation (MLE) to find the weights
that make the observed labels most probable. This means the model parameters are chosen to maximize
how well the predicted probabilities match the actual outcomes.

#### 3.3.1 Sigmoid

The sigmoid is a mathematical function that maps any real number into the range (0, 1), making it
useful for modeling probabilities. In logistic regression, the sigmoid function transforms a linear
combination of inputs into a probability, allowing for prediction of binary outcomes.

$$\sigma \left( z \right) = \frac{1}{1 + e^{-z}}$$

- For a given input $z$, the output will always be between (0, 1).
- The shape itself is a s-shaped curve that smoothly transitions from 0 (as $x \to -\infty$) to 1 (
  as $x \to +\infty$).
- At $x = 0$, the output is 0.5.

The sigmoid can be rewritten to represent the probability of getting an $\mathrm{output} = 1$:

$$p(C = 1 \mid x) = \frac{1}{1 + e^{-(\omega_0 + \omega^T x)}}$$

where (similar to before):

- $x$: The input variable (aka feature or predictor).
- $\omega$: The set of model parameters (weights).
- $\omega_0$: The intercept (bias term), representing the predicted value when $x = 0$.

#### 3.3.2 Decision Boundary

The decision boundary is said to occur when the sigmoid = 0.5:

$$\sigma \left( z \right) = 0.5$$
$$p \left( C = 0 \mid x \right) = p \left( C = 1 \mid x \right) = 0.5$$

which happens when:

$$\omega_0 + \omega^T x = 0$$

#### 3.3.3 Maximum Likelihood

Maximum Likelihood Estimation (MLE) chooses the parameter values that make an observed data set most
likely under the model.

$$max_{\omega} L(\omega) = \max_{\omega} \prod_{i=1}^N p(t=1 \mid x^{(i)}; \omega)^{t^{(i)}}$$
$$\Big(1 - p(t=1 \mid x^{(i)}; \omega)\Big)^{1 - t^{(i)}}$$

where:

- $N$: number of training samples.
- $x^{(i)}$: feature vector for the i-th sample.
- $t^{(i)}$: the true label for sample $i$, which is binary (either 0 or 1).
- $p(t=1 \mid x^{(i)}; \omega)$: the probability, predicted by the model with parameters $\omega$,
  that sample $i$ belongs to class 1.

##### 3.3.3.1 Maximum Log-Likelihood (+ Gradient Decent) Approach

In order to make finding the maximum likelihood easier, the log can be taken, converting the product
over $N$ samples to a sum. This allows the maximum log-likelihood to be used as the objective
function with gradient accent/decent.

$$\ell_{log}(\omega) = \sum_{i=1}^N \Big[t^{(i)} \log p(t=1 \mid x^{(i)}; \omega) +(1 - t^{(i)}) \log \big(1 - p(t=1 \mid x^{(i)}; \omega)\big)\Big]$$

- If $t^{(i)} = 1$, only the first term remains: $\log p(t=1 \mid x^{(i)}; \omega)$.
- If $t^{(i)} = 0$, only the second term remains: $\log (1 - p(t=1 \mid x^{(i)}; \omega))$.
- So, for each data point, the function rewards the model if it assigns high probability to the
  correct label.
- Note: the log-likelihood $\ell(\omega)$ is concave in $\omega$.
    - Optimizers that minimize objectives typically use the negative $J(\omega) = -\ell(\omega)$,
      which is convex.
