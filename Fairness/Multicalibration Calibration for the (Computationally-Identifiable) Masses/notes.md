# Multicalibration: Calibration for the (Computationally-Identifiable) Masses

[Link](http://proceedings.mlr.press/v80/hebert-johnson18a.html)

We can require that a predictor $f$'s predictions are group-accurate within a small $\alpha$: $|\mathbf{E}_{i\sim S}[f_i-p_i^*]|\leq \alpha$, where $S$ is a subset of the set of all individuals $\mathcal{X}$. 

Calibration strengthens the requirement of accuracy by requiring that for any value $v$, if we let $S_v = \{i\in S: f_i=v \}$, that $|\mathbf{E}_{i\sim S_v}[f_i-p^*_i]|=|v-\mathbf{E}_{i\sim S_v}[p^*_i] < \alpha|$. (Later on they slightly modify this definition to say that $f$ is $\alpha$-calibrated with respect to a group $S$ if there exists some $S'\subseteq S$ with $\mathbf{Pr}_{i\sim D}[i\in S']\geq (1-\alpha)* \mathbf{Pr}_{i\sim \mathcal{D}}[i\in S$) such that $|\mathbf{E}_{i\sim S_v \cap S'}[f_i-p^*_i]|=< \alpha|$. (Basically, the size of $S'$ is at least ($1-\alpha$) that of S). 

A strong definition of fairness would require that calibration holds on _every_ subpopulation. Of course, this is impossible with a limited training set. The definition of multicalibration is thus as follows: "A predictor $f$ is multicalibrated with respect to a family of subpopulations $\mathcal{C}$ if it is calibrated with respect to every $S \in \mathcal{C}$. The formal definition is as follows. Allow $\mathcal{C} \subseteq 2^{\mathcal{X}}$ to be a collection of subsets of $\mathcal{X}$ and $\alpha \in [0,1]$. A predictor $f$ is $(\mathcal{C},\alpha)$-multicalibrated if for all $S\in C$, $f$ is $\alpha$-calibrated with respect to $S$.

Alludes to fact that algorithm will learn a generalizable model and also that computational complexity of learning a multicalibrated predictor with respect to $\mathcal{C}$ is equivalent to weak agnostic learning $\mathcal{C}$.

# Learning multicalibrated predictors
* Algorithm maintains candidate predictor $f$
* Corrects candidate values of some subset that violates calibration
* Stops when candidate predictor is $\alpha$-calibrated on every $S\in\mathcal{C}$

Next, they define $\tilde{q}$ as a guess-and-check oracle that compares $p_{S}=\mathbf{E}_{i\sim S}[p^*_i]$ with a particular $v \in [0,1]$ and returns a ✔ if $v$ is within some range of the correct value $p_S$.

# Discussion

Interesting quote: 

> In a prediction setting where, given the data, there is still significant uncertainty in the outcome we feel that multicalibration should be considered as an alternative to balanced error rates. That said, a serious form of discrimination could arise if the uncertainty in outcomes is very different across different subpopulations; this would be a form of information-theoretic discrimination that multicalibration could help to identify, but could not remedy directly. 