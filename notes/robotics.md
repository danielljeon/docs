# Robotics

---

<details markdown="1">
  <summary>Table of Contents</summary>

<!-- TOC -->
* [Robotics](#robotics)
  * [1 Mathematical Conventions](#1-mathematical-conventions)
    * [1.1 Matrix Operations and Transforms](#11-matrix-operations-and-transforms)
  * [2 Homogenous Transformation Matrix Operator](#2-homogenous-transformation-matrix-operator)
  * [3 Basic Problems of Manipulators](#3-basic-problems-of-manipulators)
    * [3.1 FD: Forward Displacement](#31-fd-forward-displacement)
    * [3.2 ID: Inverse Displacement](#32-id-inverse-displacement)
    * [3.3 FV: Forward Velocity](#33-fv-forward-velocity)
    * [3.4 IV: Inverse Velocity](#34-iv-inverse-velocity)
    * [3.5 FF: Forward Force](#35-ff-forward-force)
    * [3.6 IF: Inverse Force](#36-if-inverse-force)
  * [4 Denavit-Hartenberg (D&H) Convention: Manipulator Representation](#4-denavit-hartenberg-dh-convention-manipulator-representation)
    * [4.1 Zero Displacement Condition](#41-zero-displacement-condition)
    * [4.2 Homogenous Transform Representation](#42-homogenous-transform-representation)
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

---

## 3 Basic Problems of Manipulators

### 3.1 FD: Forward Displacement

### 3.2 ID: Inverse Displacement

### 3.3 FV: Forward Velocity

### 3.4 IV: Inverse Velocity

### 3.5 FF: Forward Force

### 3.6 IF: Inverse Force

---

## 4 Denavit-Hartenberg (D&H) Convention: Manipulator Representation

The D&H convention is a systematic recipe for assigning coordinate frames to robot links and joints
to describe kinematics using 4 parameters. However, the D&H convention does not result in a single
unique coordinate frame representation for a given system, since multiple equally valid frame
attachments may exist.

When attaching frames under the D&H convention:

- The $z_i$ axis is aligned with the $i^{th}$ joint axis.
- The $x_i$ axis is directed along the common normal between $z_i$ and $z_{i+1}$ (or, if the axes
  intersect, chosen perpendicular to $z_i$).
- The $y_i$ axis is chosen to complete a right-handed coordinate system.

Note: $a_i$ corresponds to the common normal distance between successive $z$-axes.

The 4 parameters are defined as:

1. $a_{i-1}$ is the distance from $\hat{z}_{i-1}$ to $\hat{z}_i$ measured along $\hat{x}_{i-1}$.
2. $\alpha_{i-1}$ is the angle between $\hat{z}_{i-1}$ and $\hat{z}_i$ measured
   about $\hat{x}_{i-1}$.
3. $d_i$ is the distance from $\hat{x}_{i-1}$ to $\hat{x}_i$ measured along $\hat{z}_i$.
4. $\theta_i$ is the angle between $\hat{x}_{i-1}$ and $\hat{x}_i$ measured about $\hat{z}_i$.

### 4.1 Zero Displacement Condition

In the D&H convention, the zero displacement condition specifies that each joint
variable ($\theta_i$ for revolute or $d_i$ for prismatic) is measured relative to a reference
configuration where the parameter equals zero. This anchors the D&H parameters to the robot's
physical geometry.

### 4.2 Homogenous Transform Representation

$${}^{i-1}T_i \;=\; R_{x_{i-1}}(\alpha_{i-1}) \, D_{x_{i-1}}(a_{i-1}) \, R_{z_i}(\theta_i) \, D_{z_i}(d_i)$$

or

$${}^{i-1}T_i \;=\; D_{x_{i-1}}(a_{i-1}) \, R_{x_{i-1}}(\alpha_{i-1}) \, D_{z_i}(d_i) \, R_{z_i}(\theta_i)$$

When expanded into the full matrix from, this is represented as:

$${}^{i-1}T_i =
\begin{bmatrix}
\cos(\theta_i) & -\sin(\theta_i) & 0 & a_{i-1} \\
\sin(\theta_i) \cos(\alpha_{i-1}) & \cos(\theta_i) \cos(\alpha_{i-1}) & -\sin(\alpha_{i-1}) & -\sin(\alpha_{i-1}) d_i \\
\sin(\theta_i) \sin(\alpha_{i-1}) & \cos(\theta_i) \sin(\alpha_{i-1}) & \cos(\alpha_{i-1}) & \cos(\alpha_{i-1}) d_i \\
0 & 0 & 0 & 1
\end{bmatrix}$$
