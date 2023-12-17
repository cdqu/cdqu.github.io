# Inverse discrete time algebraic Riccati equation
In this blog, I want to talk about the inverse problem of discrete time algebraic Riccati equation (I-DARE), which I encountered during my work on the inverse optimal control (IOC) for LQR. 
IOC is to identify the objective function based on the optimal state or input trajectories. For a finite horizon discrete-time LQR problem descried as  

$$ 
\begin{aligned}
& \min J= \sum_{k=0}^{N-1} (x_k^T Q x_k + u_k^T R u_k) + x_N^T H x_N\\   
& s.t.~~ x_{k+1} = Ax_k + Bu_k , x_0 = \bar{x},
\end{aligned}
$$

the solution is the classic linear feedback controller $u_k = K_k x_k$ and the feedback gain matrix $K_k$ (time-variant in finite horizon) is calculated by DARE:  

$$
\begin{align*}
& P_{k-1} = A^TP_{k}A - A^T P_{k} B(R+ B^T P_{k}B)^{-1} B^TP_{k}A+Q \tag{1}\\
& K_k =  (R + B^T P_{k+1} B)^{-1} B^T P_{k+1} A,
\end{align*}
$$

where $P_N = H$. $H,Q,R$ are all positive definite matrices. Riccati equations are applied in various fields like LQR and Kalman filter. Note that there exist some interesting properties of DARE iteration. The sequence $P_{N:0} = P_N, P_{N=1}, ...,P_0$ is proved to be monotonous and converges to its fixed point (also the solution of continuos ARE). Now we present the I-DARE problem.

>**I-DARE Problem:** Suppose the system matrices $A,B$ are known. Given the exact sequence $K_{0:N-1}$ as the solution of  DARE, can we infer the parameter matrices $H,Q,R$ ?

There are two points we need to think over.
* **Uniqueness (scalar ambiguity)**
Notice that for a DARE iteration, the parameter sets $(H,Q,R)$ and $(\alpha H, \alpha Q, \alpha R)$ with scalar
$\alpha \in \mathbb{R}^+$ generate the same solution sequence $K_{0:N-1}$. Does the inverse problem still possess this property, i.e., if we estimate a set of parameters $\hat{H},\hat{Q},\hat{R}$ with $K_{0:N-1}$, does there exist $\hat{H}=\alpha H,\hat{Q}=\alpha Q,\hat{R}=\alpha R$ with unknown $\alpha$?

* **Identifiability**
Are $(H,Q,R)$ in I-DARE problem always identifiable? 


Please refer to my preprint paper for detailed derivation. Here I provide the following theorem. $m,n$ are the dimension of input and state vector.
>**Theorem 1** If the control horizon is set as $N < \frac{mn(n+1)(m+1)}{2}$, the true weight parameters $H,Q,R$ of the control objective will never be identified accurately, which can be utilized in preserving the system's intention.

If you are interested in IOC-LQR and this I-DARE problem, I also suggest to read [H. Zhang](https://www.sciencedirect.com/science/article/pii/S0005109819304546) (infer R) and [C. Yu](https://www.sciencedirect.com/science/article/pii/S0005109821001564) (infer Q,R).
