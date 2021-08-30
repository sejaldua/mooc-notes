# machine-learning-coursera

*Machine Learning, taught by Andrew Ng via Stanford University and Coursera*

## Week 1

### Definitions

- Arthur Samuel (1959). Machine Learning: Field of study that gives computers the ability to learn without being explicitly programmed.
- Tom Mitchell (1998) Well-posed Learning Problem: A computer program is said to *learn* from experience E with respect to some task T and some performance measure P, it its performance on T, as measured by P, improves with experience E.
  - Example: playing checkers
    - E = the experience of playing many games of checkers
    - T = the task of playing checkers
    - P = the probability that the program will win the next game

### Supervised Learning

- regression = predicting a continuous value
- classification = predicting a discrete value / boolean

#### Examples

(a) Regression - Given a picture of a person, we have to predict their age on the basis of the given picture

(b) Classification - Given a patient with a tumor, we have to predict whether the tumor is malignant or benign.

### Unsupervised Learning

Unsupervised learning allows us to approach problems with little or no idea what our results should look like. We can derive structure from data where we don't necessarily know the effect of the variables.

With unsupervised learning there is no feedback based on the prediction results.

#### Examples

Clustering: Take a collection of 1,000,000 different genes, and find a way to automatically group these genes into groups that are somehow similar or related by different variables, such as lifespan, location, roles, and so on.

Non-clustering: The "Cocktail Party Algorithm", allows you to find structure in a chaotic environment. (i.e. identifying individual voices and music from a mesh of sounds at a cocktail party).

### Model Representation

Notation:

- `m` = number of training examples
- `x` = input variable / feature
- `y` = output variable / target variable
- `(x, y)` = one training example
- $(x^{(i)}, y^{(i)})$ = ith training example

Supervised learning goal:  
Given a training set, learn a function h: X --> Y so that h(x) is a "good" predictor for the corresponding value of y  
Function h is called a hypothesis

### Cost Function

$h_{\theta}(x) = \theta_0 + \theta_1x$

$\theta_0$ and $\theta_1$ are called parameters

Choose $\theta_0$, $\theta_1$ such that $h_{\theta}(x)$ is close to $y$ for our training examples $(x,y)$  

Cost function / squared error function $J(\theta_0, \theta_1) = \frac{1}{2m}\Sigma_{i=1}^m (\hat{y}^i - y^i)^2 = \frac{1}{2m}\Sigma_{i=1}^m (h_{\theta}(x^i) - y^i)^2$ where $h_{\theta}(x^i) = \theta_0 + \theta_1x$. This takes an average difference (actually a fancier version of an average) of all the results of the hypothesis with inputs from x's and the actual output y's.

The mean is halved as a convenience for the computation of the gradient descent, as the derivative term of the square function will cancel out the $\frac{1}{2}$

Minimize $J(\theta_0$, $\theta_1$)$  with respect to $\theta_0$, $\theta_1$
