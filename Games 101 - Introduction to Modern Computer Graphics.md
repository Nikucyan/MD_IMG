# Games 101 - Introduction to Modern Computer Graphics

Lecturer: Prof. Lingqi Yan

www.cs.ucsb.edu/~lingqi/ 

---

# Linear Algebra (Lec. 2)

## Cross Product

Check whether a point $P$ is on the left side of the line $\vec{AB}$: $\vec{AB} \times \vec{AP} > 0$ (left); $\vec{AB} \times \vec{AP} < 0$ (right) 

Check whether a point $P$ is in a triangle $ΔABC$ or not: $\vec{AB} \times \vec{AP} > 0$ & $\vec{BC} \times \vec{BP} > 0$ & $\vec{CA} \times \vec{CP} > 0$ (inside)

(Corner case: $\times$ product = 0, should be defined)

## Matrices

Product can be operated: $A \cdot B = (M\times N) (N\times P)$ (OK)

- Dot Product: $\vec{a} \cdot \vec{b} = \vec{a}^T \vec{b} = \begin{pmatrix} x_a & y_a & z_a\end{pmatrix} \cdot \begin{pmatrix} x_b \\ y_b \\ z_b\end{pmatrix} = \begin{pmatrix} x_ax_b & y_ay_b & z_az_b\end{pmatrix} $
- Cross Product: $\vec{a} \times \vec{b} = A^* b =\begin{pmatrix} 0 & -z_a & y_a \\ z_a & 0 & -x_a \\ -y_a & x_a & 0\end{pmatrix} \begin{pmatrix} x_b \\ y_b \\ z_b\end{pmatrix}$

# Transformation (Lec. 3-4)

## Transformation

1. **Scale Matrix**: Ratio s: 

   ![image-20210715010441369](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210715010441369.png)

   $$
   x' = sx;\;y' = sy\,\Leftrightarrow \begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} s_x & 0 \\ 0 & s_y \end{bmatrix}\begin{bmatrix} x \\ y \end{bmatrix}
   $$

2. **Reflection Matrix**:

   ![image-20210715010427112](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210715010427112.png)

   $$
   \left\{ \begin{array} xx'=-x \\ y' = y \end{array}\right. \Leftrightarrow \begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} -1 & 0 \\ 0 & 1 \end{bmatrix}\begin{bmatrix} x \\ y \end{bmatrix}
   $$

3. **Shear Matrix**:

   ![image-20210715010400583](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210715010400583.png)

   $$
   \begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} 1 & a \\ 0 & 1 \end{bmatrix}\begin{bmatrix} x \\ y \end{bmatrix}
   $$

4. **Rotate (2D) around (0, 0), counter-clock**

   ![image-20210715010533272](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210715010533272.png)

   $$
   R_{\theta} = \begin{bmatrix} \cos{\theta} & -\sin{\theta} \\ \sin{\theta} & \cos{\theta} \end{bmatrix}
   $$

5. **Translation** (Not linear transformation)

   ![image-20210715011601111](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210715011601111.png)

   $$
   \left\{ \begin{array} xx'= x + t_x \\ y' = y + t_y \end{array}\right. \Leftrightarrow \begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} a & b \\ c & d \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} + \begin{bmatrix} t_x \\ t_y \end{bmatrix}
   $$

## Homogeneous Coordinates（齐次坐标）

(Add a third coordinate - w-coordinate)

- 2D point = (x, y, 1)^T^ 

- 2D vector = (x, y, 0)^T^  （平移不变性：Direction and magnitude only）

  $$\Rightarrow \begin{pmatrix} x' \\ y' \\ w'\end{pmatrix} = \begin{pmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1\end{pmatrix}\begin{pmatrix} x \\ y \\ 1\end{pmatrix} = \begin{pmatrix} x+t_x \\ y+t_y \\ 1\end{pmatrix}$$  In homog. coord. $\begin{pmatrix} x \\ y \\ w\end{pmatrix}$ in 2-D point: $\begin{pmatrix} x/w \\ y/w \\ 1\end{pmatrix} (w\neq 1)$

