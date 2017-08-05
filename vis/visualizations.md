## 2008 - Visualizing Data using t-SNE (needs cleanup and editing) - [van der Maaten et al. 2008](http://www.jmlr.org/papers/volume9/vandermaaten08a/vandermaaten08a.pdf) 
[DISTILL SOURCE](https://distill.pub/2016/misread-tsne/)

t-SNE is better than existing techniques at creating a single map that reveals structure
at many different scales. 

### Stochastic Neighbor Embedding (SNE) 

SNE starts by converting the high-dimensional Euclidean distances between datapoints into conditional probabilities that represent similarities. 
The similarity of datapoint $x_j$ to datapoint $x_i$ is the conditional probability, $p(j|i)$ , that $x_i$ would pick $x_j$ as
its neighbor.if neighbors were picked in proportion to their probability density under a Gaussian centered at $xi_$.

**perplexity**
which says (loosely) how to balance attention between local and global aspects of your data. 
The parameter is, in a sense, a guess about the number of close neighbors each point has.
the perplexity really should be smaller than the number of points. Implementations can give unexpected behavior otherwise

**stability and epsilon** 
Different data sets can require different numbers of iterations to converge.

**cluster size**
the two clusters look about same size in the t-SNE plots.t-SNE is a non-linear in sense of operating differently on different 
spatial regions. The t-SNE algorithm adapts its notion of “distance” to regional density variations in the data set. As a result,
it naturally expands dense clusters, and contracts sparse ones, evening out cluster sizes.
The bottom line, however, is that you cannot see relative sizes of clusters in a t-SNE plot

important points 
- Distances between clusters might not mean anything
- Random noise doesn’t always look random.
- You can see some shapes, sometimes
- For topology, you may need more than one plot



