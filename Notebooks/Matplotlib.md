# Matplotlib

Learning notes of [Matplotlib](https://www.bilibili.com/video/BV1Jx411L7LU) by [Morvan Zhou](https://space.bilibili.com/243821484/video)

## Install

In Windows:

``` bash
python -V 	# Check python version (Or `python3 -V`)
python -m pip install --upgrade pip	# Upgrade pip
python -m pip install numpy	# Install numpy first
python -m pip install matplotlib
```



## Basic

### Basic Commands

- `plt.plot(x, y)`: Plot the function `y` of `x`
- `plt.show()`: Show the figure

``` python
import matplotlib.pyplot as plt	# Import matplotlib but only .pyplot part is useful
import numpy as np

x = np.linspace(-1, 1, 50) # Generate 50 points from -1 to 1
# y = 2 * x + 1
y = x ** 2	# Try another function

plt.plot(x, y)  # plot
plt.show()  # show figure
```

Result:

![Basic1](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/image-20210912133850828.png)

### Figures

When we want to make `y1 = 2 * x + 1` and `y2 = x ** 2` in 2 figures:

``` python
plt.figure()	# when making multiple figures, use this command before very `plt.plot()`
	# (if not spec., number will show in order)
plt.plot(x, y1)  # plot the first figure

plt.figure(num = 3, figsize = (8, 5))	# specifiy as 'figure 3' and a figure size 8x5
plt.plot(x, y2)	# plot the second figure
plt.plot(x, y1, color = 'red', linewidth = 1.0, linestyle = '--')	# plot another line with specific color, line width and dashed style in 'Figure 3'

plt.show() 
```

Result: 2 windows with ‘Figure 1’ and ‘Figure 3’ (Figure 3 shown below)

![2.2_figure3](https://cdn.jsdelivr.net/gh/Nikucyan/MD_IMG//img/2.2_figure3.png)

### Coordinate

