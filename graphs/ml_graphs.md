
## 2015 - Convolutional Networks on Graphs for Learning Molecular Fingerprints- [Duvenaud et. al. 2015](http://papers.nips.cc/paper/5954-convolutional-networks-on-graphs-for-learning-molecular-fingerprints.pdf)
A novel molecular embedding approach.

### Intro
In the field of molecule feature extraction, data embeddings shouldn't always come in fixed size embeddings. In the proposed graph method **vertices** are individual atoms and edges are bonds. This approach overcomes:

* machine-optimized fingerprints instead of fixed sized vectors
* Differentiable fingerprints can be optimized to encode only relevant features.
* similarity between fingerprint fragments

![illustration](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/duvenaud) 

### Algorithm
Generalized the circular fingerprint algorithm by replacing the **Hashing** with an single layer NN and **indexing** with a softmax in order to  convert an arbitrary-sized graph into a fixed-sized vector. In essence, each atom is asked to classify itself as belonging to a single category and the sum of the labels produces the final fingerprint. **Canonicalization**, or neighborhood invariance, was achieved with summation.
There are similarities between the circular fingerprint algo and the neural one as $tanh$ can be interpreted as a hashing mechanism (with large inputs) and the sofmax and argmax as indexing.

![alt text](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/duvenaud2.png) 

### Interpretablity

In order to show interpretability they show sstructures that most activate individual features

**Takeaway**
this article illustrate a new graphical data encoding scheme that competes with the circular fingerprint algorithm. Results are domain specific...

## 2016 - Deep Neural Networks for Learning Graph Representations (DNGR) - [Cao et al. 2016](https://www.aaai.org/ocs/index.php/AAAI/AAAI16/paper/download/12423/11715) 

### Key points from abstract
- Learning graph feature representation from structural info.  
- Uses random surfing model instead of random walks. Incoporates Stacked denoising autoencoders for compression. 
- Provide analytical perspective to the objective function using the PMI matrix 

### Main contributions
1. DNNs have advantages in capturing non-linear information conveyed by graph, as opposed from linear dimensionality reduction techniques. 
2. Novel DNGR model has better empirical results in learning low dimensional vertex representations of weighted graphs. 

### Background
DeepWalk can be understood as sequencing the graph structure as a collection of linear structures (of vertexes).  
The authors are inspired by Levy & Goldberg's PPMI matrix factorization interpretation to W2V, and argue that using DNN instead of the linear truncated-SVD can be better for graph ML tasks.
Cons of sequence sampling methods – hyperparameter choosing and finite length of sequences. 
 
### The proposed method
"First, we introduce random surfing model (Sec 3.1) to capture graph structural information and generate a probabilistic co-occurrence matrix. Next we calculate the PPMI matrix based on our probabilistic co-occurrence matrix by following (Bullinaria and Levy 2007) (See Sec 2.3). After that, a stacked denoising autoen- coder (Sec 3.2) is used to learn low-dimensional vertex rep- resentations. 

![alt text](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/image.png)

**Notion of surfing** – For vertex i they define a vector $p_k$ that defines the probability of reaching the j vertex after k steps. The random surfing model has also a restart probability s.t. we have $1-\alpha$ to return to vertex 0. The explicit eq ![alt text](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/Screenshot%20from%202017-08-12%2017-32-41.png)
The main idea is that we have a transition matrix with probabilities, and non linear weighting s.t a node is defined by 

![alt text](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/Screenshot%20from%202017-08-12%2017-52-24.png)
I am not sure about the representation of the i-th vertex using r notation (very bad explanation and notations, probably the word representation as described in the SVD section).
 
**Stacked denoising autoencoders (SDAE) for compression** - Used SDAE for compressing vertex representation in a non-linear fashion. The stacked denoising part is by masking random parts of the input vector to zero, and by that modeling missing entries. The method is described to be more efficient than SVD as the inference time is linear in the number of vertices. 

*Results* -
Provided results doesn't seem be very convincing, and are unrelated to our task.



