# SQUIC Python3 Interface Package

SQUIC is a second-order, L1-regularized maximum likelihood method for performant large-scale sparse precision matrix estimation. This repository contains the source code for the Python(v3) interface of SQUIC. 

For an interactive session using SQUIC see Google Colab here
[![Generic badge](https://img.shields.io/badge/jupyter%20nbviewer-DDSG-green)](https://colab.research.google.com/drive/1iQB5hz07UMd5C1PR3w3xM3306BVcFGiO?usp=sharing)

## Installation

#### Step 1:
Download the shared library ``libSQUIC`` from www.gitlab.ci.inf.usi.ch/SQUIC/libSQUIC, and follow its README instructions. The default and recommended location for ``libSQUIC`` is the home directory, i.e., ``~/``. Note that precompiled versions are available.

#### Step 2:
Run the following command to install the library:
```angular2
pip install squic
```
_Note: The environment variable SQUIC_LIB_PATH defines the location of libSQUIC - this is by default set to the home directory of the user. If this is not the location of libSQUIC, it can be changed via terminal ``bash> export SQUIC_LIB_PATH=/path/to/squic/``_

## Example
In this example, we will use SQUIC to estimate the precision matrix of a synthetically generated dataset with correlated random variables, where the true precision matrix is tridiagonal.

```angular2
import squic
import numpy as np

# generate sample from tridiagonal precision matrix
p = 1024
n = 100
l = .4

# generate a tridiagonal matrix
a = -0.5 * np.ones(p-1)
b = 1.25 * np.ones(p)
iC_star = np.diag(a,-1) + np.diag(b,0) + np.diag(a,1)

# generate the data
L = np.linalg.cholesky(iC_star)
Y = np.linalg.solve(L.T,np.random.randn(p,n))

[X,W,info_times,info_objective,info_logdetX,info_trSX] = squic.run(Y,l)
```
