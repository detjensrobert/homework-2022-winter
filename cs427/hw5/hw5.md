---
header-includes:
  - \include{../macros.tex}
---

# CS 427 Homework 5

## Robert Detjens

---

## Question 1

*Let $G : \bits^\lambda \rightarrow \bits^{3\lambda}$ be a secure
length-tripling PRG. For each function below, state whether it is also a secure
PRG. If the function is a secure PRG, give a proof. If not, then describe a
successful distinguisher and explicitly compute its advantage.*

### (a)

\begin{center}
\codebox{
  \codebox{
    Given PRG:
  }
  \framebox{
    \codebox{
      \underline{$H(s)$:} \\
      \> $x := G(s)$ \\
      \> $y := G(\bit{0}^\lambda)$ \\
      \> return $x||y$
    }
  }
}
\end{center}

This $H()$ is not secure, as the seed given to the second call to $G()$ is not
uniformly sampled, instead fixed at $\zerol$ and directly outputted. Thus, an
attacking library knows the seed to $G()$ and can distinguish between a uniformly
sampled output and output from $G(\zerol)$. One such attack is shown below:


\begin{center}
\titlecodebox{$\A$}{
  $x || y :=$ QUERY() \\
  return $y \? G(\zerol)$
}
$\link$
\titlecodebox{$\lib{prg-real}^H$}{
  \underline{QUERY():} \\
  \> $s \gets \bits^\lambda$ \\
  \> return $G(s) || G(\zerol)$
}

\titlecodebox{$\A$}{
  $x || y :=$ QUERY() \\
  return $y \? G(\zerol)$
}
$\link$
\titlecodebox{$\lib{prg-rand}^H$}{
  \underline{QUERY():} \\
  \> $r \gets \bits^{6\lambda}$ \\
  \> return $r$
}
\end{center}

\begin{align*}
Pr[\A \link \lib{prg-real}^H \implies true] &= 1 \\
Pr[\A \link \lib{prg-rand}^H \implies true] &= \frac{1}{2^{3\lambda}}
\end{align*}

\pagebreak

### (b)

\begin{center}
\codebox{
    \codebox{
        Given PRG:
    }
    \framebox{
        \codebox{
            \underline{$H(s)$:} \\
            \> $x := G(s)$ \\
            \> $y := G(\bit{0}^\lambda)$ \\
            \> return $x \oplus y$
        }
    }
}
\end{center}

This $H()$ is secure. To prove this, we must show that $\lib{prg-real}^H$ is indistinguishable from $\lib{prg-rand}^H$. A hybrid proof follows:

\begin{center}
\titlecodebox{$\lib{prg-real}^H$}{
  \underline{$QUERY_H():$} \\
  \> $s \gets \bits^\lambda$ \\
  \> $x := G(s)$ \\
  \> $y := G(\zerol)$ \\
  \> return $x \xor y$
}
$\stackrel{?}{\equiv}$
\titlecodebox{$\lib{prg-rand}^H$}{
  \underline{$QUERY_H():$} \\
  \> $s \gets \bits^{3\lambda}$ \\
  \> return $s$
}
\end{center}

The initial call to $G()$ can be brought out into a subroutine:

\begin{center}
\fcodebox{
  \underline{$QUERY_H():$} \\
  \> \hl{$x := QUERY_G()$} \\
  \> $y := G(\zerol)$ \\
  \> return $x \xor y$
}
$\link$
\oltitlecodebox{$\lib{prg-real}^G$}{
  \underline{$QUERY_G():$} \\
  \> $s \gets \bits^\lambda$ \\
  \> return $G(s)$
}
\end{center}

We know that G is a secure PRG, and as such can replace $\lib{prg-real}^G$ with
the indistinguishable $\lib{prg-rand}^G$.

\begin{center}
\fcodebox{
  \underline{$QUERY_H():$} \\
  \> $x := QUERY_G()$ \\
  \> $y := G(\zerol)$ \\
  \> return $x \xor y$
}
$\link$
\oltitlecodebox{$\lib{prg-rand}^G$}{
  \underline{$QUERY_G():$} \\
  \> $s \gets \bits^{3\lambda}$ \\
  \> return $s$
}
\end{center}

