# Applied Statistics 3

# 2019/04/01

## Multivariate Statistical Analysis

*Review*
Linear Combinations of Variables - reduce dimensions of a dataset
L = c1y1 + c2y2 + ...
* reduces n dimensions into a single dimension
* The coefficients determine what important feature the linear combination means

if c1 = c2 = ... = 1: sum of all vars
if c1 = c2 = ... = 1/q: average of all responses
if c1 = 1, c2 = -1, c3 = 0, ....: differencet between the first two responses
if c1 = c2 = 1/2, c3 = c4 = c5 = -1/3: diff between y1 and y2 vs y3-y5

### Principle Component Analysis (PCA)

goal: variable and/or data reduction
* create a few linear combos whuch contain approximately the same info as y1, y2, ...
    - use these combos in subsequent analysis
* descr of how vars are interrelated
* if you can avoid categorical, do it. It is easier to use

Look this up - **Priniciple component regression**
a principle component (Z1) is a linear combination of your q variables

#### Properties of PCA
* ordered by variance of Z
* expect most info to be contained in first few componenets
* \# of principle components <= # of q
* Z1, Z2, ..., Zq are uncorrelated - one of the selling factors of PCA
* ai'ai = [ai1 ... aiq] * [ai1 ... a1q](V) = ai1^2 + ... = 1
* sum(Var(Zi)) = sum(var(yi))

#### Determining Coefficients
* Var(Z1) maximized with constraint a1'a1 = 1
* Var(Z2) maximized with constraint a2'a2 = 1 and cov(Z1,Z2) = 0 (uncorrelated, independent

To generate coefficents, make use of
* The original variables' covariance matrix (if using original vars)
* the original variables' correlation matrix (if using standardized original vars)

**look up eigenvectors. I forgot the definition** - roots to the determinant to make the Identity - LambdaA = 0

Original vars:
* components easier to interpret
* results are depndent on the units of measurement
* PC tend to refelct vars with largest variance

Standardized vars
* more common
* ideal to use when vars are of different scale
* use 'standardized value' when interpreting variables

**Is this like the MVUE?** - no. This is overthinking this.
**Are we modeling our parameters?** - no. This is different

#### How many principle components should be retained?
* explain 80% of total variation (arbitrary, less ideal)
* lower bound on the number of retained componenets
    - sum(lambda_i) /q (average eigenvalue)
        - where lambda_i are the eigenvalues of the covariance matrix (original) or correlation matrix (standard) 
    - avg eignevalue will be 1 when using the correlation matrix
* scree plot (a plot of eigenvalues vs the component #). Choose # of components above where the plot begins to *flatten* out

#### Interpreting PC
* Focus on the *loadings* (coefficients). Loading of > 0.5 in magnitude helps determine which vars are influential
* If all elements of the first eigenvector/coefficents/loadings are positive, PC measures *size*. i.e. a weighted avg of vars that make up the componenet
* Correlations between the vars and the principal components **finish this**

If coefficients are all positive and roughly the same size in magnitude, we can say the first principle componenet is a weighted average


### Canonical Correlation Analysis

Start with two sets of vars:
1. y1,y2,..,yq (set of response vars)
2. x1,x2,...,xq (set of explanatory vars)

goal: Find coefficients a1,a2,...,aq and b1,b2,...,bq that maximize corr between
U = a1y1 + ... + aqyq   
V = b1x1 + ... + bpxp
These are called canonical variates. Essentially linear combos of our original two sets of vars

* measure of corr between Y's and X's
* extension of multiple correlation (sqrt(R^2))
* Often a complement to multivariate regression

\# of canonical corr = s = min(p,q)
* Ui and Vi are correlated. i = 1,...,s
* Ui and Vj are uncorrelated, i != j
* Vi and Vj are uncorrelated, i != j
* Ui and Uj are uncorrelated, i != j

proportion of variance explained by ... **finish this**

#### When to use?
* where regression analysis is appropriate but theres more than one dependent variable U
* Especially useful when dependent varrs are moderately inter-related
* also used to test the independence between the independent set of vars (X's) and dependent set of vars (Y's)

#### Assumptions & Conditions
* Linearity of correlations
* Linearity of relationships
* Multivariate Normality
    - Desirable since it standardizes a distribution to allow for a higher correlation among the vars
        - Highly recommended that all vars be eval for normality and transformed if needed

#### Redundancy Analysis
* evaluates the adequacy of predication from the canonical analysis. Explains variation

# 2019/04/08

Continuing Multivariate Analysis
**Is Classification Analysis in the realm of Category Theory?**

## Discriminant Function Analysis

Assumptions
* Equal Spread
* some assume normality

Goal: classify a subject or unit into two or more groups based on info collected on independent variables

* How likely a subject is in group 1, 2, ..., or j based on the basis of a set of quantitative variables
* Groups must be clearly defined
* Distribution between groups?
    - Yes: parametric method - linear or quadratic discriminant function
    - no: nonparametric method

* come up with a single set of coefficients to apply to all groups
Construct linear combos of these vars and use them to distinguish the populations

### How to determine coefficients in [a]?
* Maximize separation between two groups

(zbar_1 - zbar_2)^2 / s^2_z) = (ybar_1 - ybar_2)^TSpl^-1(ybar_1 - ybar_2) = D^2 = Mahalanobis distance
* Like a Z-Score. Multi-dimensional generalization of the idea of measuring how many std devs away from a point is from the mean of a distribution
    - how far from the centroid is this value