- Vector + Vector  = Vector (0 + 0)

  Point - Point = Vector (1 - 1)

  Point + Vector = Point (0 + 1)

  Point + Point = ?? (1 + 1) (Actually, Mid point)

## Affine Transformation（仿射）

Affine map = linear map + translation

$\Rightarrow \begin{pmatrix} x' \\ y' \\ 1\end{pmatrix} = \begin{pmatrix} a & b & t_x \\ c & d & t_y \\ 0 & 0 & 1\end{pmatrix}\begin{pmatrix} x \\ y \\ 1\end{pmatrix}$  Dimension rises (costs)

## Inverse Transformation

$M\rightarrow M^{-1}$

## Composite Transform

**<u>Transformation Ordering Matters</u>**

Notice that the matrices are applied right to left（无交换律 但有结合律）

e.g.	$T_{(1,0)}\cdot R_{45} \cdot \begin{pmatrix} x' \\ y' \\ 1\end{pmatrix} $: $R_{45} \rightarrow T_{(1,0)}$;	$A_n(....(A_2(A_1(x))) = A_n \cdot ... \cdot A_2\cdot A_1\cdot \begin{pmatrix} x' \\ y' \\ 1\end{pmatrix}$

Pre-multiply n matrices to obtain a single matrix representing combined transform (for performance)

## Example: (Decomposing)

Rotate around point C:

Representation: $T(c)\cdot R\cdot T(-c)$	(Move to the original point - rotate - move back (-c))

## 3D Transformation

- 3D point = (x, y, z, 1)^T^ 

- 3D vector = (x, y, z, 0)^T^ 

  ~ (x/w, y/w, z/w)

### **Homogeneous Coordinates**  

$$\Rightarrow \begin{pmatrix} x' \\ y' \\ z' \\1 \end{pmatrix} = \begin{pmatrix} a & b & c & t_x \\ d & e & f & t_y \\ g & h & i & t_z \\ 0 & 0 & 0 & 1\end{pmatrix}\begin{pmatrix} x \\ y \\ z \\ 1\end{pmatrix}$$

**Order**: Linear transformation first, then translation（先线性变换，再平移）

### Rotate around Axis

x-axis: $(\vec{y}\times\vec{z} = \vec{x})$  $R_x(\alpha) = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & \cos\alpha & -\sin\alpha & 0 \\ 0 & \sin\alpha & \cos\alpha & 0 \\ 0 & 0 & 0 & 1\end{pmatrix}$		z-axis: $(\vec{x}\times\vec{y} = \vec{z})$  $R_z(\alpha) = \begin{pmatrix} \cos\alpha & -\sin\alpha & 0 & 0 \\\sin\alpha & \cos\alpha & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1\end{pmatrix}$

<u>y-axis</u>: $(\vec{z}\times\vec{x} = \vec{y})$  $R_y(\alpha) = \begin{pmatrix} \cos\alpha & 0 & \sin\alpha & 0 \\ 0 & 1 & 0 & 0 \\ -\sin\alpha & 0 & \cos\alpha & 0 \\ 0 & 0 & 0 & 1\end{pmatrix}$	（y 轴方向是相反的）

### Compose any 3D Rotations from R~x~, R~y~, R~z~

$\bold{R}_{xyz}(\alpha, \beta, \gamma) = \bold{R}_x(\alpha)\bold{R}_y(\beta)\bold{R}_z(\gamma)$

- **Rodrigues’ Rotation Formula**: By angle $\alpha$ around axis n (Euler Angles)
  $$
  \mathbf{R}(\mathbf{n}, \alpha)=\cos (\alpha) \mathbf{I}+(1-\cos (\alpha)) \mathbf{n} \mathbf{n}^{T}+\sin (\alpha) \underbrace{\left(\begin{array}{ccc}0 & -n_{z} & n_{y} \\ n_{z} & 0 & -n_{x} \\ -n_{y} & n_{x} & 0\end{array}\right)}_{\mathbf{N}}
  $$
  ![image-20210715020442614](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210715020442614.png)

## Viewing Transformations

### View / Camera Transformation（视图变换）

#### **MVP: Model-View Projection** 

(Model transformation: placing objects; View transformation: placing camera; Projection transformation)

Define camera first: 

- Position: $\vec{e}$ 
- Look-at / gaze direction: $\vec{g}$
- Up direction: $\vec{t}$

![image-20210715110858353](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210715110858353.png) 

Key Observation: If the camera and all objects move together, the “photo” will be the same => Transform to: the origin with up @ Y, look @ -Z

**Transformation Matrix** $M_{\mathrm{view}}$ in math: $M_{\mathrm{view}} = R_{\mathrm{view}}T_{\mathrm{view}}$  (Transformed to a std. coordinate)

$T_{\mathrm{view}} = \begin{bmatrix}1 & 0 & 0 & -x_e \\ 0 & 1 & 0 & -y_e \\ 0 & 0 & 1 & -z_e \\ 0 & 0 & 0 & 1 \end{bmatrix}$； $$R_{\text {view }}=\left[\begin{array}{cccc}x_{\hat{g} \times \hat{t}} & y_{\hat{g} \times \hat{t}} & z_{\hat{g} \times \hat{t}} & 0 \\ x_{t} & y_{t} & z_{t} & 0 \\ x_{-g} & y_{-g} & z_{-g} & 0 \\ 0 & 0 & 0 & 1\end{array}\right]$$

(Also known as model-view transformation)

### Projection Transformation

#### Orthographic Projection

![image-20210715112050817](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210715112050817.png) 

Camera @ 0, Up @ Y, Look @ -Z. Translate & Scale the resulting rectangle to [-1, 1]^2^ 

(Looking at / along -Z is making near and far not intuitive (n > f). OpenGL uses left hand coordinates)

In general, map a cuboid [l, r] x [b, t] x [f, n] to canonical cube [-1, 1]^3^ 

**Transformation Matrix**: $$M_{\text {ortho }}=\left[\begin{array}{cccc}\frac{2}{r-l} & 0 & 0 & 0 \\ 0 & \frac{2}{t-b} & 0 & 0 \\ 0 & 0 & \frac{2}{n-f} & 0 \\ 0 & 0 & 0 & 1\end{array}\right]\left[\begin{array}{cccc}1 & 0 & 0 & -\frac{r+l}{2} \\ 0 & 1 & 0 & -\frac{t+b}{2} \\ 0 & 0 & 1 & -\frac{n+f}{2} \\ 0 & 0 & 0 & 1\end{array}\right]$$ (Translate center 0; Scale 2)

#### Perspective Projection (Most common)

![image-20210715112333094](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210715112333094.png) (Not parallel)

![image-20210715121619362](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210715121619362.png) (Similar triangle $y' = \frac{n}{z} y$)

**Process**: Frustum（视锥）- (n - n, f - f) - Cuboid - Orthographic Proj. (M~o~) 

![](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210715112553604.png) 

（近平面不变，远平面中心不变) 

**In homogeneous coordinates**: 

$\left(\begin{array}{l}x \\ y \\ z \\ 1\end{array}\right) \Rightarrow\left(\begin{array}{c}n x / z \\ n y / z \\ \text { unknown } \\ 1\end{array}\right) \begin{gathered}\text { mult. } \\ \text { by z } \\ ==\end{gathered}\left(\begin{array}{c}n x \\ n y \\ \text { still unknown } \\ z\end{array}\right)$;  Replace z with n;  $M_{p\rightarrow o} = \begin{pmatrix}n & 0 & 0 & 0 \\ 0 & n & 0 & 0 \\ 0 & 0 & n+f & -nf \\ 0 & 0 & 1 & 0 \end{pmatrix}$ 

$M_p = M_o \cdot M_{p\rightarrow o}$

**Vertical Field of View (fovY)**	(Assuming symmetry: l = -r; b = -t)

![image-20210716002006747](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210716002006747.png) 

![image-20210716002132476](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210716002132476.png) 

$\tan\frac{\text{fovY}}{2} = \frac{t}{|n|}$; $\text{Aspect} = \frac{r}{t}$

# Rasterization (Lec. 5-7)

## Rasterize

Rasterize = Draw onto the screen

Define Screen Space: **Pixel**: (0, 0) - (Width - 1, Height - 1)（均匀小方块）; Centered @ (x + 0.5, y + 0.5) (Irrelavent to z)

![image-20210716003103499](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210716003103499.png) 

$M_{\text{viewport}} = \begin{pmatrix} \frac{width}{2} & 0 & 0 & \frac{width}{2} \\ 0 & \frac{height}{2} & 0 & \frac{height}{2} \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}$

