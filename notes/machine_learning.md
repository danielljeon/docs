# Machine Learning

---

<details markdown="1">
  <summary>Table of Contents</summary>

<!-- TOC -->
* [Machine Learning](#machine-learning)
  * [1 Intro and Data](#1-intro-and-data)
  * [2 Optimization](#2-optimization)
    * [2.1 The Cost Function](#21-the-cost-function)
    * [2.2 Local Optimization](#22-local-optimization)
      * [2.2.1 Gradient Descent](#221-gradient-descent)
    * [2.3 Global Optimization](#23-global-optimization)
      * [2.3.1 Simulated Annealing (SA)](#231-simulated-annealing-sa)
    * [2.4 Regularization](#24-regularization)
      * [2.4.1 L1 Lasso Regression](#241-l1-lasso-regression)
      * [2.4.2 L2 Ridge Regression](#242-l2-ridge-regression)
  * [3 Supervised Prediction Models](#3-supervised-prediction-models)
    * [3.1 Overview](#31-overview)
      * [3.1.1 Overfitting and Overtraining](#311-overfitting-and-overtraining)
      * [3.1.2 Bias](#312-bias)
    * [3.2 Linear Regression: Continuous Outcome Prediction](#32-linear-regression-continuous-outcome-prediction)
      * [3.2.1 Gradient Descent Approach: Mean Squared Error (MSE)](#321-gradient-descent-approach-mean-squared-error-mse)
      * [3.2.2 Normal Equation Approach](#322-normal-equation-approach)
    * [3.3 Logistic Regression: Probability Based Classification](#33-logistic-regression-probability-based-classification)
      * [3.3.1 Sigmoid](#331-sigmoid)
      * [3.3.2 Decision Boundary](#332-decision-boundary)
      * [3.3.3 Maximum Likelihood](#333-maximum-likelihood)
        * [3.3.3.1 Maximum Log-Likelihood](#3331-maximum-log-likelihood)
        * [3.3.3.2 Binary Cross Entropy (BCE)](#3332-binary-cross-entropy-bce)
    * [3.4 Decision Trees: Rules-based Hierarchical Splits](#34-decision-trees-rules-based-hierarchical-splits)
      * [3.4.1 Information Gain and Impurity](#341-information-gain-and-impurity)
      * [3.4.2 Entropy](#342-entropy)
      * [3.4.3 Gini Index and Weighted Sum](#343-gini-index-and-weighted-sum)
      * [3.4.4 Overfitting and Pruning](#344-overfitting-and-pruning)
      * [3.4.5 Decision Regression Trees](#345-decision-regression-trees)
      * [3.4.6 Random Forest: Bagging](#346-random-forest-bagging)
      * [3.4.7 Adaboost: Adaptive Boosting on Trees](#347-adaboost-adaptive-boosting-on-trees)
    * [3.5 Support Vector Machines (SVM)](#35-support-vector-machines-svm)
      * [3.5.1 Kernal](#351-kernal)
      * [3.5.2 Slack Variables: Regularization Effect](#352-slack-variables-regularization-effect)
    * [3.6 K-Nearest Neighbour (KNN): Hard Classification and Regression](#36-k-nearest-neighbour-knn-hard-classification-and-regression)
      * [3.6.1 Distance Measurements](#361-distance-measurements)
      * [3.6.2 Weighted K-Nearest Neighbour](#362-weighted-k-nearest-neighbour)
  * [4 Unsupervised Prediction Models](#4-unsupervised-prediction-models)
    * [4.1 K-Means Clustering: Hard Clustering](#41-k-means-clustering-hard-clustering)
      * [4.1.2 Standardization and Normalization](#412-standardization-and-normalization)
    * [4.2 Gaussian Mixture Models (GMM): Soft Classification](#42-gaussian-mixture-models-gmm-soft-classification)
<!-- TOC -->

</details>

---

## 1 Intro and Data

---

## 2 Optimization

### 2.1 The Cost Function

The cost function, also known as the loss function (often written as the function $J$) measures how
well a machine learning model's predictions match the actual target values. It quantifies the error
between the expected and predicted outputs, providing the feedback signal that guides learning.

In essence, the objective is to minimize the cost function, allowing the model to achieve the most
accurate and generalizable predictions possible.

### 2.2 Local Optimization

Finds the nearby most optimal solution (but might not be the global optimal solution).

#### 2.2.1 Gradient Descent

Gradient descent is an iterative optimization algorithm that minimizes an cost function by
moving the current guess down the slope (opposite to the gradient), taking small incremental steps.

$$a_{n+1} = a_n - \eta_n \nabla J(a_n)$$

where:

- $a_{n}$: The current iterate at iteration n.
- $a_{n+1}$: The next iterate after applying the gradient descent step.
- $\eta_n$: The learning rate or step size at iteration n.
- $\nabla J(a_n)$: The **gradient** or derivative of the cost function $J$ at $a_n$.

Gradient descent can stop at the following general conditions:

1. The change in the cost function is smaller than a chosen threshold.
2. The magnitude of the gradient becomes very small (derivative/slope is near zero).
3. The maximum number of iterations is reached.

Heuristic optimization methods often incorporate randomization or other exploratory strategies to
reduce the chance of getting trapped in local minima and to improve the likelihood of finding a
global minimum.

YouTube,
3Blue1Brown: [Gradient descent, how neural networks learn | Deep Learning Chapter 2](https://youtu.be/IHZwWFHWa-w)

### 2.3 Global Optimization

Finds the absolute best solution across the entire problem space, not just the nearest one.

- Often uses stochastic, population-based, or probabilistic methods to escape local traps and
  explore broadly.

#### 2.3.1 Simulated Annealing (SA)

> **Background**: The term "annealing" comes from metallurgy: the process of heating a metal and
> then cooling it slowly so that its atoms settle into a low-energy (stable) crystalline structure.
> - Cool too quickly = atoms "freeze" in random positions maintaining defects.
> - Cool slowly = atoms can rearrange to a lower total energy (optimal structure).

The SA algorithm searches for the global minimum of an objective function $J(x)$ by:

1. Start at a random point $x_0$.
2. Make small random moves.
3. Accept not only the model improvements, but sometimes even worse moves.
4. Gradually reduce the probability of accepting worse moves over time.

The change in the cost function is given by:

$$\Delta E = J(x') - J(x)$$

The acceptance likelihood for the new solution is given by the following equation:

$$P = e^{-\Delta E/T}$$

where:

- $\Delta E$: The simulated annealing energy, or change in the cost function.
    - If $\Delta E < 0$: Accept the new solution (it's better).
    - If $\Delta E > 0$: Accept with probability.
- $J(x)$: The current cost.
- $J(x')$: The new cost.
- $T$: The simulated current annealing temperature, or probability factor for acceptance.
    - In the probability function $P$:
        - Higher $T$ = likely accepted.
        - Lower $T$ = less likely to be accepted.

### 2.4 Regularization

Regularization adds a penalty term to the cost function to discourage overly complex models (large
weights). This prevents overfitting and encourages generalization.

#### 2.4.1 L1 Lasso Regression

L1 regularization (Lasso) adds a penalty equal to the sum of the absolute values of the model
weights. This encourages sparsity by pushing some coefficients exactly to zero, effectively
performing feature selection and simplifying the model.

L1 penalty term:
$$R_{L1}(\omega) = \sum_{j=1}^{M} |\omega_j| = ||\omega||_1$$

L1 regularized cost function (component form):
$$J_{L1}(\omega) = \frac{1}{2N} \sum_{i=1}^{N} (y_i - \hat y_i)^2 + \lambda \sum_{j=1}^{M} |\omega_j|$$

L1 regularized cost function (vector form):
$$J_{L1}(\omega) = \frac{1}{2N} ||y - X\omega||_2^2 + \lambda ||\omega||_1$$

#### 2.4.2 L2 Ridge Regression

L2 regularization (Ridge) adds a penalty equal to the sum of the squared weights, shrinking all
coefficients smoothly toward zero without eliminating them. This prevents large weights, stabilizes
learning, and reduces overfitting while maintaining all features' contributions.

L2 penalty term:
$$R_{L2}(\omega) = \sum_{j=1}^{M} \omega_j^2 = ||\omega||_2^2$$

L2 regularized cost function (component form):
$$J_{L2}(\omega) = \frac{1}{2N} \sum_{i=1}^{N} (y_i - \hat y_i)^2 + \lambda \sum_{j=1}^{M} \omega_j^2$$

L2 regularized cost function (vector form):
$$J_{L2}(\omega) = \frac{1}{2N} ||y - X\omega||_2^2 + \lambda ||\omega||_2^2$$

---

## 3 Supervised Prediction Models

### 3.1 Overview

#### 3.1.1 Overfitting and Overtraining

Overfitting happens when a prediction model captures noise or random fluctuations in the training
data instead of the underlying patterns that generalize to unseen data. Or in other words, **the
model memorizes the training data instead of learning general rules**.

#### 3.1.2 Bias

Bias is the error due to overly simplistic assumptions about the data.

- A high-bias model underfits, it is too simple it misses on capturing complex relationships.
- A low-bias model overfits, it is too complex it captures noise and exact training data.

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

#### 3.2.1 Gradient Descent Approach: Mean Squared Error (MSE)

The objective function is the mathematical expression that should be minimized in order to find the
best-fitting line (or hyperplane), in linear regression this is known as the Mean Squared Error
(MSE):

$$J(\omega) = \frac{1}{2 N} \sum_{n=1}^{N} \left( f(x_n, \omega) - t_n \right)^2$$

- $J(\omega)$: The error (cost) function, measures how well the model with parameters $\omega$ fits
  the data.
- $\omega$: The parameter vector (weights) of the model.
    - These are what need to be optimized/solved for.
- $N$: The number of training examples (data points).
- $x_n$: The input feature vector for the $n$-th training example.
- $f \left( x_n, \omega \right)$: The prediction (hypothesis output) of the model for input $x_n$
  using parameters $\omega$.
- $t_n$: The target value (ground truth output) corresponding to input $x_n$.
- $\left( f(x_n, \omega) - t_n \right)^2$: The squared error between the actual value and the
  predicted value for the $n$-th example.
- $\frac{1}{2}$: A scaling factor included for mathematical convenience, since it cancels the
  exponent when differentiating during gradient descent. (No effect on optimization result).
- $\frac{1}{N}$: Averaging (mean) for all data points.

#### 3.2.2 Normal Equation Approach

The normal equation is a direct algebraic solution for the model parameters (weights) without
needing iterative updates like gradient descent.

$$\omega = (X^T X)^{-1} X^T \hat{y}$$

- $X$: The design matrix (rows = training examples, columns = features).
- $\hat{y}$: The vector of target values.
- $\omega$: The vector of model weights.

### 3.3 Logistic Regression: Probability Based Classification

Logistic regression predicts the probability of a binary outcome by applying the sigmoid function to
a linear combination of input features.

Instead of minimizing squared error (as in linear regression), logistic regression uses maximum
likelihood estimation (MLE) to find the model parameters that make the observed labels most probable
under the model. In other words, it adjusts the weights to maximize how well the predicted
probabilities align with the actual outcomes.

Although logistic regression produces nonlinear (S-shaped) sigmoid outputs, it remains a linear
model at its core, the decision boundary is defined by a linear equation of the input features. The
sigmoid function simply transforms this linear relationship into a continuous probability between 0
and 1, enabling the model to perform smooth, probabilistic classification.

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

$$\max_{\omega} L(\omega) = \max_{\omega} \prod_{i=1}^N p(t=1 \mid x_i; \omega)^{t_i}$$
$$\Big(1 - p(t=1 \mid x_i; \omega)\Big)^{1 - t_i}$$

where:

- $N$: number of training samples.
- $x_i$: feature vector for the i-th sample.
- $t_i$: the true label for sample $i$, which is binary (either 0 or 1).
- $p(t=1 \mid x_i; \omega)$: the probability, predicted by the model with parameters $\omega$,
  that sample $i$ belongs to class 1.

##### 3.3.3.1 Maximum Log-Likelihood

In order to make finding the maximum likelihood easier, the log can be taken, converting the product
over $N$ samples to a sum. This allows the maximum log-likelihood to be used as the objective
function with gradient accent/decent.

$$\ell_{log}(\omega) = \sum_{i=1}^N \Big[t_i \log p(t=1 \mid x_i; \omega) +(1 - t_i) \log \big(1 - p(t=1 \mid x_i; \omega)\big)\Big]$$

- If $t_i = 1$, only the first term remains: $\log p(t=1 \mid x_i; \omega)$.
- If $t_i = 0$, only the second term remains: $\log (1 - p(t=1 \mid x_i; \omega))$.
- So, for each data point, the function rewards the model if it assigns high probability to the
  correct label.
- Note: the log-likelihood $\ell(\omega)$ is concave in $\omega$.

##### 3.3.3.2 Binary Cross Entropy (BCE)

Real life optimizers that minimise the cost function typically use the inverse of the Maximum
Log-Likelihood, known as the Binary Cross Entropy (BCE):

$$J(\omega) = - \frac{1}{N} \ell_{log}(\omega)$$

- The mean is also taken to keep the loss independent of dataset size.

### 3.4 Decision Trees: Rules-based Hierarchical Splits

In a decision tree, each node represents a subset of the training data.

At the root node, all data samples are present. As the tree grows and splits on feature thresholds,
the dataset is divided into smaller subsets that pass down to the child nodes.

Thus, the number of data points contained in each node decreases with depth, as each branching point
partitions the data further.

The impurity of a node measures how mixed or homogeneous the data is with respect to the target
variable (class labels).

- A pure node contains samples from only one class, resulting in zero impurity (e.g., entropy = 0,
  Gini = 0).
- An impure node contains samples from multiple classes, indicating greater uncertainty in
  classification.

The goal of a decision tree algorithm is to select splits that reduce impurity the most, producing
child nodes that are more homogeneous than their parent. This reduction in impurity is quantified
using measures such as Information Gain (based on entropy) or Gini Reduction (based on the Gini
Index).

Basic Process:

1. Select the best feature to split the data based on an impurity measure.
    - Greedy search.
    - Information Gain with entropy.
    - Gini Index and Weighted Sum.
2. Split the dataset into subsets according to the selected feature's values.
3. Evaluate purity of resulting nodes (check if all samples belong to one class).
4. Repeat recursively for each subset until:
    1. Nodes are pure meaning zero impurity/uncertainty, training data memorization **(bad)**.
    2. Stopping condition is met (e.g., max depth, min samples per leaf)

#### 3.4.1 Information Gain and Impurity

Information Gain (IG) is a measure used in decision trees to decide which attribute to split on at
each step. It quantifies how much "impurity" in the dataset can be reduced after splitting the data
based on a given attribute.

$$IG = I_{parent} - \sum_{i \in V(A)} \frac{N_i}{N} I_i$$

where:

- $I$: The impurity measure (entropy or Gini index).
- $V(A)$: The set of possible values of attribute $A$.
- $N_i$: The number of samples in the child node $i$.
- $N$: The total number of samples in the parent node.
- $\sum_{i \in V(A)} \frac{N_i}{N}$: The weighted average impurity of child nodes after splitting on
  attribute $A$.

#### 3.4.2 Entropy

The entropy or uncertainty is calculated as follows:

$$H(S) = -\,p(+) \log_{2} p(+) - p(-) \log_{2} p(-)$$

where:

- $p(+)$: Is the probability of positive examples.
- $p(-)$: Is the probability of negative examples.

#### 3.4.3 Gini Index and Weighted Sum

The Gini Index is another common impurity measure used in decision trees. It quantifies how often a
randomly chosen sample from the dataset would be incorrectly labeled if it were randomly assigned a
label according to the distribution of labels in that node.

$$Gini = 1 - \sum_{i=1}^{n} p_i^2$$

where:

- $p_i$: The probability (proportion) of samples belonging to class $i$ in the node.
- $n$: The number of possible classes.

#### 3.4.4 Overfitting and Pruning

By nature, decision trees tend to be low bias and overfit, meaning they often capture very specific
noise or learn the training data outright. When a decision tree overfits, pruning is used to
simplify it.

The idea is to replace certain unnecessary subtrees with leaf nodes that predict the majority class.
This is tested on a validation set (separate from the training and test sets). If accuracy improves
or stays the same, the prune is kept; if not, it's reverted. This prevents the model from memorizing
noise and helps it generalize better.

This greatly slightly increases bias while greatly reducing variance.

- Sometimes helps to split data into 3: training, testing and pruning.

#### 3.4.5 Decision Regression Trees

A Regression Tree is a type of decision tree designed for predicting continuous target values.
Unlike classical decision trees, which are generally used for categorical outcomes, regression trees
recursively split the data into regions (nodes) that minimize prediction error-typically measured by
variance within each subset. Here, variance serves as the analogue of "impurity" in classification
trees.

The objective is to create leaf nodes that represent smaller, more homogeneous ranges of the overall
continuous target space, ensuring that each leaf provides a good local approximation of the target
value.

When regression trees talk about variance, they're not referring to the "spread of the input
feature values (x)" - they're referring to the spread of the target values (y or t) within that node
around their mean prediction.

Process:

1. Select an attribute (feature) to test.
    - Eventually, all attributes may be considered as potential split candidates.
    - The selection uses a greedy search for the split that minimizes prediction error.
2. Identify possible split points for each continuous attribute:
   $$n_{\mathrm{samples}} - 1$$
    - Typically, the midpoints between sorted values.
3. Compute the mean target value for each subset formed by a split:
   $$\hat y_{\tau} = \frac{1}{N_{\tau}} \sum_{x_n \in Y_{\tau}}^{N} t_n$$

   where:

    - $\hat{y}_{\tau}$: The predicted output for subset (leaf) $\tau$.
    - $N_{\tau}$: The number of samples in subset $Y_{\tau}$.
    - $t_n$: The target value (label/output) for sample $x_n$.
    - $x_n$: A single data sample/observation.
    - $Y_{\tau}$: The subset of data samples belonging to region/leaf $\tau$.
    - $N$: The total number of samples in the dataset.
4. Calculate the error (Sum of Squared Residuals) for each subset:

   $$E_{\tau} = \sum_{i=1}^{N} \left( y_i - \hat{y}_{\tau} \right)^2$$

   where:

    - $E_{\tau}$: The total squared error for subset $\tau$.
    - $y_i$: The observed/true value of the i-th sample.
    - $i$: The index running over the samples in the dataset (from 1 to N).

   The total error for each split is given by:

   $$E = \sum_{\tau=1}^{T} E_{\tau}$$

   where:

    - $E$: The total error across all subsets.
    - $\tau$: The index of the subset (e.g., a region, cluster, or leaf in a tree).
5. Select the split that yields the lowest total error.
6. Repeat recursively for each new subset until a stopping criterion is met.

#### 3.4.6 Random Forest: Bagging

A Random Forest is an ensemble of decision trees trained on random subsets of the data and/or
features. Each tree acts as an independent predictor, and the final prediction is the average
(for regression) or majority vote (for classification) of all trees.

$$\hat{H}_{bag}(x) = \frac{1}{N} \sum_{n=1}^{N} H_n(x)$$

- $\hat{H}_{bag}(x)$: The bagged (averaged) predictor output for input $x$.
- $H_n(x)$: The prediction made by the $n$-th model (or learner) on input $x$.
- $x$: The input sample or data point.

Process:

1. Divide the training data set into N random subsets.
2. Build $N$ trees to generate multiple hypothesis $Hn(x)$.
    - Consider only a random subset of features at each split throughout the forest.
3. Use the forest to predict the target variable of test instances as the average of all trees in
   the forest.

#### 3.4.7 Adaboost: Adaptive Boosting on Trees

1. Assign equal weights to the dataset for all $i$.
   $$\omega_i = \frac{1}{N}$$
2. Train a weak learner (e.g., a decision stump).
3. Calculate the learner's weighted error:
   $$Error = \frac{\sum_i \omega_i I \left( y_i \ne h_i(x_i)\right)}{\sum_i \omega_i}$$
4. Calculate the importance factor $\alpha$, for each data point factor.
   $$\alpha = \frac{1}{2} \ln \left( \frac{1 - \text{Error}}{\text{Error}} \right)$$

   where:

    - $\alpha$: The weight assigned to a weak learner (in boosting).
    - $\text{Total Error}$: The weighted error rate of the weak learner.
        - For example: the sum of misclassified sample weights divided by the total.
5. Reweight the data points with the calculated importance factors.
    - Correct sample: $\omega_{i} \leftarrow \omega_{i} e^{-\alpha}$.
    - Incorrect sample: $\omega_{i} \leftarrow \omega_{i} e^{\alpha}$.

   where:

    - $\omega_i$: The weight (importance factor) of the $i$-th training sample
6. Normalize the new weights.
   $$\omega_{i} \leftarrow \frac{\omega_{i}}{\sum_{i=1}^{N} \omega_{i}}$$

   where:

    - $\sum_{i=1}^{N} \omega_i$: Is the normalization constant, it ensures all weights sum to 1.
7. Repeat the process for several rounds to build multiple weak learners.
8. Final prediction:

$$H(x) = \text{sign} \left( \sum_t \alpha_t h_t (x) \right)$$

### 3.5 Support Vector Machines (SVM)

SVM tries to maximize the margin between two classes: the margin is the distance between the
decision boundary and the closest data points from each class (called support vectors).

Suppose you have two features, SVM finds a line that separates the classes:

$$\omega^T x + b = 0$$

where:

- $\omega$: The weight vector (defines the orientation of the hyperplane).
- $b$: The bias (defines the offset from origin).

SVM solves an optimization problem to maximize the margin, which is equivalent to minimizing
$|| \omega ||^2$, subject to all points being correctly classified (or mostly correct if
soft-margin). In other words, find the flattest separating hyperplane (smallest $\omega$) that
correctly divides the data, to maximize the margin between classes.

The objective function is given by:

$$J(\omega, b) = \frac{1}{2} || \omega ||^2$$

where:

- $\omega$: The weight vector.
- $b$: The bias.
- $|| \omega ||^2$: The squared magnitude of the weight vector.
- $\frac{1}{2}$: The scaling factor simplifying differentiation.

To optimize the SVM cost function, the lagrange duality can be used, giving a closed-form
relationship at optimality:

$$w = \sum^N_{i=1} \alpha_i y_i x_i$$

where:

- $x_i$: The input feature vector of the $i$-th data point.
- $y_i$: The class or true label value of the $i$-th data point.
- $\alpha_i$: The Lagrange Multiplier.
    - Each $\alpha_i$ corresponds to one constraint: $y_i(\omega^T x_i + b) \ge 1$
        - Conceptually, $\alpha_i$ represents how strongly each training point influences the
          boundary.

The classification decision is made by the $\text{sign}$ of $\omega^T x + b$:

$$
\hat{y} = \begin{cases} +1, & \text{if } w^T x + b \geq 0 \\
-1, & \text{if } w^T x + b < 0 \end{cases}
$$

#### 3.5.1 Kernal

A kernel allows SVMs to draw curved, non-linear decision boundaries in the original feature space by
performing a linear separation in a higher-dimensional (often implicit) feature space. This process
is done efficiently using the kernel trick, which avoids explicitly computing the transformation
into that higher-dimensional space.

The kernal function $K(x_i, x_j)$ replaces the dot product between data points in the Lagrange dual
formulation, resulting in the decision function:

$$f(x) = \text{sign}\left( \sum_i \alpha_i y_i K(x_i, x) + b \right)$$

#### 3.5.2 Slack Variables: Regularization Effect

The slack variable $\xi_i$ allows some points to violate the margin constraint, it measures how
much each point breaks the rule. The SVM then balances two competing goals:

1. Keep the margin as wide as possible.
2. Keep total violations (sum of $\xi_i$) as small as possible.

Thus, the soft-margin SVM optimization objective is expressed as:

$$J(\omega, b, \xi) = \frac{1}{2}||\omega||^2 + C \sum_i \xi_i$$

- Subject to: $y_i(\omega^T x_i + b) \ge 1 - \xi_i, \xi_i \ge 0$

### 3.6 K-Nearest Neighbour (KNN): Hard Classification and Regression

K-Nearest Neighbours (KNN) is a supervised learning algorithm used for classification or regression.
However, unlike other prediction models, KNN does not train a model, it just lazily stores the
training data and uses it directly to make predictions.

1. Store the training data.
2. Select a number of neighbours to consider $k$.
3. When a new data point comes in, measure its distance to all training points.
4. Make a prediction:
    1. **Classification**: Vote within the labels of the $k$ nearest neighbours of the new data
       point, and whichever label appears most often "wins."
    2. **Regression**: Compute the average (or sometimes weighted average) of the values from
       the $k$ nearest neighbours.

#### 3.6.1 Distance Measurements

**Euclidian Distance:** essentially by using a summed pythagorean theorem the distance between
points can be found.

$$D(x, y) = \sqrt{\sum_{i=1}^{n} (x_i - y_i)^2}$$

where:

- $x$: The first data point (in an n-dimensional vector).
- $y$: The second data point (in an equal n-dimensional vector).

**Hamming Distance:** Finding the difference between binary numbers, for example:

```
D(0b100011, 0b110110)
= ones_count(0b100011 XOR 0b110110)
= ones_count(0b010101)
= 3
```

**Cosine Distance:** Calculating the cos of the angle between points to measure the distance.

$$D(x, y) = 1 - \frac{x \cdot y}{||x|| ||y||}$$

where:

- $x$: The first data vector.
- $y$: The second data vector.
- $x \cdot y$: The dot product of vectors $x$ and $y$ given by $\sum_i x_i y_i$.
- $||x||$: The Euclidean norm (magnitude) of vector x, given by $\sqrt{\sum_i x_i^2}$

#### 3.6.2 Weighted K-Nearest Neighbour

In Weighted KNN, not all neighbours are treated equally. Closer neighbours are assumed to be more
relevant to the prediction than farther ones. So, each neighbour is assigned a weight based on its
distance to the query point.

Various weighting functions exist, for example the inverse distance weighting is given by:

$$w_i = \frac{1}{D(x_i, y_i)}$$

---

## 4 Unsupervised Prediction Models

### 4.1 K-Means Clustering: Hard Clustering

Unlike KNN, K-Means Clustering is an unsupervised prediction method, working to find natural groups
in unlabeled data.

Decision boundaries are made separating points (or feature values) into zones for prediction.

By calculating the distance between each feature and cutting the distance line
equidistant, perpendicular splits are created known as a Voronoi Diagram.

Process:

1. Select a number of clusters or groups $k$.
2. Plot the cluster centroids randomly in the problem space.
3. Calculate the distance between each feature data point to each cluster centroid.
    - See KNN distance measurement notes.
4. Assign each feature data point to its respective shortest distance cluster.
5. Update the cluster centroids to the mean (average) of all the points assigned to that cluster.

Thus, the cost function defined by:

$$J ( C_i, \mu_i ) = \sum_{i=1}^{k} \sum_{x_j \in C_i} || x_j - \mu_i ||^2$$

where:

- $k$: The number of clusters.
- $C_i$: The set of points assigned to cluster $i$.
- $\mu_i$: The centroid (mean) of cluster $i$.
- $x_j$: The data point.
- $|| x_j - \mu_i ||^2$: The euclidian distance (can be swapped for other distance measurement
  functions used in KNN).

#### 4.1.2 Standardization and Normalization

In cases for datasets or applications with various features with large differences in units of
measure, the data can be preprocessed for a better distance comparison.

Values can be normalized to be in the range 0 to 1.

Values can also be standardized by scaling each feature's values ensuring a mean of $\mu = 0$ and a
variance of $\sigma^2 = 1$.

$$\sigma^2 = \frac{\sum (x_i - \mu)^2}{N} = 1$$

### 4.2 Gaussian Mixture Models (GMM): Soft Classification

A Gaussian Mixture Model (GMM) assumes data points come from a mix of several Gaussian (bell-shaped)
distributions, each with its own mean and spread. Instead of hard-assigning each point to a single
cluster like k-means, GMM gives probabilities (soft assignments) of belonging to each cluster. This
makes GMM more flexible, since it can model elliptical clusters of different sizes and orientations,
while k-means only captures spherical clusters of equal variance. In short, GMM is a probabilistic,
more general version of clustering compared to k-means.

$$p(X \mid \mu, \Sigma) = \frac{1}{\sqrt{(2 \pi)^d \lvert \Sigma \rvert}} \, e^{-\tfrac{1}{2} (X - \mu)^{T} \Sigma^{-1} (X - \mu)}$$

where:

- $X$: The data point (a $d$-dimensional feature vector).
- $\mu$: The mean vector, representing the center (expected value) of the distribution.
- $\Sigma$: The covariance matrix, capturing the spread and correlation between features.
- $|\Sigma|$: The determinant of the covariance matrix, representing the volume (overall variance)
  of the distribution.
- $\Sigma^{-1}$: The inverse of the covariance matrix, used to scale distances according to feature
  correlations.
- $d$: The number of dimensions or features in $X$.
- $(X - \mu)^T \Sigma^{-1} (X - \mu)$: The squared Mahalanobis distance, a generalized distance
  measure accounting for correlations.
- $(2\pi)^{d/2} |\Sigma|^{1/2}$: The normalization constant ensuring the total probability
  integrates to 1.
- $p(X \mid \mu, \Sigma)$: The probability density of observing data point $X$ given
  parameters $\mu$ and $\Sigma$.