scalings from linear discriminant function are not the same as [a]^T = Spl^-1(ybar_1 - ybar_2)
```r
# transpose of a matrix in R
solve(a) 
```

### D.A. for Several Groups

Goal: Find a vector a that maximally separates zbar_1, zbar_2, ..., zbar_k

going to get a z per group when dealing with multiple groups

How? Use the H matrix from MANOVA in place of (ybar_1 - ybar_2)^T and E in place of Spl

H = how spread out between each groups
E = how spread out within each group

**review slides here. The math needs more explaining**

**TODO: Review Linear algebra**
* Eigenvectors & Eigenvalues
* Inverse & Transpose


These discriminant functions are uncorrelated
They show the dimensions or directions of differences among y1,y2,...,yk 
The relative importance of each discriminant function can be assessed by considering its eigenvalue as a proportion of the total

Matrix E^-1H is not symmetric. To do this in R:
* Find matrix U that is the Cholesky factorization of E. E = U^tU
* Find the eigenvector b of the matrix (U^-1)^THU^-1
* a = U^-1b is an eigenvector of E^-1H

### Standardized Discriminant Functions
Helps with interpretation. The largest magnitude contributes the most to the equation. Like PCA and CCA

### Tests of Significance
* Two Group Case
    - To test H_0: a = 0. Use Hotelling's T^2
* Several Group Case
    - Wilks' lambda since eigenvalues are the same as the eigenvalues from the MANOVA
    - V_m = [N - 1 - 0.5(p + k)]sum(ln(1 + lambda_i))
        - p = # of vars
        - k = # of groups
        - V_m ~ chisq distribution (p−m+1)(k−m) degrees of freedom

Can perform forward, backward, and stepwise selection to determine the predictors that are most most significant for discriminating against others

## Classification Analysis

Def: the predictive aspect of discriminant analysis
* allocate obs into groups
* of ten gets called discriminant analysis in scientific literature
* In engineering & C.S - Pattern Recognition
* Some writers use classification to describe cluster analysis

start with sampling unit whose group membership is unknown
unit is assigned to a group on the basis of the vector of p measure values, y, associate with the unit.
must have a previously obtained sample of obs vectors from each group

One approach? compare y with mean vectors ybar_1,...,ybar_k of the k samples & assign the unit to the group whos ybar_i closest to y.

**No Assumptions about distributions**

covar_1 = covar_2  (same covariance matrix)

if prior probabilities p1 and p2 are known for 2 pops, the classification rule can be modified. We need to know the densities of each population in order to incorporate into the classical rule.

Normal Based Classification Rule
* f(y|G_1) ~ N_p (mu_1, sigma) and it is known ahead of ttime that p1 of the obs are from G_1
* same as above for mu_2

**can you get a tie?** - not likely with classification. If using k nearest neighbor, then possibly

What if covar_1 = covar_2 = ... = covar_k does not hold?
Distance function is now D_i^2(y) = (y - ybar_i)^TS_i^-1(y - ybar_i) where S_i is the sample covariaance for the ith group

### Estimating Misclassification Rates
Error Rate: probability of misclassification
Correct classification Rate: Complement of Error Rate

A simple estimate of the error rate is to plug the values back in and see how many matched.

### Improved Estimates of Error Rates

For large samples the apparent error rate has only a small amount of bias for estimating the actual error rate. For samall samples?

Holdout/leave-one-out/cross-validation method
* All but one observations is used to compute the classification rule
* then used to classify the omitted obs

### Nearest Neighbor Classification Rule
The earliest nonparametric classification method - k nearest neighbor rule
First compute the distance from an obs y_i  ti all other points y_i using the distance function (y_i - y_j)^TS_pl^-1(y_i - y_j), j != i

if majority of the k points belong to G1, assign y_i to G1, else G2

How to choose k?
* choose k near sqrt(ni) for a typical ni
* one could try several values of k and use the one with the best error rate.

# 2019/04/15

More Multivariate Statistics. More details generally in a Multivariate or Data Mining Course

## Cluster Analysis

Goal: Separate individual obs/items into groups/clusters on the basis of values for the p vars measure on each var.
* items called **objects** but essentially rows
* anything in same cluster is as similar as possible
* many distance measures but typically Euclidean Distance is
* Cluster analysis is a type of unsupervised classification because we don't know the nature of the groups or the num of groups before we classify into clusters

What is the difference between this and Linear Classification

### Applications
* Marketing - distinct clusters of consumer pops so several different marketing strategies can be used for the clusters
* Ecology - classify plants/animals into groups based on characteristics
* Genetics - seperate genes into classes based on expression ratios measured at diff time pts

### Assumptions
* N objects/cases/rows of data
* K clusters/groups
* If K is known, then number of ways to partition N into K is a **stirling number of the second kind**
* If K is not known, the num of possible partitions is more massive