### Sampling (Approximate a Triangle)

Rasterization as 2D sampling

For triangles, each pixel is whether **inside / outside** should be checked.

```c++
for (int x = 0; x < xmax; ++x)
    for (int y = 0; y < ymax; ++y)
        image[x][y] = inside(tri, x + 0.5, y + 0.5);
```

#### Evaluating `inside(tri, x, y)`

3 cross products: $\vec{AB}$, $\vec{BC}$, $\vec{CA}$  (Same symbol = inside)

(Want: All required points (pixels) inside the triangle)

**Edge cases:** covered by both tri. 1 and 2: Not process / specific

Instead of checking all pixels on the screen, using **incremental triangle traversal** / a **bouding box** (AABB) can sometimes be faster

![image-20210716004224152](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210716004224152.png) Suitable for thin and rotated triangles

### Artifacts （瑕疵）

(Error / Mistakes / Inaccuracies)

Jaggies (Too low sampling rate) / Noire / Wagon wheel effect (Signal changing too fast for sampling)

Idea: Blur - Sample

### Frequency Domain

In freq. domain: $f= \frac{1}{T}$ 

**Fourier Transform**: 
$$
f(x) = \frac{A}{2} + \frac{2A \cos(t\omega)}{\pi} - \frac{3A \cos(3t\omega)}{3\pi} + \frac{2A \cos(5t\omega)}{5\pi} - \frac{2A\cos(7t\omega)}{7\pi}+ ...
$$
(Higher freq. needs faster sampling. Or samples erroneously a low freq. signal)

