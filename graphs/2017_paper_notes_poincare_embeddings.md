## Poincaré Embeddings for Learning Hierarchical Representations Maximilian (Nickel & Kiela, 2017)
[Nice blogpost in gensim](https://rare-technologies.com/implementing-poincare-embeddings)
### Intro
This embedding method defines parsimonious representation of data in the Poincaré unit ball (a hyperbolic space),when in different from Euclidean embeddings, both the hierarchy and similarity are captured by the embeddings.
While traditional methods successfully embed symbolic information on objects in a space where semantic similarity is captured by distance or inner product (see *Word2Vec*, *Node2Vec*, *DeepWalk* etc), they are constrained by the linear property of the space they live in as very large dimensions are required to model graphical relations. Complex datasets, which manifest power-law distribution can be often be represented by hierarchical structures such as trees (applies both to social networks and text). Effectively, this means that graphical structures with significant clustering can be organized in a hierarchy ([Ravasz 2003](https://journals.aps.org/pre/abstract/10.1103/PhysRevE.67.026112)).

### Why hyperpolic space? Poincaré embeddings?
The hyperbolic space is a space with constant negative curvature, and it is well suited for hierarchical structures, thus it recently became popular in network science,  as it has been shown that any finite tree can be embedded into a finite hyperbolic space ([Gromov 1987 ,Wiki page](https://en.wikipedia.org/wiki/Hyperbolic_group)).
For example, the number of children nodes on tree with some large [branching factor](https://en.wikipedia.org/wiki/Branching_factor), will grow exponentially as we stepping away from the source node. In hyperbolic geometry this can be modeled in 2 dimensions, where nodes in a distance $l$ from the root node are places in a hyperbolic sphere with radius $l$. This type of construction is possible as hyperbolic disc area and circle length grow exponentially with their radius.
In a personal tone, the reduction of linear distance of $k$ hops in Euclidean space, to the geometrical ratio is very efficient and data specific, as it reduces the combinatorial complexity of the embeddings.
	
![Demonstration of metrics of angle and lenght in the Poincaré ball](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/poincare2.png?raw=true) 

see ["Hyperbolic Geometry of Complex Networks"](https://arxiv.org/pdf/1006.5169.pdf) for thorough details.
The Poincaré half plane model is diffrentiable, thus suited to gradient-based optimization. Other alternatives include [the Beltrami-Klein hyperboloid model](https://en.wikipedia.org/wiki/Beltrami%E2%80%93Klein_model) which is less suited in terms of the derivative of the distance function. 
The distance metric in the Poincaré ball model is given by 
$$d({\bf u,  v}) = arcosh\Bigg({1+2\frac{\|{\bf u-v}\|^2}{(1-\|{\bf u}\|^2)(1-\|{\bf v}\|^2)}}\Bigg) $$
This equationis symmetric, where the norm indicate on the hierarchy of objects and the distance is encoded be their similarity.

### Optimization and training

For a full and thorough implementation description + tips and tricks, I highly recommend on [Gensim's implementation blogpost](https://rare-technologies.com/implementing-poincare-embeddings#2_the_journey_of). 
Stochastic Riemannian optimization techniques, such as RSGD, can be carried out within the Poincaré ball. Practically, the gradient is calculated over the tangent space to a point $\theta \in B^d$ on the manifold. In order to derive the Riemannian  gradient from the Euclidian gradient they simple rescale $\nabla_{Euclidian}$ by the inverse triangledown  Poincaré ball metric tensor - 
$$ g_x ^{-1} = \biggl (\frac{2}{1-\lVert x \lVert^2  }\biggl )^{-2}\cdot {g^E}^{-1}$$
where ${g^E}^{-1}$ is the inverse of the Euclidean metric tensor.
The update rule is the same as the GD method where
$$\theta_{t+1} = \theta_t-\eta _t\nabla_R \ell oss (\theta_t)$$
In order to assure that the embeddings would remain within the Poincaré ball they use a unit sphere projection operator. 

The training was initialized with uniform distribution randomly selected embeddings $\in U(-10^{-3}, 10^{-3}))$. The training used a two step scheme - First they trained an initial "burn in" phase for better angular initialization with reduced training rate. Next, learning rate was increased and training was regular.

### Results
In the paper, the Poincaré distance is compared with Euclidean and Translational distances, where the latter is an asymmetric measure $d(u,v) = \rVert u-v+r\lVert ^2$  and $r$ is a learnable translation vector. This metric is not fully unsupervised and particularly useful when hierarchical information is available. 
The embeddings are tested by the **reconstruction** abilities (for capacity measurement) and **link prediction** performance (generalization measurement).
WordNet embeddings are learned with a log-softmax like loss function of
$$\ell oss(\Theta) = \sum_{(u,v)\in D} \log {\frac{e^{-d(u,v)}}{\sum _{v' \in \text{Negative}}e^{-d(u,v')}}}$$
The reconstruction is measured by the Mean Average Precision of the ranked reconstructed list for each noun , $v$. The link preiction quality is captured by the rank of each relationship $(u,v)$ among the ground-truth negative examples for $u$ which were not in the hypernymy training set.
![Results](https://github.com/NoamGit/paper_reviews/blob/master/graphs/pictures/poincare3.png?raw=true)

#### Network embeddings
The evaluation datasets were of scientific collaboration (ASTROPH, CONDMAT, GRQC, and HEPPH). The " softmax" function of having a link between co-authors was 
$$P((u,v) = 1|\Theta)=\frac{1}{e}$$.
Results were SOTA with Poincaré embeddings

### Take away
- Poincaré embeddings enable very parsimonious representations whats allows us to learn high-quality embeddings of large-scale taxonomies
- Excellent link prediction results indicate that hyperbolic geometry can introduce an important structural bias for the embedding of complex symbolic data.
- State-of-the-art results for predicting lexical entailment suggest that the hierarchy in the embedding space corresponds well to the underlying semantics of the data