### Types of Clustering methods (oldest to newest)
1. Hierarchical
2. Partitioning
3. Model-based

#### Hierarchical Methods

Cluster data in a series of n steps, joining obs together step-by-step to form clusters

How to determine the **closest** clusters at any given step? Maybe Euclidean distance

**Pros**
* computational speed for small datasets
* dendogram gives a dendogram for a variety of k clusters

##### Linkage methods

**single linkage** or nearest neighbor
The method for joining clusters are  whos min dist between obj is smallest. i.e. joins cluster A and B with smallest:

d_AB = min(d_ij)

where d_ij is the distance between an element in A and B. 


**complete linkage**

the opposite of above where we look for max instead of min.


**average linkage**

joins cluster whos avg dist between objs is smallest

##### Aggolomerative clustering

Each object is its own cluster, each containing a single object.

At each step, the two closest clusters merge into a single cluster, and so forth.

last step contains 1 cluster that has n objects

```r
hclust()
```

##### Dendrogram

H.A produces not one parition but mutliple. its basically a tree graph. Shows all the clusters

**Wouldn't we analyze each cluster to see if it has some significance?**

**How does it cut the tree? Mathematically**

##### Applied

```r
# calculate euclidean distances. doesnt require anything additionall
dist(df)
```

##### Standardization
* Div each col by its sample std dev so all vars have a std dev of 1
* Div each var by its sample range (max - min).
* Z-scores

#### Partitioning

first determine k then typically attempt to find the partition into k clusters that optimizes som objective function

for a fixed value of K, seek best possible partition for that k.

Many values of K can be chosen in order to see if a specific metric is being satisfied.

##### K-means Clustering

Find the partition of n objectes into k clusters that min(within-cluster Sum of squares)

traiditionak K-means uses Euclidean distance beween two clusters.

Goal to min sum of these squared euclidean sistances

WSS = sum(sum(d^2_E(yi,ybar_c)))

final clustering result somewhat depnds on the initial config of the objects/rows

In practice, good to rerun the algo a few times (with diff starting pts) to make sure the result is stable

##### Wards method

Mix of Hierarchical and K-means.

##### K-medoids 

robust alternative to k-means. Attempts to minimize the crierion

Crit_md = sum(sum(d(y_i, m_c)))

M-c is a medoid, or "most representative object". Think of it as a p-variate median

Like K-means, the K-medoids algo does not globally minmize its criterion in general.

Pros:
* function can accept a dissimilarity matrix as well as raw data matrix.
* it generates silhouttes for K clusters so dont need to decide ahead of time

Cons:
* computationally infeasible for n > 5000

Other criteria for choosing k include the Dunn Index and the Davies-bouldin Index

#### Model-based

Assumes that the pop generating the data consists of k subpops which correspond to the k clusters we seek.

There for the distr for the data is assumed to be composed of k-densities.

#### Clustering Model Setup

**A lot of slides on the math behind this. Review slides for more info.**

Binary clustering can pose a problem because 0 -0 and 1 - 1 are weighted equally. There are cases where this is not desirable. For example, rare data.

A dissimilarity matrix can be created to define a distance between binary clusters. See slide 60-61 for more info.

**Would we cluster using multiple methods and compare?**

## Multidimensional Scaling

Use distances to measure how diff multivariate obs were from each other. Can Take a multivariate dataset (a set of p-dim vectors) and calculate distances between pairs of vectors

Both multidimensional scaling and correspondance analysis are techniquest related to distances.

This can be viewed as a way of generating ...

### Classical MD scaling (MDS)

Given a n x n matrix, goal is to construct a **map** containing multivariate points

There are no unique best spolutions where to place points on map.

Sometimes known as Principle Coordinates Analysis

## Correspondence Analysis

contingency table presents sample values for two categorical variables

Test for independence betweeen two categorical vars

can be used to supplement chi-squared test

### Chi-squared distance

col proportions with entries p_ij = n_ij / n_i

### Interpretation

all row and cats are labeled on plot.

Two row cats near each other have similar conditional distr. across cols

Two col cats have simiplar profiles down thr wos

A row and col cat close together tend to appear more often that would be expected under independence


# 2019/04/22

## Office Hour chat with Phil Yates

How important is multivariate normality? What can we do without it?
- its important for determining the importance of Linear Discriminant Functions but not actually important for doing the predictive portion of it. The classification portion has no underlying assumptions about distributions though it is 

I did QDA because we had binary variables. 
- Binary variables cannot be multivariate linear so its fine.
- Logistic regression is the appropriate tool for this sort of classification anyway

Interpreting variables that are standardized. Does standardizing mean that interpretation is not causal? At least maybe interpretation in a nice way. decreeasing/increasing one standardized unit is not very helpful to interpretation.
- when you standardize variables, interpretation already gets weird. encapsulating them in PC adds another layer of abstraction.

What are some things that I can work on improving?
- interpretation & communication (everyone can use work on it)
- being more confident in my results
- other than that, I am top of the class so I am doing well as is


