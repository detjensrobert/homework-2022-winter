---
header-includes:
  - \include{../macros.tex}
---

# CS 427 Homework 3

## Robert Detjens

---

#### 1. You have seen a 2-out-of-2 secret sharing scheme based on XOR. Generalize that scheme to a 3-out-of-3 scheme. Clearly describe the `Share` and `Reconstruct` algorithms, then prove that the scheme is secure. The new scheme should just use XOR (and not Shamir secret sharing).


\begin{center}
\framebox{
  \codebox{
    $\M = \bits^\ell$ \\
    $t = 3$ \\
    $n = 3$
  }
  \qquad
  \codebox{
    \underline{Share($m$):} \\
    % \> if $|U| \ge 3$: return \bit{err} \\
    \> $s_1 \gets \bits^\ell$ \\
    \> $s_2 \gets \bits^\ell$ \\
    \> $s_3 := s_1 \oplus s_2 \oplus m$ \\
    \> return $\{s_i \ | \ i \in U\}$
  }
  \qquad
  \codebox{
    \underline{Reconstruct($s_1, s_2, s_3$):} \\
    \> $m := s_1 \oplus s_2 \oplus s_3$ \\
    \> return ($m$)
  }
}
\end{center}

`Share` works much the same as with 2-of-2, now with an extra share generated
randomly and used to create the final share XORd with the first share and
message. `Reconstruct` XORs all the shares together to get the message, again same as 2-of-2.

To prove that this scheme is secure, a calling library must not behave any differently when the scheme is carried out on either message; i.e. these two libraries must behave identically:

\begin{center}
\titlecodebox{$\lib{tsss-L}^\Sigma$}{
  \underline{Share($m_L, m_R, U$):} \\
  \> if $|U| \ge 3$: return \bit{err} \\
  \> $s_1 \gets \bits^\ell$ \\
  \> $s_2 \gets \bits^\ell$ \\
  \> $s_3 := s_1 \oplus s_2 \oplus m_L$ \\
  \> return $\{s_i \ | \ i \in U\}$
}
$\equiv$
\titlecodebox{$\lib{tsss-R}^\Sigma$}{
  \underline{Share($m_L, m_R, U$):} \\
  \> if $|U| \ge 3$: return \bit{err} \\
  \> $s_1 \gets \bits^\ell$ \\
  \> $s_2 \gets \bits^\ell$ \\
  \> $s_3 := s_1 \oplus s_2 \oplus m_R$ \\
  \> return $\{s_i \ | \ i \in U\}$
}
\end{center}

We do this with a hybrid proof. The library can be split into multiple identical branches without change, then consolidated into branches which do not return one of the share values:

