# ExponentialUtilities.jl: High-Performance Matrix Exponentiation and Products

ExponentialUtilities is a package of utility functions for matrix functions of exponential type, 
including functionality for the matrix exponential and phi-functions. The tools are used by the 
exponential integrators in OrdinaryDiffEq.

## Installation

To install ExponentialUtilities.jl, use the Julia package manager:

```julia
using Pkg
Pkg.add("ExponentialUtilities")
```

## Example

```julia
using ExponentialUtilities

A = rand(2,2)
exponential!(A)

v = rand(2); t = rand()
expv(t,A,v)
```

## Matrix-phi-vector product

The main functionality of ExponentialUtilities is the computation of matrix-phi-vector products. The phi functions are defined as

```
ϕ_0(z) = exp(z)
ϕ_(k+1)(z) = (ϕ_k(z) - 1) / z
```

In exponential algorithms, products in the form of `ϕ_m(tA)b` is frequently encountered. Instead of computing the matrix function first and then computing the matrix-vector product, the common alternative is to construct a [Krylov subspace](https://en.wikipedia.org/wiki/Krylov_subspace) `K_m(A,b)` and then approximate the matrix-phi-vector product.

### `expv` and `phiv`

```julia
expv(t,A,b;kwargs) -> exp(tA)b
phiv(t,A,b,k;kwargs) -> [ϕ_0(tA)b ϕ_1(tA)b ... ϕ_k(tA)b][, errest]
```

For `phiv`, *all* `ϕ_m(tA)b` products up to order `k` is returned as a matrix. This is because it's more economical to produce all the results at once than individually. A second output is returned if `errest=true` in `kwargs`. The error estimate is given for the second-to-last product, using the last product as an estimator. If `correct=true`, then `ϕ_0` through `ϕ_(k-1)` are updated using the last Arnoldi vector. The correction algorithm is described in [1].

You can adjust how the Krylov subspace is constructed by setting various keyword arguments. See the Arnoldi iteration section for more details.

## Contributing

- Please refer to the
  [SciML ColPrac: Contributor's Guide on Collaborative Practices for Community Packages](https://github.com/SciML/ColPrac/blob/master/README.md)
  for guidance on PRs, issues, and other matters relating to contributing to ModelingToolkit.
- There are a few community forums:
    - the #diffeq-bridged channel in the [Julia Slack](https://julialang.org/slack/)
    - [JuliaDiffEq](https://gitter.im/JuliaDiffEq/Lobby) on Gitter
    - on the [Julia Discourse forums](https://discourse.julialang.org)
    - see also [SciML Community page](https://sciml.ai/community/)