**Filtering**: Getting rid of certain freq. contents (high / low / band / ...)

Filter out high freq. (Blur) - Low pass filter (e.g. Box function)

Theorem: Spatial domain  ·  Filter (Convolution Kernel)   =  ...

​						$\downarrow$ $\mathcal{F}$ 	$F(\omega)=\int_{-\infty}^{\infty} f(x) e^{-2 \pi i \omega x} d x$	  	$\uparrow$ Inverse Fourier Transform	

​				 Fourier domain  $\times$ 	Fourier filter					=  ...

- **Convolution**（卷积）: （加权平均滤波器）Product in spatial domain = Convolution in frequency domain

  Wider Kernal = Lower Frequency (Blurer)

**Sampling = Repeating Frequency Contents**

- **Aliasing** = Mixing freq. contents (sampling too slow)

  ![image-20210719012208952](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210719012208952.png) 

  Reduction: Increasing sampling rate; Antialiasing (Blur - Sample, Make F contents "narrower")

-  **Antialiasing ** = Limiting, then repeating

   ![image-20210719012533952](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210719012533952.png) 

**Solution**: Convolve $f(x. y)$ by a 1-pixel box-blur & Sample pixel's center

### Antialiasing Techniques

**MSAA (Multisample Antialiasing)**

By supersampling (same effects as blur, then sampling) (1 pixel -> 2x2 / 4x4 & Average)

Cost: Increase the computation work (4x4 = 16 times) - Key pixel distribution

**FXAA (Fast Approximation AA)**

后期处理降锯齿，使用边界

**TAA (Temporal AA)** 

Use the previous one frame for AA

