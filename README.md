# Minimal cycles for curves
Given points on a curve, computes the dimension-1 persistent homology and the minimal generators for each persistent homology class. 

## Install
1. Download [Julia](https://julialang.org/downloads/).
2. Download [Gurobi](https://www.gurobi.com/academia/academic-program-and-licenses/) and obtain a free academic license.

## Quick start

```
import Pkg
Pkg.activate(".")
ENV["GUROBI_HOME"] = PATH_TO_GUROBI
include("src/compute_PH_minimal.jl")
PH_minimal.compute_PH_minimal_generators(INPUT_PATH, OUTPUT_PATH)
```
* `PATH_TO_GUROBI`: For Mac, this is likely "/Library/gurobi912/mac64". This allows Julia to find Gurobi. 
* `INPUT_PATH` must have extensions `tsv` or `npy`. Input must be an array of the x, y, z-coordinates of the points. Array must have size (n, 3) or (3,n), where `n` is the number of points
* `OUTPUT_PATH` must have extension `json`.


## Algorithm 
Given points in three dimensions, we first compute its persistent homology in dimension 1. For each point in the persistence diagram, we compute its generator via the following:

1. Find length-minimal generators with rational coefficients using [Li et al, 2021](https://github.com/TDAMinimalGeneratorResearch/minimal-generator) 
	* In most cases, the generators will have coefficients in $\\{ 0, 1, -1 \\}$. We consider such generators as having coefficients in $\mathbb{F}_2$. Proceed to step 2.
	* If a generator has coefficients other than $\\{0, 1, -1\\}$, we recompute the length-minimizing generators while requiring integer coefficients. Most generators will have coefficients in $\\{ 0, 1, -1 \\}$. We then consider such generators as having coefficients in $\mathbb{F}_2$. Proceed to step 2.
	* If the generator still has coefficients other than $\\{0, 1, -1\\}$, then we consider the support of the generator (the collection of 1-simplices that have a non-zero coefficient) as the length-minimal generator. Proceed to step 2.
		* To disable such handling of coefficients other than $\\{0, 1, -1\\}$, provide the argument `allow_nonbinary_coefficients = false` when calling the function `PH_minimal.compute_PH_minimal_generators()`. If the length-minimal generators have coefficients other than $\\{0, 1, -1\\}$, the function will print an error message and not proceed to step 2.
2. Perform [jump-minimization](https://dl.acm.org/doi/pdf/10.4108/eai.3-12-2015.2262453) to find homologous generators with minimal amount of jumps between consecutive points. 

For details of the algorithm, we refer the user to <font color="red">ADD LINK TO hyperTDA paper.</font>


## Examples
For visualizations of the minimal generators, please see the notebook `example/example.ipynb`.
