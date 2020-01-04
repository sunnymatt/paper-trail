# Tracking and Improving Information in the Service of Fairness

[Link](https://arxiv.org/pdf/1904.09942.pdf)

Study a formal framework for measuring the *information content* of predictors. Discusses notion of *refinement*, which increases the "overall informativeness of predictions" without losing informatin already contained in $z$. Increasing information content through refinements improves downstream selection rules across a wide range of fairness measures.

Caution against calbration: if we have calibrated risk scores where the scores are much more "confident" about $A$ than $B$, then a fixed threshold may select all qualified individuals in $A$ and none of the individuals from $B$.

Dfference between true risk distribution and the observed risk distribution represents a certain "information loss."

### Refinements
$I(z)$: a global measure of how informative a calibrated predictor $z$ is over the population of individuals; intuitively, as $I(z)$ increases, uncertainty in an individual's outcome decreases. 

But what does it mean for one set of risk scores to have "better information" than the other? A calibrated predictor $\rho : \mathcal{X} \rightarrow [0,1]$ is a **refinement** of $z$ if $\rho$ hasn't "forgotten" any information contained by $z$.

**Formally**: $\rho$ refines $z$ if $\mathbf{E}_{x\sim \mathcal{X}}[\rho(x) | z(x)=v]=v$.

**Cool lemmas**: If $\rho$ is a refinement of $z$, then for all selection rates $\beta\in[0,1]$, the true positive rate, false positive rate, and positive predictive value of $\rho$ will be better than or equal to that of $z$. Nice!

## Preliminaries
$\mathcal{X}$: domain of individuals

$\mathcal{Y} = {0,1}$: binary outcome space

$\mathcal{D}$, supported on $\mathcal{X}\times\mathcal{Y}$: joint distribution of individuals and outcomes

$x,y\sim \mathcal{D}$: independent random draw from $\mathcal{D}$

$S\subseteq \mathcal{X}$: subpopulation

$x,y\sim \mathcal{D}_S$: shorthand for data distribution conditioned on $x\in S$

$x\sim S$: denote a random sample from marginal distribution over $\mathcal{X}$ conditioned on membership in $S$

$z: \mathcal{X}\rightarrow[0,1]$ :  predictor that maps individuals to a real-valued *score*

$\text{supp}(z)$: the support of $z$ (I think this means the elements in $\mathcal{X}$ such that the score given by $z$ is non-negative?)

$p^*(x)=\mathbf{Pr}_\mathcal{D}[y=1|x]$ represents the inherent uncertainty in the outcome given the individual. The outcome $y$ for an individual $x\in\mathcal{X}$ is drawn independently from $\textsf{Ber}(p^*(x))$.

$\mathcal{R}^z(v)=\mathbf{Pr}_{x\sim\mathcal{X}}[z(x)=v]$: the risk score distribution induced by a predictor $z$ paired with the marginal distribution over $\mathcal{X}$. For a subpopulation $S\subseteq\mathcal{X}$, we denote by $\mathcal{R}^z_S$ the score distribution conditioned on $x\in S$.

## Measuring information in binary prediction
We quantify the "informativeness" of a predictor by the uncertainty in an individual's outcome given their score; mathematically, the authors use *variance*. For a Bernoulli random variable with expected value $p$, $\mathbf{Var}(Ber(p))=p*(1-p)$.

They define information content as follows for a predictor $z : \mathcal{X}\rightarrow[0,1]$:

$$I_S(z)=1-4*\mathbf{E}_{x\sim S}[z(x)(1-z(x))]$$

(The above factor of 4 acts as a normalization such that $I(z)\in[0,1]$.) Intuitive property that follows: the information content will be 0 for a calibrated predictor that always outputs 1/2.

**Information loss** is defined by comparing a predictor to the Bayes optimal predictor. $L_S(p^*;z)=4*\mathbf{E}_{v\sim\mathcal{R}^z_S, x\sim S_{z(x)=v}}[(p^*(x)-v)^2]$. Essentially, the variance of the true risk for an individual sampled amongst those receiving predicted risk score $v$ will be lower when the risk score distribution $\mathcal{R}_z$ provides *more information* about the true risk score distribution $\mathcal{R}_{p^*}$.

The authors then show that information content and information loss actually capture the same notion!

## The value of information in fair prediction
Upshot: refining the underlying predictions used to choose a selection policy results in an improvement in utility, parity, or impact (or combination of the three). 🤯


## Mechanism for refining predictors
We can evaluate information content $I(z)$ directly, but estimating the information loss $L(p^*;z)$ is generally impossible.

Refinement distance: Let $q,z: \mathcal{X}\rightarrow[0,1]$ be calibrated predictors. The *refinement distance* from $z$ to $q$ is given as: 

$$D_R(z;q)=\sum_{v\in \text{supp}(z)}{\mathcal{R}^z(v)*|\mathbf{E}_{x\sim\mathcal{X}_{z(x)=v}}[q(x)]-v|}$$

Intuitively, refinement distance averages the refinement "disagreements" over all values in the support.

**Main algorithmic result**: authors introduce an algorithm $\textsf{merge}$ that takes as input two calibrated predictors $q,z$ and produces a predictor $\rho$ that is a refinement of both $q,z$.

$\textsf{merge(z,q)}$ :

* Let $\mathcal{Z}=\{\mathcal{X}_{z(x)=v} : v\in \text{supp}(z)\}$
* Let $\mathcal{Q}=\{\mathcal{X}_{q(x)=u} : u\in \text{supp}(q)\}$
* For $Z_v\in\mathcal{Z}$ and $Q_u\in\mathcal{Q}$:
    * $\mathcal{X}_{vu}$=$Z_v \cap Q_u$
    * $\rho(x)\leftarrow \mathbf{E}_{x\sim\mathcal{X}_{vu}}[p^*(x)]$

Interpreting the updates: the $\textsf{merge}$ algorithm takes two different calibrated predictors and combines them into a refinement. This is obviously helpful when, for example, a lender wants to combine predictions from different sources. In addition, this framework also helps us a way to *reason about the marignal informativeness of individual boolean features*; the greater the difference between $\mathbf{E}_{x\sim\phi^{-1}(0)}[p^*(x)]$ and $\mathbf{E}_{x\sim\phi^{-1}(1)}[p^*(x)]$, the more informative.

* This can help us show that a subpopulation S is experiencing discrimination under a predictor z if merging the predictor based solely on S changes the information content! Neat.

# Lingering questions

What is $\text{supp}(z)$? (page 7)

What is a linear program? (page 15)