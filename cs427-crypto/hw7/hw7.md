---
header-includes:
  - \include{../macros.tex}
---

# CS 427 Homework 7

## Robert Detjens

---

## Question 1

*Let $F$ be a secure PRP with block length $n + \lambda$. Consider the following encryption scheme:*

\begin{center}
\framebox{
  \codebox{
    \> $\K = \bits^\lambda$ \\
    \> $\M = \bits^n$ \\
    \> $\C = \bits^{n+\lambda}$
  }
  \qquad
  \codebox{
    \underline{$\KeyGen$:} \\
    \> $k \gets \bits^\lambda$ \\
    \> return $k$
  }
  \qquad
  \codebox{
    \underline{$\Enc(k, m)$:} \\
    \> $r \gets \bits^\lambda$ \\
    \> $c := F(k, m||r)$ \\
    \> return $c$
  }
}
\end{center}

*(a) Write the corresponding $\Dec$ algorithm.*

\fcodebox{
  \underline{$\Dec(k, c)$:} \\
  \> $m := F^{-1}(k, c)$ \\
  \> return $m[0..\lambda]$ (first $\lambda$ bits)
}

*(b) Prove the scheme has CPA\$ security.*

In order to prove CPA$ security, the following two libraries must be indistinguishable:

\begin{center}
\titlecodebox{$\lib{cpa\$-real}$}{
  $k \gets \bits^\lambda$ \\
  \\
  \underline{$\subname{Ctxt}(m \in \bits^n)$:} \\
  \> $r \gets \bits^\lambda$ \\
  \> $c := F(k, m||r)$ \\
  \> return $c$
}
$\stackrel{?}{\indist}$
\titlecodebox{$\lib{cpa\$-rand}$}{
  \underline{$\subname{Ctxt}(m \in \bits^n)$:} \\
  \> $c \gets \bits^{n+\lambda}$ \\
  \> return $c$
}
\end{center}

To prove this, a hybrid proof is used to show that these libraries are indeed distinguishable:

\begin{center}
\fcodebox{
  $k \gets \bits^\lambda$ \\
  \\
  \underline{$\subname{Ctxt}(m \in \bits^n)$:} \\
  \> $r \gets \bits^\lambda$ \\
  \> \hl{$x := m||r$} \\
  \> $c := F(k, \mhl{x})$ \\
  \> return $c$
}
\qquad
\begin{minipage}{0.3\textwidth}
$m || r$ is pulled out into a new variable. This has no change on the effect of the program.
\end{minipage}
\end{center}

\begin{center}
\fcodebox{
  $k \gets \bits^\lambda$ \\
  \\
  \underline{$\subname{Ctxt}(m \in \bits^n)$:} \\
  \> \hl{$x \gets \bits^{n + \lambda}$} \\
  \> $c := F(k, x)$ \\
  \> return $c$
}
\qquad
\desc{
  As $x$ is partially made of the randomly sampled $s$, it is indistinguishable from a uniform sample here, as $x$ is always different even with a fixed $m$.
}
\end{center}

\begin{center}
\fcodebox{
  \underline{$\subname{Ctxt}(m \in \bits^n)$:} \\
  \> $x \gets \bits^{n + \lambda}$ \\
  \> $c := \mhl{\subname{Lookup}(x)}$ \\
  \> return $c$
}
$\link$
\oltitlecodebox{$\lib{PRP-real}^F$}{
  $k \gets \bits^\lambda$ \\
  \\
  \underline{$\subname{Lookup}(x \in \bits^{n+\lambda})$:} \\
  \> return $F(k, x)$
}
\qquad
\desc{
  Calls relating to F have been pulled out into a separate library. This has no change on the effect of the program.
}
\end{center}

\begin{center}
\fcodebox{
  \underline{$\subname{Ctxt}(m \in \bits^n)$:} \\
  \> $x \gets \bits^{n + \lambda}$ \\
  \> $c := \subname{Lookup}(x)$ \\
  \> return $c$
}
$\link$
\oltitlecodebox{$\lib{PRP-rand}^F$}{
  $T :=$ empty dict \\
  \\
  \underline{$\subname{Lookup}(x \in \bits^{n+\lambda})$:} \\
  \> if $T[x]$ undefined: \\
  \> \> $T[x] \gets \bits^{n+\lambda} \backslash T.values$ \\
  \> return $T[x]$
}
\qquad
\desc{
  As F is a secure PRP, $\lib{PRP-rand}^F$ is indistinguishable from $\lib{PRP-real}^F$ and can be switched out without changing
}
\end{center}

