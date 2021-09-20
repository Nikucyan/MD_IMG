# CFD – Advection and Diffusion

[CFD之扩散和对流](https://www.bilibili.com/video/BV13E411B7bK)

---

**General Diffusion Advection Equation**
$$
\frac{\partial c}{\partial t} + u\nabla c = D\laplacian c
$$


## Numerical Differentiation Basics

### Finite Differences (FD)

$$
\begin{aligned}
u_{i+1} &= u_i + hu'_i + \frac{h^2}{2!} u''_i +\frac{h^3}{3!} u_i''' +O(h^4)\\
u_{i-1} &= u_i - hu'_i + \frac{h^2}{2!} u''_i -\frac{h^3}{3!} u_i''' +O(h^4)
\end{aligned}
$$

#### Basic Differences

![plot1_num](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/plot1_num.png)

- Forward Difference (FD):	
  $$
  u_i' = \frac{u_{i+1}-u_i}{h} + O(h)
  $$

- Backward Difference (BD):	
  $$
  u_i' = \frac{u_i - u_{i-1}}{h} + O(h)
  $$

- Central Difference (CD): (Higher accuracy)

  - 1st Order:	​
    $$
    u'_i = \frac{u_{i+1} - u_{i-1}}{2h} + O(h^2)
    $$

  - 2nd Order (Usually CD):    
    $$
    u''_i = \frac{u_{i+1} - 2u_i + u_{i-1}}{h^2} + O(h^2)
    $$

#### Taylor Table

Use certain stencil points to get a certain order of Approximation

3 points -> highest accuracy: 2nd order; 4 pt -> 3rd order

-> Example: $u_{i-2}$, $u_{i-1}$, $u_i$ and $u_{i+1}$ to approximate $u_i'$​ of the highest order. (At most 4 equations)
$$
\begin{aligned}
& a_{-2}u_{i-2}+a_{-1}u_{i-1}+a_0u_i+a_1u_{i+1}\\
= &a_{-2}\left(u_i-2\Delta u_i'+\frac{\left(-2\Delta\right)^2}{2!}u_i^{\prime\prime}-\frac{\left(-2\Delta\right)^3}{3!}u_i^{\prime\prime\prime}+\frac{\left(-2\Delta\right)^4}{4!}u_i^{\left(4\right)}+O\left(\Delta^5\right)\right)+ \\
& a_{-1}\left(u_i-\Delta u_i^\prime+\frac{\left(-\Delta\right)^2}{2!}u_i''-\frac{\left(-\Delta\right)^3}{3!}u_i^{\prime\prime\prime}+\frac{\left(-\Delta\right)^4}{4!}u_i^{\left(4\right)}+O\left(\Delta^5\right)\right)+ \\
& a_0u_i+
a_1\left(u_i+\Delta u_i^\prime+\frac{\left(\Delta\right)^2}{2!}u_i^{\prime\prime}+\frac{\left(\Delta\right)^3}{3!}u_i^{\prime\prime\prime}+\frac{\left(\Delta\right)^4}{4!}u_i^{\left(4\right)}+O\left(\Delta^5\right)\right)\\
= & \left(a_{-2}+a_{-1}+a_0+a_1\right)u_i+\Delta\left(-2a_{-2}-a_{-1}+a_1\right)u_i'+
\frac{\Delta^2}{2!}\left(\left(-2\right)^2a_{-2}+\left(-1\right)^2a_{-1}+1^2a_1\right)u_i''+\\
&\frac{\Delta^3}{3!}\left(\left(-2\right)^3a_{-2}+\left(-1\right)^3a_{-1}+1^3a_1\right)u_i'''+O\left(\Delta^4\right)

\end{aligned}
$$
The section of $u_i'$​ has a result of 1 and other sections equal to 0, which is
$$
\left\{\begin{matrix}a_{-2}+a_{-1}+a_0+a_1=0\\-2a_{-2}-a_{-1}+a_1=1\\\left(-2\right)^2a_{-2}+\left(-1\right)^2a_{-1}+1^2a_1=0\\\left(-2\right)^3a_{-2}+\left(-1\right)^3a_{-1}+1^3a_1=0\\\end{matrix}\right.
$$
The third order approx. result: (for lower accuracy just reduce the amount of equations)
$$
u_i^\prime+O\left(\Delta^3\right)=\frac{\frac{1}{6}u_{i-2}-u_{i-1}+\frac{1}{2}u_i+\frac{1}{3}u_{i+1}}{\Delta}=\frac{u_{i-2}-6u_{i-1}+3u_i+2u_{i+1}}{6\Delta}
$$
Leading Error:
$$
\frac{\Delta^3}{4!}\left(\left(-2\right)^4\frac{1}{6}+\left(-1\right)^4\left(-1\right)+1^4\frac{1}{3}\right)u_i^{\left(4\right)}=\frac{\Delta^3}{12}u_i^{\left(4\right)}
$$

#### Basic Concepts

**Consistency**: numerical scheme vs differential equation: **Truncation error**

\+ **Stability**: properties of the numerical scheme with determined parameters (spatial and time steps)

= **Convergence**: numerical solution vs the exact solution of the differential equation

### von Neumann Stability Analysis

Applying Fourier expansion for the pt. with index $j$ @ timestep $n$, $i$ is the imaginary part.
$$
c_j^n=\xi_j^ne^{ik_j x_j}
$$
Want: $|\xi|\leq 1$​​ (in complex domain should be $\xi$​ in the unit circle ($\sqrt{\text{Im}^2 + \text{Re} ^2} \leq 1$​))

### Boundary Conditions

- **Dirichlet Condition**: Given the values at the boundary;
- **Neumann Condition**: Given the derivative at the boundary.



## Diffusion 


### 1D Steady Diffusion

(The equation itself is linear for second order derivative 0)

Discretization:
$$
\begin{aligned}
&\frac{\partial ^2 c}{\partial x^2} = 0\\
\Rightarrow & \frac{c_{i+1}-2c_i+c_{i-1}}{\Delta x^2}=0\\
&c_i=\frac{c_{i+1}+c_{i-1}}{2}
\end{aligned}
$$

### 2D Steady Diffusion

Add y direction
$$
\begin{aligned}
&\frac{\partial^2c}{\partial x^2}+\frac{\partial^2c}{\partial y^2}=0\\
\Rightarrow &\frac{c_{i+1,j}-2c_{i,j}+c_{i-1,j}}{\Delta x^2}+\frac{c_{i,j+1}-2c_{i,j}+c_{i,j-1}}{\Delta y^2}=0\\
\end{aligned}
$$
If $\Delta x  = \Delta y$: (5 pt. scheme)
$$
c_{i+1,j}+c_{i-1,j}+c_{i,j+1}+c_{i,j-1}-4c_{i,j}=0
$$

### 1D Unsteady Diffusion

Heat Equation / Mass Transfer Equation (PDE)
$$
\frac{\partial c}{\partial t} = D \frac{\partial ^2 c}{\partial x^2}
$$

#### FTCS (Explicit) (No Source)

FTCS = Forward Time Center Space 
$$
\begin{aligned}
\frac{c_i^{n+1}-c_i^n}{\Delta t}=D\frac{c_{i+1}^n-2c_i^n+c_{i-1}^n}{\Delta x^2}\\
c_i^{n+1}=c_i^n+D\frac{\Delta t}{\Delta x^2}\left(c_{i+1}^n-2c_i^n+c_{i-1}^n\right)
\end{aligned}
$$
(Superscripts = timestep numbers; Subscripts = Spatial positions (in x-dir.))

- Introduce **Evolutionary Parameter**: ($r > 0$)

$$
r= D\frac{\Delta t}{\Delta x^2}
$$

​		After organization:
$$
c_i^{n+1}=\left(1-2r\right)c_i^n+r\left(c_{i+1}^n+c_{i-1}^n\right)
$$
![image-20210920001829606](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210920001829606.png)

​		**Truncation Error**: $O(\Delta t,\Delta x^2)$

- **von Neumann Analysis**: (Stability test)

$$
\begin{aligned}
\xi^{n+1}e^{ik\ x_i} &=\left(1-2r\right)\xi^ne^{ik\ x_i}+r\left(\xi^ne^{ik\ x_{i+1}}+\xi^ne^{ik\ x_{i-1}}\right)\\
\xi&=1-2r+r\left(e^{ik\Delta x}+e^{-ik\Delta x}\right)=1+2r\left(\cos{\left(k\Delta x\right)}-1\right)=1-4r\sin^2{\left(\frac{k\Delta x}{2}\right)}
\end{aligned}
$$

​		So the stable condition for $r$: ($r$​ has an upper limit)
$$
\begin{aligned}
&-1\le1-4r\sin^2{\left(\frac{k\Delta x}{2}\right)}\le1\\
\Rightarrow &\quad 0\le r\le\frac{1}{2}
\end{aligned}
$$
​		$\Delta t$ should be sufficiently small compared to $\Delta x$

- **Remark**: 
  1. True domain of independence: the horizontal line $t = (n+1)\Delta t$
  2. for $r > 1/2$: $c^n_i$​ has a negative weight => unstable

- **Modified Equation**
  $$
  \begin{aligned}\left[\frac{\partial c}{\partial t}\right]_i^n-D\left[\frac{\partial^2c}{\partial x^2}\right]_i^n&=-\frac{\Delta t}{2}\left[\frac{\partial^2c}{\partial t^2}\right]_i^n+\frac{D}{12}\Delta x^2\left[\frac{\partial^4c}{\partial x^4}\right]_i^n+O\left(\Delta t^2,\Delta x^4\right)\\
  &=-\frac{1}{2}D\Delta x^2\left(r-\frac{1}{6}\right)\left[\frac{\partial^4c}{\partial x^4}\right]_i^n+O\left(\Delta t^2,\Delta x^4\right)
  \end{aligned}
  $$
  (LHS = Real PDE; RHS = Numerical Equation + Truncation Error)

  - $0\leq r<\frac{1}{6}$: 4th order anti-dissipative (反耗散), need further investigation to guarantee stability
  - $r = \frac{1}{6}$: Truncation Error: $-\frac{1}{540}D\Delta x^4\left[\frac{\partial^6c}{\partial x^6}\right]_i^n+O\left(\Delta t^3,\Delta x^6\right)$
  - $\frac{1}{6} < r \leq \frac{1}{2}$: 4th order dissipative (the peak will be lower and lower by numerical dissipative)

#### FTCS (With Point Source)

The point source here is represented as $Q_i$​ in a spatial step $\Delta x$ (because although actually it’s a point, in numerical method the intensity of a point source will become infinity)
$$
\begin{aligned}
\frac{c_i^{n+1}-c_i^n}{\Delta t}=D\frac{c_{i+1}^n-2c_i^n+c_{i-1}^n}{\Delta x^2}+\frac{Q_i^n}{\Delta x}\\
c_i^{n+1}=\left(1-2r\right)c_i^n+r\left(c_{i+1}^n+c_{i-1}^n\right)+\frac{Q_i^n\Delta t}{\Delta x}

\end{aligned}
$$
#### BTCS (Implicit)

$$
\begin{align}
\frac{c_j^{n+1}-c_j^n}{\Delta t}=\frac{D}{\Delta x^2}\left(c_{j+1}^{n+1}-2c_j^{n+1}+c_{j-1}^{n+1}\right)\\
-rc_{j-1}^{n+1}+\left(1+2r\right)c_j^{n+1}-rc_{j+1}^{n+1}=c_j^n 
\end{align}
$$

Can prove **unconditionally stable** (e.g. von Neumann). But computation **cost higher** than explicit.

- **Tridiagonal First Row**: (Dirichlet Boundary) 
  $$
  (1+2r)c_1^{n+1} - rc_2^{n+1} = c_1^n + rc ^{n+1}_0
  $$
  This is going to apply using matrices operation. $c_0^{n+1}$ is moved to the RHS for the first row. For the second row no need to move because the LHS will start with $c_1^{n+1}$.

  Can be proved **4th order dissipative (stable)** (Leading error) by modified equation

#### Leapfrog

$$
\frac{c_i^{n+1}-c_i^{n-1}}{2\Delta t}=D\frac{c_{i+1}^n-2c_i^n+c_{i-1}^n}{\Delta x^2}
$$

##### **du Fort-Frankel** 

(an alternative version of Leapfrog)
$$
\frac{c_i^{n+1}-c_i^{n-1}}{2\Delta t}=\frac{D}{\Delta x^2}\left(c_{i+1}^n-c_i^{n+1}-c_i^{n-1}+c_{i-1}^n\right)
$$
Difference: No $c_i^n$​ in the equation

Not uniformly consistent.

![image-20210920162558365](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210920162558365.png)

##### Crank-Nicolson

Can achieve 2nd order accuracy by 1st order equation. Use leapfrog for $c_i^{n+\frac{1}{2}}$ (assumption, $c^{n+\frac{1}{2}}$ is regarded as the average of the $n$ and $n+1$) in the RHS.
$$
\begin{align}
\frac{c_i^{n+1}-c_i^n}{\Delta t}&=D\frac{c_{i+1}^{n+\frac{1}{2}}-2c_i^{n+\frac{1}{2}}+c_{i-1}^{n+\frac{1}{2}}}{\Delta x^2}\\
c_i^{n+1}=c_i^n&+\frac{r}{2}\left[\left(c_{i+1}^{n+1}-2c_i^{n+1}+c_{i-1}^{n+1}\right)+\left(c_{i+1}^n-2c_i^n+c_{i-1}^n\right)\right]\\
-rc_{i-1}^{n+1}+&\left(2+2r\right)c_i^{n+1}-rc_{i+1}^{n+1}=rc_{i-1}^n+\left(2-2r\right)c_i^n+rc_{i+1}^n

\end{align}
$$
Tridiagonal Matrix.

Can prove **unconditionally stable** (e.g. von Neumann). But computation **cost higher** than explicit.

![image-20210920163233281](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210920163233281.png)

**Another way of interpreting C-N:** for $c_i^{n+\frac{1}{2}}$​, the lower part uses FTCS (explicit) and the upper part uses BTCS (implicit).
$$
\begin{align}
\frac{c_i^{n+\frac{1}{2}}-c_i^n}{\frac{\Delta t}{2}}&=D\frac{c_{i-1}^n-2c_i^n+c_{i+1}^n}{\Delta x^2}\\
\frac{c_i^{n+1}-c_i^{n+\frac{1}{2}}}{\frac{\Delta t}{2}}&=D\frac{c_{i-1}^{n+1}-2c_i^{n+1}+c_{i+1}^{n+1}}{\Delta x^2}

\end{align}
$$

- **Truncation Error**: $O(\Delta t^2, \Delta x^2)$

#### $\theta$ Method (Unification)

(Weighted average explicit-implicit)
$$
c_i^{n+1}=c_i^n+r\left[\theta\left(c_{i+1}^{n+1}-2c_i^{n+1}+c_{i-1}^{n+1}\right)+\left(1-\theta\right)\left(c_{i+1}^n-2c_i^n+c_{i-1}^n\right)\right]
$$

- $\theta = 0$: FTCS
- $\theta = 1$: BTCS
- $\theta = \frac{1}{2}$: C-N

### 2D Unsteady Diffusion

$$
\frac{\partial c}{\partial t}=D\left(\frac{\partial^2c}{\partial x^2}+\frac{\partial^2c}{\partial y^2}\right)
$$

#### FTCS (Explicit)

$$
\frac{c_{i,j}^{n+1}-c_{i,j}^n}{\Delta t}=D\left[\frac{c_{i+1,j}^n-2c_{i,j}^n+c_{i-1,j}^n}{\Delta x^2}+\frac{c_{i,j+1}^n-2c_{i,j}^n+c_{i,j-1}^n}{\Delta y^2}\right]
$$

$$
c_{i,j}^{n+1}=c_{i,j}^n+D\frac{\Delta t}{\Delta x^2}\left(c_{i+1,j}^n-2c_{i,j}^n+c_{i-1,j}^n\right)+D\frac{\Delta t}{\Delta y^2}\left(c_{i,j+1}^n-2c_{i,j}^n+c_{i,j-1}^n\right)
$$





## Advection

### 1D Advection

$$
\frac{\partial c}{\partial t}+u\frac{\partial c}{\partial x}=0\ (\text{suppose}\ u>0)
$$

- **Exact solution**: a wave propagating along the characteristic lines ($x-ut = \text{const}$​) with constant amplitude (without subsidence).
- **Real influence region** (for one point): **points on the characteristic line**.
- **Inherent problem**: **algorithmic dissipation** and **dispersion** in the truncation errors.

#### FTBS (Explicit Upwind)

FTBS = Forward Time Backward Space
$$
\begin{aligned}
\frac{c_i^{n+1}-c_i^n}{\Delta t}+u\frac{c_i^n-c_{i-1}^n}{\Delta x}=0\\
c_i^{n+1}=c_i^n-\frac{u\Delta t}{\Delta x}\left(c_i^n-c_{i-1}^n\right)\\
\text{Cr}=\frac{u\Delta t}{\Delta x}\;\; \text{Courant\ number}

\end{aligned}
$$

- **Courant Number**: represents the ratio of the true and numerical advection velocities. 

  - $\mathrm{Cr }= 1$: In one time step travelled one space step
  - $\mathrm{Cr }> 1$: In one time step travelled more than one space step
  - $\mathrm{Cr }< 1$: In one time step travelled less than one space step

  The iterative $c^{n+1}_i$​ becomes: (The weighted average of the 2 $c^n$)

$$
c_i^{n+1}=c_i^n-\text{Cr}\left(c_i^n-c_{i-1}^n\right)=\left(1-\text{Cr}\right)c_i^n+\text{Cr}\,c_{i-1}^n
$$

![image-20210920095507393](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210920095507393.png)

​		For stability, $0\le \text{Cr}\le 1$​

​		**Truncation Error**: $O(\Delta x, \Delta t)$

- **Modified Equation Analysis**
  $$
  \left[\frac{\partial c}{\partial t}\right]_i^n+u\left[\frac{\partial c}{\partial x}\right]_i^n=0 \Rightarrow \left[\frac{\partial^2c}{\partial x\partial t}\right]_i^n=-u\left[\frac{\partial^2c}{\partial x^2}\right]_i^n=-\frac{1}{u}\left[\frac{\partial^2c}{\partial t^2}\right]_i^n
  $$

  $$
  \begin{aligned}
  \left[\frac{\partial c}{\partial t}\right]_i^n+u\left[\frac{\partial c}{\partial x}\right]_i^n-0&=-\frac{\Delta t}{2}\left[\frac{\partial^2c}{\partial t^2}\right]_i^n-\frac{\Delta t^2}{6}\left[\frac{\partial^3c}{\partial t^3}\right]_i^n-\ldots+u\left({\frac{\Delta x}{2}\left[\frac{\partial^2c}{\partial x^2}\right]}_i^n-{\frac{\Delta x^2}{6}\left[\frac{\partial^3c}{\partial x^3}\right]}_i^n+\ldots\right)\\
  &=\left(-\frac{\Delta t}{2}\left[\frac{\partial^2c}{\partial t^2}\right]_i^n+u{\frac{\Delta x}{2}\left[\frac{\partial^2c}{\partial x^2}\right]}_i^n\right)+\left(-\frac{\Delta t}{6}\left[\frac{\partial^3c}{\partial t^3}\right]_i^n-u{\frac{\Delta x}{6}\left[\frac{\partial^3c}{\partial x^3}\right]}_i^n\right)+\ldots
  \end{aligned}
  $$

  

  Connected with the numerically solution and replace with the Courant Number:
  $$
  \left[\frac{\partial c}{\partial t}\right]_i^n+u\left[\frac{\partial c}{\partial x}\right]_i^n=\frac{1}{2}u\Delta x\left(1-\mathrm{Cr}\right)\left[\frac{\partial^2c}{\partial x^2}\right]_i^n-\frac{1}{6}u\Delta x^2\left(1-\mathrm{Cr}\right)\left(1-2\mathrm{Cr}\right)\left[\frac{\partial^3c}{\partial x^3}\right]_i^n+\ldots
  $$
  **Leading Error** (Section that contributes most error):
  $$
  \left[\frac{\partial c}{\partial t}\right]_i^n=\left(\frac{1}{2}u\Delta x\left(1-\mathrm{Cr}\right)\right)\left[\frac{\partial^2c}{\partial x^2}\right]_i^n
  $$
  Resembling **diffusion** (Not want)

  - When error = 0, requires $\mathrm{Cr} = 1$; $c^{n+1}_i = c^{n}_{i-1}$​ (Exact solution)
  - $\text{Cr}< 1$: Numerical diffusion (smoothing)
  - $\text{Cr} > 1$: Anti-diffusion ($\mathrm{D} < 0$, the peak will be very high -> $\infty$​)

- **Truncation Error**

  - Second Order: (Even)

    - $$
      a\frac{\partial ^2 c}{\partial x^2}\rightarrow \text{Numerically\ diffusion}
      $$

    - $$
      -a\frac{\partial ^2 c}{\partial x^2}\rightarrow \text{Anti-diffusion}
      $$

  - Third Order: (Odd) (Dispersion -> oscillation solution)

    - $$
      \pm a\frac{\partial^3c}{\partial x^3}\rightarrow \text{Numerical\ dispersion}
      $$

  - Fourth Order: (Reverse 2nd order)

    - $$
      -a\frac{\partial^4c}{\partial x^4}\rightarrow \text{Numerical\ diffusion}
      $$

    - $$
      +a\frac{\partial^4c}{\partial x^4}\rightarrow \text{Anti-diffusion}
      $$

- **Courant-Friedrichs-Lewy (CFL) Stability Condition** (Necessary but not sufficient condition)

  In order for a method to be stable, the difference domain of dependence must contain the differential one.

- **von Neumann Stability Analysis**
  $$
  \begin{aligned}
  c_i^{n+1}&=\left(1-\mathrm{Cr}\right)c_i^n+\mathrm{Cr}c_{i-1}^n \\
  \xi^{n+1}e^{ikx_i}&=\left(1-\mathrm{Cr}\right)\xi^ne^{ikx_i}+\mathrm{Cr}\xi^ne^{ikx_{i-1}}\\
  \xi&=1-\mathrm{Cr}+\mathrm{Cr}e^{-ik\Delta x}=1-\mathrm{Cr}+\mathrm{Cr}\cos{\left(k\Delta x\right)}-i\mathrm{Cr}\sin{\left(k\Delta x\right)}
  
  \end{aligned}
  $$

  $$
  \begin{aligned}
  \left|\xi\right|^2&=\left(1-\mathrm{Cr}+\mathrm{Cr}\cos{\left(k\Delta x\right)}\right)^2+\mathrm{Cr}^2\sin^2{\left(k\Delta x\right)}\\
  &=1+2\mathrm{Cr}\left(\mathrm{Cr}-1\right)\left(1-\cos{\left(k\Delta x\right)}\right)=1+4\mathrm{Cr}\left(\mathrm{Cr}-1\right)\sin^2{\left(\frac{k\Delta x}{2}\right)}
  \end{aligned}
  $$

  The condition of stable: $0< \mathrm{Cr}\le 1$

#### *FTFS (Downwind)

$$
\begin{align}
&\frac{c_i^{n+1}-c_i^n}{\Delta t}+u\frac{c_{i+1}^n-c_i^n}{\Delta x}=0\\
c_i^{n+1}&=c_i^n-\mathrm{Cr}\left(c_{i+1}^n-c_i^n\right)=-\mathrm{Cr}c_{i+1}^n+\left(1+\mathrm{Cr}\right)c_i^n
\end{align}
$$

Depend on points that a forward, disobey physics.

**Unconditionally unstable**.

#### *FTCS (Explicit Euler)

$$
\frac{c_i^{n+1}-c_i^n}{\Delta t}+u\frac{c_{i+1}^n-c_{i-1}^n}{2\Delta x}=0
\Rightarrow c_i^{n+1}=c_i^n-\frac{\mathrm{Cr}}{2}\left(c_{i+1}^n-c_{i-1}^n\right)
$$

- **von Neumann**
  $$
  \begin{aligned}
  \xi^{n+1}e^{ikx_i}&=\xi^ne^{ikx_i}-\frac{\mathrm{Cr}}{2}(\xi^ne^{ikx_{i+1}}-\xi^ne^{ikx_{i-1}})\\
  \xi&=1-\frac{\mathrm{Cr}}{2}\left(e^{ik\Delta x}-e^{-ik\Delta x}\right)=1-\mathrm{Cr}\cos{(k\Delta x)}
  \end{aligned}
  $$
  Impossible to get $|\xi| \leq 1$

#### BTCS (Implicit Euler)

(Most of the implicit algorithms are defined as backward algorithms)

Actually solve a tri-diagonal matrix.
$$
\begin{aligned}
\frac{c_i^{n+1}-c_i^n}{\Delta t}+u\frac{c_{i+1}^{n+1}-c_{i-1}^{n+1}}{2\Delta x}=0\\
\frac{\mathrm{Cr}}{2}c_{i+1}^{n+1}+c_i^{n+1}-\frac{\mathrm{Cr}}{2}c_{i-1}^{n+1}=c_i^n

\end{aligned}
$$

- **von Neumann**
  $$
  \begin{aligned}
  \frac{Cr}{2}\xi^{n+1}e^{ikx_{i+1}}+\xi^{n+1}e^{ikx_i}-\frac{Cr}{2}\xi^{n+1}e^{ikx_{i-1}}&=\xi^ne^{ikx_i}\\
  \Rightarrow\frac{Cr}{2}\xi\left(e^{ik\Delta x}-e^{-ik\Delta x}\right)+\xi&=1\\
  \xi=\frac{1}{Cr\ i\sin{\left(k\Delta x\right)}+1}&\\
  \left|\xi\right|^2=\frac{1}{1+\left(Cr\sin{\left(k\Delta x\right)}\right)^2}&\le1
  \end{aligned}
  $$
  So this scheme is **unconditionally stable**. $\Delta t $ and $\Delta x$​​ can be chosen especially to converge faster (larger $\Delta t$)

#### BTBS (Implicit Downwind)

$$
c_i^{n+1}=\left(\frac{1}{1+\mathrm{Cr}}\right)c_i^n+\left(\frac{\mathrm{Cr}}{1+\mathrm{Cr}}\right)c_{i-1}^{n+1}
$$

![image-20210920115838716](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210920115838716.png)

This scheme is **unconditionally stable**.

But BTBS assumes that **boundary information travels infinitely fast** into the solution domain, thus the solution **resembles more that of the diffusion equation** than one for advection. 

#### Leapfrog

(2nd order acc.)
$$
\begin{align}
\frac{c_i^{n+1}-c_i^{n-1}}{2\Delta t}+u\frac{c_{i+1}^n-c_{i-1}^n}{2\Delta x}=0\\
c_i^{n+1}=c_i^{n-1}-Cr\left(c_{i+1}^n-c_{i-1}^n\right)

\end{align}
$$
![image-20210920120236700](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210920120236700.png)

- **von Neumann**
  $$
  \begin{align}
  \xi^{n+1}e^{ikx_i}&=\xi^{n-1}e^{ikx_i}-\mathrm{Cr}\left(\xi^ne^{ikx_{i+1}}-\xi^ne^{ikx_{i-1}}\right)\\
  \xi^2&=1-2\mathrm{Cr}\xi i\sin{(k\Delta x)}\\
  \xi^2&+2\mathrm{Cr}\xi i\sin{\left(k\Delta x\right)}-1=0\\
  \Rightarrow&\xi=-\mathrm{Cr}\ i\sin{\left(k\Delta x\right)}\pm\sqrt{1-\left(\mathrm{Cr}\sin{\left(k\Delta x\right)}\right)^2}
  \end{align}
  $$

  - If $1-(\text{Cr}\sin(k\Delta x))^2 \ge0$ $\Rightarrow \left|\xi\right|^2=1-\left(\text{Cr}\sin{\left(k\Delta x\right)}\right)^2+\left(\text{Cr}\sin{\left(k\Delta x\right)}\right)^2=1$
  - If $1-(\text{Cr}\sin(k\Delta x))^2 < 0$​ $\Rightarrow |\xi|^2 >1$

  So the condition of stable: $\mathrm{Cr} \le 1$​.

- **Remark**: Leapfrog is not self-starting i.e. need another scheme to start.





## Advection Diffusion 

### 1D SS Advection Diffusion

$$
\begin{align}
&u\frac{\partial c}{\partial x}=\frac{\partial^2c}{\partial x^2}\\
u\frac{c_{i+1}-c_{i-1}}{2\Delta x}-&D\frac{c_{i+1}-2c_i+c_{i-1}}{\Delta x^2}=0

\end{align}
$$

#### Grid cell Peclet Number

The ratio between advection and diffusion to compare which one is dominant. In the numerical method definition:
$$
\begin{aligned}
\mathrm{P}=\frac{u\Delta x}{D}=\frac{\mathrm{Cr}}{r}=\frac{\frac{u\Delta t}{\Delta x}}{\frac{D\Delta t}{\Delta x^2}}&\\
\left(2-\mathrm{P}\right)c_{i+1}-4c_i+\left(2+\mathrm{P}\right)c_{i-1}&=0
\end{aligned}
$$
Solve $c$ (suppose $c$​ is of the form of polynomial)
$$
\begin{aligned}
c_j = k^j&\\
\Rightarrow k_1=1,\,&k_2=\frac{2+\mathrm{P}}{2-\mathrm{P}}\\
c_j=a_1&+a_2\left(\frac{2+\mathrm{P}}{2-\mathrm{P}}\right)^j

\end{aligned}
$$
If $\text{P}>2$​​, huge oscillation occurs. So we should limit $0<\text{P}<2$

(Trivious solution $\mathrm{P}=2$)

#### Generalized Upwind

Introduce $\alpha$ to reduce the oscillation
$$
\begin{align}
\frac{dc}{dx}&=\frac{1}{2\Delta x}\left[\left(1-\alpha\right)\left(c_{j+1}-c_j\right)+\left(1+\alpha\right)\left(c_j-c_{j-1}\right)\right]\\
\frac{u}{2\Delta x}&[\left(c_{i+1}-c_{i-1}\right)-\alpha\left(c_{i+1}-2c_i+c_{i-1}\right]-D\frac{c_{i+1}-2c_i+c_{i-1}}{\Delta x^2}=0\\
c_j&=k^j\\
\Rightarrow k_1=&1,k_2=\frac{2+\left(1+\alpha\right)\mathrm{P}}{2-\left(1-\alpha\right)\mathrm{P}}
\end{align}
$$
We are actually solving
$$
u\frac{dc}{dt}-\left(D+\frac{1}{2}\alpha u\Delta x\right)\frac{d^2c}{dx^2}=0
$$
(Problem: $D$ is increasing, higher diffusion, smoother => inaccuracy)

### 1D Unsteady Advection Diffussion

$$
\frac{\partial c}{\partial t}+u\frac{\partial c}{\partial x}=\frac{\partial^2c}{\partial x^2}
$$

#### FTBS for Advection, FTCS for Diffusion

$$
\frac{c_i^{n+1}-c_i^n}{\Delta t}+u\frac{c_i^n-c_{i-1}^n}{\Delta x}=D\frac{c_{i+1}^n-2c_i^n+c_{i-1}^n}{\Delta x^2}
$$

- **Truncation Error**: $O(\Delta x, \Delta t)$​ (1st order)

#### Both FTCS

$$
\begin{aligned}\frac{c_i^{n+1}-c_i^n}{\Delta t}+u\frac{c_{i+1}^n-c_{i-1}^n}{2\Delta x}=D\frac{c_{i+1}^n-2c_i^n+c_{i-1}^n}{\Delta x^2}\\
c_i^{n+1}=c_i^n+r\left(c_{i+1}^n-2c_i^n+c_{i-1}^n\right)-\frac{\mathrm{Cr}}{2}\left(c_{i+1}^n-c_{i-1}^n\right)
\end{aligned}
$$

- **von Neumann**
  $$
  \begin{aligned}
  \xi&=1+r\left(e^{ik\Delta x}-2+e^{-ik\Delta x}\right)-\frac{\mathrm{Cr}}{2}\left(e^{ik\Delta x}-e^{-ik\Delta x}\right)\\
  &=1+2r\left(\cos{\left(k\Delta x\right)}-1\right)-i\mathrm{Cr}\sin{\left(k\Delta x\right)}\\
  &=1-4r\sin^2{\left(\frac{k\Delta x}{2}\right)}-iCr\sin{(k\Delta x)}\\
  \left|\xi\right|^2&=\left(1-4r\sin^2{\left(\frac{k\Delta x}{2}\right)}\right)^2+Cr^2\sin^2{\left(k\Delta x\right)}
  \end{aligned}
  $$

  - If $k\Delta x = \pi$:
    $$
    \left(1-4r\right)^2\le1 \Rightarrow0\le r\le\frac{1}{2}
    $$

  - If $k\Delta x \rightarrow 0$: (neglect the 4th order terms)
    $$
    \begin{aligned}\left(1-4r\left(\frac{k\Delta x}{2}\right)^2\right)^2+\mathrm{Cr}^2\left(k\Delta x\right)^2&=1-2r\left(k\Delta x\right)^2+\mathrm{Cr}^2\left(k\Delta x\right)^2\\
    &=1+\left(\mathrm{Cr}^2-2r\right)\left(k\Delta x\right)^2\le1\\
    \Rightarrow \mathrm{Cr}^2 &\leq 2r
    \end{aligned}
    $$

  To recap: $\mathrm{Cr}^2\leq 2r\leq1$

#### Crank-Nicolson

$$
\begin{aligned}
\frac{c_i^{n+1}-c_i^n}{\Delta t}+\frac{1}{2}u\left(\frac{c_{i+1}^{n+1}-c_{i-1}^{n+1}}{2\Delta x}+\frac{c_{i+1}^n-c_{i-1}^n}{2\Delta x}\right)
=&\frac{1}{2}D\left(\frac{c_{i+1}^{n+1}-2c_i^{n+1}+c_{i-1}^{n+1}}{\Delta x^2}-\frac{c_{i+1}^n-2c_i^n+c_{i-1}^n}{\Delta x^2}\right)\\
-\left(r+\frac{\mathrm{Cr}}{2}\right)c_{i-1}^{n+1}+\left(2+2r\right)c_i^{n+1}-\left(r-\frac{\mathrm{Cr}}{2}\right)c_{i+1}^{n+1}
=&\left(r+\frac{\mathrm{Cr}}{2}\right)c_{i-1}^n+\left(2-2r\right)c_i^n+\left(r-\frac{\mathrm{Cr}}{2}\right)c_{i+1}^n
\end{aligned}
$$

- **von Neumann**
  $$
  -\left(r+\frac{\mathrm{Cr}}{2}\right)\xi e^{-ik\Delta x}+\left(2+2r\right)\xi-\left(r-\frac{\mathrm{Cr}}{2}\right)\xi e^{ik\Delta x}=\left(r+\frac{\mathrm{Cr}}{2}\right)e^{-ik\Delta x}+2-2r+\left(r-\frac{\mathrm{Cr}}{2}\right)e^{ik\Delta x}
  $$

  $$
  \xi\left(-2r\cos{\left(k\Delta x\right)}+2i\sin{\left(k\Delta x\right)}+2+2r\right)=2r\cos{\left(k\Delta x\right)}-2i\sin\left(k\Delta x\right)+2-2r
  $$

  $$
  \begin{align}\xi&=\frac{r\left(\cos{\left(k\Delta x\right)}-1\right)-i \sin\left(k\Delta x\right)+1}{-r\left(\cos{\left(k\Delta x\right)}-1\right)+i\sin{\left(k\Delta x\right)}+1}\\
  &=\frac{1-2r\sin^2{\left(\frac{k\Delta x}{2}\right)}-i\sin\left(k\Delta x\right)}{1+2r\sin^2{\left(\frac{k\Delta x}{2}\right)}+i\sin{\left(k\Delta x\right)}}<1
  \end{align}
  $$

  **Unconditionally stable**.

### 2D Unsteady Advection Diffusion

Containing point source / sink ($\delta$ means only at a point there is $Q$) and even reactions (in this equation 1st order kinects)
$$
\frac{\partial c}{\partial t}+u\frac{\partial c}{\partial x}+v\frac{\partial c}{\partial y}=D\left(\frac{\partial^2c}{\partial x^2}+\frac{\partial^2c}{\partial y^2}\right)+\delta\left(\mathrm{source/sink}\right)Q-kc(\mathrm{reaction/decay})
$$