\begin{center}
\framebox{
  \codebox{
    \underline{Share($m_L, m_R, U$):} \\
    \> if $|U| \ge 3$: return \bit{err} \\
    \> \hl{if $U = \{1\}$} \\
    \> \> $s_1 \gets \bits^\ell$ \\
    \> \> $s_2 \gets \bits^\ell$ \\
    \> \> $s_3 := s_1 \oplus s_2 \oplus m_L$ \\
    \> \> return $\{s_i \ | \ i \in U\}$ \\
    \> \hl{elseif $U = \{2\}$} \\
    \> \> $s_1 \gets \bits^\ell$ \\
    \> \> $s_2 \gets \bits^\ell$ \\
    \> \> $s_3 := s_1 \oplus s_2 \oplus m_L$ \\
    \> \> return $\{s_i \ | \ i \in U\}$ \\
    \> \hl{elseif $U = \{3\}$} \\
    \> \> $s_1 \gets \bits^\ell$ \\
    \> \> $s_2 \gets \bits^\ell$ \\
    \> \> $s_3 := s_1 \oplus s_2 \oplus m_L$ \\
    \> \> return $\{s_i \ | \ i \in U\}$ \\
  }
  \codebox{
    \> \\
    \> \\
    \> \hl{if $U = \{1, 2\}$} \\
    \> \> $s_1 \gets \bits^\ell$ \\
    \> \> $s_2 \gets \bits^\ell$ \\
    \> \> $s_3 := s_1 \oplus s_2 \oplus m_L$ \\
    \> \> return $\{s_i \ | \ i \in U\}$ \\
    \> \hl{elseif $U = \{1, 3\}$} \\
    \> \> $s_1 \gets \bits^\ell$ \\
    \> \> $s_2 \gets \bits^\ell$ \\
    \> \> $s_3 := s_1 \oplus s_2 \oplus m_L$ \\
    \> \> return $\{s_i \ | \ i \in U\}$ \\
    \> \hl{elseif $U = \{2, 3\}$} \\
    \> \> $s_1 \gets \bits^\ell$ \\
    \> \> $s_2 \gets \bits^\ell$ \\
    \> \> $s_3 := s_1 \oplus s_2 \oplus m_L$ \\
    \> \> return $\{s_i \ | \ i \in U\}$ \\
    \> \hl{else return $\emptyset$}
  }
}
$\equiv$
\fcodebox{
  \underline{Share($m_L, m_R, U$):} \\
  \> if $|U| \ge 3$: return \bit{err} \\
  \> \hl{if $U \in \{\{1\}, \{2\}, \{1, 2\}\}$}
      // does not return $s_3$ \\
  \> \> $s_1 \gets \bits^\ell$ \\
  \> \> $s_2 \gets \bits^\ell$ \\
  \> \> $s_3 := s_1 \oplus s_2 \oplus m_L$ \\
  \> \> return $\{s_i \ | \ i \in U\}$ \\
  \> \hl{elseif $U \in \{\{3\}, \{1, 3\}\}$}
      // does not return $s_2$ \\
  \> \> $s_1 \gets \bits^\ell$ \\
  \> \> $s_2 \gets \bits^\ell$ \\
  \> \> $s_3 := s_1 \oplus s_2 \oplus m_L$ \\
  \> \> return $\{s_i \ | \ i \in U\}$ \\
  \> \hl{elseif $U = \{2, 3\}$}
      // does not return $s_1$ \\
  \> \> $s_1 \gets \bits^\ell$ \\
  \> \> $s_2 \gets \bits^\ell$ \\
  \> \> $s_3 := s_1 \oplus s_2 \oplus m_L$ \\
  \> \> return $\{s_i \ | \ i \in U\}$ \\
  \> else return $\emptyset$
}
\end{center}

\pagebreak

Due to properties of XOR, generating the shares can be done in any order
without changing functionality, so we can rearrange the order in which these are
generated as below. Each branch does not return the finally-generated share, so
the message used can be changed without affecting the return value.

\begin{center}
$\equiv$
\fcodebox{
  \underline{Share($m_L, m_R, U$):} \\
  \> if $|U| \ge 3$: return \bit{err} \\
  \> if $U \in \{\{1\}, \{2\}, \{1, 2\}\}$ \\
  \> \> $s_1 \gets \bits^\ell$ \\
  \> \> $s_2 \gets \bits^\ell$ \\
  \> \> $s_3 := s_1 \oplus s_2 \oplus m_L$ \\
  \> \> return $\{s_i \ | \ i \in U\}$ \\
  \> elseif $U \in \{\{3\}, \{1, 3\}\}$ \\
  \> \> $s_1 \gets \bits^\ell$ \\
  \> \> \hl{$s_3 \gets \bits^\ell$} \\
  \> \> \hl{$s_2 := s_1 \oplus s_3 \oplus m_L$} \\
  \> \> return $\{s_i \ | \ i \in U\}$ \\
  \> elseif $U = \{2, 3\}$ \\
  \> \> $s_2 \gets \bits^\ell$ \\
  \> \> \hl{$s_3 \gets \bits^\ell$} \\
  \> \> \hl{$s_1 := s_2 \oplus s_3 \oplus m_L$} \\
  \> \> return $\{s_i \ | \ i \in U\}$ \\
  \> else return $\emptyset$
}
$\equiv$
\fcodebox{
  \underline{Share($m_L, m_R, U$):} \\
  \> if $|U| \ge 3$: return \bit{err} \\
  \> if $U \in \{\{1\}, \{2\}, \{1, 2\}\}$ \\
  \> \> $s_1 \gets \bits^\ell$ \\
  \> \> $s_2 \gets \bits^\ell$ \\
  \> \> $s_3 := s_1 \oplus s_2 \oplus \mhl{m_R}$ \\
  \> \> return $\{s_i \ | \ i \in U\}$ \\
  \> elseif $U \in \{\{3\}, \{1, 3\}\}$ \\
  \> \> $s_1 \gets \bits^\ell$ \\
  \> \> $s_3 \gets \bits^\ell$ \\
  \> \> $s_2 := s_1 \oplus s_3 \oplus \mhl{m_R}$ \\
  \> \> return $\{s_i \ | \ i \in U\}$ \\
  \> elseif $U = \{2, 3\}$ \\
  \> \> $s_2 \gets \bits^\ell$ \\
  \> \> $s_3 \gets \bits^\ell$ \\
  \> \> $s_1 := s_2 \oplus s_3 \oplus \mhl{m_R}$ \\
  \> \> return $\{s_i \ | \ i \in U\}$ \\
  \> else return $\emptyset$
}
\end{center}