Statistical course notes in Graduate Program
- not monte carlo
- stochastic sounds cool

## Topics for tonight

1. Inferences for the difference of two proportions
2. Inference about the ratio of two odds
3. Inference from Retrospective Studies

## Inferences for the difference of two proportions

AKA a Bernoulli Random Variable

mean{Y_i} = P - P(Y = 1 or Y = 0)

Var(Y) = sum(Pr(Y = y) * (y - mu)^2) 
= (1 - P)(0 - P)^2 + P(1 - P)^2
= P(1 - P)[P + (1 - P)]
= P(1 - P)

Mean(Y_i) = P

**interlude on writing the proof for E(Y) and Var(Y) for a Binomial Distribution**

Wald confidence interval = uses unbiased estimator for pi

## Odds

the probability of something happening. Noted by lowercase omega

2/7 chance of winning, 5/7 chance of losing. Odds are (2/7) / (5/7) = 2/5 = 2:5

### properties

* omega >= 0
* if P = .5, then omega = 1
    - "equal odds", "even odds", "50-50 odds"
* if omega is the odds of a success, then 1/omega is the odds of a failure
* if omega is the odds of a success then
    P = omega / omega + 1

### Odds Ratio

phi = omega_1 / omega_2 = 5

How to interpret?

phi = omega_1 / omega_2 = 5 => omega_1 = 5omega_2

Interpretation
* The odds of "success" in group 1 is 5 times the odds of "success" in group 2

The sample odds ratio is
phi = omega_1 / omega_2
|   | Response    |
|   | Yes  | No   |
| 1 | n_11 | n_12 |
| 2 | n_21 | n_22 |

phi = n11 * n22 / n21 * n12

Why is the odds ratio preferred to a difference in population proprtions?
* phi tends to remain more nearly constant over levels of confounding variables
* phi is the only parameter that can be used to compare two groups of resp. from a retrospective study
* phi extends nicely to regression analysis, particularly logistic regression models

### Sampling Distr of the Log of the Estimated Odds Ratio

the sampling distr of logphi, where phi is the **sample odds** ratio, has the following properties
* E(log(phi^hat)) approx log(phi)
* Var(log(phi^hat)) approx (1 /(n_1p_1(1 - p_1))) + (1 /(n_2p_2(1 - p_2)))
    - looks like binary distribution
* if n1 and n2 are sufficiently large, the sampling distr is approximately normal

H_0 : phi = 1 <=> log(phi) = 0 <=> omega_1 = omega_2 <=> P_1 = P_2

### Retrospective sampling only uses the odds ratio

The odds ratio is the only parameter that describes binary response outcomes for the explanatory categories that can be estimated from retrospective data.

The odds ratio is the same regardless of which factor is considered the response

## Studies

**Prospective Study**
* subjects selected from or assigned to group with specified explanatory factor levels
* Their responses are then determined

**Retrospective Study**
* subjects selected from groups with specified response levels
* their explanatory factores are then determined.

Why retrospective?
* if response props are small, huge samples are needed in a prospective study to get enough yes outcomes to permit inferences to be drawn
* this problem is bypassed in a retrospective study

## Summary
* pops of binary responses can be compared by estimating P1 - P2 or pho
* Tests and confidence intervals are based on the approx normality of P1 - P2 or on the approx normality of log phi
* techniques apply to randomized experiments
* choice to compare proportions or odds is subjective.
* if data samples retropsecctively, can only estimate phi

# 2019/04/29

* Population Models for 2 X 2 Tables of Counts
* The Chi-Squared Test
* Fisher's Exact Test
* Combining Results from Several Tables with Equal Odds Ratios

Would encounter this in a Categorical Data Analysis course

# Population Models for 2 X 2 Tables of Counts

H0 : pi2 - pi1 = 0

equal to

H0 : w2 / w1 = theta = 1

These are called **tests of homogeneity**.
- is the binary response the same across pops?
- two populations

**Tests of independence** - Is there an association between row and column factors without specifying one of them as a response variable?
H0: The row cat is independent of the column cat
- one population (key difference between test of homogeneity and independence)

## Sampling Schemes

**Poisson Sampling** - Frequency of Success over a period of time or space
- Random sample from a single pop
- Each member falls into one of the four cells of the 2 X 2 table
- **No marginal totals known in advance**
- Used for a test of homogeneity and independence

**Multinomial Sampling** - K categories for a sample of N
- Similar to Poisson except the **total sample size, T, is fixed in advance**
- Used for tests of homogeneity and independence

**Prospective Product Binomial Sampling**
- more than one binomial distr. present
    - two binomial populations that im working with
- Random samples selected from each population
- **Row totals are fixed in advance**
- Used for a test of Homogeneity

**Retrospective Product Binomial Sampling**
- flip explanatory and response variable from Prospective Product Bin. Sampling
- Same as above except **Column Totals are fixed in advance**
- Used for Test of Homogeneity but only for the odds ratio

**Randomized Binomial Experiment**
- subjects randomly allocated to the two levels of the explanatory factor (rows of the table)
- follows prospetive product binomial sampling except instead of a random sampling, we uses randomization of subjects into groups
- Used for Tests of Homogeneity

