# Gradient Descent 

---

**Gradient descent** is an optimization algorithm used to minimize some cost function by iteratively moving in the direction of steepest descent as defined by the negative of the gradient. In machine learning, we use gradient descent to update the parameters of our model. 
It works by iteratively adjusting the parameters in the direction that reduces the error (or cost) of the model's predictions. **Parameters** refer to coefficients in Linear Regression and weights in neural networks.


![gradient](https://ml-cheatsheet.readthedocs.io/en/latest/_images/gradient_descent.png)

Consider the 3-dimensional graph below in the context of a cost function. Our goal is to move from the mountain in the top right corner (high cost) to the dark blue sea in the bottom left (low cost). The arrows represent the direction of steepest descent (negative gradient) from any given point–the direction that decreases the cost function as quickly as possible.

Starting at the top of the mountain, we take our first step downhill in the direction specified by the negative gradient. Next we recalculate the negative gradient (passing in the coordinates of our new point) and take another step in the direction it specifies. We continue this process iteratively until we get to the bottom of our graph, or to a point where we can no longer move downhill–a local minimum.

![graph](https://ml-cheatsheet.readthedocs.io/en/latest/_images/gradient_descent_demystified.png)

**Learning rate**

The learning rate (α) is a hyperparameter that determines how big of a step the algorithm takes in the direction of the gradient. It's crucial to choose an appropriate learning rate. With a high learning rate we can cover more ground each step, but we risk overshooting the lowest point since the slope of the hill is constantly changing. With a very low learning rate, we can confidently move in the direction of the negative gradient since we are recalculating it so frequently. A low learning rate is more precise, but calculating the gradient is time-consuming, so it will take us a very long time to get to the bottom.

**Cost function**

A Loss Functions tells us “how good” our model is at making predictions for a given set of parameters. The cost function has its own curve and its own gradients. The slope of this curve tells us how to update our parameters to make the model more accurate.

The cost function measures how well the model's predictions match the actual data. For linear regression, a common cost function is the mean squared error (MSE)

> Cost(m, b) = 1/n * Σ(y_actual - y_predicted)^2

Here, n is the number of data points, y_actual is the actual y-value of a data point, and y_predicted is the y-value predicted by the model for that data point.

The gradient of the cost function indicates the direction of the steepest increase, so moving in the opposite direction will lead to a decrease in the cost.

> m = m - learning_rate * (∂Cost/∂m)
> 
> b = b - learning_rate * (∂Cost/∂b)













