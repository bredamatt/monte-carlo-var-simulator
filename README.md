# monte-carlo-var-simulator

This is a C++ implementation for a Monte-Carlo Value at Risk simulation engine used to simulate risk factors on a portfolio of assets.


## C++ Notes

### Const functions

A "const function", denoted with the keyword const after a function declaration, makes it a compiler error for this class function to change a member variable of the class. 

### Signed versus unsigned numbers

Signed numbers use sign flag or can be distinguish between negative values and positive values. Unsigned numbers store only positive numbers.

### Using `enum` to perform different calculations 

Returns can be defined in mutliple ways. Add return types in `ReturnType.h`, and implement these with a `case` block in `TSHandler.cpp`.

## Statistics notes

### Covariance 

Covariance measures the joint variability of two random variables. In essence, it measures the degree to which two random variables deviate from their expected values in similar ways.

For example, the (average) covariance between variables x and y over some time period T, can be calculated as:

<!-- $$
cov_{x,y} = \frac{\sum_{t=1}^{T}(x_{t} - \bar{x})(y_{t} - \bar{y})}{T - 1} 
$$ --> 

<div align="center"><img src="svg/dY66FRPqp8.svg"/></div>

where the means of the variable x is define as:

<!-- $$
\bar{x} = \frac{\sum_{t=1}^{T}{x_{t}}}{T-1}
$$ --> 

<div align="center"><img src="svg/c4s7H0HgH5.svg"/></div>


### Covariance matrix 

A covariance matrix represents the covariance between multiple variables i and j, in which the (i,j) element in the matrix is the covariance between the variables i and j.

Intuitively, the covariance matrix generalizes the notion of variance to multiple dimensions. As an example, the variation in a collection of random points in two-dimensional space cannot be characterized fully by a single number, nor would the variances in the i and j directions contain all of the necessary information.

For a random vector X, we denote the covariance matrix of X as:

<!-- $$
\pmb{K_{\pmb{XX}}}
$$ --> 

<div align="center"><img src="svg/1NYReOJb0e.svg"/></div>


More generally, if the entries in the column vector:

<!-- $$
\pmb{X} = \{ X_{1}, X_{2}, ..., X_{n} \}^{T}
$$ --> 

<div align="center"><img src="svg/Bl1s3DT9Am.svg"/></div>

are random variables, each with finite variance and expected value / mean, the covariance matrix of this random vector is the matrix whose (i, j) entry is the covariance between random variables Xi and Xj, defined as follows:

<!-- $$
\pmb{K}_{X_{i}X_{j}} = cov[X_{i}, X_{j}] = E[(X_i) - E[X_{i}])(X_{j} - E[X_{j}])]
$$ --> 

<div align="center"><img src="svg/H4dhxGePZv.svg"/></div>


### Cholesky Decomposition

To simulate correlated variables from a given set of non-correlated variables with the condition that the simulated variables should be correlated according to a given covariance matrix created from historical observations, we use Cholesky decomposition. 

In other words, given a covariance matrix created from historical observations, we can simulate correlated variables with a set of non-correlated variables.

More generally, we are looking to decompose the covariance matrix using a lower triangle matrix L, such that the covariance matrix may be defined as:

<!-- $$
\pmb{K}_{\pmb{XX}} = \pmb{L} * \pmb{L}^{T}
$$ --> 

<div align="center"><img src="svg/QENDBL5Yh6.svg"/></div>

The formula to compute the lower triangle matrix based on the entries in the covariance matrix is defined as: 

<!-- $$ 
\pmb{L} = \begin{pmatrix} 
\sqrt{\pmb{K}_{11}} & 0 & 0 \\
\pmb{K}_{21} / \pmb{L}_{11} & \sqrt{\pmb{K}_{22} - \pmb{L}^{2}_{21}} & 0 \\
\pmb{K}_{31} / \pmb{L}_{11} & (\pmb{K}_{32} - \pmb{L}_{31}\pmb{L}_{21}) / \pmb{L}_{22} & \sqrt{\pmb{K}_{33} - \pmb{L}_{31}^{2} - \pmb{L}_{32}^{2}}
\end{pmatrix}
$$ --> 

<div align="center"><img src="svg/nGEGfEWddX.svg"/></div>

See `CholeskyDecomposition.cpp` and `CholeskyDecomposition.h` for the implementation.

Some useful formulas to bear in mind wrt. entries in the lower triangular matrix are defined below.

### Diagonal entries

<!-- $$
\pmb{L}_{jj} = \sqrt{\pmb{K}_{jj} - \sum_{j=i}^{k-1}\pmb{L}_{jk}^{2}}
$$ --> 

<div align="center"><img src="svg/MCtDnbma2k.svg"/></div>

Intuitively, this means take the square root of the (j,j) entry in the covariance matrix and subtract the square of the sum of entries to the left of the diagonal in the lower triangle matrix. 

### Non-diagonal entries

<!-- $$
\pmb{L}_{i,j} = \frac{\pmb{K}_{i,j} - \sum_{k=1}^{j-1} \pmb{L}_{j,k} \pmb{L}_{i,k}}{\pmb{L}_{jj}}
$$ --> 

<div align="center"><img src="svg/F47HhzBo2o.svg"/></div>


Intuitively, this means take the entry of the covariance matrix and subtract the sum of any entries to the left in the lower triangle matrix (there are none, as we are moving downwards from top left corner to bottom left corner) and divide the the top left entry in the lower triangle matrix. In other words, it simplifies to:

<!-- $$
\pmb{L}_{i,j} = \frac{\pmb{K}_{i,j}} {\pmb{L}_{1,1}}
$$ --> 

<div align="center"><img src="svg/36Tv7OCNAX.svg"/></div>

## Day 1

1. Implement TimeSeriesHandler and ReturnType enumerator class.

The TimeSeriesHandler should:

- generate a covariance matrix 
- generate spot rates 
- transform to returns,
- compute covariance for two risk-factors

2. Implement Cholesky decomposition from covariance matrix 

- add formulas for easier comprehension of code

## Day 2 

## Day 3

## Day 4 