We can inline this subroutine back into $QUERY_H()$:

\begin{center}
\fcodebox{
  \underline{$QUERY_H():$} \\
  \> \hl{$x \gets \bits^{3\lambda}$} \\
  \> $y := G(\zerol)$ \\
  \> return $x \xor y$
}
\end{center}

We can now break out the XOR and $x$ into another subroutine, defining $\ell = 3\lambda$:

\begin{center}
\fcodebox{
  \underline{$QUERY_H():$} \\
  \> $y := G(\zerol)$ \\
  \> return \hl{$EAVESDROP(y)$}
}
$\link$
\oltitlecodebox{$\lib{otp-real}$}{
  \underline{$EAVESDROP(m):$} \\
  \> $k \gets \bits^{\ell}$ \\
  \> return $k \xor m$
}
\end{center}

$\lib{otp-rand}$ is equivalent to $\lib{otp-real}$, and can be substituted without changing the behaviour of the calling program:

\begin{center}
\fcodebox{
  \underline{$QUERY_H():$} \\
  \> $y := G(\zerol)$ \\
  \> return $EAVESDROP(y)$
}
$\link$
\oltitlecodebox{$\lib{otp-rand}$}{
  \underline{$EAVESDROP(m):$} \\
  \> $c \gets \bits^{\ell}$ \\
  \> return $c$
}
\end{center}

The subroutine can be inlined:

\begin{center}
\fcodebox{
  \underline{$QUERY_H():$} \\
  \> $y := G(\zerol)$ \\
  \> \hl{$c \gets \bits^{3\lambda}$} \\
  \> return \hl{$c$}
}
\end{center}

$y$ is no longer used and can be removed without affecting execution, and this
hybrid library is now equivalent to $\lib{prg-rand}^H$:


\begin{center}
\fcodebox{
  \underline{$QUERY_H():$} \\
  \> \hl{$c \gets \bits^{3\lambda}$} \\
  \> return \hl{$c$}
}
$\equiv$
\titlecodebox{$\lib{prg-rand}^H$}{
  \underline{$QUERY_H():$} \\
  \> $s \gets \bits^{3\lambda}$ \\
  \> return $s$
}
\end{center}

\pagebreak

### (c)

\begin{center}
\codebox{
    \codebox{
        Given PRG:
    }
    \framebox{
        \codebox{
            \underline{$H(s)$:} \\
            \> $x||y||z := G(s)$ \\
            \> $w := G(x)$ \\
            \> return $x||y||z||w$
        }
    }
}
\end{center}

This $H()$ is not secure, as the seed given to the second call to $G()$ is not
uniformly sampled, instead reliant on $x$ which is exposed as part of the
output. Thus, an attacking library knows the seed to $G()$ and can distinguish
between a uniformly sampled output and the output from $G(x)$. One such attack
is shown below:


\begin{center}
\titlecodebox{$\A$}{
  $x || y || z || w_1 || w_2 || w_3 :=$ QUERY() \\
  $w := w_1 || w_2 || w_3$ \\
  return $w \? G(x)$
}
$\link$
\titlecodebox{$\lib{prg-real}^H$}{
  \underline{QUERY():} \\
  \> $x || y || z \gets \bits^{3\lambda}$ \\
  \> return $x || y || z || G(x)$
}

\titlecodebox{$\A$}{
  $x || y || z || w_1 || w_2 || w_3 :=$ QUERY() \\
  $w := w_1 || w_2 || w_3$ \\
  return $w \? G(x)$
}
$\link$
\titlecodebox{$\lib{prg-rand}^H$}{
  \underline{QUERY():} \\
  \> $r \gets \bits^{6\lambda}$ \\
  \> return $r$
}
\end{center}

\begin{align*}
Pr[\A \link \lib{prg-real}^H \implies true] &= 1 \\
Pr[\A \link \lib{prg-rand}^H \implies true] &= \frac{1}{2^{3\lambda}}
\end{align*}
