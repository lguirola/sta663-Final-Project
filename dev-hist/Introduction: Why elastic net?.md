# Introduction: Why elastic net?

Regularized regression has now become one of the major tools of supervised learning for data analysis. Although its intellectual origins go as far as to the James-Stein estimator (1961) [1] and have an inmediate antecedent in the Tikhonov regularization (Ridge regression), a recent monograph reports how [[2]](http://web.stanford.edu/~hastie/StatLearnSparsity/index.html), since Tibshirani's original contribution (1996) [[3]](http://statweb.stanford.edu/~tibs/lasso/lasso.pdf) this field has been a vibrant one for the last twenty years. 

A key factor behind this prominence is the ruthless pragmatism of regularization methods. Traditional statistics and econometrics, in particular those of a frequentist flavor, evaluated estimator according to desirable asymptotic properties whose relevance held only under specific assumptions. In contrast, by balancing bias and variance and limiting overfitting, regularization is specifically designed for prediction purposes whose output can be fed into actual decision problems.

A particular advantage of LASSO and its successors (of which elastic net is a prominent example) is that it performs variable selection, according to its predictive power. LASSO appears as a convenient method to find relations between variables. This is of particular importance at least in two respects. When dealing with sparse data, standard OLS regression is not operative since the system is overdetermined. Moreover, when the functional form of the relation is not known, and cannot be approached succesfully by a linear model, it allows to fit more complex functional forms which otherwise may make the parameter space grow exponentially.

In this project we approach the implementation of Elastic net using a coordinate descent algorithm. Elastic net is a technique originally proposed in by Zou Hastie (2005) which combines the penalty of the LASSO (absolute value) with that of ridge regression (quadratic). Following [[Friedman et al 2007 , pg 6]](http://arxiv.org/pdf/0708.1485.pdf), it the solution solves the following problem:

$$min_{\beta} \frac{1}{2} \sum (y_i -\sum x_{ij} \beta_j)^2 + \lambda \sum_{j} (\alpha |\beta_j| + (1-\alpha) \frac{\beta_{j}^2}{2})$$

As it can be seen, the system is a modified OLS, in which a conver combination of a quadratic and an absolute value penalty is imposed on the coefficients. 

Our choice of the Elastic Net method is guided by its generality and flexibility. As it can be seen, for extreme values of $\alpha \in [0,1]$, the problem collapses in a LASSO or Ridge regression minimization. Alternatively, for a parametrization of $\lambda = 0$, the system provides an OLS solution. We chose to emulate this versatility from the package _[glmnet](https://web.stanford.edu/~hastie/glmnet/glmnet_alpha.html)_ by Hatie and Qian. 

Elastic net is also more flexible than its alternatives. At the cost of incorporate an additional parameter, it accomodates convex combinations of the LASSO and Ridge penalty.  A problem recognized in LASSO concerns that it tends to over-shrink those variables that have a small but still non-null effect, as well as those that are highly correlated.  This problem is compounded by that of the non-uniqueness of the solution [5](http://arxiv.org/abs/1206.0313): in the presence of highly correlated or quasi-colinear predictors, both may overlap in their predictive power and the algorithm may choose either of them, depending on the starting point. An alternative to fix this problem is the group lasso [6], which selects groups of coefficients that naturally should be considered as going together. Elastic net attenuates this problem by combining the ridge and lasso penalties as a mid point.

#Choosing coefficients by coordinate descent

The original Zou Hastie [4] paper proposed an homotophy method, LARS-EN, based on the original algorithm proposed by Efron et all (2004) [[7]](http://statweb.stanford.edu/~tibs/ftp/lars.pdf). Currently, however, a particularly efficient algorithm for computing the solution is the so called, _coordinate descent method_. If the data are standardized so that $ \frac{\sum x_i^2}{n} =1 $ and $\sum x_i=0$, the algorithm can be defined as:

$$ \beta_j \leftarrow \frac{S(\frac{\sum_i x_{ij} (y_i - \hat{y_i^j})_{+}}{n} , \lambda \alpha )}{1+(\lambda(1-\alpha) )} $$

Where the soft threshold operator is given by

$$S(\beta, \tau) = sign(\beta) (|\beta|-\tau)$$

And $\hat{y_i^j} = \sum_{k \neq j } x_{ik} \beta_{k}$ stands for the fitted values of the standing betas ignoring the jth column.

The algorithm cycles through the vector of coefficients updating them one at a time through simple OLS. It then applies a correction for the penalization factors. The soft threshold operator allows to solve the problem that the objective function is not differentiable in the neighborhood of zero. Interestingly, thus, the algorithms simplifies dramatically the optimization problem and makes it computationally tractable. From the functional form, the algorithm is guaranteed to converged to a global minimum. 



[1] James, W.; Stein, C. (1961), "Estimation with quadratic loss", Proc. Fourth Berkeley Symp. Math. Statist. Prob. 1, pp. 361–379
[2] [Hastie, Tibshirani, Wainwright, 2015](http://web.stanford.edu/~hastie/StatLearnSparsity/index.html) _Statistical Learning with Sparsity_, CRC Press
[3] [Tibshirani 1996](http://statweb.stanford.edu/~tibs/lasso/lasso.pdf), _Regression Shrinkage via the LASSO_ Journal of Royal Statistical Society, Series B
[4] [Zou, Hastie](https://web.stanford.edu/~hastie/Papers/B67.2%20(2005)%20301-320%20Zou%20&%20Hastie.pdf), 2005,  _Regularization and variable selection via the elastic net_ J. R. Statist. Soc. B (2005) 67, Part 2, pp. 301–320
[5] Tibshirani [The Lasso Problem and Uniqueness](http://arxiv.org/abs/1206.0313) 
[6] Yuan, Lin, 2006_Model selection and estimation in regression with grouped variables_ J. R. Statist. Soc. B (2006)
[7] [Efron,Hastie, Johnstone, Tibshirani (2004)](http://statweb.stanford.edu/~tibs/ftp/lars.pdf) The Annals of Statistics 2004, Vol. 32, No. 2, 407–499 
[8] [Friedman, Hastie, Ho, Tibshirani](http://arxiv.org/pdf/0708.1485.pdf) _Pathwise coordinate descent for optimization_ The Annals of Applied Statistics
2007, Vol. 1, No. 2, 302–332 
