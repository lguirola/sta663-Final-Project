## Abstract

In this project, we implement the elastic net (Zou Hastie 2005) technique for regularized regression. As described in the original contribution, Elastic Net is a generalized regularization method tha encompasses LASSO and Ridge regression as special cases. While, unlike Ridge regression, it performs variable selection (and is thus suitable for sparse data), it attenuates the problem of LASSO to overshrink coefficients of variables that are likely to be grouped. To compute it, we intend to choose among the several techniques proposed for it, including, but not exclusively, the LARS-EN suggested in the [original paper](http://stanford.io/1OWWTyZ), as well as algorithms such as coordinate gradient descent [Friedman,Hastie, Tibshirani (2010)](https://www.jstatsoft.org/article/view/v033i01), and others described in [Hastie, Tibshirani, Wainwright, chapter 5](https://www.crcpress.com/Statistical-Learning-with-Sparsity-The-Lasso-and-Generalizations/Hastie-Tibshirani-Wainwright/9781498712163). 


## Preliminary section headings

1. Literature review: this part of the paper will brieflly describe the technique and link it to the special cases of LASSO and Ridge regression.
2. Algorithm for implementation: we still have to decide which of the available algorithms we will use. Perhaps we could compare how each of them performs.
3. Algorithm for the choice of the tuning parameters, most likely by Cross Validation.  
4. Application to a dataset and comparative performance of distinct optimization approaches.
5. 

## Optimization and High-Performance Computing

We are planning to optimize our algorithms with vectorization, trying to use numpy functions and data structures as much as possible, and just-in-time compiler. Also, we would use Cython, Numbda or Pypy considering the raw data and what it is better to implement. Of cousrse, since we have been introduced to C/C++ in the course, we also consider using C++ to write our static methods and use Python to call to make computing fast.

On the other hand, we consider using multiprocessing to conduct parallel computing. With multiprocessing we may avoid the global interpreter lock issue with Pool package.