These branches can now be rearranged as to be identical and recondensed into a
single branch, which is identical to $\lib{tsss-R}^\Sigma$:

\begin{center}
$\equiv$
\fcodebox{
  \underline{Share($m_L, m_R, U$):} \\
  \> if $|U| \ge 3$: return \bit{err} \\
  \> if $U \in \{\{1\}, \{2\}, \{1, 2\}\}$ \\
  \> \> $s_1 \gets \bits^\ell$ \\
  \> \> $s_2 \gets \bits^\ell$ \\
  \> \> $s_3 := s_1 \oplus s_2 \oplus m_R$ \\
  \> \> return $\{s_i \ | \ i \in U\}$ \\
  \> elseif $U \in \{\{3\}, \{1, 3\}\}$ \\
  \> \> $s_1 \gets \bits^\ell$ \\
  \> \> \hl{$s_2 \gets \bits^\ell$} \\
  \> \> \hl{$s_3 := s_1 \oplus s_2 \oplus m_R$} \\
  \> \> return $\{s_i \ | \ i \in U\}$ \\
  \> elseif $U = \{2, 3\}$ \\
  \> \> $s_1 \gets \bits^\ell$ \\
  \> \> \hl{$s_2 \gets \bits^\ell$} \\
  \> \> \hl{$s_3 := s_1 \oplus s_2 \oplus m_R$} \\
  \> \> return $\{s_i \ | \ i \in U\}$ \\
  \> else return $\emptyset$
}
$\equiv$
\colorbox{highlightcolor}{
\titlecodebox{$\lib{tsss-R}^\Sigma$}{
  \underline{Share($m_L, m_R, U$):} \\
  \> if $|U| \ge 3$: return \bit{err} \\
  \> $s_1 \gets \bits^\ell$ \\
  \> $s_2 \gets \bits^\ell$ \\
  \> $s_3 := s_1 \oplus s_2 \oplus m_R$ \\
  \> return $\{s_i \ | \ i \in U\}$
}
}
\end{center}

\pagebreak

### 2. I used 2-out-of-10 Shamir secret sharing over $\Z_{11}$ to share a secret. Alice’s share was (4,6) and Bob’s was (7,1). Two shares should be enough to reconstruct the secret. So, what was the secret, and what were the other 8 shares? Show your work.

2-of-10 secret sharing means the polynomial is linear -- degree 1. ($d + 1 = 2$)
To reconstruct the original polynomial, and thus the secret, we need to connect the polynomial in mod-11 space. This is done by reconstructing the LaGrange polynomials for each point:

$$l_0 = \frac{x - x_1}{x_0 - x_1} = \frac{x - 7}{4 - 7} = -\frac{x - 7}{3}$$
$$l_1 = \frac{x - x_0}{x_1 - x_0} = \frac{x - 4}{7 - 4} = \frac{x - 4}{3}$$

The original polynomial can now be reconstructed from the LaGrange terms:

\begin{align*}
f(x) &= y_0 l_0 + y_1 l_1 \\
     &= -6 \frac{x - 7}{3} + \frac{x - 4}{3} \\
     &= -\frac{5x}{3} + \frac{38}{3}
\end{align*}

The original secret is $f(0) = \frac{38}{3} \% 11 = 9$.
