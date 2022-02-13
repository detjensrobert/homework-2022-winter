---
header-includes:
  - \include{../macros.tex}
---

# CS 427 Homework 8

## Robert Detjens

---

## Question 1

*Let $F$ be a secure PRP with block length $\lambda$. Consider the following encryption mode:*

\begin{center}
\fcodebox{
	\underline{$\Enc(k, m_1 || ... || m_l)$:} \\
	\> $c_0 \gets \bits^\lambda$ \\
	\> for $i=1$ to $l$: \\
	\> \> $c_i := F(k, m_i) \oplus c_{i-1}$ \\
	\> return $c_0 || ... || c_l$
}
\end{center}

*(a) Write the corresponding $\Dec$ algorithm.*

\fcodebox{
	\underline{$\Dec(k, c_0 || ... || c_l)$:} \\
	\> for $i=1$ to $l$: \\
	\> \> $m_i := F^{-1}(k, c_i \oplus c_{i-1}) $ \\
	\> return $m_1 || ... || m_l$
}

*(b) Prove this mode does not achieve CPA\$ security*

Consider an attacking library $\A$ linked against two libraries $\lib{CPA\$-real}$ and $\lib{CPA\$-rand}$:

\begin{center}
\titlecodebox{$\A$}{
	$a_0 || ... || a_l := \Eavesdrop(\{\bit{0}\}^{\lambda * l})$ \\
	return $a_1 \xor a_0 == a_2 \xor a_1$
}
\qquad
\titlecodebox{$\lib{CPA\$-real}$}{
  $k \gets \bits^\lambda$ \\
  \\
  \underline{$\Eavesdrop(m \in \M)$:} \\
	\> $c_0 \gets \bits^\lambda$ \\
	\> for $i=1$ to $l$: \\
	\> \> $c_i := F(k, m_i) \oplus c_{i-1}$ \\
	\> return $c_0 || ... || c_l$
}
\qquad
\titlecodebox{$\lib{CPA\$-rand}$}{
  \underline{$\Eavesdrop(m \in \M)$:} \\
  \> $x \gets \bits^{\lambda * l}$ \\
  \> return $c$
}
\end{center}

When $\A$ is linked against $\lib{CPA\$-real}$, $\lib{CPA\$-real}$ leaks every $F(k, m_i)$, as each $c_i$ can be XOR'd with $c_{i-1}$ to get $F(k, m_i)$. As $k$ is constant across all invocations of $F$ and $F$ is (by definition of being a PRP) deterministic, a repeated input such as all zeros will produce the same output for each call to $F$. Thus, every $F(k, m_i)$ will be the same and can be checked for.

When linked against $\lib{CPA\$-rand}$, there is no such guarantee.

\begin{align*}
Pr[\A \link \lib{CPA\$-real} \implies true] &= 1 \\
Pr[\A \link \lib{CPA\$-rand} \implies true] &= \frac{1}{2^{\lambda * l}}
\end{align*}

As an attacker can distinguish between $\Enc$ and a random sampling, this $\Enc$ does not have CPA$-security.

\pagebreak

## Question 2

*Implementers are sometimes cautious about IVs in block cipher modes and may attempt to “protect” them. One idea for protecting an IV is to prevent it from directly appearing in the ciphertext. The modified CBC encryption below sends the IV through the block cipher before including it in the ciphertext:*

\begin{center}
\fcodebox{
	\underline{$\subname{Enc}(k, m_1 || ... || m_l)$:} \\
	\> $c_0 \gets \bits^{blen}$ \\
	\> \highlightline{$c'_0 := F(k, c_0)$} \\
	\> for $i=1$ to $l$: \\
	\> \> $c_i := F(k, m_i \oplus c_{i-1})$ \\
	\> return $\mathhighlight{c'_0} || c_1 || ... || c_l$
}
\end{center}

*This ciphertext can be decrypted by first computing $c_0 := F^{-1}(k, c'_{0})$ and then doing usual CBC decryption on $c_0 || ... || c_l$.*

*Show that this new scheme is not CPA secure.*

Consider an attacking library $\A$ linked against two libraries $\lib{CPA-L}$ and $\lib{CPA-R}$:

\begin{center}
\titlecodebox{$\A$}{
	$a_0 || ... || a_l := \Eavesdrop(\{\bit{0}\}^{\lambda * l}, \{\bit{1}\}^{\lambda * l})$ \\
	return $a_0 \? a_1$
}
\end{center}

\begin{center}
\titlecodebox{$\lib{CPA-L}$}{
  $k \gets \bits^\lambda$ \\
  \\
  \underline{$\Eavesdrop((m_1 || ... || m_l)_L, (m_1 || ... || m_l)_R \in \M)$:} \\
	\> $c_0 \gets \bits^{blen}$ \\
	\> $c'_0 := F(k, c_0)$ \\
	\> for $i=1$ to $l$: \\
	\> \> $c_i := F(k, (m_i)_L) \oplus c_{i-1}$ \\
	\> return $c'_0 || c_1 || ... || c_l$
}
\qquad
\titlecodebox{$\lib{CPA-R}$}{
  $k \gets \bits^\lambda$ \\
  \\
  \underline{$\Eavesdrop((m_1 || ... || m_l)_L, (m_1 || ... || m_l)_R \in \M)$:} \\
	\> $c_0 \gets \bits^{blen}$ \\
	\> $c'_0 := F(k, c_0)$ \\
	\> for $i=1$ to $l$: \\
	\> \> $c_i := F(k, (m_i)_R) \oplus c_{i-1}$ \\
	\> return $c'_0 || c_1 || ... || c_l$
}
\end{center}

As $F$ is (by definition of being a PRP) deterministic and $k$ is constant across invocations, when the first input block is $\zerol$, the input to $F(k, m_1 \xor c_0)$ will be $F(k, \zerol \xor c_0) = F(k, c_0) = c'_0$. Thus, an attacking library can use a input string of all zeros and check that the first two blocks  $c'_0$ and $c_1$ are the same. With any other input text, the input to $F$ for $c_1$ will be distinct and thus the output will be completely different.

\begin{align*}
Pr[\A \link \lib{CPA-L} \implies true] &= 1 \\
Pr[\A \link \lib{CPA-R} \implies true] &= \frac{1}{2^{\lambda * l}}
\end{align*}

As an attacker can distinguish between the two libraries, this scheme is not CPA secure.