## 2017 - Inductive Representation Learning on Large Graphs - [Hamilton et al. 2017](https://arxiv.org/pdf/1706.02216) 
[code and project page](http://snap.stanford.edu/graphsage/)

Mainly concerns on the transductive/inductive approach. Inductive approaches enable greater generalization power and evolving graph setting, while transductive are a term of inference that rely on specific examples for predicting specific type of outcome.

![alt text](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/inductive_1.png)

The evolution concept is manifested in a feature aggregation procedure (see figure above). Compared to other approaches, the writers claim to use feature information in order to train the model for unseen nodes. The study focuses on creating generating useful representations for individual nodes.


### The proposed method

![alt text](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/inductive_2.png)
The h denotes the representation of a node.line 5 is the neural structure and AGGREGATE can be any of many possible aggregation functions (such as Mean-aggragator, LSTM aggregator and Pooling aggregator).
The connection to the Weisfeiler-Lehman Isomorphism test provides theoretical context for learning structure of node neighborhood. 
 
### Experiment- Reddit posts classification

Baselines – random classifier, logistic classifier (no graph structure).Reddit  task - predict which community different Reddit posts belong to. Data description - "In total this dataset contains 232,965 posts with an average degree of 492. We use the first 20 days for training and the remaining days for testing (with 30% used for validation)." 
They found out that K=2 (2-link subgraph neighborhood) gave a 10-15% boost in accuracy compared to K=1, however raising K >2 only 
 
### Theoretical justification

They show that their Algorithm is capable of approximating clustering coefficients to an arbitrary degree of precision. 
The basic idea behind the proof is that if each node has a unique feature representation, then we can learn to map nodes to indicator vectors and identify node neighborhoods




## 2013 - Spectral Networks and Locally Connected Networks on Graphs - [Bruna et al. 2013](http://arxiv.org/abs/1312.6203) 
[youtube talk](https://www.youtube.com/watch?v=xk17mfFxkag)
 
Defines 2 resolutions to look on convolutions on graphs -
 
**Spatial reconstruction** - 
The authors view convolutions as hierarchical local receptive fields. This analysis assumes that we are dealing with local neighborhood $S$, that are obtained by thresholding the parent graph. A key point is to use multiscale clustering (such as dyadic clustering) for pooling and subsampling the graphs.
This type of construction benefits for requiring less assumptions (such as graph regularity). However it is claimed to be naive and not allow weight sharing across different locations in the graph
 
**Spectral construction** - 
The global graph structure is used together with its graph-Laplacian to generalize the convolution operator.
Exploits the spectral nature of convolutions in the Fourier domain in order to define the graph spectral definition as ( convolution in the fourier domain is simply the diagonal operation): $x*h = F^{-1} diag(Fh) * Fx$
where $ K_{k,l} = exp(\frac{{-2\pi(k\cdot l)}}{N^d}) $
 
### convolution on graph
The concept of convolution on graph is defined as:
![alt text](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/image.png)
CNNs are referred as using the covariance operator (which is diagonalized by the Fourier basis) as the similarity kernel.
Convolution on graph is a Linear operator which can be formulated as:
 $x*_{G}h: = Vdiag(h)V^Tx$  where x is the graph and $h$ the operator
 
The issue of non-compact support of the filters in Fourier domain (F-domain), is a computational burden, as the decay of a function in the Euclidian domain is translated into smoothness in the F-domain. Localized kernel derive smoother Fourier representation, therefore they induce smoother kernels by a $\kappa$ interpolation kernel with restricted spatial support s.t. $h = \kappa \cdot \tilde{h}$
Fact is that smoothness requires similarity between eigenvectors of the Laplacian, therefore a 1D extension of the graph is developed (this is an open problem.Also, this part is the most vague/unclear concept in the paper.)
 
### important sources and background material -
- _Adam Coates and Andrew Y Ng. Selecting receptive fields in deep networks 2011_
- _Raif M. Rustamov and Leonidas Guibas. Wavelets on graphs via deep learning. In NIPS, 2013_
- _F. R. K. Chung. Spectral Graph Theory. American Mathematical Society_
- _U. von Luxburg. A tutorial on spectral clustering. Technical Report 149, 08 2006._
- _Matan Gavish, Boaz Nadler, and Ronald R. Coifman. Multiscale wavelets on trees, graphs
and high dimensional data: Theory and applications to semi supervised learning. In Johannes
Frankranz and Thorsten Joachims, editors, ICML, pages 367–374, 2010._


![alt text](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/Screenshot%20from%202017-05-20%2014-33-10.png)




## 2017 - Convolutional Neural Networks on Graphs with Fast Localized Spectral Filtering - [Defferrard et al. 2017](http://arxiv.org/abs/1606.09375)

Very efficient implementation of graph CNN with nice spectral interpretation of the filters using Graph Signal Processing (GSP - see [Shuman et al. 2013](https://arxiv.org/pdf/1211.0053.pdf)). 
The fast implementation is due to the use of Chebyshev polynomial approximations instead of Fourier basis decomposition. The polynomial approximation drives an K-local interpretation that is computationally efficient. 
  
It presents an interesting graph coarsing and pooling approach that I didn't fully grasp. The results were not evaluated on a graph problem that resembles our constraints, as the edges weren't represented by vectors. It is demonstrated on classification tasks (MNIST and 20NEWS)

![alt text](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/Screenshot%20from%202017-05-13%2015-39-02.png)
