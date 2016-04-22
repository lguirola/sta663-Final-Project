#Progress report as of April 21
## Xiaodong Zhai, Luis Guirola


We started with the idea of implementing the elastic net paper by Zou and Hastie 2005. A detailed description of this project can be found the "[abstract and heading.md](https://github.com/lguirola/sta663-Final-Project/blob/master/Abstract%20and%20headings.md)" document. So far, we have completed an extended version of this motivation, with a description of the technique and the algorithm to be implemented in the document called "[Introduction: Why elastic net?.md](https://github.com/lguirola/sta663-Final-Project/blob/master/Introduction:%20Why%20elastic%20net%3F.md)", which will be the introductory/theoretical part of our write up. 

In terms of working routine, we agreed to centralize the workflow upton github, which provides and excelend platform for interactive development, but also got used to meet about once a week to solve particular things and schedule to-do task for the week. We tried to combine efforts and peer code monitoring at each steps in order to correct errors and maximize efficiency in debugging. We agreed on a number of coding guidelines that we drafted in a document called "code-conventions.md". 

On the coding front, the current state of our work is as follows. We first proceeded to code the __coordinate descent__ algorithm for elastic net in plain Python. We ran into some complications that got us stuck in the debugging for a while. To assess the accuracy of our results, we compared them with alternative data sets -the diabetes data from  sklearn, and a simulated sparse data sets- and results obtained from other packages -from sklearn in particular. 

Once we managed to obtain satisfactory results and to wrap up the code in a function, we proceeded to the optimization step in which we are currently working. A simple comparison of the code performance with sklearn shown that ours did substantially worse 3.6/240 ms. More strikingly, compiling it with Numba did not achieve any improvement at all -we attribute this to the format of the code. These efforts can be verified in [this notebook](https://github.com/lguirola/sta663-Final-Project/blob/master/elastic_net_function_wrapper.ipynb). Starting from these points, each of us concentrated in rewriting the code in static Cython and C++, a task we are close to have achieve at the moment (the corresponding notebooks are [here](https://github.com/cliburn/sta-663-2016/blob/master/projects/FinalProjectGuide.ipynb) and [here]()

By April 21 evening, after over 40 hours work we finally implemented the coordinate descent in C++ and successfully wrapped it in Python by Cython. Here we have encoutered two main problems: 1) matrix its computation including multiplication and inner product 2) using armadillo in C++ and wrapping with Cython at the same will cause unknown error in compiler, which is not clear yet given even almost no google results can be found.

So we just abandoned armadillo in C++, and started with another idea: since it is pain to passing and transfering 2d array in C++ (with whatever array of pointers or pointers of pointers), we wil consider using 1d array. This is becasue with numpy ndarray, it does not matter much an array is 1d or 2d - it is more of a user perspective rather than hardware perspective. So to reshape any (m,n) matrix into (m*n,1) will be almost no time consuming: it is just how we see the array in API.

Thus, the C++ accelerating path is:

1) reshape data_X matrix to 1d array
2) with Cython, passing all arguments to C++ code
3) calcualte the results in C++, with memory manipulation to record the result
4) getting the (un)converged betas

And it works. Basically with C++ the algorithm imporves roughly more than 50%. Of course, it is slower than sklearn package. Especially with high-dimensional sparse matrix, it is even slower - but still much faster than naive code.

This can be found [here](https://github.com/shldngzh/project-sta-663-final/blob/master/wrapping_cpp_final_with_profiling.ipynb)
