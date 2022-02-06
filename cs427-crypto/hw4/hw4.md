---
header-includes:
  - \include{../macros.tex}
---

# CS 427 Homework 4

## Robert Detjens

---

### 1. Write a program to compute $BirthdayProb(q, N)$ exactly.

$$
BirthdayProb(q, N) = 1 - \prod_{i = 1}^{q - 1}{(1 - \frac{i}N)} = 1 - (1 - \frac1N)(1 - \frac2N)...(1 - \frac{q - 1}N)
$$

```rb
# in Ruby:
def birthday(q, n)
  1 - (1..q-1).map{ |i| 1 - i.to_f / n }.reduce(:*)
end
```

(a) What is the smallest $q$ such that $BirthdayProb(q, 356) \ge 0.99$?

    ```rb
    (2..356).find { |q| birthday(q, 356) > 0.99 }
    # => 57 (0.9912713186154262)
    ```

(b) What is the smallest $q$ such that $BirthdayProb(q, 2^{16}) \ge 0.99$?

    ```rb
    (2..2**16).find { |q| birthday(q, 2**16) > 0.99 }
    # => 776 (0.9900135276209049)
    ```

### 2. Write a program to repeatedly fetch 2 bytes (16 bits) from your system’s built-in random number generator, and stop when it sees the first repeated sample.

```rb
require 'set'

def try
  prev = Set[]
  (1..).each { |i| break i unless prev.add? Random.bytes(2) }
end
```

(a) Based on the rule-of-thumb from the book/videos, how many samples are expected before seeing a repeat? Please
justify/explain your answer.

    We have $2^{16}$ possible values here, so we expect $\sqrt{2^{16}} = 256$ samples before seeing a collision.

(b) Run your program many times and report the average number of samples before seeing a repeat. Does it match what you
expected?

    After 1,000,000 tries, average was 321.795234.

    While this is off by around 100 samples, with regard to the possible sample space of $2^{16}$ this difference is
    miniscule and within the range of the rule of thumb.

### 3. Suppose you want to enforce password rules so that at least $2{128}$ passwords satisfy the rules. How many characters long must the passwords be, in each of these cases?

$(numchars)^\lambda >= 2^{128}$, solve for $\lambda$:

(a) Passwords consist of `[a-z]`.

    28 chars needed for 26 possible characters

(b) Passwords consist of `[a–zA–Z]`.

    23 chars needed for 52 possible characters

(c) Passwords consist of `[a-zA-Z0-9]`.

    22 chars needed for 62 possible characters

(d) Passwords consist of `[a-zA-Z0-9]` + ``!@#$%^&*(){}[]-_=+~`"':;?>.<, \|``.

    20 chars needed for 95 possible characters
