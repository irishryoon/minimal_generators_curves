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

1. Find edge-length minimizing generators with rational coefficients. 
	* This step is performed using [Li et al, 2021](https://github.com/TDAMinimalGeneratorResearch/minimal-generator) 
	* In most cases, the generators will have coefficients in $\{ 0, 1, -1 \}$. We consider such generators as having coefficients in $\mathbb{Z}/(2)$. 
2. Perform jump-minimization to find homologous generators with minimal amount of jumps between consecutive points. 

For details of the algorithm, we refer the user to <font color="red">ADD LINK TO hyperTDA paper.</font>


## Examples
For visualizations of the minimal generators, please see the notebook `example/example.ipynb`.
