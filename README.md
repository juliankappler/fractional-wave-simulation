# Fractional-wave-equation: Python module for the numerical solution of nonlinear fractional wave equations

## About

This repository contains python code to numerically calculate the solution <i>U(x,t)</i> of a **nonlinear fractional wave equation**

 <p align="center">
<img src="https://latex.codecogs.com/svg.latex?\small&space;\frac{\partial^\alpha&space;U}{\partial&space;t^{\alpha}}&space;=&space;D(\partial_x&space;U)&space;\frac{&space;\partial^2&space;U}{\partial&space;x^2}&space;\qquad&space;(1)," title="\small \frac{\partial^\alpha U}{\partial t^{\alpha}} = D(\partial_x U) \frac{ \partial^2 U}{\partial x^2} \qquad (1)," />
 </p>

on a domain <i>(x,t) &#8712; [0,L] &#215; [0,T]</i>, and with boundary conditions

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\begin{matrix}&space;U(x,t=0)&space;&=&&space;(\partial_t&space;U)(x,t=0)&space;&=&&space;0&space;&&space;\forall&space;x&space;\in&space;(0,L)&space;\\&space;U(x=0,t)&space;&=&&space;U_0(t)&space;&&space;&&space;&\forall&space;t&space;\in&space;(0,T)&space;\\&space;U(x=L,t)&space;&=&&space;U_L(t)&space;&&space;&&&space;\forall&space;t&space;\in&space;(0,T)&space;\end{matrix}" title="\begin{matrix} U(x,t=0) &=& (\partial_t U)(x,t=0) &=& 0 & \forall x \in (0,L) \\ U(x=0,t) &=& U_0(t) & & &\forall t \in (0,T) \\ U(x=L,t) &=& U_L(t) & && \forall t \in (0,T) \end{matrix}" />
</p>
where <i>U<sub>0</sub>(t)</i> and <i>U<sub>L</sub>(t)</i> are given functions that model time-dependent boundaries.

The exponent <i>&alpha; &#8712; (1,2)</i> defines the fractional derivative in Eq. (1), and the function <i>D</i>, which depends on  <i>&#8706;<sub>x</sub>U</i> pointwise, constitutes the nonlinearity.

This python code was written by Julian Kappler to carry out the numerical simulations for the publication <a href="#ref_1">Ref. [1]</a>. The numerical algorithm is given in detail in the supplemental section S VII of <a href="#ref_1">Ref. [1]</a>, and the present code and example notebooks follow the notation of the reference. For the temporal discretization of the fractional derivative, the code uses the method from <a href="#ref_2">Ref. [2]</a>.

## Installation

To install the module fractional_wave_equation, clone the repository and run the installation script:

```bash
>> git clone https://github.com/juliankappler/fractional-wave-equation.git
>> cd fractional-wave-equation
>> python setup.py install
```

## Example

To run a simulation, the domain <i>(x,t) &#8712; [0,L] &#215; [0,T]</i>, the functions <i>D</i>, <i>U<subs>0</subs></i>, <i>U<subs>L</subs></i>, and the order <i>&alpha; &#8712; (1,2)</i> of the fractional derivative need to specified. For the numerical algorithm, furthermore the spatial and temporal discretization need to be specified.

Here is a simple example (a jupyter notebook with this example can be found [here](examples/Nonlinear%20fractional%20wave%20equation.ipynb)):

```Python

import fractional_wave_equation as fwe

L = 10 # spatial domain
Nx = 299 # number of gridpoints inside the spatial domain
T = 10. # temporal domain
Nt = 1000 # number of timesteps
alpha = 1.5 # order of the fractional derivative

# create dictionary for passing to the python module
parameters = {'L':L,
             'Nx':Nx,
             'T':T,
             'Nt':Nt,
             'alpha':alpha}

# Define nonlinearity and boundary functions
D = lambda dU_dx: 1 + 2*dU_dx**2 # nonlinearity
U0 = lambda t: 1.*np.exp(-2*(t-2)**2) # boundary at x = 0
UL = lambda t: 0. # boundary at x = L

# instantiate class for nonlinear simulation
nonlinear_equation = fwe.numerical.nonlinear(parameters)

# run simulation
results = nonlinear_equation.simulate(D=D,U0=U0,UL=UL)

# the simulation returns a dictionary, which contains 1D numpy arrays with the
# temporal and spatial grid, as well as a 2D numpy array with the solution U(x,t)

x, t, y = results['x'], results['t'], results['y']
# y[i,j] = U(x[j],t[i])

```

Here is a 2D plot of the solution <i>U(x,t)</i> generated by the above code (as given in [this jupyter notebook](examples/Nonlinear%20fractional%20wave%20equation.ipynb)):

![Imagel](https://raw.githubusercontent.com/juliankappler/fractional-wave-equation/master/examples/nonlinear_solution_2D_plot.jpg)


## References

<a id="ref_1">[1] **Nonlinear fractional waves at elastic interfaces**. Julian Kappler, Shamit Shrivastava, Matthias F. Schneider, and Roland R. Netz. Phys. Rev. Fluids 2, 114804 (2017). DOI: [10.1103/PhysRevFluids.2.114804](https://doi.org/10.1103/PhysRevFluids.2.114804) / [arXiv:1702.08864](https://arxiv.org/abs/1702.08864).</a>

<a id="ref_2">[2] **Numerical approximation of nonlinear fractional differential equations with subdiffusion and superdiffusion**. Changpin Li, Zhengang Zhao, YangQuan Chen. Chen, Comput. Math. Appl. 62, 855 (2011). DOI: [10.1016/j.camwa.2011.02.045](https://doi.org/10.1016/j.camwa.2011.02.045).</a>
