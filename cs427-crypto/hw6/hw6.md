---
header-includes:
  - \include{../macros.tex}
---

# CS 427 Homework 6

## Robert Detjens

---

## Question 1

> *Let $F$ be a secure PRF with $in = out = \lambda$. Define the following
> function: $F'(k, m) = F(k, m) || F(k, F(k, m))$. Show that $F'$ is not a
> secure PRF.*

$F'$ is not secure if it can be distinguished from a random library. Suppose
some attacker library $\A$, and the two libraries $\lib{prf-rand}^{F'}$ and
$\lib{prf-real}^{F'}$:

\begin{center}
\titlecodebox{$\A$}{
  $a || b := LOOKUP(\zerol)$ \\
  $x || y := LOOKUP(a)$ \\
  return $b \? x$
}
\qquad
\titlecodebox{$\lib{pfr-real}^{F'}$}{
  $k \gets \bits^\lambda$ \\ \\
  \underline{$LOOKUP(m)$} \\
  \> $x = F(k, m)$ \\
  \> $y = F(k, x)$ \\
  \> return $x || y$
}
\qquad
\titlecodebox{$\lib{pfr-rand}^{F'}$}{
  $T := []$ \\ \\
  \underline{$LOOKUP(m)$} \\
  \> if $T[m]$ is undefined: \\
  \> \> $T[m] \gets \bits^{2\lambda}$ \\
  \> return $T[m]$
}
\end{center}

\begin{align*}
Pr[\A \link \lib{pfr-real}^{F'} \implies true] &= 1 \\
Pr[\A \link \lib{pfr-rand}^{F'} \implies true] &= \frac{1}{2^{\lambda}}
\end{align*}

This attacking library $\A$ can distinguish between the real and random
libraries and thus $F'$ is not secure.

## Question 2

> *Show that a 2-round keyed Feistel cipher cannot be a secure PRP, no matter
> what its round functions are. Your attack should work without knowing the
> round keys, and it should work even with different (independent) round keys.
> Hint: a successful attack requires two queries.*

To prove that a 2-round Feistel is insecure, we must show that the following two
libraries are not indistinguishable. Consider an attacker library $\A$:

\begin{center}
\titlecodebox{$\A$}{
  $x \gets \bits^\lambda$ \\
  $y \gets \bits^\lambda$ \\
  $a || b := LOOKUP(x || \zerol)$ \\
  $c || d := LOOKUP(y || \zerol)$ \\
  return $ a \xor x  \? c \xor y$
}
\qquad
\titlecodebox{$\lib{prp-real}^F$}{
  $k_1 \gets \bits^\lambda$ \\
  $k_2 \gets \bits^\lambda$ \\ \\
  \underline{$LOOKUP(v_0 || v_1)$} \\
  \> $v_2 = F(k_1, v_1) \xor v_0$ \\
  \> $v_3 = F(k_2, v_2) \xor v_1$ \\
  \> return $v_2 || v_3$
}
\qquad
\titlecodebox{$\lib{prp-rand}^F$}{
  $T := []$ \\ \\
  \underline{$LOOKUP(v_0 || v_1)$} \\
  \> if $T[v_0 || v_1]$ is undefined: \\
  \> \> $T[v_0 || v_1] \gets \bits^{2\lambda}$ \\
  \> return $T[v_0 || v_1]$
}
\end{center}

\begin{align*}
Pr[\A \link \lib{prg-real}^{F'} \implies true] &= 1 \\
Pr[\A \link \lib{prg-rand}^{F'} \implies true] &= \frac{1}{2^{\lambda}}
\end{align*}

This attacking library $\A$ can distinguish between the real and random
libraries and thus a two-round Feistel is not secure.

## Question 3

> *Let $F$ be a secure PRF with $\lambda$-bit outputs, and let $G$ be a PRG with
> stretch $l$. Define: $F'(k, r) = G(F(k, r)) $. So $F'$ has outputs of length
> $\lambda + l$. Prove that $F'$ is a secure PRF.*