\begin{center}
\fcodebox{
  \hl{$T :=$ empty dict} \\
  \\
  \underline{$\subname{Ctxt}(m \in \bits^n)$:} \\
  \> $x \gets \bits^{n + \lambda}$ \\
  \> \hl{if $T[x]$ undefined:} \\
  \> \> \hl{$T[x] \gets \bits^{n+\lambda} \backslash T.values$} \\
  \> $c := \mhl{T[x]}$ \\
  \> return $c$
}
\qquad
\desc{
  The library has been inlined. This has no change on the effect of the program.
}
\end{center}

\begin{center}
\fcodebox{
  \underline{$\subname{Ctxt}(m \in \bits^n)$:} \\
  \> $c := \mhl{\bits^{n+\lambda}}$ \\
  \> return $c$
}
\qquad
\desc{
  The associative dict $T$ produces a randomly sampled output from $\bits^{n+\lambda}$ for each input, and since the input $x$ is randomly sampled from $\bits^{n+\lambda}$ on each invocation, this mapping can be removed without affecting the output.
}
\end{center}

\begin{center}
\titlecodebox{$\lib{cpa\$-rand}$}{
  \underline{$\subname{Ctxt}(m \in \bits^n)$:} \\
  \> $c \gets \bits^{n+\lambda}$ \\
  \> return $c$
}
\qquad
\desc{
  This is now the $\lib{cpa\$-rand}$ library. Thus, $\lib{cpa\$-real}$ and $\lib{cpa\$-rand}$ are indistinguishable and this scheme is CPA\$-secure.
}
\end{center}

\pagebreak

## Question 2

*Let $F$ be a secure PRP with blocklength $\lambda$. Show that the following construction does not have CPA/CPA\$ security:*

\begin{center}
\fcodebox{
  \underline{$\Enc(k, m)$:} \\
  \> $s_1 \gets \bits^\lambda$ \\
  \> $s_2 := s_1 \oplus m$ \\
  \> $x := F(k, s_1)$ \\
  \> $y := F(k, s_2)$ \\
  \> return $(x, y)$
}
\end{center}

Consider an attacking library $\A$ linked against two libraries $\lib{CPA\$-real}$ and $\lib{CPA\$-rand}$:

\begin{center}
\titlecodebox{$\A$}{
  $x, y := \subname{Ctxt}(\zerol)$ \\
  return $x \? y$
}
\qquad
\titlecodebox{$\lib{CPA\$-real}$}{
  $k \gets \bits^\lambda$ \\
  \\
  \underline{$\subname{Ctxt}(m \in \M)$:} \\
  \> $s_1 \gets \bits^\lambda$ \\
  \> $s_2 := s_1 \oplus m$ \\
  \> $x := F(k, s_1)$ \\
  \> $y := F(k, s_2)$ \\
  \> return $(x, y)$
}
\qquad
\titlecodebox{$\lib{CPA\$-rand}$}{
  \underline{$\subname{Ctxt}(m \in \M)$:} \\
  \> $x \gets \bits^\lambda$ \\
  \> $y \gets \bits^\lambda$ \\
  \> return $(x, y)$
}
\end{center}

When $\A$ is linked against $\lib{CPA\$-real}$, $s_2$ is $s_1 \oplus \zerol = s_1$, so $F$ will produce the same output for both $x$ and $y$ as it is a PRP and thus deterministic. When linked against $\lib{CPA\$-rand}$, $x$ and $y$ are randomly sampled and have no bearing on each other at all.

\begin{align*}
Pr[\A \link \lib{pfr-real} \implies true] &= 1 \\
Pr[\A \link \lib{pfr-rand} \implies true] &= \frac{1}{2^{\lambda}}
\end{align*}

As an attacker can distinguish between $\Enc$ and a random sampling, this $\Enc$ does not have CPA$-security.