**Super Resolution (DLSS - Deep Learning Supersampling)**

## Visibility / Occlusion 

### Z (Depth) Buffering（深度缓存）

**Point Algorithm**（由远到近 - 不好处理相互重叠的关系）

**Z-buffer**: Frame buffer for color values; z-buffer for depth (Smaller z - closer (darker); Larger z - farther (lighter))

**Algorithm** during Rasterization

```c
for (each triangle T)
	for (each sample (x, y, z) in T)
		if (z < zbuffer[x, y])		    // Closest sample so far （是否更新该像素，判断 z）
			framebuffer[x, y] = rgb;	// Update color
			zbuffer[x, y] = z;		    // Update depth （相同的可更可不更 - 可能为抖动）
		else
			;						  // Do nothing
```

Complexity: O(n) 

​	For n triangles, only check. No other relativity.

Order doesn't matter

# Shading (Lec. 7-10)

Apply a material to an object

## Shading Model (Blinn-Phong Reflectance Model)

Light reflected towards camera.

![image-20210721175148434](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210721175148434.png) 

Viewer direction, v; Surface normal, n; Light direction, l (for each of many lights); Surface parameters (color, shininess, …)  

**Shading is local** - No shadows will be generated

### Diffuse Reflection

Light is scattered uniformly in all directions (Surface color is the same for all viewing direction)

#### Light Falloff (Beer Lambert's Law)

![image-20210721175802013](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210721175802013.png) 

#### Lambertian (Diffuse) Shading 

Shading independent of view direction
$$
L_{d}=k_{d}\left(I / r^{2}\right) \max (0, \mathbf{n} \cdot \mathbf{l})
$$
($L_d$​ - diffusely reflected light; $k_d$​ - diffuse coefficient (color), $k_d = 0$: black (all absorbed), $1$: white; $(I/r^2)$​ - energy arrived at the shading point; $\max(0, \mathbf{n} \cdot \mathbf{l})$​ - energy received by the shading point​; irrelavent to $\hat{v}$)

### Specular

Intensity depends on view direction (Bright near mirror reflection direction)

$\hat{v}$ closes to mirror direction - half vector near normal

![image-20210722001128103](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210722001128103.png) 

$\bold{h} = \text{bisector} (\bold{v},\bold{l}) = \frac{\bold{v}+\bold{l}}{||\bold{v}+\bold{l}||}$​ - 半程向量

![image-20210722001625078](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210722001625078.png)
$$
\begin{aligned}
L_{s}&=k_{s}\left(I / r^{2}\right) \max (0, \cos \alpha)^{p} \\
&=k_{s}\left(I / r^{2}\right) \max (0, \mathbf{n} \cdot \mathbf{h})^{p}
\end{aligned}
$$
($L_s$​ - specularly reflected light; $k_s$​ - specular coefficient; $p$ - narrows the reflection lobe)

$p$ 越大，反射高光面积越小；$k_s$​ 越大，高光越明显

 (Only use $\bold{R}$ and $\bold{l}$ (mirror) - Phong relax model)

### Ambient

Not depend on anything 

Fake approximation

$L_a = k_a I_a$​ 	($L_a$​ - reflected ambient light; $k_a$ - ambient coefficient)

![image-20210722003002721](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210722003002721.png) 

### Summary

$$
\begin{aligned}
L &=L_{a}+L_{d}+L_{s} \\
&=k_{a} I_{a}+k_{d}\left(I / r^{2}\right) \max (0, \mathbf{n} \cdot \mathbf{l})+k_{s}\left(I / r^{2}\right) \max (0, \mathbf{n} \cdot \mathbf{h})^{p}
\end{aligned}
$$

<img src="https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210722003122186.png" alt="image-20210722003122186" style="zoom:100%;" />

## Shading Frequencies

(Face / Vertex / Pixel)

### Shading Methods

