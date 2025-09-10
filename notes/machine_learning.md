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