**Hypergeometric Probability Distribution**
- **both row and column totals are fixed**
- if interest is strictly focused on the odds ratio, statistical analysis may be conducted conditionally on the row and column totals
- Used in Fisher's Exact Test: nice with a really small sample size

Odds Ratio can be used with any Sampling Scheme!

## Pearson Chi-Squared Test for Goodness of Fit

**Observed Count**: num of units that fall into a cell
**expected Count**: num of units predicted by theory to fall into cell (when H0 is true)

chi-squared = sum((Observed - Expected)^2 / Expected) // summed over all cells of the table


if H0 is true, chi-square approx chi-square where df = num of cells - 1

Basic GoF Test is Mendels Chi-Square Test

### Chi-Squared Test of Independence in a 2 X 2 Table

Proportion of Counts in Column 1: C1 / T

**Limitations of Chi-Squared Test**
- only product is a p-value
- no associated parameter to describe the degree of dependence
- the alternative hypothesis is very general
    - if trying to find the source of a dependency, look at the expected ratios vs the actual ratios
- when more than two rows and coluns are involved, there may be a more specific form of dependence to explore

### Chi-Squared Test of Independence in an R X C Table

When H0 is true, sampling distribution of X^2 has an approximate chi-square distribution with (r - 1) X (c - 1) df where r is the num of rows and c is num of cols.

### Randomization Distribution of the Difference in Sample Proportions
- let pi_d = pi_1 - pi_2
- What is the proportion of differences at least/most pi_d in size (one-sided)
- What is the proportion of differences at least or at most pi_d in size (two-sided)

### Hypergeometric Distribution for one sided p-values
- shortcut for the p-value
N1, N2 = size of populations for two groups
n = size of sample
N - Total population size
P(K = k) = (N1 k) (N2 n-k) / (N n)

- focus on n11
- expected count is R1C1/T
- If n11 is greater than its expected count, we want values of k = n11, n11 - 1,.., min(R1, C1). If n11 is less its expected count, we want values of k = 1,..,n11

## Fishers Exact Test
- randomization test based on statistic pi_1 - pi_2
- when data is observational, it is thought of as a permutation test
    - useful interpretation when entire pop has been sampled or when sample is not random
    - permutation test equivalent to a sampling distribution
        - allows for inference about pop parameters with randoms samples from Poisson, multinomial, or product binomial sampling schemes
        - exact p-values can be obtained for tests of equal population props, or equal pop odds, or for independence

### Mantel-Haenszel Excess

**Excess** is a name given to the observed count minus the expected count in one of the cells of the 2 X 2 table. If we focus on n11, the excess is n11 - R1C1/T (this is like a residual for the cell)

when H0 is true, E(Excess) = 0, Var(Excess) = R1R2C1C2 / TT(T - 1)

- For 2 X 2 tables of counts, the excess is an approximation to Fisher's Exact Test
- p-value is close but not identical to the one from the Z-test for equal pop props and the chu0squared test for independence
- excess stats from several 2X2 tables 

Can I develop an overall association if I had a third factor? Essentially try to do a weighted average of the odds ratio from each 2 X 2 table. A 2 X 2 table per third variable (or confounding variable) will exist. Is it safe to assume that the odds ratios are the same for all the tables?

- the sum of all expected counts in all the tables should be the same
**the only test that can be combined over several tables**

Tests for conditional independence and homogenous association for the k conditional odds ratios in k 2 X 2 tables. It combines sample odds ratios for the partial k tables into a single summary measure of partial association. It is appropriate for prospective and retrospective observational data, and randomized experiments.

#### Assumptions
- Odds ratio is the same in each 2 X 2 table.
    - H0: X and Y are conditionally independent given Z (theta_{XY(k)} = 1)
    - one Test for this assumption is the Breslow-Day Statistic
- Sum of the expected counts (added over all tables) should be at least 5, one for each of the four cell positions.


## Mantel-Haenszel Chi-Squared Test

The **Mantel-Haenszel Chi-Squared Test** is a more powerful alternative to the Pearson Chi-Squared Test when at least one of the factors are **ordinal**. (not to be confused with the aforementioned test)

r = some measure of the sample correlation between the two factors
n = sample value

M^2 = (n - 1)r^2

H0: independence vs HA: rho != 0

When H0 is true, the sampling distr of M^2 is approx chi-square with 1 degree of freedom.

Define an ordinal as a midpoint for a range for response variables.



# 2019/05/06

* Logistic Regression
* Estimation of Logistic Regression Coefficients
* Drop-In Deviance Test

## Generalized Linear Models

**def**: probability model in which the mean of a response variable is related to explanatory variables through a regression equation

### Link Functions

**Link Function**: a specified function of mu equal to the regression structure
* g(mu) = B0 + B1X1 + ... + BpXp

#### Normal

Used for Ordinary Least Squares Regression
Link = Identity
Function = g(mu) = mu

#### Poisson

Used to Count occurrences in a fixed amount of time/space
Link = Log
Function = log(mu)

#### Bernoulli, Binomial, Categorical, Multinomial

