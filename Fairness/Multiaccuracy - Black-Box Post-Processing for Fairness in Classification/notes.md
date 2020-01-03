# Multiaccuracy: Black-Box Post-Processing for Fairness in Classification

[Link](https://arxiv.org/pdf/1805.12317.pdf)

Introduction implies that this method will be applicable to settings in which we (a) don't want to or can't retrain the underlying (biased) model or (b) only have access to model predictions.

Setting: we are given "black-box" access to a classifier $f_0$ and a validation set of labeled samples drawn from a distribution $\mathcal{D}$. Our goal is to _audit_ $f_0$ to determine whether it satisfies a notion of fairness called _multiaccuracy_, which requires that predictions be _unbiased on every (identifiable) subpopulation_. We can also post-process $f_0$ to produce a new classifier $f$ that is multiaccurate, **without** adversely affecting subpopulations on which $f_0$ was already accurate. 

# Setting and multiaccuracy
$\mathcal{X}$: input space

$\mathcal{D}$: the valdiation data distribution supported on $\mathcal{X}$

$y$: a function $\mathcal{X} \rightarrow \{0,1\}$ that maps inputs to their label

$S$: subset of $\mathcal{X}$

$\chi_{S}(x)$: the characteristic function of $S$ where $\chi_{S}(x)=1$ if $x\in S$ and 0 otherwise

Post-processing learner receives as input a small sample of labeled validation data $\{(x,y(x)\}$ where $x\sim \mathcal{D}$ and "black-box access" to an initial model $f_0: \mathcal{X}\rightarrow [0,1]$. 

## Multiaccuracy
Definition: let $\alpha\geq 0$ and let $\mathcal{C}\subseteq [-1,1]^\mathcal{X}$ be a class of functions n $\mathcal{X}$. A hypothesis $f:\mathcal{X}\rightarrow[0,1]$ is $(\mathcal{C},\alpha)$-multiaccurate if for all $c\in C$: $\mathbf{E}_{x\sim\mathcal{D}}[c(x)*(f(x)-y(x))]\leq\alpha$.

Example: we could allow $\mathcal{C}$ to be $\chi_S$ (and its negation) for each subset in the collection. 

### Classifiication accuracy from multiaccuracy
The above definition guarantees that predictions are statistically unbiased. The authors show that if there is a functin in $\mathcal{C}$ that correlates well with the label function on a significant subpopulation $S$, then multi-accuracy translates into a guarantee on _classification accuracy_ on this subpoulation.

## Auditing for multiaccuracy
We use a learning algorithm $\mathcal{A}$ to audit a classifier $f$ for multiaccuracy. It aims to learn a function $h$ that **correlates with the residual function** $f-y$. (This auditor can then be used to solve the post-processing problem).

Definition: Let $\alpha>0, m\in \mathbb{N}, \mathcal{A} : (\mathcal{X}\times [-1,1])^m\rightarrow[-1,1]^\mathcal{X}$. A hypothesis $f$ passes $(\mathcal{A},\alpha)$-multiaccuracy auditing if for $h=\mathcal{A}(D;f-y):$ $\mathbf{E}_{x\sim\mathcal{D}}[h(x)*(f(x)-y(x))]\leq \alpha$.

The idea here is that if any "computationally-identifiable" subgroup correlates with the residual function $f-y$, then it won't pass  multiaccuracy auditing.

# Post-processing for multiaccuracy
Description of algorithm:

* Input space partitioned into positive/negative predictions from the black-box classifier
* Uses the learning algorithm $\mathcal{A}$ to search over $\mathcal{X}, \mathcal{X}_0, \mathcal{X}_1$ to find any function that correlates significantly with the current residual
* If algorithm $\mathcal{A}$ successfully returns a function $h$, algorithm updates predictions multiplicatively according to $h$, augmenting the model at each iteration

In practice, the authors use an algorithm $\mathcal{A}_\ell$ that learns a "smoothed version of the partial derivative function of the cross-entropy loss with respect to the predictions." if the magnitude of the residal $|f(x)-y(x)|$ is not too close to 1 for most $x\in \mathcal{X}$, ten the learned partial derivative functin should correlate well with the true gradient. This is shown empirically.

# Lingering questions
In eq (1) on page 4: if the expectation is taken over $x\sim D$, then won't the $\alpha$ multiaccuracy guarantee actually be more "lax" for less frequent groups? e.g., if the natural frequency of a subgroup $S$ is 5% and our $\mathcal{C}$ is $\chi_S$, then this $(C,\alpha)$ multiaccuracy guarantee will be 20 times less restrictive on the statistical bias of predictions on the minority subgroup than on the majority subgroup.

> Resolved: on page 5, the authors note that $\mathcal{D}$ could reflect the true population distribution or could be aspirational, as the classification error guarantee improves as the density of the protected subpopulation $S$ grows.