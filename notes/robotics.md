# Robotics

---

<details markdown="1">
  <summary>Table of Contents</summary>

<!-- TOC -->
* [Robotics](#robotics)
  * [1 Mathematical Conventions](#1-mathematical-conventions)
    * [1.1 Matrix Operations and Transforms](#11-matrix-operations-and-transforms)
    * [1.2 Quadrant-Aware Trigonometric Functions](#12-quadrant-aware-trigonometric-functions)
  * [2 Homogenous Transformation Matrix Operator](#2-homogenous-transformation-matrix-operator)
  * [3 Denavit-Hartenberg (D&H) Convention: Manipulator Representation](#3-denavit-hartenberg-dh-convention-manipulator-representation)
    * [3.1 Zero Displacement Condition](#31-zero-displacement-condition)
    * [3.2 Homogenous Transform Representation](#32-homogenous-transform-representation)
  * [4 Basic Problems of Manipulators](#4-basic-problems-of-manipulators)
    * [4.1 Forward Displacement (FD)](#41-forward-displacement-fd)
    * [4.2 Inverse Displacement (ID)](#42-inverse-displacement-id)
      * [4.2.1 Inverse of Transformation Equations](#421-inverse-of-transformation-equations)
      * [4.2.2 Square and Add Trigonometric Equations](#422-square-and-add-trigonometric-equations)
      * [4.2.3 Trigonometric Identities to Simplify Coupled Terms](#423-trigonometric-identities-to-simplify-coupled-terms)
      * [4.2.4 Tangent Half-Angle Substitution](#424-tangent-half-angle-substitution)
      * [4.2.5 Geometric Substitution or Projection](#425-geometric-substitution-or-projection)
    * [4.3 Forward Velocity (FV)](#43-forward-velocity-fv)
    * [4.4 Inverse Velocity (IV)](#44-inverse-velocity-iv)
    * [4.5 Forward Force (FF)](#45-forward-force-ff)
    * [4.6 Inverse Force (IF)](#46-inverse-force-if)
<!-- TOC -->

</details>

---

## 1 Mathematical Conventions

### 1.1 Matrix Operations and Transforms

In pure linear algebra, matrix operations/composition is conducted right to
left.

In robotics applications, homogeneous transforms encode the relative pose
between coordinate frames.

Suppose we have:

- Frame {0} (the base),
- Frame {1} (the first joint),
- Frame {2} (the second joint).

We define transforms:

- ${}^0T_1$: transform that expresses coordinates of a vector in frame {1} with
  respect to frame {0}.
- ${}^1T_2$: transform that expresses coordinates of a vector in frame {2} with
  respect to frame {1}.

If we want the transform from {2} to {0}, we
write: ${}^0T_2 = {}^0T_1 \, {}^1T_2$

This appears as left-to-right chaining, mirroring the physical kinematic chain
(base to joint 1 to joint 2). But when applied to a vector:
$$\mathbf{p}_0 = {}^0T_1 \big( {}^1T_2 \, \mathbf{p}_2 \big)$$

the actual multiplication is still right-to-left.

**Notation is read logically left to right, mathematical calculations are done
right to left.**

### 1.2 Quadrant-Aware Trigonometric Functions

In robotics, special trigonometric functions like $\mathrm{Atan2}(y, x)$ are
used instead of the standard one-argument versions, such
as $\tan^{-1} \left( y / x \right)$. These functions are quadrant-aware, meaning
they consider the signs of both x and y to determine the correct angle direction
in the full 360 degree plane.

This is important because robotic joints and end-effectors often move through
multiple quadrants, and a normal arctangent function would lose that sign
information, leading to incorrect angles or sudden jumps in orientation.

Functions like `atan2` ensure that the calculated joint or tool angles always
match the true geometric direction of motion, which is essential for accurate
positioning, smooth control, and avoiding discontinuities in inverse kinematics.

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

- The axis vector $\mathbf{k} = [k_x, k_y, k_z]^T$ must be normalized such
  that $| k | = 1$. Otherwise, $R_k(\theta)$ will not be a pure rotation (it
  will scale as well as rotate).

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

- **Right-hand rule:** If your right-hand thumb is along the positive axis (say
  +z), then your curled fingers indicate the positive rotation direction.
  So $R_z(\theta)$ rotates a point in the xy-plane counterclockwise when looking
  down the +z-axis.
- **Active rotation:** The vector is rotated in a fixed coordinate frame.

---

## 3 Denavit-Hartenberg (D&H) Convention: Manipulator Representation

The D&H convention is a systematic recipe for assigning coordinate frames to
robot links and joints to describe kinematics using 4 parameters. However, the
D&H convention does not result in a single unique coordinate frame
representation for a given system, since multiple equally valid frame
attachments may exist.

When attaching frames under the D&H convention:

- The $z_i$ axis is aligned with the $i^{th}$ joint axis.
- The $x_i$ axis is directed along the common normal between $z_i$
  and $z_{i+1}$ (or, if the axes intersect, chosen perpendicular to $z_i$).
- The $y_i$ axis is chosen to complete a right-handed coordinate system.

Note: $a_i$ corresponds to the common normal distance between successive z-axes.

The 4 parameters are defined as:

1. $a_{i-1}$ is the distance from $\hat z_{i-1}$ to $\hat z_i$ measured
   along $\hat x_{i-1}$.
2. $\alpha_{i-1}$ is the angle between $\hat z_{i-1}$ and $\hat z_i$ measured
   about $\hat x_{i-1}$.
3. $d_i$ is the distance from $\hat x_{i-1}$ to $\hat x_i$ measured
   along $\hat z_i$.
4. $\theta_i$ is the angle between $\hat x_{i-1}$ and $\hat x_i$ measured
   about $\hat z_i$.

### 3.1 Zero Displacement Condition

In the D&H convention, the zero displacement condition specifies that each joint
variable ($\theta_i$ for revolute or $d_i$ for prismatic) is measured relative
to a reference configuration where the parameter equals zero. This anchors the
D&H parameters to the robot's physical geometry.

### 3.2 Homogenous Transform Representation

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

---

## 4 Basic Problems of Manipulators

| Problem                       | Known                   | Find                    | Equation                                | Field               |
|-------------------------------|-------------------------|-------------------------|-----------------------------------------|---------------------|
| **Forward Displacement (FD)** | Joint angles            | End-effector pose       | $X = f(\theta)$                         | Position kinematics |
| **Inverse Displacement (ID)** | End-effector pose       | Joint angles            | $\theta = f^{-1}(X)$                    | Position kinematics |
| **Forward Velocity (FV)**     | Joint velocities        | End-effector velocities | $\dot{X} = J(\theta) \dot{\theta}$      | Velocity kinematics |
| **Inverse Velocity (IV)**     | End-effector velocities | Joint velocities        | $\dot{\theta} = J^{-1}(\theta) \dot{X}$ | Velocity kinematics |
| **Forward Force (FF)**        | Joint torques           | End-effector forces     | $F = J^{-T}(\theta) \tau$               | Statics             |
| **Inverse Force (IF)**        | End-effector forces     | Joint torques           | $\tau = J^{T}(\theta) F$                | Statics             |

Useful conversions:

| Formula                                                             | Result                                                                                   |
|---------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| $\sin\theta = a$                                                    | $\theta = \mathrm{Atan2}\left(a, \pm\sqrt{1 - a^2}\right)$                               |
| $\cos\theta = b$                                                    | $\theta = \pm \mathrm{Atan2}\left(\sqrt{1 - b^2}, b\right)$                              |
| $a\cos\theta + b\sin\theta = 0$                                     | $\theta = \mathrm{Atan2}(a, -b)$ and $\theta = \mathrm{Atan2}(-a, b)$                    |
| $a\cos\theta + b\sin\theta = c$                                     | $\theta = \mathrm{Atan2}(b, a) \pm \mathrm{Atan2}\left(\sqrt{a^2 + b^2 - c^2}, c\right)$ |
| $a\cos\theta - b\sin\theta = c$ and $a\sin\theta + b\cos\theta = d$ | $\theta = \mathrm{Atan2}(ad - bc, ac + bd)$                                              |

Useful identities:

| Identity                 | Use Case        | Formula                                       |
|--------------------------|-----------------|-----------------------------------------------|
| Sine Difference Identity | Parallel Joints | $\sin(A - B) = \sin A \cos B - \cos A \sin B$ |

### 4.1 Forward Displacement (FD)

Also called Forward Kinematics, this problem determines the position and
orientation of the end-effector in Cartesian space given the known joint
variables (angles for revolute joints, displacements for prismatic joints).

### 4.2 Inverse Displacement (ID)

Also called Inverse Kinematics, this problem determines the joint variables
required to reach a desired end-effector position and orientation.

In many manipulators, this involves solving nonlinear trigonometric equations
that include multiple joint variables, such as terms
like $\sin(\theta_2 + \theta_3)$ or $\cos\theta_2 \cos\theta_3$.

To isolate one variable, we often use algebraic and trigonometric manipulation
to remove extra variables:

#### 4.2.1 Inverse of Transformation Equations

Sometimes, the homogeneous transformation between frames is expressed as:

$${}^0T_3 = {}^0T_1 \cdot {}^1T_2 \cdot {}^2T_3$$

If you are trying to isolate a particular joint (for example, $\theta_2$), you
can pre- or post-multiply by the inverse of one of the known transformations:

$${}^1T_3 = ({}^0T_1)^{-1} \cdot {}^0T_3$$

This helps move known terms to one side, reducing dimensionality and isolating
the unknown joint. Conceptually, taking inverses "peels off" known links from
the kinematic chain.

#### 4.2.2 Square and Add Trigonometric Equations

When equations involve both $\sin\theta$ and $\cos\theta$, squaring and adding
them can eliminate one variable. Example:

$$x = L_1\cos\theta + L_2\cos(\theta + \phi)$$
$$y = L_1\sin\theta + L_2\sin(\theta + \phi)$$

Squaring and adding gives:

$$x^2 + y^2 = L_1^2 + L_2^2 + 2L_1L_2\cos\phi$$

The variable $\theta$ cancels out, allowing you to solve directly for $\phi$
using the law of cosines. This trick is especially useful for planar 2R
manipulators.

#### 4.2.3 Trigonometric Identities to Simplify Coupled Terms

Expand compound angle expressions to separate joint terms using:

$$\sin(a + b) = \sin a \cos b + \cos a \sin b$$
$$\cos(a + b) = \cos a \cos b - \sin a \sin b$$

For example, $\sin_{2,3}$ and $\cos_{2,3}$ can be expanded
into $\sin\theta_2$, $\cos\theta_2$, $\sin\theta_3$, and $\cos\theta_3$ terms,
making it easier to isolate one joint variable at a time.

#### 4.2.4 Tangent Half-Angle Substitution

When both $\sin\theta$ and $\cos\theta$ appear in a single equation, substitute:

$$t = \tan\frac{\theta}{2}, \quad \sin\theta = \frac{2t}{1+t^2}, \quad \cos\theta = \frac{1-t^2}{1+t^2}$$

This converts trigonometric equations into rational polynomials, which can then
be solved algebraically (often with the quadratic formula). This trick is
powerful in 3D manipulator kinematics or for symbolic derivations.

#### 4.2.5 Geometric Substitution or Projection

Projecting the manipulator's geometry onto a plane (e.g., $xy$ or $xz$) allows
geometric reasoning instead of heavy algebra. For example, by applying the law
of cosines to a 2-link arm:

$$\cos\phi = \frac{L_1^2 + L_2^2 - r^2}{2L_1L_2}, \quad r = \sqrt{x^2 + y^2}$$

This approach replaces multiple trigonometric equations with a direct geometric
relationship, simplifying the inverse problem significantly.

### 4.3 Forward Velocity (FV)

Given the joint velocities, determine the end-effector's linear and angular
velocities.

### 4.4 Inverse Velocity (IV)

Given the desired end-effector velocity, determine the required joint
velocities.

### 4.5 Forward Force (FF)

Determine the forces and torques at the end-effector (in Cartesian space) that
result from known joint torques.

### 4.6 Inverse Force (IF)

Determine the joint torques required to generate a desired end-effector force.