Outcome of a single binary response, OR # of successes, OR outcome of a single K-way occurrence
Link = Logit
Function = log( pi / (1 - pi))


## Logistic Regression

**Logit Function** - Known as Log odds because it is the log function of the odds where the odds of success = pi
g(pi) = log(pi / (1 - pi)) = log(theta) = B0 + B1X1 + ... BpXp = eta

**Logistic function** - Inverse of the Logit function
pi = exp(eta) / (1 + exp(eta))

**extra reading**
The logistic function is a sigmoid that lies between y:[0,1] and x:[-inf,inf]. This gives it the properties of a Prob. Distr. Function. The center of the sigmoid is determined by the linear model.

In normal regression, the var of the resp var does not depend on the explanatory vars

E(Y) = pi
Var(Y) = pi (1 - pi)
log(pi / (1- pi)) = B0 + B1X1 + ... BpXp 

What does it mean to be a generalized linear model?
* There is a function out there which converts the response to a linear function.
* E(Y | X1, ..., Xp) is not Linear in B's
* The non-linearity is contained in the Link function 
    - once the link function is used, the resulting B's are linear

**QUESTION: What makes the log transformation of the odds special? Is it because it adds an euler number to put it in a similar class to make it similar to normal?**
    - **ANSWER**: The logit function is derived as a linearized transformation of the logistic function. Hence the name Logistic Regression. We are regressing the mean of the log odds

**logistic formula**
w = pi / (1 - pi) = exp(B0 + ... )

w_A = exp(B0 + B1A)
w_B = exp(B0 + B1B)

Odds of A/B
w_A/w_B = exp(B1(A - B))

SAS uses maximum likelihood estimation for param estimates in logistic regression. R uses iteratively reweighted least squares to estimate the logistic regression parameters. These should yield basically the same logistic regression models.

the estimated odds of surviving for a 50 year old woman versus a 20 year old woman:

pho^hat = exp(Coefficient * (50 - 20))

the estimated odds of a woman surviving vs a man of the same age where 1 indicates a Female and 0 indicates a Male:

pho^hat = exp(Coefficient * (1 - 0))

### Maximum Likelihood Estimation

Pr(Y = y) = pi^y (1 - pi)^1-y

Join prob. mass function:

P(Y1 = y1, ...) = multiply(Pr(Yi = yi))

.... <proofs>

To Find Maximum Likelihood estimators of the logistic regression coefficients:
* set each of these p + 1 partial derivatives equal to 0
* The solutions for the parameters to this system of equations, denoted as Bhat0, bhat1, ...., are the maximum likelihood estimators for the logistic regression coefficients
* The solution to this system of equations does not exist in closed form. Iterative computational procedures, like Newton-Raphson, are used.

#### Properties of MLE

If a model is correct and the sample size is large enough:
* MLE are essentially unbiased (larger the sample size, less bias built in)
* Formulas exist for estimating the std devs of the sampling distr. of the estimators
* The estimators are about as precise as any nearly unbiased estimators that can be obtained (smallest variance aka MVUE aka BLUEs)
* Shapes of the sampling distributions are approx. normal

For small samples, it is best to label C.I.s and test results as approx.

When working with Asymptotic Normal Results, these procedures are called Wald Procedures. This assumes large sample sizes to make everything statistically okay

### Drop-In-Deviance Test

In logistic regression, we use a likelihood ratio test (LRT). Analogous to the extra sum of squares F-test in linear regression.

When H0 true (aka reduced model is correct model)

LRT ~ approx. Chi-squared(nu)

where nu = diff(num_param_full, num_param_reduced). With generalized linear models, a quantity called deviance is used.

Deviance = Sum of squared residuals

To test whether all coefficients are 0: Reduced Model => Intercept Only Model

To test significance of a single term:
* reduced model: full model minus the single term
* not the same as Wald's test for a single coefficient
* If the two give different results, the drop-in-deviance test has the more reliable p-value

C.I for a single coefficient can be constructed for a single coefficient form the theory of the drop-in-deviance test
* also know as likelihood ratio C.I. or profile likeilhood C.I.
* this is used in R

#### LRT 

**The LRT for GLM is often called the drop-in-deviance test**

LMAX = Maximum Likelihood Function
deviance = -2 * log(LMAX)

LRT = deviance_reduced - deviance_full
    = 2 * log(LMAX_full) - 2 * log(LMAX_reduced)


**QUESTION: What is the significance of 2 in the deviance?**
    - **ANSWER**: <awaiting email>

### Strategies for Data Analysis Using Logistic Regression

1. Identify questions of interest
2. Find models that fit the data that allow questions to be answered through the inference about parameters

Model terms and adequacy of the logistic model must be checked
* For model terms, informal testing of extra terms such as squared or interaction terms is important
* For model adequacy:
    - Hosmer-Lemeshow GoF test has an approx chi-square distr.
    - Deviance res. plots vs predicted values and each of the predictor vars - we want the loess function to be as flat as possible 
    - More complicated GoF tests out there for Logistic Regression but starting for this now

Can use AIC and BIC for Model selection?

