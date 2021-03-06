# Lyapunov Exponents
Lyapunov exponents measure rates of separation of nearby trajectories in the flow
of a dynamical system. The [Wikipedia](https://en.wikipedia.org/wiki/Lyapunov_exponent) and the [Scholarpedia](http://www.scholarpedia.org/article/Lyapunov_exponent) entries have a lot of valuable information about the history and usage of these quantities.

This page treats systems where the equations of motion are known. If instead
you have numerical data, see the [nonlinear timeseries analysis page](nlts).

## Lyapunov Spectrum
The function `lyapunovs` calculates the entire spectrum of the Lyapunov
exponents of a system:
```@docs
lyapunovs
```
---
As you can see, the documentation string is detailed and self-contained. For example,
the lyapunov spectrum of the [folded towel map](http://www.scholarpedia.org/article/Hyperchaos)
is calculated as:
```@example lyap
using DynamicalSystems

ds = Systems.towel()
λλ = lyapunovs(ds, 10000)
```
Similarly, for a continuous system, e.g. the Lorenz system, you would do:
```@example lyap
lor = Systems.lorenz(ρ = 32.0) #this is not the original parameter!
λλ = lyapunovs(lor, 10000,
dt = 0.1, diff_eq_kwargs = Dict(:abstol => 1e-9, :reltol => 1e-9))
```

## Maximum Lyapunov Exponent
The function `lyapunov` calculates the maximum lyapunov exponent of a system, much
more efficiently than getting the first result of `lyapunovs`:
```@docs
lyapunov
```
---
For example:
```@example lyap
using DynamicalSystems

henon = Systems.henon()
λ = lyapunov(henon, 10000, d0 = 1e-7, threshold = 1e-4, Ttr = 100)
```
The same is done for continuous systems:
```@example lyap
using DynamicalSystems, OrdinaryDiffEq

ross = Systems.roessler(a = 0.1, b = 0.1, c = 14.0) #not original parameters
λ = lyapunov(ross, 100000, dt = 10.0,
diff_eq_kwargs = Dict(:solver => Vern8(), :abstol=>1e-9, :reltol=>1e-9),
Ttr = 100.0)
```
