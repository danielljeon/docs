# Engineering Basics

---

<details markdown="1">
  <summary>Table of Contents</summary>

<!-- TOC -->
* [Engineering Basics](#engineering-basics)
  * [1 Derivatives](#1-derivatives)
    * [1.1 Rules](#11-rules)
    * [1.2 Common](#12-common)
    * [1.3 Trigonometric](#13-trigonometric)
  * [2 Integrals](#2-integrals)
    * [2.1 Indefinite Integral Rules](#21-indefinite-integral-rules)
    * [2.2 Common](#22-common)
    * [2.3 Trigonometric](#23-trigonometric)
  * [3 Laplace Transform Pairs](#3-laplace-transform-pairs)
  * [4 Fourier Analysis](#4-fourier-analysis)
    * [4.1 Fourier Series](#41-fourier-series)
    * [4.2 Fourier Transform](#42-fourier-transform)
      * [4.2.1 Discrete Fourier Transform (DFT)](#421-discrete-fourier-transform-dft)
<!-- TOC -->

</details>

---

## 1 Derivatives

### 1.1 Rules

Power rule:
$$\frac{d}{dx}\left(x^a\right)=a\cdot x^{a-1}$$
Derivative of a constant:
$$\frac{d}{dx}\left(a\right)=0$$
Sum difference rule:
$$\left(f\pm g\right)'=f'\pm g'$$
Constant out:
$$\left(a\cdot f\right)'=a\cdot f'$$
Product rule:
$$\left(f\cdot g\right)'=f'\cdot g+f\cdot g'$$
Quotient rule:
$$\left(\frac{f}{g}\right)'=\frac{f'\cdot g-g'\cdot f}{g^2}$$
Chain rule:
$$\frac{df\left(u\right)}{dx}=\frac{df}{du}\cdot \frac{du}{dx}$$

### 1.2 Common

$$\frac{d}{dx}\left(\ln \left(x\right)\right)=x^{-1}$$
$$\frac{d}{dx}\left(\ln \left(\left|x\right|\right)\right)=x^{-1}$$
$$\frac{d}{dx}\left(e^x\right)=e^x$$
$$\frac{d}{dx}\left(\log _a\left(x\right)\right)=\frac{1}{x\ln \left(a\right)}$$
$$\frac{d}{dx}\left(\sin \left(x\right)\right)=\cos \left(x\right)$$
$$\frac{d}{dx}\left(\cos \left(x\right)\right)=-\sin \left(x\right)$$
$$\frac{d}{dx}\left(\tan \left(x\right)\right)=\sec ^2\left(x\right)$$

### 1.3 Trigonometric

$$\frac{d}{dx}\left(\sec \left(x\right)\right)=\frac{\tan \left(x\right)}{\cos \left(x\right)}$$
$$\frac{d}{dx}\left(\csc \left(x\right)\right)=\frac{-\cot \left(x\right)}{\sin \left(x\right)}$$
$$\frac{d}{dx}\left(\cot \left(x\right)\right)=-\frac{1}{\sin ^2\left(x\right)}$$

---

## 2 Integrals

### 2.1 Indefinite Integral Rules

Power rule:
$$\int x^adx=\frac{x^{a+1}}{a+1}, \ \quad \ a\ne -1$$
Integration by parts:
$$\int \ uv'=uv-\int \ u'v$$
Integral of a constant:
$$\int f\left(a\right)dx=x\cdot f\left(a\right)$$
Take the constant out:
$$\int a\cdot f\left(x\right)dx=a\cdot \int f\left(x\right)dx$$
Sum rule:
$$\int f\left(x\right)\pm g\left(x\right)dx=\int f\left(x\right)dx\pm \int g\left(x\right)dx$$
Add a constant to the solution:
$$\mathrm{If \ }\frac{dF\left(x\right)}{dx}=f\left(x\right)\mathrm{ \ then \ }\int f\left(x\right)dx=F\left(x\right)+C$$
Integral substitution:
$$\int f\left(g\left(x\right)\right)\cdot g'\left(x\right)dx=\int f\left(u\right)du, \ \quad u=g\left(x\right)$$

### 2.2 Common

- Assume definite integrals can be simply reversed from the previously listed
  derivatives chart. For example: $\int e^xdx=e^x$
  and $\int x^{-1} dx=\ln \left(x\right)$.

$$\int |x|dx=\frac{x\sqrt{x^2}}{2}$$
$$\int \sin \left(x\right)dx=-\cos \left(x\right)$$
$$\int \cos \left(x\right)dx=\sin \left(x\right)$$

### 2.3 Trigonometric

$$\int \sec ^2\left(x\right)dx=\tan \left(x\right)$$
$$\int \csc ^2\left(x\right)dx=-\cot \left(x\right)$$
$$\int \frac{1}{\sin ^2\left(x\right)}dx=-\cot \left(x\right)$$
$$\int \frac{1}{\cos ^2\left(x\right)}dx=\tan \left(x\right)$$

---

## 3 Laplace Transform Pairs

$$X \left( s \right) = \int_{-\infty}^{\infty} x \left( t \right) e^{-st} dt$$

|           $$f \left( t \right)$$           |                                                   $$F= \left( s \right)$$                                                    |
|:------------------------------------------:|:----------------------------------------------------------------------------------------------------------------------------:|
|        $$n\delta \left( t \right)$$        |                                                            $$n$$                                                             |
|           $$u \left( t \right)$$           |                                                       $$\frac{1}{s}$$                                                        |
|                $$e^{-at}$$                 |                                                      $$\frac{1}{s+a}$$                                                       |
|                   $$t$$                    |                                                      $$\frac{1}{s^2}$$                                                       |
|                  $$t^n$$                   |                                                    $$\frac{n!}{s^{n+1}}$$                                                    |
|                $$te^{-at}$$                |                                             $$\frac{1}{ \left( s+a \right) ^2}$$                                             |
|               $$t^ne^{-at}$$               |                                          $$\frac{n!}{ \left( s+a \right) ^{n+1}}$$                                           |
|      $$\sin \left( \omega t \right)$$      |                                             $$\frac{ \omega }{s^2+ \omega ^2}$$                                              |
|      $$\cos \left( \omega t \right)$$      |                                                 $$\frac{s}{s^2+ \omega ^2}$$                                                 |
| $$\sin \left( \omega t + \theta \right)$$  |                                  $$\frac{s\sin\theta+ \omega \cos\theta}{s^2+ \omega ^2}$$                                   |
| $$\cos \left( \omega t + \theta \right)$$  |                                  $$\frac{s\cos\theta- \omega \sin\theta}{s^2+ \omega ^2}$$                                   |
|  $$e^{-at} \sin \left( \omega t \right)$$  |                                    $$\frac{ \omega }{ \left( s+a \right) ^2+ \omega ^2}$$                                    |
|  $$e^{-at} \cos \left( \omega t \right)$$  |                                      $$\frac{s+a}{ \left( s+a \right) ^2+ \omega ^2}$$                                       |
| $$\frac{ d^{n}f \left( t \right) }{dt^n}$$ | $$s^nF \left( s \right) -s^{n-1}f \left( 0 \right) -s^{n-2} f' \left( 0 \right) -f^{ \left( n-1 \right) } \left( 0 \right)$$ |
|     $$\int_0^t f \left( t \right) dt$$     |                                              $$\frac{1}{s} F \left( s \right)$$                                              |

---

## 4 Fourier Analysis

### 4.1 Fourier Series

The fourier series represents **periodic** signals as a sum of sinusoids.

$$x \left( t \right) = A_0 + \sum_{n = 1}^{\infty} A_n \cos \left( n \omega_0 t \right) + B_n \sin \left( n \omega_0 t \right)$$

where:

$$\omega_0 = \frac{2 \pi}{T}$$
$$A_0 = \frac{1}{T} \int_{-T/2}^{T/2} x \left( t \right) dt$$

- $n$: The harmonic number, starting from 1.
    - Harmonics are integer multiples of the fundamental frequency ($n = 1$).
        - If the fundamental frequency is $f_0$, the first harmonic would
          be $2f_0$, the second harmonic $3f_0$, and so on.
- $T$: The signal time period (time for 1 full cycle).

and for integer $n > 0$:

$$A_n = \frac{2}{T} \int_{-T/2}^{T/2} x \left( t \right) \cos \left( n \omega_0 t \right) dt$$
$$B_n = \frac{2}{T} \int_{-T/2}^{T/2} x \left( t \right) \sin \left( n \omega_0 t \right) dt$$

Amplitude or Fourier Magnitude is given by:

$$| C_n | = \sqrt{A_n^2 + B_n^2}$$

### 4.2 Fourier Transform

The Fourier transform represents a signal as a sum of sinusoid's of continuous
frequencies, allowing for the analysis of **non-periodic** signals in the
frequency domain.

$$X \left( j \omega \right) = \int_{-\infty}^{\infty} x \left( t \right) e^{-j \omega t} dt$$
$$x \left( t \right) = \frac{1}{2 \pi} \int_{-\infty}^{\infty} X \left( j \omega \right) e^{j \omega t} dt$$

#### 4.2.1 Discrete Fourier Transform (DFT)

$$y \left( f \right) = \frac{2}{N} \sum_{r = 0}^{N - 1} y \left( r \Delta t \right) e^{-i 2 \pi \frac{kr}{N}}$$

where:

- $\frac{2}{N}$: A normalization factor (depending on conventions, this could
  also be $\frac{1}{N}$, $\frac{1}{\sqrt{N}}$, or similar).
- $y(r \Delta t)$: The original signal sampled at discrete time
  steps $r \Delta t$.
- $e^{-i 2 \pi \frac{kr}{N}}$: The complex exponential term used to extract the
  frequency components.
    - This is effectively creating Fourier series sines and cosines at various
      frequencies.
- $N$: The total number of discrete samples.
- $r$: The discrete time index.
- $\Delta t$: The time step, given by the following:
    - $\Delta t = \frac{T}{N}$.
- $k$: The frequency index corresponding to a discrete frequency component.
    - Each value of $k$ corresponds to a specific frequency in the transformed
      domain.
    - The relationship between $k$ and the actual frequency depends on the
      sampling rate $f_s$:
        - $f_k = \frac{k}{N} f_s$.

For more information:

- YouTube,
  MATLAB: [Understanding the Discrete Fourier Transform and the FFT](https://youtu.be/QmgJmh2I3Fw)