AIC = deviance + log(n) * p
BIC = deviance + 2 * p (2 is not related to the fact that deviance contains -2)

Can try to fit a simple model and observe the residuals to see if there's a pattern that might warrant a transformation on the data

## Matched Case-Control Study

In a 2 X 2 table,

Case-control studies match a single control with each case are an important application having matched-pairs data.

For a binary response Y, each case (Y = 1) is matched with a control (Y = 0) according to certain criteria that could affect the response.

The study observes cases and controls on the predictor variable X and analyzes the XY association. 

Analysis of matched case-control studies is accomplished by using **conditional likelihood logistic regression**.
- Differs from other models studies so far by permitting each subject to have their own prob. distr.

log(pi_i1 / (1 - pi_i1)) = B0i + B1
log(pi_i2 / (1 - pi_i2)) = B0i

Another way to write this is: 

    log(pi_it / (1 - pi_it)) = B0 + B1X_it

where x_i1 = 1, x_i2 = 0.

The {B0i} permit probs. to vary among subjects. This can be extended to K predictors but typically one variable is of special interest; the other are covariates being controlled.

### Probit Regression

Any cumulative distr function F(pi) has characteristic simlar to the logit function
* steadily increasing from -inf to inf as pi goes from 0 to 1

**QUESTION: Is this just a function that produces an increasing sigmoid?**

One popular choice is to choose F(pi) to be the inverse of the cumulative standard normal prob distr. function.

as long as pi is between 0.2 and 0.8, it is similar to logistic regression

### Discriminant Analysis using Logistic Regression

L.R may be used to predict future binary responses.

L.R is preferable to standard discriminant functional analysis solutions when the explanatory vars have non-normal distr. It is nearly as efficient as the standard tools when the explanatory vars have normal distr.

Cannot use this for retrospective sampling because retrospective studies only deal in odds ratios.

# 2019/05/13

* Logistic Regression for Binomial Responses
* Model Assessment
* Inferences Abt Logistic Regression Coefficients

## Logistic Regression for Binomial Responses

Y_i ~ Bin(m_i, pi_i)

Modeling Binomial Counts
* sometimes data listed as counted proportions (Y/ m)
* m must be known!

Different than a continuous proportion
* numer and denom not integers unless due to rounding
* no m involved
* transformed and modeled using ordinary regression methods

## Model Assessment

* Scatterplots: Empirical logits vs explanatory vars
    - log odds vs explanatory vars. Log odds is linear with respect to X. Log odds on Y, explanatory on X. If it looks linear, then all is well! if not, a transformation may be needed
* Examination of Residuals
* Deviance Goodness-of-fit test
    - drop-in-deviance test with intercept-only and your proposed model

Similar in feel to what we typically do with ordinary regression.

### Examination of Residuals

**Deviance Residual**: sum of all n squared deviance residuals is the deviance statistic
* Measures a kind of discrepency in the likelihood function to the fit of the model at each observation

<formula>

**Pearson Residual**: observed binomial response variable minus estimated mean, divided by estimated std dev. (Like a Z score)
* constructed to have roughly mean 0 and var 1

<formula>

If you have at least 5 trials in any binomial responses, then any residual greater than 2 in magnitude may be a possible outlier.
- No discernable pattern indicates that the error terms are normal

### Deviance Goodness-of-Fit Test

Analogous to an extra sum of squares test for comparing the model of interest to a general model with a separate population proportion for each obs.

Intercept significance is only significant if a value of 0 is significant. However, with odds ratios it tends to wash out so it doesn't mean much.

### Wald vs Drop-in-deviance Test?

If Hypothesis inolves several coefficients
* Can only use drop-in-deviance test

If involves a single coefficient
* either test can be used
* p-value from d-in-d is generally more accurate than Wald Test
* Wald's test is easier to obtain is usually satisfactory for conducting tests of secondary importance (i.e. informal testing of extra terms when model building) and when the p-value is clearly large or clearly very small.

## Extra Binomial Variation (Over dispersion)

Y_i ~ i.i.d Bin(m_i, pi_i)

If binomial trials are not independent or important explanatory vars are not included in the model for pi_i?
* response counts will no longer have binomial distributions!

Typically the variance of the Y_is is greater than expected from a binomial distribution

What happens when there is overdispersion in the binomial logistic regression model?
* Regression parameter estimates will not be seriously biased
* But their standard errors will tend to be smaller than they should be
* p-value will tend to be too small
* C.I. will tend to be too narrow

Inference Tools
* **quasi-likelihood approach** is the easiest to use and works in most situations
    - assumes relationship between mean and var(Y) rather than a specific prob. distr. for Y
    - take usual variance formula but multiples it by a constant that is itself estimated using the data
    - quasi-likelihood estimates are not MLE because the method does not specify a distr. for Y so there is not a likelihood function.

psi > 1 == overdispersion

### Checking for Extra-Binomial Variation

**Consider whether overdispersion is likely in the particular responses**
* are the binary responses included in each count unlikely to be independent?
* are obs with identical values of the explanatory vars likely to have different pi_i's?
* Is the model for pi somewhat naive?

