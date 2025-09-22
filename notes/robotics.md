# Robotics

---

<details markdown="1">
  <summary>Table of Contents</summary>

<!-- TOC -->
* [Robotics](#robotics)
  * [1 Mathematical Conventions](#1-mathematical-conventions)
    * [1.1 Matrix Operations and Transforms](#11-matrix-operations-and-transforms)
  * [2 Homogenous Transformation Matrix Operator](#2-homogenous-transformation-matrix-operator)
<!-- TOC -->

</details>

---

## 1 Mathematical Conventions

### 1.1 Matrix Operations and Transforms

In pure linear algebra, matrix operations/composition is conducted right to left.

In robotics applications, homogeneous transforms encode the relative pose between coordinate frames.

Suppose we have:

- Frame {0} (the base),
- Frame {1} (the first joint),
- Frame {2} (the second joint).

We define transforms:

- ${}^0T_1$: transform that expresses coordinates of a vector in frame {1} with respect to frame
  {0}.
- ${}^1T_2$: transform that expresses coordinates of a vector in frame {2} with respect to frame
  {1}.

If we want the transform from {2} to {0}, we write: ${}^0T_2 = {}^0T_1 \, {}^1T_2$

This appears as left-to-right chaining, mirroring the physical kinematic chain (base to joint 1 to
joint 2). But when applied to a vector:
$$\mathbf{p}_0 = {}^0T_1 \big( {}^1T_2 \, \mathbf{p}_2 \big)$$

the actual multiplication is still right-to-left.

**Notation is read logically left to right, mathematical calculations are done right to left.**

---

## 2 Homogenous Transformation Matrix Operator

The general homogeneous transformation matrix of a system is given by:

$$\mathbf{H}=\begin{bmatrix} \mathbf{R} & \mathbf{t}\\ \mathbf{0}^T & 1\end{bmatrix} \quad \text{with } \mathbf{R}\in\{R_x,R_y,R_z,R_k\},\ \mathbf{t}=[x\ y\ z]^T$$

Further rotational and translational transforms are described as:

$$
R_x(\theta) =
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & \cos\theta & -\sin\theta & 0 \\
0 & \sin\theta & \cos\theta & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

$$
R_y(\theta) =
\begin{bmatrix}
\cos\theta & 0 & \sin\theta & 0 \\
0 & 1 & 0 & 0 \\
-\sin\theta & 0 & \cos\theta & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

$$
R_z(\theta) =
\begin{bmatrix}
\cos\theta & -\sin\theta & 0 & 0 \\
\sin\theta & \cos\theta & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

$$
R_k(\theta) =
\begin{bmatrix}
k_x^2 (1 - \cos\theta) + \cos\theta & k_x k_y (1 - \cos\theta) - k_z \sin\theta & k_x k_z (1 - \cos\theta) + k_y \sin\theta & 0\\
k_x k_y (1 - \cos\theta) + k_z \sin\theta & k_y^2 (1 - \cos\theta) + \cos\theta & k_y k_z (1 - \cos\theta) - k_x \sin\theta & 0\\
k_x k_z (1 - \cos\theta) - k_y \sin\theta & k_z k_y (1 - \cos\theta) + k_x \sin\theta & k_z^2 (1 - \cos\theta) + \cos\theta & 0\\
0 & 0 & 0 & 1
\end{bmatrix}
$$

- The axis vector $\mathbf{k} = [k_x, k_y, k_z]^T$ must be normalized such that $| k | = 1$.
  Otherwise, $R_k(\theta)$ will not be a pure rotation (it will scale as well as rotate).

$$
T =
\begin{bmatrix}
1 & 0 & 0 & x \\
0 & 1 & 0 & y \\
0 & 0 & 1 & z \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

These transforms assume a standard right hand rule and active rotation.

- **Right-hand rule:** If your right-hand thumb is along the positive axis (say +z), then your
  curled fingers indicate the positive rotation direction. So R_z(\theta) rotates a point in the
  xy-plane counterclockwise when looking down the +z-axis.
- **Active rotation:** The vector is rotated in a fixed coordinate frame.
