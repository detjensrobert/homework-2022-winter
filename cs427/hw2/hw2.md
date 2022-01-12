---
header-includes:
  - \usepackage{mathtools}
  - \include{../macros.tex}
mainfont: DejaVuSans.ttf
---

# CS 427 Homework 2

## Robert Detjens

---

### 1. Formally show that non-zero `KeyGen` does not satisfy one-time secrecy.

\titlecodebox{$\lib{cpa-L}^\Sigma$}{
    $k \gets \Sigma.\KeyGen$ \\[8pt]

    \underline{$\subname{challenge}(m_L, m_R)$:} \\
    \> $c := \Sigma.\Enc(k,\mathhighlight{m_L})$ \\
    \> return $c$
}

### 2. Use a hybrid proof to prove that if $\Sigma$ satisfies one-time secrecy, then so does $\Sigma^2$.
