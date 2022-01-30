---
header-includes:
  - \include{../macros.tex}
---

# CS 427 Homework 6

## Robert Detjens

---

## Question 1

*Let $F$ be a  secure PRF with $in = out = \lambda$. Define the following function:*

$$F'(k, m) = F(k, m) || F(k, F(k, m))$$

*Show that $F'$ is not a secure PRF.*

## Question 2

*Show that a 2-round keyed Feistel cipher cannot be a secure PRP, no matter what its round functions are. Your attack should work without knowing the round keys, and it should work even with different (independent) round keys. Hint: A successful attack requires two queries.*

## Question 3

*Let $F$ be a secure PRF with $\lambda$-bit outputs, and let $G$ be a PRG with stretch $l$. Define:*

$$F'(k, r) = G(F(k, r))$$

*So $F'$ has outputs of length $\lambda + l$. Prove that $F'$ is a secure PRF.*
