---
header-includes:
  - \include{../macros.tex}
---

# CS 427 Homework 2

## Robert Detjens

---

### 1. Formally show that non-zero `KeyGen` does not satisfy one-time secrecy.

\begin{center}
\fcodebox{
  \underline{$\subname{KeyGen}$:} \\
  \> do \\
  \> \> $k \gets \bits^\lambda$ \\
  \> until $k \ne 0$ \\
  \> return $k$
}
\end{center}

If we hypothesize some calling function $\A$, linked against either $\L_{ots-L}$
or $\L_{ots-R}$, as defined below:

\begin{center}
\titlecodebox{$\A$:}{
  $c := \subname{Eavesdrop}(\zerol, \onel)$ \\
  return $c \? \zerol$
}

\titlecodebox{$\lib{ots-L}$}{
  \underline{$\subname{Eavesdrop}(m_L, m_R \in \bits^\lambda)$:} \\
  \> $k \gets \subname{KeyGen}$ \\
  \> $c := k \oplus m_L$ \\
  \> return $c$
}
\qquad
\titlecodebox{$\lib{ots-R}$}{
  \underline{$\subname{Eavesdrop}(m_L, m_R \in \bits^\lambda)$:} \\
  \> $k \gets \subname{KeyGen}$ \\
  \> $c := k \oplus m_R$ \\
  \> return $c$
}
\end{center}

For `KeyGen` to satisfy one-time secrecy, $\A$ should perform the same
when linked against either library:

$$
Pr[\A \link \lib{ots-L} \implies true] = Pr[\A \link \lib{ots-R} \implies true]
$$

When $\A$ is linked to $\lib{ots-L}$, $c$ is computed with $m_L \ (\zerol)$. The
probability of  $\A$ returning true is 0, as the key from this `KeyGen` is never
zero. XOR only outputs $\zerol$ if both inputs are $\zerol$ or $\onel$, and as
$k$ will never be $\zerol$ with this `KeyGen`, neither will the output.

When $\A$ is linked to $\lib{ots-R}$, $c$ is computed with $m_R \ (\onel)$. The
probability $\A$ returning true is $1 / (2^\lambda - 1)$, being when the key is
all ones. This is $2^\lambda - 1$ instead of $2^\lambda$ to account for $\zerol$
not being in the search space.

These probabilities are not equal, and thus this `KeyGen` does not satisfy
one-time secrecy.

\pagebreak

### 2. Use a hybrid proof to prove that if $\Sigma$ satisfies one-time secrecy, then so does $\Sigma^2$.

\begin{center}
\framebox{
  \codebox{
    \> $\K = (\Sigma.\K)^2$ \\
    \> $\M = \Sigma.\M$ \\
    \> $\C = \Sigma.\C$ \\

    \underline{$\subname{KeyGen}()$:} \\
    \> $k_1 \gets \Sigma.\K$ \\
    \> $k_2 \gets \Sigma.\K$ \\
    \> return $(k_1, k_2)$
  }
  \qquad
  \codebox{
    \underline{$\subname{Enc}((k_1, k_2), m)$:} \\
    \> $c_1 := \Sigma.\Enc(k_1, m)$ \\
    \> $c_2 := \Sigma.\Enc(k_2, m)$ \\
    \> return $(c_1, c_2)$
  }
  \qquad
  \codebox{
    \underline{$\subname{Dec}((k_1, k_2), (c_1, c_2))$:} \\
    \> $m_1 := \Sigma.\Dec(k_1, c_1)$ \\
    \> $m_2 := \Sigma.\Dec(k_2, c_2)$ \\
    \> if $m_1 \ne m_2$ return \bit{err} \\
    \> return $m_1$
  }
}
\end{center}

$\Sigma^2$ satisfies one-time secrecy if `Eavesdrop` on two ciphertexts is
indistinguishable:

\begin{center}
\titlecodebox{$\lib{ots-L}^{\Sigma^2}$}{
  \underline{$\subname{Eavesdrop}(m_L, m_R \in \Sigma^2.\M)$:} \\
  \> $k_1, k_2 := \Sigma^2.KeyGen$ \\
  \> $c_1, c_2 := \Sigma^2.Enc((k_1, k_2), m_L)$ \\
  \> return $(c_1, c_2)$
}
$\equiv$
\titlecodebox{$\lib{ots-R}^{\Sigma^2}$}{
  \underline{$\subname{Eavesdrop}(m_L, m_R \in \Sigma^2.\M)$:} \\
  \> $k_1, k_2 := \Sigma^2.KeyGen$ \\
  \> $c_1, c_2 := \Sigma^2.Enc((k_1, k_2), m_R)$ \\
  \> return $(c_1, c_2)$
}
\end{center}

To show this, we transform $\lib{ots-L}^{\Sigma^2}$ into
$\lib{ots-R}^{\Sigma^2}$ using the known-good $\lib{ots-L/R}^{\Sigma}$:

