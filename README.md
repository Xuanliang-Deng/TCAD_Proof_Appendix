# TCAD_Proof_Appendix
Proofs of existing response time analyses and their compatibilities with RT dependency

## Proof of ZLL's incompatibility with RT dependency
In this section, we give proof that the schedulability test ZLL from the study [<sup>1</sup>](#zhou17) does not satisfy the conditions of the RT dependency. Zhou et al. proposed an improvement over the RTA-LC schedulability test [<sup>2</sup>](#Guan09) to calculate a more accurate carry-in workload of tasks. The carry-in workload can be computed by equations in the study [<sup>1</sup>](#zhou17) provided below,

$$W_k^{CI}(\tau_i,x) = \lfloor{\frac{[[ x - C_i ]]_0}{T_i}} \rfloor  \cdot C_i + C_i + \beta$$
where 

$$\beta = [[ \max_i(\gamma)]]_0^{C_i-1}, \gamma = [[ x-C_i ]]_0 \mod T_i$$

$$\min_i(x)  = \max_{1\leq y \leq x}\{[[ y - \lfloor{\Omega_i(y)/m}\rfloor ]]_0^{C_i}\}$$

$$\max_i(x)  = C_i - \min_i(T_i-x)$$


Based on the above definitions, we have,

$$\begin{array}{ll}
    \beta  & = \min(\max_i([[ x-C_i ]]_0 \mod \ T_i),\ C_i-1) \\
    & = \min(\max_i(\max(x-C_i,0) \mod \ T_i),\ C_i-1)\\
    & = \min(C_i - \min_i(T_i-\max(x-C_i,0) \mod \ T_i),\ C_i-1)
\end{array}$$

Denote $z = T_i-\max(x-C_i,\ 0) \mod \ T_i $, the above equation can be written as,

$$
\begin{array}{ll}
    \beta  & = \min(C_i - \max_{1 \leq y \leq z}\{ [[ y - \lfloor \Omega_i(y)/m \rfloor ]]_0^{C_i} \},\ C_i-1)
\end{array}
$$

Note that the tasks are ordered by decreasing priorities. The analyzed task is $\tau_k$, $\Omega_i(y)$ is the max possible interference on $\tau_{i,i<k}$ $(i<k)$, generated by $$\tau_{j,(j<i<k)}$$. 

Plug Eq.~\eqref{eqn:28} into Eq.~\eqref{eqn:omegak_detail} in~\cite{Zhou17} to replace $\alpha$ with $\beta$,
\begin{equation}\label{eqn:omegak_detail_app}
\begin{array}{lrl}
    \Omega_k(x) & = \max(
    \sum_{\tau_i\in \tau^{NC}}(\llbracket  \lfloor{ \frac{x}{T_i}}  \rfloor  \cdot C_i + \llbracket x \mod \ T_i \rrbracket^{C_i}  \rrbracket_0^{x-C_k+1}) \\ 
    & + \sum_{\tau_i\in\tau^{CI}}( \llbracket \lfloor{\frac{\llbracket x - C_i \rrbracket_0}{T_i}} \rfloor  \cdot C_i + C_i + \beta \rrbracket_0^{x-C_k+1})
\end{array}
\end{equation}
Since in Eq.~\eqref{eqn:28}, $\Omega_i(y)$ is the interference of task $\tau_{j,(j<i<k)}$ on task $\tau_{i,(i<k)}$, which depends on the relative order of task $\tau_i$ in the higher priority task set. This violates the conditions A1, and A2 of RT dependency. 



\section{Proof of improved-RTA-LC's incompatibility with RT dependency}
\begin{pf}
\textcolor{blue}{
In \cite{Guan15}, Guan et al. improved the analysis precision of their previous schedulability test RTA-LC \cite{Guan09} by replacing $\alpha$ with a tighter bound $\Delta^{new}(\tau_i,x)$, the more accurate carry-in workload is computed by,
}

\begin{equation}
   I_k^{CI}(\tau_i,x) = \llbracket \lfloor \llbracket x-C_i \rrbracket_0/T_i \rfloor C_i + C_i + \Delta^{new}(\tau_i,x)   \rrbracket^{x-C_k+1}
\end{equation}
where $\Delta^{new}(\tau_i,x)$ is the upper bound of the carry-in job of $\tau_i$ computed by,
\begin{equation}
    \Delta^{new}(\tau_i,x) = \max_{l\in [0,C_i]}{\llbracket \Delta(\tau_i,x,R_i^{k-i,l}) \rrbracket^l}
\end{equation}
Note that $\Delta(\tau_i,x,R_i^{k-i,l})$ is the $\alpha$ in RTA-LC by replacing the response time $R_i$ with the new $R_i^{k-i,l}$, which is the minimal solution of the following equation,
\begin{equation}\label{eqn:32}
    x = \lfloor \frac{\Omega_k(x) + (k-i)\cdot l}{M} \rfloor + C_k - l
\end{equation}
In the above equation, to compute $R_k^{k-i,l}$ we need to know the values of $k,\ i,\ l$ first, which are the indices of relative orders in the higher priority task set. Therefore when computing the response time of analyzed task $\tau_k$, it is dependent on the relative orders of tasks in $\tau_k$'s higher priority task set, this violates the conditions A1, A2 of RT dependency. 
\end{pf}

\section{Proof of EPE-ZLL's incompatibility with RT dependency}\label{appendix_c}
In this section, we give proof that the latest schedulability test EPE-ZLL does not satisfy the conditions of RT dependency. First, we give a set of definitions proposed in \cite{EPE_ZLL_zhou2021}. Let $\mathbf{L},\mathbf{P}$ be two time intervals respectively,
\begin{equation}
    \mathbf{L}=[r_{k,l}, r_{k,l}+UR_k^a]
\end{equation}
\begin{equation}
    \mathbf{P}=[r_{k,l}+UR_k^a, x)
\end{equation}

Let $c = \llbracket r_{k,l}+x-r_{i,j}\rrbracket^x$,  $b = r_{k,l}+UR_k^a-r_{i,j}$. 

\subsection{Lower bound of interference of $\tau_i$ in $\mathbf{L}$ (Lemma 1)}
\begin{equation}
    I_k^{CI}(\tau_j, UR_k^a, a) = \llbracket W_k^{CI}(\tau_j, UR_k^a)\rrbracket^{UR_k^a-a}
\end{equation}
\begin{equation}
    I_k^{NC}(\tau_j, UR_k^a, a) = \llbracket W_k^{NC}(\tau_j, UR_k^a)\rrbracket^{UR_k^a-a}
\end{equation}
\begin{equation}
    I_k^{DIFF}(\tau_j, UR_k^a, a) = I_k^{CI}(\tau_j, UR_k^a, a) - I_k^{NC}(\tau_j, UR_k^a, a)
\end{equation}
\begin{equation}
    \Theta_{i,k}(a,z) = \sum_{j<k\ and\ j \neq i}I_k^{NC}(\tau_j, UR_k^a, a) + \sum_{\tau_j \in \mathbf{T}_z}I_k^{DIFF}(\tau_j, UR_k^a, a)
\end{equation}
z = m - 1 or m - 2 depends on whether $\tau_i$ is a carry-in task or non-carry-in task over $\mathbf{L}$.

\textbf{Lemma 1}: gives two lower bounds of the interference on the target job $J_{k,l}$ by any higher priority task $\tau_i$ in $\mathbf{L}$.
\begin{equation}
    I_{i,k}^{CI}(a) = m(UR_k^a - a) - \Theta_{i,k}(a,m-2)
\end{equation}
\begin{equation}
    I_{i,k}^{NC}(a) = m(UR_k^a - a) - \Theta_{i,k}(a,m-1)
\end{equation}

\subsection{Lower bound of accumulative time for parallel execution in $\mathbf{L}$}
\textbf{Lemma 2} If $J_{i,j}$ has execution after $r_{k,l} + UR_k^a$, the lower bound will be,
\begin{equation}
    \vartheta_{i,k}(a,c) = \max\{x'|UR_k^{x'} \leq c\}
\end{equation}
\textbf{Lemma 3} If $J_{i,j}$ has no execution after $r_{k,l} + UR_k^a$, the lower bound will be,
\begin{equation}
    {\vartheta'}_{i,k}(a,c) = \llbracket C_{i}'+\max\{x'|UR_k^{x'} \leq c\} -b \rrbracket _0
\end{equation}
where $C_{i}'$ is the real execution time of $J_{i,j}$.\\
\textbf{Theorem 1} The workload of $\tau_i$ in $\mathbf{L}$, depends on whether $J_{i,j}$ has execution after $\mathbf{L}$ or not,
\begin{equation}
    W_{i,k}^{CI}(a) = I_{i,k}^{CI}(a) + \vartheta_{i,k}(a,c)
\end{equation}
\begin{equation}
    W_{i,k}^{NC}(a) = I_{i,k}^{NC}(a) + \vartheta_{i,k}(a,c)
\end{equation}
\begin{equation}
    \overline{W_{i,k}^{CI}}(a) = I_{i,k}^{CI}(a) + {\vartheta'}_{i,k}(a,c)
\end{equation}
\begin{equation}
    \overline{W_{i,k}^{CI}}(a) = I_{i,k}^{NC}(a) + {\vartheta'}_{i,k}(a,c)
\end{equation}

\subsection{Proof}
In \cite{EPE_ZLL_zhou2021}, the WCRT upper bound of $\tau_k$ under $C_k = a+1$ is the minimal solution of,
\begin{equation}
    x \geq \lfloor{\frac{\Psi_k(a,x)}{m}}\rfloor + a + 1
\end{equation}
where $\Psi_k(a,x)$ is given by,
\begin{equation}
    \Psi_k(a,x) = \llbracket m(UR_k^a - a) + \sum_{1 \leq i \leq k}W_{i,k}^{\max}(a,x)\rrbracket^{\Omega_k(x+UR_k^a)}
\end{equation}
Since the main idea is to first calculate $UR_k^1$ by an existing RTA test and then gradually increase $C_k$ to calculate $UR_k^a$ under different a ($1 \leq a \leq C_k$). $UR_k^a$ will be a known constant when we calculate $UR_k^{a+1}$, therefore the left term in the bracket is known as constant. The term $\Omega_k(x+UR_k^a)$ is defined in \cite{Guan09}. \textit{If} the response times of higher priority tasks are known, then this term will also be constants. To check whether \textit{EPE} satisfies the conditions A1, A2 of RT dependency, we only need to focus on the second term $\sum_{1 \leq i \leq k}W_{i,k}^{\max}(a,x)$ in bracket.  

To calculate the maximum workload of $\tau_i$ in $\mathbf{P}$, the loop iterates from $b = 0$ to $b = T_i$. To derive $W_{i,k}(a,b,x)$, 
\begin{equation}
  W_{i,k}(a,b,x) =
    \begin{cases}
      \llbracket C_i - W_{i,k}^{CI}(a)\rrbracket ^{\lambda} &, \text{if $b > UR_k^a$}\\
      \llbracket \llbracket W_1 \rrbracket_{W2}^d \rrbracket^{\lambda} &, \text{if $b \leq UR_k^a$}\\
    \end{cases}       
\end{equation}
where $d = C_i - \vartheta_{i,k}(a,c)$. Note that $c = \llbracket r_{k,l}+x-r_{i,j}\rrbracket^x$ where $r_{i,j}$ represents the release time of last job (of a higher priority task $\tau_i$ than $\tau_k$) released no later than $r_{k,l}+UR_k^a$. Even if we know the response times of all other tasks (other than $\tau_k$), the relative order of higher priority tasks still influences the value of $r_{i,j}$, which violates conditions A1 and A2 of RT dependency. In short, if we swap the priorities of tasks in the $hp(k)$, the release pattern of jobs will change, which changes the $r_{i,j}$.

## References
<div id = "zhou17"></div>
- [1] Zhou, Q., Li, G., & Li, J. (2017). Improved carry-in workload estimation for global multiprocessor scheduling. IEEE Transactions on Parallel and Distributed Systems, 28(9), 2527-2538. 

<div id = "Guan09"></div>
- [2] Guan, N., Stigge, M., Yi, W., & Yu, G. (2009, December). New response time bounds for fixed priority multiprocessor scheduling. In 2009 30th IEEE Real-Time Systems Symposium (pp. 387-397). IEEE.


