---
header-includes:
  - \include{../macros.tex}
---

# CS 427 Homework 9

## Robert Detjens

---

## Question 1

*Let $\Sigma_1$ and $\Sigma_2$ be two (possibly different) encryption schemes with the same message space $\M$. Below we define an encryption scheme $\Sigma'$*

\begin{center}
\framebox{
  \codebox{
        \underline{$\Sigma'.\KeyGen$:} \\
        \> $k_1 \gets \Sigma_1.\KeyGen$ \\
        \> $k_2 \gets \Sigma_2.\KeyGen$ \\
        \> return $(k_1, k_2)$
    }
    \qquad
    \codebox{
        \underline{$\Sigma'.\Enc((k_1, k_2), m)$:} \\
        \> $c_1 \gets \Sigma_1.\Enc(k_1, m)$ \\
        \> $c_2 \gets \Sigma_2.\Enc(k_2, m)$ \\
        \> return $(c_1, c_2)$
    }
    \qquad
    \codebox{
        \underline{$\Sigma'.\Dec((k_1, k_2), (c_1, c_2))$:} \\
        \> $m_1 \gets \Sigma_1.\Dec(k_1, c_1)$ \\
        \> $m_2 \gets \Sigma_2.\Dec(k_2, c_2)$ \\
        \> if $m_1 = m_2$: \\
        \> \> return $m_1$ \\
        \> return $\bit{err}$
    }
}
\end{center}

*Show that $\Sigma'$ does not have CCA security, even if both $\Sigma_1$ and $\Sigma_2$ have CCA security.*

$\Sigma'$ is CCA-secure if the following libraries are indistinguishable:

\newcommand{\Seen}{\mathcal{S}}

\begin{center}
\titlecodebox{$\lib{cca-L}^{\Sigma'}$}{
  $k_1, k_2 \gets \Sigma'.\KeyGen$ \\
  $\Seen := \emptyset$ \\
  \\

  \underline{$\Eavesdrop(m_L, m_R \in \Sigma'.\M)$:} \\
  \> if $|m_L| \ne |m_R|$ return err \\
  \> $c_1, c_2 := \Sigma'.\Enc((k_1, k_2), m_L)$ \\
  \> $\Seen := \Seen \cup \{(c_1, c_2)\}$ \\
  \> return $(c_1, c_2)$ \\
  \\

  \underline{$\Dec((c_1, c_2) \in \Sigma'.\C)$:} \\
  \> if $(c_1, c_2) \in \Seen$ return err \\
  \> return $\Sigma'.\Dec((k_1, k_2), (c_1, c_2))$
}
$\stackrel{?}{\indist}$
\titlecodebox{$\lib{cca-R}^\Sigma$}{
  $k_1, k_2 \gets \Sigma'.\KeyGen$ \\
  $\Seen := \emptyset$ \\
  \\

  \underline{$\Eavesdrop(m_L, m_R \in \Sigma'.\M)$:} \\
  \> if $|m_L| \ne |m_R|$ return err \\
  \> $c := \Sigma'.\Enc((k_1, k_2), m_R)$ \\
  \> $\Seen := \Seen \cup \{(c_1, c_2)\}$ \\
  \> return $(c_1, c_2)$ \\
  \\

  \underline{$\Dec((c_1, c_2) \in \Sigma'.\C)$:} \\
  \> if $(c_1, c_2) \in \Seen$ return err \\
  \> return $\Sigma'.\Dec((k_1, k_2), (c_1, c_2))$
}
\end{center}

Consider an attacking library $\A$:

\begin{center}
\titlecodebox{$\A$}{
  $a, b, c, d \gets \M$ \\
  $w, x := \Eavesdrop(a||b, a||b)$ \\
  $y, z := \Eavesdrop(a||d, c||d)$ \\
  return $w \? y$
}
\end{center}

When $\A$ is linked against $\lib{cca-L}^{\Sigma'}$, the inner $\Sigma_1$ scheme is passed the same plaintext $a$ across both invocations of $\Eavesdrop$. This would cause the f


## Question 2

*Consider the encryption scheme below where $F$ is a secure PRP*

\begin{center}
\fcodebox{
  \underline{$\Enc(k, m)$:} \\
  \> $r \gets \bits^{\lambda}$ \\
  \> $x := F(k, m \oplus r) \oplus r$ \\
  \> return $(r, x)$
}
\end{center}

*Describe the corresponding decryption algorithm and show that the scheme does not have CCA security.*

\fcodebox{
  \underline{$\Dec(k, r, x)$:} \\
  \> $m := F(k, x \oplus r) \oplus r$ \\
  \> return $m$
}
