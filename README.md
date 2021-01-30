# stokes-collection

This is a collection of symmetric and quasi-definite linear systems in MAT format.

<p align="center">
  [<b><i>M </i></b>&nbsp;&nbsp;&nbsp;<b><i> A</i></b>]&nbsp; [<b><i>x</i></b>]            =           [<b><i>b</i></b>]
  <br>
  [<b><i>Aᵀ</i></b>&nbsp;&nbsp;      <b><i>-N</i></b>]&nbsp; [<b><i>y</i></b>]&nbsp;&nbsp;&nbsp;&nbsp;[<b><i>c</i></b>]
</p>

```julia
using LinearAlgebra, SparseArrays, MAT

grids = Dict("Q1-P0" => false,
             "Q1-Q1" => false)

dict = Dict("channel_domain" => false,
            "flow_over_a_backward_facing_step" => false,
            "lid_driven_cavity" => false,
            "colliding_flow" => false)

for grid in keys(grids)
  if grids[grid]
    for pb in keys(dict)
      if dict[pb]
        print("Reading data... ")
        file = matopen("./$pb/$grid/A.mat")
        A = read(file, "A")
        close(file)
        file = matopen("./$pb/$grid/b.mat")
        b = read(file, "b")[:]
        close(file)
        file = matopen("./$pb/$grid/c.mat")
        c = read(file, "c")[:]
        close(file)
        file = matopen("./$pb/$grid/M.mat")
        M = read(file, "M")
        close(file)
        file = matopen("./$pb/$grid/N.mat")
        N = read(file, "N")
        N = N + 1e-5 * I
        close(file)
        println("✔")

        K = [M A; A' -N]
        D = [b; c]
      end
    end
  end
end
```
