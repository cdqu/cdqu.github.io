---
title: 'State Estimation for the Observer'
date: 2024-05-18
permalink: /posts/2024/05/state-estimation/
tags:
  - state estimation
  - linear systems
---


# State Estimation for the Observer

State estimation is always a popular topic in the control theory. We will consider the discrete-time linear invariant system here.
The system is described as

$$
x_{k+1} = A x_k +Bu_k,~ y_k = Cx_k + \omega_k,
$$

where $A \in \mathbb{R}^{n \times n}, B \in \mathbb{R}^{n \times m}, C \in \mathbb{R}^{p \times n}$, $x_k, u_k, y_k$ are system state, input and output vectors and $\omega_k$ is the i.i.d. Gaussian measurement noise. As an observer, we want to estimate the true state $x_k$ from output $y_k$.

To analyze this state estimation problem, let's write the system dynamics for $N$ steps in a single equation:

$$
\underbrace{\begin{pmatrix}
y_0 \\ y_1 \\ \vdots \\ y_{N-1}
\end{pmatrix}}_{\mathbf{y}} = \underbrace{\begin{pmatrix}
0 &0&0&\cdots&0 \\ CB & 0&0&\cdots & 0 \\ \vdots &\vdots& &  \\ CA^{N-2}B & CA^{N-1}B & &\cdots & 0
\end{pmatrix}}_{\mathcal{L}(A,B,C)} \underbrace{\begin{pmatrix}
u_0 \\ u_1 \\ \vdots \\ u_{N-1}
\end{pmatrix}}_{\mathbf{u}} + \underbrace{\begin{pmatrix}
C \\ CA^1 \\ \vdots \\ CA^{N-1}
\end{pmatrix}}_{\mathcal{O}(C,A)} x_0 + \underbrace{\begin{pmatrix}
\omega_0 \\ \omega_1 \\ \vdots \\ \omega_{N-1}
\end{pmatrix}}_{\mathbf{\omega}},
$$

which is

$$
\begin{equation}
    \mathbf{y} = \mathcal{L}(A,B,C) \mathbf{u} + \mathcal{O}(C,A) x_0 + \mathbf{\omega}.
\end{equation}
$$


### (a) with both u and y

From (3), we find that if we obtain both $\mathbf{y}, \mathbf{u}$, then the initial state $x_0$ can be optimally estimated by

$$
\hat{x}_0 = \mathcal{O}(C,A)^{\dagger} (\mathbf{y} - \mathcal{L}(A,B,C) \mathbf{u})
$$

as the ordinary least square (OLS) estimator.
> The state estimation $\hat{x}_{0:N}$ is unique if $\mathcal{O}(C,A)$ has full column rank.

Note that "$\mathcal{O}(C,A)$ has full column rank" represents $rank(\mathcal{O}(C,A)) =n$, which is the classic observability condition. Therefore, the observability is to ensure a unique $x_0$ given $\mathbf{y}, \mathbf{u}$.

### (b) with y only

However, sometimes we cannot get access to the input $\mathbf{u}$. If we only have the observation $\mathbf{y}$, then there should be more strict assumptions on matrix $C$. 
>The state at time $k$ can be directly estimated by $\hat{x}_k = C^{\dagger} y_k$ if $C$ has full column rank ($rank(C) = n$). 

In this case, $C$ cannot be a "fat matrix" ($p < n$), which is unfortunately a common situation in the realistic application. The number of variables in the output vector is less than that in the state.

### (c) with y and $x_0$

Another choice for state estimation without inputs is to use the initial state $x_0$. In some scenarios the initial state can be accurately observed. Then we have another dynamic equation from $k=1$ to $k=N$ as

$$
\underbrace{\begin{pmatrix}
y_1 \\ y_2 \\ \vdots \\ y_N
\end{pmatrix}}_{\tilde{\mathbf{y}}} = \underbrace{\begin{pmatrix}
CB \\ CAB & CB \\ \vdots & & \ddots \\ CA^{N-1}B & \cdots & \cdots & CB
\end{pmatrix}}_{\tilde{\mathcal{L}}(A,B,C)} \underbrace{\begin{pmatrix}
u_0 \\ u_1 \\ \vdots \\ u_{N-1}
\end{pmatrix}}_{\mathbf{u}} + \underbrace{\begin{pmatrix}
CA \\ CA^2 \\ \vdots \\ CA^{N}
\end{pmatrix}}_{\mathcal{O}(CA,A)} x_0 + \underbrace{\begin{pmatrix}
\omega_1 \\ \omega_2 \\ \vdots \\ \omega_N
\end{pmatrix}}_{\tilde{\mathbf{\omega}}},
$$

which is

$$
\begin{equation}
    \tilde{\mathbf{y}} = \tilde{\mathcal{L}}(A,B,C) \mathbf{u} + \mathcal{O}(CA,A) x_0 + \tilde{\mathbf{\omega}}.
\end{equation}
$$

Now we need to firstly estimate the input sequence $\hat{\mathbf{u}}$, then calculate the state estimation with $\hat{\mathbf{u}}$ and system dynamic. Similarly to (a), with $\tilde{\mathbf{y}}$ and $x_0$ the optimal estimation to inputs is

$$
\hat{\mathbf{u}} = \tilde{\mathcal{L}}(A,B,C)^{\dagger} (\tilde{\mathbf{y}}-\mathcal{O}(CA,A) x_0).
$$

And the state estimation is

$$
{\begin{pmatrix}
\hat{x}_1 \\ \hat{x}_2 \\ \vdots \\ \hat{x}_N
\end{pmatrix}} = {\begin{pmatrix}
B \\ AB & B \\ \vdots & & \ddots \\ A^{N-1}B & \cdots & \cdots & B
\end{pmatrix}} \hat{\mathbf{u}} + {\begin{pmatrix}
A \\ A^2 \\ \vdots \\ A^{N}
\end{pmatrix}} x_0.
$$

> The state estimation is unique if matrix $CB$ has a full column rank ($rank(CB) = m$).

If $CB$ has full column rank, then matrix $\tilde{\mathcal{L}}(A,B,C)$ has full column rank and its left pseudo-inverse is unique. 

### (d) Kalman Filter (with y, u and $x_0$)

Kalman filter is a widely used method for state estimation and we will not talk it in detailed here (there are loads of blogs introducing KF on the web). We point out that Kalman filter is not included in any above cases we listed, while it can deal with the system with process noises:

$$
x_{k+1} = A x_k +Bu_k + \nu_k,~ y_k = Cx_k + \omega_k.
$$

For this system, the $N$ steps dynamic equation is written as

$$
\tilde{\mathbf{y}} = \tilde{\mathcal{L}}(A,B,C) \mathbf{u} + \mathcal{O}(CA,A) x_0 + \begin{pmatrix}
C \\ CA & C \\ \vdots & & \ddots \\ CA^{N-1} & \cdots & \cdots & C
\end{pmatrix} \begin{pmatrix}
\nu_0 \\ \nu_1 \\\vdots \\ \nu_{N-1}
\end{pmatrix}+ \tilde{\mathbf{\omega}}.
$$

It is obvious that the previous estimation (estimate $x_0$ with y+u or estimate u with y+$x_0$) fail due to the existence of the process noise $\nu_k$. The Kalman filter needs all the $\mathbf{y},\mathbf{u},x_0$.

Hope this blog helps you build a more comprehensive and clearer understanding of the state estimation with output $y_k$. Contact me without hesitation if you find any mistakes in the blog. 
