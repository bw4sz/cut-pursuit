A working set strategy for minizing, over a graph G = (V, E),
functionals of the form, for all `x in R^V`,

       F(x) = f(x) +  g(x) + sum_{uv in E} la_d1_uv |x_u — x_v|,

where `f` is convex and differentiable and `g` is convex and separable 
according to V, that is `g(x) = sum_{v in V}`, and `la_d1 in R^E` are 
penalization weights.

Several C++ implementations for specific functionals f and g are
available, summed up below.

See the full documentations in */mex/include.

Interface with MATLAB available, see documentations in */mex/doc.

### Graph_quadratic_d1_l1
#### General form (quadratic functional penalized by fused LASSO):

      F(x) = 1/2 ||y — A x||^2 + ||x||_{d1,La_d1}  + ||x||_{l1,La_l1} 

where `y in R^N`, `x in R^V`, `A` is an arbitrary real matrix in `R^{N-by-|V|}`

      ||x||_{d1,La_d1} = sum_{uv in E} La_d1_uv |x_u — x_v|,
      ||x||_{l1,La_l1} = sum_{v  in V} La_l1_v |x_v|,

with the possibility of adding a positivity constraint on the coordinates of `x`,
      `F(x) + i_{x >= 0}`.

- CP_PFDR_graph_quadratic_d1_l1_mex implements the general case, when `A` is a
dense matrix
- CP_PFDR_graph_quadratic_d1_l1_AtA_mex implements the general case, but `A`
is only provided as `A^t A`, useful for instance when `N > |V|` or when
many iterations are expected and memory is not a concern
- CP_PFDR_graph_l22_d1_l1_mex implements the particular case

       F(x) = 1/2 ||y — x||_{l2,La_l2}^2 + ||x||_{d1,La_d1}  + ||x||_{l1,La_l1}

where `x, y in R^V`, and

       ||y — x||_{l2,La_l2}^2 = sum_{v in V} la_l2_v (y_v — x_v)^2.

###  Graph_quadratic_d1_bounds
#### General form (quadratic functional penalized by TV and box constraints):

        F(x) = 1/2 ||y — A x||^2 + ||x||_{d1,La_d1}  + i_{[m, M]}(x)

where `y in R^N, x in R^V, A in R^{N-by-|V|}`, and

        ||x||_{d1,La_d1} = sum_{uv in E} la_d1_uv |x_u — x_v|,
        i_{[m, M]}(x) = 0          if for all v in V, m <= x_v <= M,
                        +infinity  otherwise

- CP_PFDR_graph_quadratic_d1_bounds_mex implements the general case,
when `A` is a dense matrix
- CP_PFDR_graph_quadratic_d1_bounds_AtA_mex implements the general case,
but `A` is only provided as `A^t A`, useful for instance when `N > |V|` or
when many iterations are expected and memory is not a concern
- CP_PFDR_graph_l22_d1_bounds_mex implements the particular case

        F(x) = 1/2 ||y — x||_{l2,La_l2}^2 + ||x||_{d1,La_d1}  + i_{[m, M]}(x)

where `x, y in R^V`, and

        ||y — x||_{l2,La_l2}^2 = sum_{v in V} la_l2_v (y_v — x_v)^2