A "yes" to any of these questions should caution you in using the binomial model!

**Examnining the deviance GoF test after fitting the model**
* large deviance GoF stat indicates specified explanatory vars not ...

How to estimate psi?

psi^hat = sum(Pres_i^2) / (n - p)

When using max. quasi-likelihood estimates, tests and confidence interval for a single coefficient change. Use t instead of z because psi is used in the standard error.

Drop-in-deviance F-Test when Overdispersion is present

F = (DinD / D )/ psi^hat

where D represent the num of params in the full model 

quasi-binomial does not affect the regression coefficients. It affects the standard errors.

# Logistic Models for Multilevel Categorical Responses

**Multinomial Logits**

Let J be the number of categories for Y. Let [pi_1, pi_J] be the props for each category.

**Ordinal Categorical Responses**

When response categories are ordered, the logits can utilize the ordering. A cumulative probability for Y is the probability that Y falls at or below a particular point. ...


# 2019/05/20

* Log Linear Regression Models for Poisson Responses
* Model Assessment
* Inferences about Log-Linear Regression Coefficients
* Extra-Poisson varation and the log-linear model
* Issues with research design & Sample size calculations

## Log-Linear Models (Poisson GLM)

For Y, the number of success in a given time or space interval. The poisson distr. is most appropriate for counts of rare events that occur at completely random points in space or time.
* Also works reasonably well for "Count" data where spread increases with the mean (a.k.a. number of occurrences).
* Will not have constant variance here since mean is tied to variance

### Characteristics of Poisson
* Var(Y) = mu(Y) = mu
* Distr. tends to be right-skewed and is most pronounced when the mean is small
* larger means tend to be well-approx. by a normal distr.

### Poisson Log-Linear Model

If Y is Poisson, mu{Y|X_1,...,X_p} = mu
then, log(mu) = B0 + sum(B_i * X_i)

In generalized Linear Model Terms:
* Family - Poisson
* Link - log
Log link helps straighten the relationship between the predictors and the response; however, variance will still be non-constant after the transformation.

### Interpretation

Multiplicative effect on mean. Can also convert to an estimated percent increase.
exp(B_1) = <int>

This is different than linear regression where exp(B_1) gives the odds ratio.

**First partial derivative of a function is sometimes referred to as a score function**

### Model Assessment
* Scatterplots
* Residuals
    - Deviance Residual for Poisson Regression (more reliable for detecting outliers)
    - Pearson Residual for Poisson Regression

If Poisson means are at least 5 (large), the distr. of both of these resids are approx std normal.
* if > 5% of residuals exceed 2 in magnitude or if one or two res greatly exceed 2, ther are problems in the fit
If Poisson means < 5, (small)
* Neihter set of res follows a normal distr very well
* So comparison to std normal provides poor lack of fit

**Deviance goodness of fit test** is an informal assessment of the adequacy of a fitted model
* not particularly good at detecting model inadequacies
    - use in conjunction with plots and tests of model terms for assessing the adequacy of a particular fit
* A large p-value indicates
    - either the model is inadequate OR
    - that insufficient data are avilable to detct inadequacies
* Small p-value indicates
    - the model for the mean is incorrect OR
    - Poisson is an inadequate model for the response OR
    - a few severely outlying obs contaminate the data.

### Extra-Poisson Variation

Overdispersion in log-linear models?
* Unmeasured effects
* Clustering of events
* Other contaminating influences

Result? More var in responses than predicted by a Poisson Distr.

How to check for extra-poisson variation?
* Think about whether extra-Poisson variation (overdispersion) is likely
* Fit a negative binomial model and check to see if psi > 1...
* compare the sample variance to sample averages for groups of responses with identical explanatory variable values
* Examine the deviance goodness-of-fit test after fitting a rich models
* Examine res to see if a large deviance statistic may be due to outliers

Extra-poisson variation should be expected when:
* important explanatory vars are not available
* individuals with the same level of expl vars may for some reason behave differently
* events making up the count are cluster or spaced systematically through timeo r space instead of randomly spaced

**Plot Variances against Average**
* if poisson, samples vars and sample means should be equal

**Fit a Rich Model**
* If poisson means are large, deviance GoF test can be examined
* Large deviance statistic will indicated inadequacies with the Poisson assumption rather than inadequacies with insufficient explanatory var terms.

**Look for outliers**
* Deviance Statistic might be large due to a few outliers
* Individual deviance should not be judged an outlier simply due to magnitude > 2
* Look to see if res appear "natural" in the context of the distr of other deviance res (thats a good thing)

Exact same idea for inferences as extra-binomial variation in a logistic regression model.

## Negative Binomial Regression
Alternative to quasi-likelihood estimation in Poisson regression when overdispersion is present is negative binomial regression.

Uses an additional parameter, phi, to model the count variation

mu{Yi | Xi1,...,Xip} = mu_i
Var(yi|Xi1,...,Xip) = mu_i(1 + phi * mu_i)

Strategies are the same with Poisson regression
* except no need to investigate extra-Poisson Varation
* Test and C.I. based on Wald and DinD/likelihood ratio theory with same issues as poisson.