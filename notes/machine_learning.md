# Machine Learning

## Optimization

### Gradient Descent

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

### Linear Regression

Linear regression models the relationship between input features and a continuous output by fitting
a straight line (or hyperplane) through the data. The model learns weights that minimize the squared
error between predictions and actual values, either by solving a closed-form equation (normal
equation) or using gradient descent to iteratively adjust parameters.

$$h(x, \omega) = \omega_0 + \omega_1 x + \omega_2 x^2 + \cdots + \omega_M x^M \\ = \omega_0 + \sum_{j=0}^{M} \omega_j x^j$$

where:

- $h$: Is the hypothesis that predicts the output given input data points $x$.
- $x$: The input variable (aka feature or predictor).
- $\omega$: The set of model parameters (weights).
- $\omega_0$: The intercept (bias term), representing the predicted value when \(x = 0\).
- $\omega_j$: The coefficient (weight) for the $j$-th power of $x$.
    - Determines how much influence the term $x^j$ has on the prediction.
- $M$: The degree of the polynomial model. For example:
    - If $M = 1$, the model is simple linear regression (a straight line).
    - If $M > 1$, the model is polynomial regression.

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
    - In polynomial regression, this would
      be $h \left( x_n, \omega \right) = \sum_{j=0}^M \omega_j x_n^j$.
- $t_n$: The target value (ground truth output) corresponding to input \(x_n\).
- $\left( t_n - f(x_n, \omega) \right)^2$: The squared error between the actual value and the
  predicted value for the $n$-th example.
- $\frac{1}{2}$: A scaling factor included for mathematical convenience, since it cancels the
  exponent when differentiating during gradient descent. (No effect on optimization result.)