- Shade each **triangle** (**flat shading**) - Not good for smooth surface
- Shade each **vertex** (**Gouraud shading**) - Interporate colors from vertices
- Shade each **pixel** (**Phong shading**) - Full shading model each pixel (Not the Blinn-Phong Reflectance Model)

 ![image-20210726162249468](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726162249468.png) ![image-20210726162302011](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726162302011.png) ![image-20210726162311304](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726162311304.png) 

![image-20210726163228332](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726163228332.png) 

### Pre-Vertex Normal Vectors

Vertex normal - Average surrounding face normals	$N_v =\frac{\sum_i N_i}{||\sum_i N_i||}$ 

![image-20210726163313740](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726163313740.png) 

~ Barycentric interpolation of vertex normals (Need to normalize)

## Graphics (Real-time Rendering) Pipeline

~ GPU

- Input: Model, View, Projection transforms
- Rasterization: Sampling triangle coverage
- Fragment Processing: Z-Buffering visibility test / Shading / Texture mapping

![image-20210726163726899](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726163726899.png) 

## Texture Mapping

纹理映射 - UV ((u,v) coordinate) ($u, v \in [0, 1]$)

### Barycentric Coordinates

### Interpolation Access Triangles

Specific values <u>@ vertices</u> / smoothly varing <u>across triangles</u>

#### Barycentric Coordinates

A coordinate system for triangles $(\alpha, \beta, \gamma)$​​​​ (every point could be represented in this form). Inside the triangle if all three coordinates are non-negative

The barycentric coordinate of $A$​ is:  $(\alpha, \beta, \gamma) = (1, 0, 0)$​ ;  $(x,y) = \alpha A + \beta B + \gamma C =A$​

<img src="https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726174026150.png" alt="image-20210726174026150" style="zoom:50%;" /> 	$(x,y) = \alpha A + \beta B + \gamma C$ ;  $\alpha + \beta + \gamma = 1$	$\alpha\leq 1,\, \beta\leq 1,\, \gamma \leq1$

Geometric viewpoint — proportional **areas**:  $\alpha = \frac{A_A}{A_A+A_B+A_C}$​​​ ;  $\beta = \frac{A_B}{A_A+A_B+A_C}$​​​ ;  $\gamma = \frac{A_C}{A_A+A_B+A_C}$​​​ 

**Centroid**:  $(\alpha, \beta, \gamma) = (\frac{1}{3}, \frac{1}{3}, \frac{1}{3})$ ;  $(x,y) = \frac{1}{3}A + \frac{1}{3} B + \frac{1}{3} C$	$(A_A = A_B = A_C = \frac{1}{3})$​

Express **any point**: 
$$
\begin{aligned}
\alpha &=\frac{-\left(x-x_{B}\right)\left(y_{C}-y_{B}\right)+\left(y-y_{B}\right)\left(x_{C}-x_{B}\right)}{-\left(x_{A}-x_{B}\right)\left(y_{C}-y_{B}\right)+\left(y_{A}-y_{B}\right)\left(x_{C}-x_{B}\right)} \\
\beta &=\frac{-\left(x-x_{C}\right)\left(y_{A}-y_{C}\right)+\left(y-y_{C}\right)\left(x_{A}-x_{C}\right)}{-\left(x_{B}-x_{C}\right)\left(y_{A}-y_{C}\right)+\left(y_{B}-y_{C}\right)\left(x_{A}-x_{C}\right)} \\
\gamma &=1-\alpha-\beta
\end{aligned}
$$
**Color** interpolation for every point: linear interpolation	$V = \alpha V_A + \beta V_B + \gamma V_C$ (could be any property)

Disadvantage: Not invariant under projection (depth matters)

(3D - Use 3D interpolation OK; 2D - rather than use projection)

### Apply: Diffuse Color

``` pseudocode
for each rasterized screen sample (x,y)		// Usually a pixel center
    (u,v) = evaluate texture coordinate at (x,y)	// Applying Barycentric coordinates
    texcolor = texture.sample(u,v);
	set sample’s color to texcolor;  	// Usually diffuse kd (Blinn-Phong)
```

### Texture Magnification (AA)

(used for insufficient resolution)

