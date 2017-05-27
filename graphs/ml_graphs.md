## 2013 - Spectral Networks and Locally Connected Networks on Graphs - [Bruna et al. 2017](http://arxiv.org/abs/1312.6203) 
[youtube talk](https://www.youtube.com/watch?v=xk17mfFxkag)
 
Defines 2 resolutions to look on convolutions on graphs -

**Spatial reconstruction** - 
Views convolutions as hierarcical local receptive fields (_see Adam Coates and Andrew Y Ng. Selecting receptive fields in deep networks 2011_). This analysis assumes that we are dealing with local neighberhoods $S$, that are obtained by thresholding the parent graph. A key point is to use multiscale clustering (such as dyadic clustering) for pooling and subsampling the graphs.
This type of construction benefints for requiring less assumptions (such as regularity). However it is claimed to be naive and to not allow weight sharing across different locations in the graph

**Spectral construction** - 
The global graph structure is used togheter with its graph-Laplacian to generalize the convolution operator.
Exploits the spectral nature of convolutions in the Fourier domain in order to define the graph spectral definition as ( convolution in the fourier domain is simply the diagonal operation): $x*h = F^{-1} diag(Fh) * Fx$
where $ K_{k,l} = exp(\frac{{-2\pi(k\cdot l)}}{N^d}) $

 - Convolution on graph is a Linear operator
 - $x*_{G}h: = Vdiag(h)V^Tx$  where x is the graph and $h$ the operator
 -  Localized kernel derive smoother fourier representation, therefore they induced smoother kernels by a $\kappa$ interpolation kernel with restricted spatial support s.t. $h = \kappa \cdot \tilde{h}$
 - Fact is that smoothness requires similarity between eigenvectors of the Laplacian, therefore a 1D extension of the graph is developed (this is an open problem. Extend after full article review)




![alt text](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/Screenshot%20from%202017-05-20%2014-33-10.png)

## 2017 - Convolutional Neural Networks on Graphs with Fast Localized Spectral Filtering - [Defferrard et al. 2017](http://arxiv.org/abs/1606.09375)

Very efficient implementation of graph CNN with nice spectral interpretation of the filters using Graph Signal Processing (GSP - see [Shuman et al. 2013](https://arxiv.org/pdf/1211.0053.pdf)). 
The fast implementation is due to the use of Chebyshev polynomial approximations instead of Fourier basis decomposition. The polynomial approximation drives an K-local interpretation that is computationally efficient. 
  
It presents an interesting graph coarsing and pooling approach that I didn't fully grasp. The results were not evaluated on a graph problem that resembles our constraints, as the edges weren't represented by vectors. It is demonstrated on classification tasks (MNIST and 20NEWS)

![alt text](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/Screenshot%20from%202017-05-13%2015-39-02.png)
