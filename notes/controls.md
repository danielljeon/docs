# Control Systems

---

<details markdown="1">
  <summary>Table of Contents</summary>

<!-- TOC -->
* [Control Systems](#control-systems)
  * [1 Resources](#1-resources)
  * [2 Intro to Control Theory](#2-intro-to-control-theory)
  * [3 Time and Frequency Domain](#3-time-and-frequency-domain)
  * [4 Linear Time Invariant Systems](#4-linear-time-invariant-systems)
  * [5 Transfer Function](#5-transfer-function)
  * [6 Fourier Transform](#6-fourier-transform)
  * [7 Block Diagrams](#7-block-diagrams)
  * [8 Step Response](#8-step-response)
  * [9 Nichols Chart, Nyquist Plot, and Bode Plot](#9-nichols-chart-nyquist-plot-and-bode-plot)
    * [9.1 Bode Plot](#91-bode-plot)
  * [10 Stability Phase and Gain Margins](#10-stability-phase-and-gain-margins)
  * [11 Routh-Hurwitz Criterion](#11-routh-hurwitz-criterion)
  * [12 Root Locus Method](#12-root-locus-method)
  * [13 Nyquist Stability Criterion](#13-nyquist-stability-criterion)
  * [14 PID Controllers](#14-pid-controllers)
  * [15 PID Implementation in Software](#15-pid-implementation-in-software)
    * [15.1 Discrete Time Difference Equation](#151-discrete-time-difference-equation)
      * [15.1.1 The Tustin Transform](#1511-the-tustin-transform)
      * [15.1.2 Discrete Time Difference Equation PID](#1512-discrete-time-difference-equation-pid)
    * [15.2 Practical Considerations](#152-practical-considerations)
      * [15.2.1 Derivative Amplifies High Frequency Noise](#1521-derivative-amplifies-high-frequency-noise)
      * [15.2.2 Derivative "Kick" During Set-Point Changes](#1522-derivative-kick-during-set-point-changes)
      * [15.2.3 Integrator Windup](#1523-integrator-windup)
      * [15.2.4 System Amplitude Limitations](#1524-system-amplitude-limitations)
      * [15.2.5 Sample Time Implications](#1525-sample-time-implications)
      * [15.2.6 Noise](#1526-noise)
  * [16 PID Tuning](#16-pid-tuning)
  * [17 Time Delays in Control Systems](#17-time-delays-in-control-systems)
  * [18 Gain Scheduling](#18-gain-scheduling)
  * [19 State-Space](#19-state-space)
<!-- TOC -->

</details>

---

## 1 Resources

YouTube Playlist, Brian
Douglas: [Classical Control Theory](https://youtube.com/playlist?list=PLUMWjy5jgHK1NC52DXXrriwihVrYZKqjk)

YouTube Playlist,
MATLAB: [Control Systems in Practice](https://youtube.com/playlist?list=PLn8PRpmsu08pFBqgd_6Bi7msgkWFKL33b)

---

## 2 Intro to Control Theory

YouTube,
MATLAB: [Everything You Need to Know About Control Theory](https://youtu.be/lBC1nEq0_nk)

Some (hopefully) motivation:

- YouTube,
  MATLAB: [What Control Systems Engineers Do | Control Systems in Practice](https://youtu.be/ApMz1-MK9IQ)
- YouTube, Brian
  Douglas: [Why Learn Control Theory](https://youtu.be/oBc_BHxw78s)

---

## 3 Time and Frequency Domain

YouTube, Brian
Douglas: [Control Systems Lectures - Time and Frequency Domain](https://youtu.be/noycLIZbK_k)

---

## 4 Linear Time Invariant Systems

YouTube, Brian
Douglas: [Control Systems Lectures - LTI Systems](https://youtu.be/3eDDTFcSC_Y)

---

## 5 Transfer Function

YouTube,
MATLAB: [What are Transfer Functions? | Control Systems in Practice](https://youtu.be/2Xl7--Df3g8)

---

## 6 Fourier Transform

YouTube, Brian
Douglas: [Introduction to the Fourier Transform (Part 1)](https://youtu.be/1JnayXHhjlg)

YouTube, Brian
Douglas: [Introduction to the Fourier Transform (Part 2)](https://youtu.be/kKu6JDqNma8)

---

## 7 Block Diagrams

---

## 8 Step Response

YouTube,
MATLAB: [The Step Response | Control Systems in Practice](https://youtu.be/USH75nuHV6w)

---

## 9 Nichols Chart, Nyquist Plot, and Bode Plot

YouTube,
MATLAB: [Nichols Chart, Nyquist Plot, and Bode Plot | Control Systems in Practice](https://youtu.be/QAfk8TuOM68)

### 9.1 Bode Plot

YouTube, Brian Douglas:

1. [Control System Lectures - Bode Plots, Introduction](https://youtu.be/_eh1conN6YM)
2. [Bode Plots by Hand: Real Constants](https://youtu.be/CSAp9ooQRT0)
3. [Bode Plots by Hand: Poles and Zeros at the Origin](https://youtu.be/E6R2XUEyRy0)
4. [Bode Plots by Hand: Real Poles or Zeros](https://youtu.be/O2Cw_4zd-aU)
5. [Bode Plots by Hand: Complex Poles or Zeros](https://youtu.be/GIlx9Yu__y8)
6. [CORRECTION: Bode Plots by Hand: Complex Poles or Zeros](https://youtu.be/4d4WJdU61Js)

---

## 10 Stability Phase and Gain Margins

YouTube, Brian
Douglas: [Gain and Phase Margins Explained!](https://youtu.be/ThoA4amCAX4)

---

## 11 Routh-Hurwitz Criterion

YouTube, Brian Douglas:

1. [Routh-Hurwitz Criterion, An Introduction](https://youtu.be/WBCZBOB3LCA)
2. [Routh-Hurwitz Criterion, Special Cases](https://youtu.be/oMmUPvn6lP8)
3. [Routh-Hurwitz Criterion, Beyond Stability](https://youtu.be/wGC5C_7Yy-s)

---

## 12 Root Locus Method

YouTube, Brian Douglas:

1. [The Root Locus Method - Introduction](https://youtu.be/CRvVDoQJjYI)
2. [Sketching Root Locus Part 1](https://youtu.be/eTVddYCeiKI)
3. [Sketching Root Locus Part 2](https://youtu.be/jb_FiP5tKig)

- [Root Locus Plot: Common Questions and Answers](https://youtu.be/WLBszzT0jp4)
- [Gain a better understanding of Root Locus Plots using Matlab](https://youtu.be/pG3_b7wuweQ)

---

## 13 Nyquist Stability Criterion

YouTube, Brian Douglas:

1. [Nyquist Stability Criterion, Part 1](https://youtu.be/sof3meN96MA)
2. [Nyquist Stability Criterion, Part 2](https://youtu.be/tsgOstfoNhk)

---

## 14 PID Controllers

YouTube,
MATLAB: [What Is PID Control? | Understanding PID Control, Part 1](https://youtu.be/wkfEZmsQqiA)

---

## 15 PID Implementation in Software

YouTube, Phil's
Lab: [PID Controller Implementation in Software - Phil's Lab #6](https://youtu.be/zOByx3Izf5U)

- Referenced code: [github.com/pms67/PID](https://github.com/pms67/PID).

### 15.1 Discrete Time Difference Equation

Objective: s-domain → z-domain → difference equation.

|                                                           Continuous time (s-domain)                                                            |
|:-----------------------------------------------------------------------------------------------------------------------------------------------:|
| $$G \left( s \right) = \frac{ \bar{u} \left( s \right) }{ \bar{e} \left( s \right) } = K_{P} + K_{I} \frac{1}{s} + K_{D} \frac{s}{s \tau + 1}$$ |

#### 15.1.1 The Tustin Transform

To convert from a continuous time (s-domain) equation to discrete time (
z-domain), we apply the **Tustin transform (aka bilinear transform)**.

Substitute all instances of $s$ with the following:

$$s \rightarrow \frac{2}{T} \frac{z - 1}{z + 1}$$

- $T$ is the discrete sampling time in seconds.

Converting to a difference equation:

- Recall that any multiplication with $z$ is a time shift of 1 sample.

$$ \bar{y} \left( z \right) = \bar{x} \left( z \right) \cdot z^{-1} \rightarrow y \left[ n \right] = x \left[ n - 1 \right] $$

- $n$ is the (independent) current time sample value.

#### 15.1.2 Discrete Time Difference Equation PID

After applying the tustin transform and on the continuous time PID equations we
get the following:

| $$u \left[ n \right] = p \left[ n \right] + i \left[ n \right] + d \left[ n \right]$$                                                                                                     |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Proportional: $$p \left[ n \right] = K_{P} \cdot e \left[ n \right]$$                                                                                                                     |
| Integral: $$i \left[ n \right] = \frac{ K_{I} T }{ 2 } \left( e \left[ n \right] + e \left[ n - 1 \right] \right) + i \left[ n - 1 \right]$$                                              |
| Derivative: $$d \left[ n \right] = \frac{ 2 K_{D} }{ 2 \tau + T } \left( e \left[ n \right] + e \left[ n - 1 \right] \right) + \frac{ 2 \tau - T }{ 2 \tau + T } d \left[ n - 1 \right]$$ |

- Current **proportional** value is the product of the gain and current error.
- Current **integral** term is a product with the summation of the current and
  previous term and a sum with the previous integral term.
- Current **derivative** term is a product with the summation of the current and
  previous term and a sum with the previous derivative term.

```c
// Calculate current error.
float error = setpoint - measurement;

// Proportional.
float proportional = pid->kp * error;

// Integrator.
pid->integrator = pid->integrator + 0.5f * pid->ki * pid->t_sample * (error + pid->prev_error);

// Differentiator.
pid->differentiator = (2.0f * pid->kd * (error - pid->prev_error)
                      + (2.0f * pid->tau - pid->t_sample) * pid->differentiator)
                      / (2.0f * pid->tau + pid->t_sample);

// Compute output.
pid->output = proportional + pid->integrator + pid->differentiator;

// Save error for next calculation.
pid->prev_error = error;
```

### 15.2 Practical Considerations

#### 15.2.1 Derivative Amplifies High Frequency Noise

A **derivative filter** can to mitigate high frequency noise.

However, there will likely be the adverse effect of reduced derivative
responsiveness.

#### 15.2.2 Derivative "Kick" During Set-Point Changes

Quick step change in the set-point can can result in large impulse changes. This
can be mitigated using **derivative-on-measurement**. Instead of taking the
derivative of the error signal, the derivative of the fed-back sensor
measurement is used, eliminating the impulse "kick".

$$d \left[ n \right] = \frac{ 2 K_{D} }{ 2 \tau + T } \left( measurement + prev \ measurement \right) + \frac{ 2 \tau - T }{ 2 \tau + T } d \left[ n - 1 \right]$$

```c
// Differentiator.

// Derivative-on-measurement band limiting, negative sign in front of equation.
pid->differentiator = -(2.0f * pid->kd * (measurement - pid->prev_measurement)
                      + (2.0f * pid->tau - pid->t_sample) * pid->differentiator)
                      / (2.0f * pid->tau + pid->t_sample);

// Save measurement for next calculation.
pid->prev_measurement = measurement;
```

#### 15.2.3 Integrator Windup

**Integrator windup** is when the output is saturated or reaches extremely large
values due to system maximum limitations. For example, a motor can physically
only spin so fast, without any limits the integrator value can compound
continuously. Setting some form of **anti-windup** mechanism to clamp a limit
will prevent the integrator from reaching extreme values.

```c
// Anti-windup integrator clamping.
if (pid->integrator > pid->integral_max) {
  pid->integrator = pid->integral_max;

} else if (pid->integrator < pid->integral_min) {
  pid->integrator = pid->integral_min;
}
```

For more information:

- YouTube,
  MATLAB: [Anti-windup for PID control | Understanding PID Control, Part 2](https://youtu.be/NVLXCwc8HzM)

#### 15.2.4 System Amplitude Limitations

Somewhat similar to integrator windup, systems are physically limited. Without
proper consideration, a control system can incorrectly command values which are
not reachable by the system. For example, a stepper motor controller might have
a minimum step or maximum speed. If a command too small or too large is given,
the system will not be able to respond correctly. To resolve this, a
**controller output clamp** can be implemented to prevent exceeding
physical/safety bounds.

```c
// Apply output limits.
if (pid->output > pid->limit_max) {
  pid->output = pid->limit_max;

} else if (pid->output < pid->limit_min) {
  pid->output = pid->limit_min;
}
```

#### 15.2.5 Sample Time Implications

Choosing a sample time is crucial to ensuring a control system is actually able
to react to changes. A general baseline could be for the controller to be 10
times more responsive/reactive than that of the system changes.

$$T_{controller} < \frac{ T_{system} }{ 10 }$$

#### 15.2.6 Noise

- YouTube,
  MATLAB: [Noise Filtering in PID Control | Understanding PID Control, Part 3](https://youtu.be/7dUVdrs1e18)

---

## 16 PID Tuning

YouTube,
MATLAB: [A PID Tuning Guide | Understanding PID Control, Part 4](https://youtu.be/sFOEsA0Irjs)

---

## 17 Time Delays in Control Systems

YouTube,
MATLAB: [Why Time Delay Matters | Control Systems in Practice](https://youtu.be/wouhREkqZa0)

---

## 18 Gain Scheduling

YouTube,
MATLAB: [What Is Gain Scheduling? | Control Systems in Practice](https://youtu.be/YiUjAV1bhKs)

---

## 19 State-Space

YouTube, MATLAB:

1. [Introduction to State-Space Equations | State Space, Part 1](https://youtu.be/hpeKrMG-WP0)
2. [What is Pole Placement (Full State Feedback) | State Space, Part 2](https://youtu.be/FXSpHy8LvmY)
3. [A Conceptual Approach to Controllability and Observability | State Space, Part 3](https://youtu.be/BYvTEfNAi38)
4. [What Is Linear Quadratic Regulator (LQR) Optimal Control? | State Space, Part 4](https://youtu.be/E_RDCFOlJx4)