\begin{center}
\scalebox{0.8}{
  \titlecodebox{$\lib{ots-L}^{\Sigma^2}$}{
    \underline{$\subname{Eavesdrop}(m_L, m_R)$:} \\
    \> \highlightline{$k_1, k_2 := \Sigma^2.KeyGen$} \\
    \> \highlightline{$c_1, c_2 := \Sigma^2.Enc((k_1, k_2), m_L)$} \\
    \> return $(c_1, c_2)$
  }

  $\equiv$
  \titlecodebox{$\lib{ots-L}^{\Sigma^2}$}{
    \underline{$\subname{Eavesdrop}(m_L, m_R)$:} \\
    \> \highlightline{$k_1 \gets \Sigma.\K$} \\
    \> \highlightline{$k_2 \gets \Sigma.\K$} \\
    \> \highlightline{$c_1 := \Sigma.Enc(k_1, m_L)$} \\
    \> \highlightline{$c_2 := \Sigma.Enc(k_2, m_L)$} \\
    \> return $(c_1, c_2)$
  }

  $\equiv$
  \titlecodebox{$\lib{ots-L}^{\Sigma^2}$}{
    \underline{$\subname{Eavesdrop}(m_L, m_R)$:} \\
    \> \highlightline{$c_1 := \subname{Eavesdrop}_\Sigma(m_L, m_R)$} \\
    \> \highlightline{$c_2 := \subname{Eavesdrop}_\Sigma(m_L, m_R)$} \\
    \> return $(c_1, c_2)$
  }
  $\link$
  \titlecodebox{$\lib{ots-L}^{\Sigma}$}{
    \underline{$\subname{Eavesdrop}_\Sigma(m_L, m_R)$:} \\
    \> $k \gets \Sigma.\K$ \\
    \> $c := \Sigma.Enc(k, m_L)$ \\
    \> return $c$
  }
}
\end{center}

The first step inlines `KeyGen` and `Enc` from $\Sigma^2$ as defined into
`Eavesdrop`, which has no functionality change. Then, we pull out both pairs of
$\Sigma$`.KeyGen` & $\Sigma$`.Enc` into `Eavesdrop`$_\Sigma$ in
$\lib{ots-L}^{\Sigma}$ as defined for $\Sigma$, again with no functionality
change as

\begin{center}
\scalebox{0.8}{
  $\equiv$
  \titlecodebox{$\lib{ots-L}^{\Sigma^2}$}{
    \underline{$\subname{Eavesdrop}(m_L, m_R)$:} \\
    \> $c_1 := \subname{Eavesdrop}_\Sigma(m_L, m_R)$ \\
    \> $c_2 := \subname{Eavesdrop}_\Sigma(m_L, m_R)$ \\
    \> return $(c_1, c_2)$
  }
  $\link$
  \titlecodebox{$\mathhighlight{\lib{ots-R}^{\Sigma}}$}{
    \underline{$\subname{Eavesdrop}_\Sigma(m_L, m_R)$:} \\
    \> $k \gets \Sigma.\K$ \\
    \> $c := \Sigma.Enc(k, \mathhighlight{m_R})$ \\
    \> return $c$
  }

  $\equiv$
  \titlecodebox{$\lib{ots-L}^{\Sigma^2}$}{
    \underline{$\subname{Eavesdrop}(m_L, m_R)$:} \\
    \> \highlightline{$k_1 \gets \Sigma.\K$} \\
    \> \highlightline{$k_2 \gets \Sigma.\K$} \\
    \> \highlightline{$c_1 := \Sigma.Enc(k_1, m_R)$} \\
    \> \highlightline{$c_2 := \Sigma.Enc(k_2, m_R)$} \\
    \> return $(c_1, c_2)$
  }

  $\equiv$
  \titlecodebox{$\lib{ots-L}^{\Sigma^2}$}{
    \underline{$\subname{Eavesdrop}(m_L, m_R)$:} \\
    \> \highlightline{$k_1, k_2 := \Sigma^2.KeyGen$} \\
    \> \highlightline{$c_1, c_2 := \Sigma^2.Enc((k_1, k_2), m_R)$} \\
    \> return $(c_1, c_2)$
  }
}
\end{center}

As we are assuming that $\Sigma$ satisfies one-time secrecy,
$\lib{ots-L}^{\Sigma}$ and $\lib{ots-R}^{\Sigma}$ are interchangeable and can be
swapped without functionality change. `Eavesdrop`$_\Sigma$ is then re-inlined
into `Eavesdrop`, and then pulled out into `KeyGen` and `Enc` as defined for
$\Sigma^2$.

\begin{center}
\scalebox{0.8}{
  \titlecodebox{$\lib{ots-L}^{\Sigma^2}$}{
    \underline{$\subname{Eavesdrop}(m_L, m_R)$:} \\
    \> $k_1, k_2 := \Sigma^2.KeyGen$ \\
    \> $c_1, c_2 := \Sigma^2.Enc((k_1, k_2), m_R)$ \\
    \> return $(c_1, c_2)$
  }
  $\equiv$
  \titlecodebox{$\lib{ots-R}^{\Sigma^2}$}{
    \underline{$\subname{Eavesdrop}(m_L, m_R)$:} \\
    \> $k_1, k_2 := \Sigma^2.KeyGen$ \\
    \> $c_1, c_2 := \Sigma^2.Enc((k_1, k_2), m_R)$ \\
    \> return $(c_1, c_2)$
  }
}
\end{center}

Thus, $\lib{ots-L}^{\Sigma^2}$ is now interchangeable with $\lib{ots-R}^{\Sigma^2}$ and
$\Sigma^2$ satisfies one-time secrecy.
