# Machine Learning

---

<details markdown="1">
  <summary>Table of Contents</summary>

<!-- TOC -->
* [Machine Learning](#machine-learning)
  * [1 Intro and Data](#1-intro-and-data)
  * [2 Optimization](#2-optimization)
    * [2.1 The Cost Function](#21-the-cost-function)
    * [2.2 Gradient Descent](#22-gradient-descent)
  * [3 Supervised Prediction Models](#3-supervised-prediction-models)
    * [3.1 Overview](#31-overview)
      * [3.1.1 Overfitting and Overtraining](#311-overfitting-and-overtraining)
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
      * [3.4.1 Information Gain and Entropy](#341-information-gain-and-entropy)
        * [3.4.1.1 Overfitting and Pruning](#3411-overfitting-and-pruning)
      * [3.4.2 Gini Index and Weighted Sum](#342-gini-index-and-weighted-sum)
    * [3.4.3 Decision Regression Trees](#343-decision-regression-trees)
    * [3.4.4 Random Forest: Bagging](#344-random-forest-bagging)
    * [3.4.5 Adaboost: Adaptive Boosting on Trees](#345-adaboost-adaptive-boosting-on-trees)
    * [3.4.6 Adaboost and Random Forest](#346-adaboost-and-random-forest)
  * [4 Unsupervised Prediction Models](#4-unsupervised-prediction-models)
    * [4.1 K-Means Clustering](#41-k-means-clustering)
      * [4.1.1 K-Nearest Neighbour: Hard Classification](#411-k-nearest-neighbour-hard-classification)
        * [4.1.1.1 Distance Measurements](#4111-distance-measurements)
        * [4.1.1.2 Standardization and Normalization](#4112-standardization-and-normalization)
        * [4.1.1.3 Weighted K-Nearest Neighbour](#4113-weighted-k-nearest-neighbour)
    * [4.2 Gaussian Mixture Models (GMM): Soft Classification](#42-gaussian-mixture-models-gmm-soft-classification)
<!-- TOC -->

</details>

---

## 1 Intro and Data

---

## 2 Optimization

### 2.1 The Cost Function

The cost function, also known as the loss function, measures how well a machine learning model's
predictions match the actual target values. It quantifies the error between the expected and
predicted outputs, providing the feedback signal that guides learning.

### 2.2 Gradient Descent

Gradient descent is an iterative optimization algorithm that minimizes an objective function by
moving the current guess down the slope (opposite to the gradient), taking small incremental steps.

$$a_{n+1} = a_n - \eta_n \nabla f(a_n)$$

where:

- $a_{n+1}$: The next iterate after applying the gradient descent step.
- $a_{n}$: The current iterate at iteration n.
- $\eta_n$: The learning rate or step size at iteration n.
- $p_{n}$: The gradient of the cost function at the iteration n.

Heuristic optimization methods often incorporate randomization or other exploratory strategies to
reduce the chance of getting trapped in local minima and to improve the likelihood of finding a
global minimum.

YouTube,
3Blue1Brown: [Gradient descent, how neural networks learn | Deep Learning Chapter 2](https://youtu.be/IHZwWFHWa-w)

---

## 3 Supervised Prediction Models

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

#### 3.2.1 Gradient Descent Approach: Mean Squared Error (MSE)

The objective function is the mathematical expression that should be minimized in order to find the
best-fitting line (or hyperplane), in linear regression this is known as the Mean Squared Error
(MSE):

$$E(\omega) = \frac{1}{2} \sum_{n=1}^{N} \left( f(x_n, \omega) - t_n \right)^2$$

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

##### 3.3.3.1 Maximum Log-Likelihood

In order to make finding the maximum likelihood easier, the log can be taken, converting the product
over $N$ samples to a sum. This allows the maximum log-likelihood to be used as the objective
function with gradient accent/decent.

$$\ell_{log}(\omega) = \sum_{i=1}^N \Big[t^{(i)} \log p(t=1 \mid x^{(i)}; \omega) +(1 - t^{(i)}) \log \big(1 - p(t=1 \mid x^{(i)}; \omega)\big)\Big]$$

- If $t^{(i)} = 1$, only the first term remains: $\log p(t=1 \mid x^{(i)}; \omega)$.
- If $t^{(i)} = 0$, only the second term remains: $\log (1 - p(t=1 \mid x^{(i)}; \omega))$.
- So, for each data point, the function rewards the model if it assigns high probability to the
  correct label.
- Note: the log-likelihood $\ell(\omega)$ is concave in $\omega$.

##### 3.3.3.2 Binary Cross Entropy (BCE)

Real life optimizers that minimise the objective function typically use the inverse of the Maximum
Log-Likelihood, known as the Binary Cross Entropy (BCE):

$$J(\omega) = - \frac{1}{N} \ell_{log}(\omega)$$

- The mean is also taken to keep the loss independent of dataset size.

### 3.4 Decision Trees: Rules-based Hierarchical Splits

Decision trees are prediction models that uses a series of simple yes/no rules to split data into
branches, creating a hierarchical structure that ends in outcomes or predictions. Each split is
based on a feature that best separates the data, making the model easy to interpret and visualize,
like following a flowchart to reach a decision.

Process:

1. Select an attribute as the start of the tree.
    - Greedy search.
    - Information Gain and Entropy.
    - Gini Index and Weighted Sum.
2. Split the Data.
3. Check the homogeneity of new splits.
4. Branch out or terminate with a leaf (terminate on zero entropy/uncertainty attributes).

#### 3.4.1 Information Gain and Entropy

Is a measure used in decision trees to decide which attribute to split on at each step. It
quantifies how much "uncertainty" (entropy) in the dataset is reduced after splitting the data based
on a given attribute.

$$IG(S, A) = H(S) - \sum_{v \in V(A)} \frac{|S_v|}{|S|} H(S_v)$$

where:

- $H(S)$: The entropy of the full dataset.
- $S$: The entire dataset (or the subset of data currently at a node in the decision tree).
- $S_v$: The subset after splitting on value $v$.
- $V(A)$: The set of possible values of attribute $A$.
- $\sum_{v \in V(A)} \frac{|S_v|}{|S|} H(S_v)$: is the weighted average entropyof the subsets after
  splitting on attribute $A$.

The entropy or uncertainty is calculated as follows:

$$H(S) = -\,p(+) \log_{2} p(+) - p(-) \log_{2} p(-)$$

where:

- $p(+)$: Is the probability of positive examples.
- $p(-)$: Is the probability of negative examples.

##### 3.4.1.1 Overfitting and Pruning

When a decision tree overfits, pruning is used to simplify it. The idea is to replace certain
subtrees with leaf nodes that predict the majority class. This is tested on a validation set
(separate from the training and test sets). If accuracy improves or stays the same, the prune is
kept; if not, it's reverted. This prevents the model from memorizing noise and helps it generalize
better.

- Sometimes helps to split data into 3: training, testing and pruning.

#### 3.4.2 Gini Index and Weighted Sum

$$Gini = 1 - \sum_{i=1}^{n} p_i^2$$

where:

- $p_i$: The probability of class $i$.
- $n$: The number of classes.

### 3.4.3 Decision Regression Trees

Process:

1. Select an attribute to test, note that eventually all attributes must be tested.
2. Identify possible splits between each data point ($n_{\mathrm{attributes}} - 1$).
3. Calculate the averages of each sub-dataset formed by each split.
   $$\hat{y}_{\tau} = \frac{1}{N_{\tau}} \sum_{x_n \in Y_{\tau}}^{N} t_n$$

   where:

    - $\hat{y}_{\tau}$: The predicted/estimated output for subset $\tau$.
    - $N_{\tau}$: The number of samples in subset $Y_{\tau}$.
    - $t_n$: The target value (label/output) for sample $x_n$.
    - $x_n$: A single data sample/observation.
    - $Y_{\tau}$: The subset of data samples belonging to region/leaf $\tau$.
    - $N$: The total number of samples in the dataset.
4. Calculate the error of each subset for every split and add them together.
   The error for each subset is given by:

   $$E_{\tau} = \sum_{i=1}^{N} \left( y_{(i)} - \hat{y}_{\tau} \right)^2$$

   where:

    - $E_{\tau}$: The total squared error for subset $\tau$.
    - $y_{(i)}$: The observed/true value of the i-th sample.
    - $i$: The index running over the samples in the dataset (from 1 to $N$).

   The total error for each split is given by:

   $$E = \sum_{\tau=1}^{\tau} E_{\tau}$$

   where:

    - $E$: The total error across all subsets.
    - $\tau$: The index of the subset (e.g., a region, cluster, or leaf in a tree).
5. Select the split with the least respective error.
6. Repeat the steps for all attributes.

### 3.4.4 Random Forest: Bagging

$$\hat{H}_{bag}(x) = \frac{1}{N} \sum_{n=1}^{N} H_n(x)$$

- $\hat{H}_{bag}(x)$: The bagged (averaged) predictor output for input $x$.
- $H_n(x)$: The prediction made by the $n$-th model (or learner) on input $x$.
- $x$: The input sample or data point.

Process:

1. Divide the training data set into N random subsets.
2. Build $N$ trees to generate multiple hypothesis $Hn(x)$
    - Randomly overwrite the entropy decision and force data splits throughout the forest
3. Use the forest to predict the target variable of test instances as the average of all trees in
   the forest.

### 3.4.5 Adaboost: Adaptive Boosting on Trees

1. Assign equal we.ights to the dataset.
2. Select a stump.
3. Calculate the importance factor $\alpha$, for each datapoint factor.
   $$\alpha = \tfrac{1}{2} \log_{e} \left( \frac{1 - \text{Total Error}}{\text{Total Error}} \right)$$

   where:

    - $\alpha$: The weight assigned to a weak learner (in boosting).
    - $\text{Total Error}$: The weighted error rate of the weak learner.
        - For example: the sum of misclassified sample weights divided by the total.
4. Reweight the data points with the calculated importance factors.
    - Correct sample: $\omega_{(i)} \leftarrow \omega_{(i)} e^{-\alpha}$.
    - Incorrect sample: $\omega_{(i)} \leftarrow \omega_{(i)} e^{\alpha}$.

   where:

    - $\omega_{(i)}$: weight (importance factor) of the $i$-th training sample
5. Normalize the new weights.
   $$\omega_{(i)} \leftarrow \frac{\omega_{(i)}}{\sum_{i=1}^{N} \omega_{(i)}}$$

   where:

    - $\sum_{i=1}^{N} \omega_{(i)}$: Is the normalization constant, it ensures all weights sum to 1.
6. Make the newly formed dataset.
7. Repeat steps with the new dataset.

### 3.4.6 Adaboost and Random Forest

$$\alpha = \tfrac{1}{2} \log_{e} \left( \frac{1 - \text{Total Error}}{\text{Total Error}} \right)$$

where:

- $\alpha$: The weight assigned to a weak learner (in boosting).
- $\text{Total Error}$: The weighted error rate of the weak learner (fraction of sample weights
  misclassified).

---

## 4 Unsupervised Prediction Models

### 4.1 K-Means Clustering

#### 4.1.1 K-Nearest Neighbour: Hard Classification

Decision boundaries are made separating points (or feature values) into zones for prediction.

By calculating the distance between each feature and cutting the distance line
equidistant, perpendicular splits are created known as a Voronoi Diagram.

##### 4.1.1.1 Distance Measurements

**Euclidian Distance:** essentially by using a summed pythagorean theorem the distance between
points can be found.

$$\mathrm{ED} = \left\lVert x^{(a)} - x^{(b)} \right\rVert = \sqrt{ \sum_{j=1}^{d} \left( x_j^{(a)} - x_j^{(b)} \right)^2 }$$

where:

- $d$: The number of points (features in machine learning).

**Hamming Distance:** Finding the difference between binary numbers, for example:

`100011 XOR 110110 = 010101 -> 3 ones`

**Cosine Distance:** Calculating the cos of the angle between points to measure the distance.

##### 4.1.1.2 Standardization and Normalization

In cases for datasets or applications with various features with large differences in units of
measure, the data can be preprocessed for a better distance comparison.

Values can be normalized to be in the range 0 to 1.

Values can also be standardized by scaling each feature's values ensuring a mean of $\mu = 0$ and a
variance of $\sigma^2 = 1$.

$$\sigma^2 = \frac{\sum (x_i - \mu)^2}{N} = 1$$

##### 4.1.1.3 Weighted K-Nearest Neighbour

$$w_i = \frac{1}{\mathrm{ED}_i}$$

### 4.2 Gaussian Mixture Models (GMM): Soft Classification

A Gaussian Mixture Model (GMM) assumes data points come from a mix of several Gaussian (bell-shaped)
distributions, each with its own mean and spread. Instead of hard-assigning each point to a single
cluster like k-means, GMM gives probabilities (soft assignments) of belonging to each cluster. This
makes GMM more flexible, since it can model elliptical clusters of different sizes and orientations,
while k-means only captures spherical clusters of equal variance. In short, GMM is a probabilistic,
more general version of clustering compared to k-means.

$$p(X \mid \mu, \Sigma) = \frac{1}{\sqrt{(2 \pi)^d \lvert \Sigma \rvert}} \, e^{-\tfrac{1}{2} (X - \mu)^{T} \Sigma^{-1} (X - \mu)}$$