**Texel**: A pixel on a texture（纹理元素 / 纹素）

#### Easy Cases

- Nearest
- Bilinear (Interpolation) - Pretty good results at reasonable costs
- Bicubic (Interpolation) - Instead of 4 points (bilinear), using 16 (4x4) points for 1 lerp

##### Bilinear Magnification

![image-20210726224355412](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726224355412.png) 

- Linear Interpolation (1D):  $\mathrm{lerp} (x, v_0, v_1) = v_0 +x(v_1 - v_0)$​
- Helper Lerps:  $u_0 = \text{lerp}(s,u_{00}, u_{10})$ ;  $u_1 = \mathrm{lerp} (s, u_{01}, u_{11})$ (Horizontal)
- Final Vertical Lerp:  $f(x, y) = \mathrm{lerp} (t, u_0, u_1)$​ (Vertical)

Lerp - Linear Interpolation

#### Hard Cases

##### Problem of point sampling textures 

(can be solved by supersampling but costly)

Near: Jaggies (Minification / Downsampling for too high resolution) ; Far: Moire (Upsampling)

<img src="https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726225139847.png" alt="image-image-20210726225139847" style="zoom:100%;" /> 

##### Mipmap

Allowing (fast, approx., square) range queries

![image-20210726225937700](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726225937700.png) 

(Image Pyramid) "Mip Hierachy": level = D

- Estimate texture footprint using texture coordinates of neighboring screen samples

  ![image-20210726230157286](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726230157286.png) 

- $$
  D = \log_2 L \quad L=\max \left(\sqrt{\left(\frac{d u}{d x}\right)^{2}+\left(\frac{d v}{d x}\right)^{2}}, \sqrt{\left(\frac{d u}{d y}\right)^{2}+\left(\frac{d v}{d y}\right)^{2}}\right)
  $$

  ![image-20210726230307099](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726230307099.png) 层查询 - 后者为选择较大 L 是近似（UV)

Near - range quires in low D level Mipmap; Far - high D level 

​	If want more continuous results: interpolation between levels

##### Trilinear Interpolation

![image-20210726230541800](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210726230541800.png) 

Linear interpolation (lerp) based on continuous D value

##### Limitations

In far and complex region - Overblur (box)

