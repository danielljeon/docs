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
  * [4 Basic Problems of Manipulators with (D&H) Convention](#4-basic-problems-of-manipulators-with-dh-convention)
    * [4.1 Forward Displacement (FD)](#41-forward-displacement-fd)
    * [4.2 Inverse Displacement (ID)](#42-inverse-displacement-id)
      * [4.2.1 Inverse of Transformation Equations](#421-inverse-of-transformation-equations)
      * [4.2.2 Square and Add Trigonometric Equations](#422-square-and-add-trigonometric-equations)
      * [4.2.3 Trigonometric Identities to Simplify Coupled Terms](#423-trigonometric-identities-to-simplify-coupled-terms)
      * [4.2.4 Tangent Half-Angle Substitution](#424-tangent-half-angle-substitution)
      * [4.2.5 Geometric Substitution or Projection](#425-geometric-substitution-or-projection)
    * [4.3 Forward Velocity (FV)](#43-forward-velocity-fv)
    * [4.4 Inverse Velocity (IV)](#44-inverse-velocity-iv)
      * [4.4.1 Jacobian Matrix](#441-jacobian-matrix)
    * [4.5 Forward Force (FF)](#45-forward-force-ff)
    * [4.6 Inverse Force (IF)](#46-inverse-force-if)
  * [5 Rigid Body Dynamics](#5-rigid-body-dynamics)
    * [5.1 The Inertia Tensor](#51-the-inertia-tensor)
    * [5.2 Manipulator Dynamic Equations](#52-manipulator-dynamic-equations)
    * [5.3 Newton-Euler (Force Balance) Approach](#53-newton-euler-force-balance-approach)
      * [5.3.1 Forward Recursion: Compute Velocities and Accelerations](#531-forward-recursion-compute-velocities-and-accelerations)
      * [5.3.2 Backward Recursion: Compute Forces and Torques](#532-backward-recursion-compute-forces-and-torques)
    * [5.4 Lagrangian (Energy-Based) Approach](#54-lagrangian-energy-based-approach)
  * [6 Product of Exponentials (POE) Formulation](#6-product-of-exponentials-poe-formulation)
    * [6.1 Core Idea](#61-core-idea)
    * [6.2 Twist Representation](#62-twist-representation)
    * [6.3 Exponential Map to the Homogenous Transform](#63-exponential-map-to-the-homogenous-transform)
    * [6.4 Forward Kinematics Summary](#64-forward-kinematics-summary)
    * [6.5 Body Frame Form](#65-body-frame-form)
    * [6.6 Jacobian Matrix](#66-jacobian-matrix)
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
write: ${}^0T_2 = {}^0T_1 {}^1T_2$

This appears as left-to-right chaining, mirroring the physical kinematic chain
(base to joint 1 to joint 2). But when applied to a vector:
$$\mathbf{p}_0 = {}^0T_1 \big( {}^1T_2 \mathbf{p}_2 \big)$$

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

This forms the D&H table:

| $F_{i-1}$ | $a_{i-1}$ | $\alpha_{i-1}$ | $d_i$ | $\theta_i$ | $F_i$ |
|:---------:|:---------:|:--------------:|:-----:|:----------:|:-----:|
|    ...    |    ...    |      ...       |  ...  |    ...     |  ...  |

### 3.1 Zero Displacement Condition

In the D&H convention, the zero displacement condition specifies that each joint
variable ($\theta_i$ for revolute or $d_i$ for prismatic) is measured relative
to a reference configuration where the parameter equals zero. This anchors the
D&H parameters to the robot's physical geometry.

### 3.2 Homogenous Transform Representation

As each row in the D&H table now represents a transform, each row can be
expressed as a homogenous transformation matrix:

$$
{}^{i-1}T_i =
\begin{bmatrix}
\cos\theta_i & -\sin\theta_i\cos\alpha_{i-1} & \sin\theta_i\sin\alpha_{i-1} & a_{i-1}\cos\theta_i \\
\sin\theta_i & \cos\theta_i\cos\alpha_{i-1} & -\cos\theta_i\sin\alpha_{i-1} & a_{i-1}\sin\theta_i \\
0 & \sin\alpha_{i-1} & \cos\alpha_{i-1} & d_i \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

---

## 4 Basic Problems of Manipulators with (D&H) Convention

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
| $\sin \theta = a$ and $\cos \theta = b$                             | $\theta = \mathrm{Atan2}\left(a, b \right)$                                              |
| $\sin\theta = a$                                                    | $\theta = \mathrm{Atan2}\left(a, \pm\sqrt{1 - a^2}\right)$                               |
| $\cos\theta = b$                                                    | $\theta = \pm \mathrm{Atan2}\left(\sqrt{1 - b^2}, b\right)$                              |
| $a\cos\theta + b\sin\theta = 0$                                     | $\theta = \mathrm{Atan2}(a, -b) = \mathrm{Atan2}(-a, b)$                                 |
| $a\cos\theta + b\sin\theta = c$                                     | $\theta = \mathrm{Atan2}(b, a) \pm \mathrm{Atan2}\left(\sqrt{a^2 + b^2 - c^2}, c\right)$ |
| $a\cos\theta - b\sin\theta = c$ and $a\sin\theta + b\cos\theta = d$ | $\theta = \mathrm{Atan2}(ad - bc, ac + bd)$                                              |

Useful identities:

| Identity                 | Use Case        | Formula                                       |
|--------------------------|-----------------|-----------------------------------------------|
| Sine Difference Identity | Parallel Joints | $\sin(A - B) = \sin A \cos B - \cos A \sin B$ |

### 4.1 Forward Displacement (FD)

$$X = f(\theta)$$

Also called Forward Kinematics, this problem determines the position and
orientation of the end-effector in Cartesian space given the known joint
variables (angles for revolute joints, displacements for prismatic joints).

### 4.2 Inverse Displacement (ID)

$$\theta = f^{-1}(X)$$

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

If you have a homogeneous transformation matrix ${}^{A}_{B}T$ that describes the
pose of **frame B** relative to **frame A**, then the transformation of **frame
A** relative to **frame B** is simply its inverse:

$$
{}^{0}T_{1} = \begin{bmatrix} R & p \\
0 & 1 \end{bmatrix}
$$

where:

- $R \in SO(3)$: rotation matrix.
- $p \in \mathbb{R}^3$: position of the origin of frame B expressed in frame A.

The inverse is given by:

$$
{}^{1}T_{0} = ({}^{0}T_{1})^{-1} = \begin{bmatrix} R^T & -R^T p \\
0 & 1 \end{bmatrix}
$$

- $R^{-1} = R^T$ (rotation matrices are orthogonal).
- The translation component reverses and rotates back into the new frame.

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

$$\dot{X} = J(\theta) \dot{\theta}$$

Given the joint velocities, determine the end-effector's linear and angular
velocities.

### 4.4 Inverse Velocity (IV)

$$\dot{\theta} = J^{-1}(\theta) \dot{X}$$

Given the desired end-effector velocity, determine the required joint
velocities.

#### 4.4.1 Jacobian Matrix

The Jacobian matrix relates the speed of the end effector (vector) with the
joint angle rates (vector).

$$\dot{X} = \begin{bmatrix} v \\
\omega \end{bmatrix} = J(\theta) \dot{\theta}$$

Thus, the Jacobian can be solved by taking the derivative of the homogenous
transform, effectively relating the derivative of the position vector and
derivative of the joint angles:

$$
J(\theta) = \begin{bmatrix} \frac{\partial x}{\partial \theta_1} & \cdots & \frac{\partial x}{\partial \theta_n} \\
\frac{\partial y}{\partial \theta_1} & \cdots & \frac{\partial y}{\partial \theta_n} \\
\frac{\partial z}{\partial \theta_1} & \cdots & \frac{\partial z}{\partial \theta_n} \\
\frac{\partial \phi}{\partial \theta_1} & \cdots & \frac{\partial \phi}{\partial \theta_n} \\
\frac{\partial \theta}{\partial \theta_1} & \cdots & \frac{\partial \theta}{\partial \theta_n} \\
\frac{\partial \psi}{\partial \theta_1} & \cdots & \frac{\partial \psi}{\partial \theta_n} \end{bmatrix}
$$

### 4.5 Forward Force (FF)

$$F = J^{-T}(\theta) \tau$$

Determine the forces and torques at the end-effector (in Cartesian space) that
result from known joint torques.

### 4.6 Inverse Force (IF)

$$\tau = J^{T}(\theta) F$$

Determine the joint torques required to generate a desired end-effector force.

---

## 5 Rigid Body Dynamics

### 5.1 The Inertia Tensor

The **moment of inertia tensor** expresses how mass is distributed relative to
an axis of rotation. It determines the resistance of a rigid body to angular
acceleration about any axis through a point (usually the center of mass).

$$
\mathbf{I} =
\begin{bmatrix}
I_{xx} & -I_{xy} & -I_{xz} \\
-I_{yx} & I_{yy} & -I_{yz} \\
-I_{zx} & -I_{zy} & I_{zz}
\end{bmatrix}
= \begin{bmatrix}
\frac{1}{3} m (l^2 + h^2) & -\frac{1}{4} m w l & -\frac{1}{4} m w h \\
-\frac{1}{4} m w l & \frac{1}{3} m (w^2 + h^2) & -\frac{1}{4} m l h \\
-\frac{1}{4} m w h & -\frac{1}{4} m l h & \frac{1}{3} m (l^2 + w^2)
\end{bmatrix}
$$

This is a **symmetric 3 x 3 matrix** (since $I_{xy} = I_{yx}$, etc.),
representing how the object's mass is distributed in 3D.

Each component is defined by integrating over the body's volume $B_i$:

Mass moment of inertia:

$$
I_{xx} = \int_{B_i} (y^2 + z^2) dV
\quad\quad
I_{yy} = \int_{B_i} (z^2 + x^2) dV
\quad\quad
I_{zz} = \int_{B_i} (x^2 + y^2) dV
$$

Off-diagonal terms (products of inertia):

$$
I_{xy} = I_{yx} = \int_{B_i} xy dV
\quad\quad
I_{xz} = I_{zx} = \int_{B_i} xz dV
\quad\quad
I_{yz} = I_{zy} = \int_{B_i} yz dV
$$

- **Diagonal terms**: ($I_{xx}, I_{yy}, I_{zz}$) represent resistance to
  rotation about the x, y, and z axes, respectively.
- **Off-diagonal terms**: (e.g. $I_{xy}$ ) couple rotations between axes and
  describe how mass is distributed off the principal axes.
- The **tensor form**: allows expressing angular momentum and torque compactly:
  $$\mathbf{L} = \mathbf{I} \omega$$
  where $\mathbf{L}$ is angular momentum and $\omega$ is angular velocity.

Key Properties:

- **Symmetric:** $I_{ij} = I_{ji}$
- **Positive definite:** all principal moments $> 0$ for real mass distributions
- **Diagonalizable:** there exist **principal axes** where the inertia tensor
  becomes diagonal, simplifying rotation analysis.

The **parallel axis theorem** allows us to find the moment of inertia about any
axis
that is *parallel* to one passing through the **center of mass (C.M.)**.

Mass moment of inertia:

$$
I_{xx}^A = I_{xx}^C + m (y_c^2 + z_c^2)
\quad\quad
I_{yy}^A = I_{yy}^C + m (x_c^2 + z_c^2)
\quad\quad
I_{zz}^A = I_{zz}^C + m (x_c^2 + y_c^2)
$$

Off-diagonal terms (products of inertia):

$$
I_{xy}^A = I_{xy}^C + m x_c y_c
\quad\quad
I_{xz}^A = I_{xz}^C + m x_c z_c
\quad\quad
I_{yz}^A = I_{yz}^C + m y_c z_c
$$

where:

- $x_c$, $y_c$, $z_c$: The coordinates of the centroid (center of mass) measured
  from the new reference point. For a uniform rectangular solid where the
  reference is at one corner, these correspond to the coordinates of the
  geometric center:
    - $x_c = \frac{l}{2}$, $y_c = \frac{w}{2}$, $z_c = \frac{h}{2}$.

### 5.2 Manipulator Dynamic Equations

Manipulator dynamics equations have the general form:

$$M(\theta),\ddot{\theta} + V(\theta, \dot{\theta}) + G(\theta) = \phi$$

where:

- $M(\theta)\ddot{\theta}$: The **inertial torque** term, represents the torque
  required to produce the desired joint accelerations given the current
  configuration (depends on the manipulator's mass distribution).
- $V(\theta, \dot{\theta})$: The **Coriolis and centrifugal torque** term,
  accounts for dynamic coupling effects between joints that arise when multiple
  joints move simultaneously.
- $G(\theta)$: The **gravitational torque** term, represents the torque needed
  to hold the manipulator stationary against gravity.
- $\phi$: The **generalized actuator torque (or force) vector**, the total
  effort supplied by the actuators, in other words the control input that
  balances all dynamic effects.

### 5.3 Newton-Euler (Force Balance) Approach

The Newton-Euler method derives the equations of motion by applying
**Newton's second law** (linear and rotational) to each link of the manipulator
separately.

For each link $i$:

$$\mathbf{F}_i = m_i \mathbf{a}_{c_i}$$

$$\tau_i = I_i \alpha_i + \omega_i \times (I_i \omega_i)$$

where

- $\mathbf{F}_i$: The net external force acting on link $i$.
- $m_i$: The mass of link $i$.
- $\mathbf{a}_{c_i}$: The acceleration of the link's center of mass.
- $\tau_i$: The net torque acting on link $i$.
- $\alpha_i$: The angular acceleration.
- $\omega_i$: The angular velocity.
- $I_i$: The inertia tensor about the center of mass.

The Recursive Newton-Euler Algorithm proceeds **recursively** along the
manipulator chain:

#### 5.3.1 Forward Recursion: Compute Velocities and Accelerations

Starting from the base (link 0):

$$
\omega_i = {}^{i}R_{i-1}\, \omega_{i-1} + \dot{\theta}_i\, z_i
$$

$$
\alpha_i = {}^{i}R_{i-1}\, \alpha_{i-1} + \ddot{\theta}_i\, z_i + \omega_i \times (\dot{\theta}_i\, z_i)
$$

Then compute the linear acceleration of each link's center of mass:

$$
\mathbf{a}_{c_i} = {}^{i}R_{i-1}\left(\mathbf{a}_{c_{i-1}} + \alpha_{i-1}\times \mathbf{r}_{i-1,i} + \omega_{i-1}\times(\omega_{i-1}\times \mathbf{r}_{i-1,i})\right)
$$

#### 5.3.2 Backward Recursion: Compute Forces and Torques

Starting from the end effector (link $n$):

$$\mathbf{f}_i = {}^{i}R_{i+1}\, \mathbf{f}_{i+1} + m_i \mathbf{a}_{c_i}$$

$$\tau_i = {}^{i}R_{i+1}\, \tau_{i+1} + \mathbf{r}_{c_i} \times (m_i \mathbf{a}_{c_i}) + \mathbf{r}_{i,i+1} \times ({}^{i}R_{i+1}\, \mathbf{f}_{i+1}) + \mathbf{I}_i \alpha_i + \omega_i \times (\mathbf{I}_i \omega_i)$$

Finally, the **joint torque** at joint $i$ is:

$$\phi_i = \tau_i^T z_i$$

This gives the torque/force required at each joint to produce the desired
motion.

---

### 5.4 Lagrangian (Energy-Based) Approach

The Lagrangian approach uses an **energy formulation** (kinetic - potential
energy).

$$\mathcal{L}(\theta,\dot{\theta}) = K(\theta,\dot{\theta}) - U(\theta)$$

where:

- $K$: The kinematic energy.
- $U$: The potential energy.

The Lagrangian equation of motion is given by:

$$\tau = \frac{d}{dt} \frac{\partial L}{\partial \dot{\theta}} - \frac{\partial L}{\partial \theta}$$
$$\tau_i = \frac{d}{dt} \frac{\partial L}{\partial \dot{\theta}_i} - \frac{\partial L}{\partial \theta_i}$$

where:

- $\tau$: The joint forces and torques.

---

## 6 Product of Exponentials (POE) Formulation

The **Product of Exponentials (POE)** approach provides a compact and
coordinate-free method to describe the forward kinematics of a robotic
manipulator using the tools of **screw theory** and **matrix exponentials**.

- Note: $SE(3)$ stands for the Special Euclidean group in 3D, 4x4 matrix of
  rotations and translations.

The advantages are:

- Coordinate-free, compact, and elegant.
- Compatible with modern Lie group formulations.
- Easily differentiable for velocity and Jacobian computations.
- Suitable for numerical methods and control formulations.

### 6.1 Core Idea

In the POE formulation, the pose of the end-effector is expressed as a product
of exponentials of **twists**, each representing a joint's motion, applied to
the **home configuration** of the robot.

A **matrix exponential** generalizes the scalar exponential function to
matrices. It provides a powerful way to represent continuous transformations
such as rotations, translations, and dynamic system evolution.

For any square matrix $A$, the exponential is defined by the power series:

$$e^{A} = I + A + \frac{A^2}{2!} + \frac{A^3}{3!} + \cdots$$

This series always converges. Each term represents higher-order effects of the
transformation encoded by $A$.

- $A$ often represents an **infinitesimal generator** of motion (e.g., angular
  velocity, twist, or system dynamics).
- $e^{A}$ gives the **finite transformation** obtained by "integrating" that
  motion.

In other words, $e^{A}$ converts **local motion** to **global transformations**.

If the manipulator has $n$ joints, then the forward kinematics are given by:

$$T(q) = e^{[S_1]q_1} e^{[S_2]q_2} \cdots e^{[S_n]q_n} M$$

where:

- $T(q) \in SE(3)$: Homogeneous transformation of the end-effector frame
  relative to the base.
- $M \in SE(3)$: Home configuration (pose when all joint variables are zero).
- $[S_i] \in \mathfrak{se}(3)$: Twist of joint $i$ expressed in the base frame.
- $q_i$: Joint variable (angle for revolute, displacement for prismatic).

### 6.2 Twist Representation

Each **twist** $S_i$ represents the instantaneous motion of a joint as a 6D
vector:

$$
S_i = \begin{bmatrix} \omega_i \\
v_i \end{bmatrix}
$$

where:

- $\omega_i$: Angular velocity vector (axis of rotation).
- $v_i = -\omega_i \times q_i$ for revolute joints, with $q_i$ being a point on
  the axis.

The twist is represented as a **4 x 4 matrix** in the Lie
algebra $\mathfrak{se}(3)$:

$$
[S_i] = \begin{bmatrix} [\omega_i] & v_i \\
0 & 0 \end{bmatrix}
$$

where $[\omega_i]$ is the skew-symmetric matrix:

$$
[\omega_i] = \begin{bmatrix} 0 & -\omega_{i,z} & \omega_{i,y} \\
\omega_{i,z} & 0 & -\omega_{i,x} \\
-\omega_{i,y} & \omega_{i,x} & 0 \end{bmatrix}
$$

### 6.3 Exponential Map to the Homogenous Transform

The exponential of a twist produces a rigid-body transformation in $SE(3)$:

$$
e^{[S]q} = \begin{bmatrix} e^{[\omega]q} & (I - e^{[\omega]q})(\omega \times v) + \omega \omega^T v q \\
0 & 1 \end{bmatrix}
$$

If the joint is **prismatic**, $\omega = 0$, and the exponential simplifies
to:

$$
e^{[S]q} = \begin{bmatrix} I & v q \\
0 & 1 \end{bmatrix}
$$

The rotation matrix corresponding to $e^{[\omega]q}$ simplifies to:

$$e^{[\omega]q} = I + \sin(q)[\omega] + (1 - \cos(q))[\omega]^2$$

And thus the final power exponent is computed as:

$$
e^{[S]q} = \begin{bmatrix} R(q) & p(q) \\
0 & 1 \end{bmatrix}
$$

### 6.4 Forward Kinematics Summary

Combining all exponentials gives the end-effector transformation:

$$T(q) = e^{[S_1]q_1} e^{[S_2]q_2} \cdots e^{[S_n]q_n} M$$

This formulation naturally handles both revolute and prismatic joints and
provides a smooth mapping from joint space to Cartesian space.

### 6.5 Body Frame Form

Alternatively, the POE can be expressed in the **body frame** (expressed at the
end-effector):

$$T(q) = M e^{[S_1^B]q_1} e^{[S_2^B]q_2} \cdots e^{[S_n^B]q_n}$$

where $[S_i^B]$ are twists expressed in the body frame rather than the space
frame.

### 6.6 Jacobian Matrix

Recall the Jacobian matrix relates the speed of the end effector (vector) with
the joint angle rates (vector).

Recall given the twist vector:

$$
\xi = \begin{bmatrix} \omega \\
v \end{bmatrix}
$$

$$v = -\omega \times q$$

With the POE method the Jacobianis represented as:

$$g(\theta) = e^{\hat{\xi}_1 \theta_1} e^{\hat{\xi}_2 \theta_2} \cdots e^{\hat{\xi}_n \theta_n} g_0$$

$$
J_s(\theta) = \begin{bmatrix}
\xi_1 &
\operatorname{Ad}\big(e^{\hat{\xi}_1\theta_1}\big)\xi_2 &
\operatorname{Ad}\big(e^{\hat{\xi}_1\theta_1}e^{\hat{\xi}_2\theta_2}\big)\xi_3 &
\cdots &
\operatorname{Ad}\big(e^{\hat{\xi}_1\theta_1}\cdots e^{\hat{\xi}_{n-1}\theta_{n-1}}\big)\xi_n
\end{bmatrix}
$$

Or alternatively:

$$
J_b(\theta) = \begin{bmatrix}
\operatorname{Ad}\!\left(e^{-\hat{\xi}_n\theta_n} \cdots e^{-\hat{\xi}_2\theta_2}\right)\xi_1 &
\operatorname{Ad}\!\left(e^{-\hat{\xi}_n\theta_n} \cdots e^{-\hat{\xi}_3\theta_3}\right)\xi_2 &
\cdots &
\xi_n
\end{bmatrix}
$$

Where the adjoint operator transforms screw axes through space, rotating the
angular component and rotating/shifts linear component.

$$
\operatorname{Ad}(g) = \begin{bmatrix} R & 0 \\
[p]_\times R & R \end{bmatrix}
\quad
\text{for } g = \begin{bmatrix} R & p \\ 0 & 1 \end{bmatrix}
$$

where:

- $R$: The 3x3 rotation matrix extracted from the homogeneous transform.
- $p$: The 3 element translation vector extracted from the homogeneous
  transform.