- **Anisotropic filtering**（各向异性过滤）: look up axis-aligned rectangular zones（长方形区域搜寻）, but still have problems in diagonal footprints - Ripmap (need graphics memory, doesn't need high flops x3)
- **EWA filtering** (Time computing costs): use multiple look ups / weighted average. able to handle irregular footprints

###  Applications of Textures

#### Environmental Map

- **Environmental Map**: 有光从各个方向进入眼睛，所有光都有对应，用纹理映射环境光

  不记录深度，都认为是无限远

- **Environmental Lighting**: 环境光记录于球面 - 渲染时展开

  - Spherical EM (Problem: Prone to distortion (top and bottom))

  - Cube Map: A vector maps to cube point along that direction (6 square texture maps) - Less distortion but need direction for face computations

    <img src="https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210727001419299.png" alt="image-20210727001419299" style="zoom:33%;" /> <img src="https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210727001444460.png" alt="image-20210727001444460" style="zoom:40%;" /> $(u, v) = (1, 1)$, $x = y = z$​; $(u, v) = (0, 0)$, $x = -y = -z$

    Right face has $x > |y|$ and $x > |z|$

#### Textures Affecting Shading

##### Bump / Normal Mapping

Adding surface details without adding more triangles (Perturb surface normal per pixel)

未改变几何 - 产生凹凸错觉

- Perturb the nromal (in flatland)
  - Original surface normal:  $n(p) = (0,1)$

  - Derivative @ p:  $p = c\cdot[h(p+1) - h(p)]$

  - Perturbed normal:  $n(p) = (-dp, 1)$​.normalized()

    ![image-20210728175134284](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210728175134284.png)<img src="https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210728175103568.png" alt="image-20210728175103568" style="zoom:29.5%;" />

- Perturb the normal (in 3D) (local coordinates)
  - Original surface normal:  $n(p) = (0,0,1)$
  - Derivatives @ p:  $-\frac{dp}{du} = c_1  \cdot [h(\mathbf{u}+1) - h(\mathbf{u})]$ ;  $-\frac{dp}{dv} = c_2  \cdot [h(\mathbf{v}+1) - h(\mathbf{v})]$​
  - Perturbed normal:  $n = (-\frac{dp}{du}, -\frac{dp}{dv}, 1)$.normalized()

##### Displacement Mapping

Uses the same texture as in bumping mapping, but actually moves vertices

- 3D Procedural Noise + Solid Modeling  
- Provide Precomputed Shading
- 3D Texture and Volume Rendering

![image-20210730162847529](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210730162847529.png) 

# Geometry (Lec. 10-12)

## Representing Ways

- **Implicit**: algebraic surface / level sets（等高线法）/ constructive solid geometry (Boolean) / distance functions / fractal ... 

  (Don’t tell exact positions,tell spe. relationships. e.g. sphere: $x^2 + y^2 + z^2 = 1$, generally $f(x, y, z) = 0$)

  Hard for sampling but easy for test in / outside

  ​	**Distance functions**:

  <img src="https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210803161758591.png" alt="image-20210803161758591.png" style="zoom:40%;" />

  ![image-20210803161825884](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210803161825884.png)

  ​	**Level Set**: (CT / MRI)

  ![image-20210803161910976](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210803161910976.png)

- **Explicit**: point cloud / polygon-mesh (.obj) / subdivision / NURBS / ...

  (Generally, $f: \R^2 \rightarrow \R^3: (u, v) \rightarrow (x, y, z)$)
  
  Hard to test in / outside but easy to sample

## Curves

### Bézier Curves (Explicit)

- 3 pt. - quadratic Bézier 

  Connect $b_0$ - all $b_0^2$ - $b_2$​ (every $t$ in $[0,1]$)

  ![image-20210803162714678](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210803162714678.png) 

- 4 pt. - cubic Bézier 

  <img src="https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210803162911437.png" alt="image-20210803162911437" style="zoom:45%;" /> 

(De Casteljau’s Algorithm)

![image-20210803163059425](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210803163059425.png) 

#### Evaluating Bézier Curves 

**Algebraic Formula**  
$$
\begin{aligned}
\mathbf{b}_0^1 (t) =& (1-t) \mathbf{b}_0 +t \mathbf{b}_1\\
\mathbf{b}_1^1 (t) =& (1-t) \mathbf{b}_1 +t \mathbf{b}_2\\
\\
\mathbf{b}_0^2 (t) =& (1-t) \mathbf{b}_0^1 +t \mathbf{b}_1^1\\
\\
\Rightarrow \mathbf{b}_0^2 (t) =& (1-t)^2 \mathbf{b}_0 +2t(1-t) \mathbf{b}_1 + t^2 \mathbf{b}_2
\end{aligned}
$$
**Bernstein form** of a Bézier curve of order n:  
$$
\mathbf{b}^{n}(t)=\mathbf{b}_{0}^{n}(t)=\sum_{j=0}^{n} \mathbf{b}_{j} B_{j}^{n}(t)
$$
($\mathbf{b}^n$​​ - Bezier curve order n (vector polynomial degree n); $\mathbf{b}_j$​​ - Bezier control points (vector in $\R^N$​​); $B_j^n$​ - Bernstein polynomial (scalar polynomial of degree n))

where **Bernstein polynomials**:
$$
B_{i}^{n}(t)=\left(\begin{array}{l}n \\ i\end{array}\right) t^{i}(1-t)^{n-i}
$$
<img src="https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210803164056094.png" alt="image-20210803164056094" style="zoom:45%;" /> At anywhere, sum of $b_i^3=1$.

### B-Splines





## Surfaces

### Bézier Surfaces

### Triangles and Quads

#### Subdivision

#### Simplification

#### Regularization





