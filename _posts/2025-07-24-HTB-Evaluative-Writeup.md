---
title: 'HTB Evaluative Writeup (challenge)'
date: 2025-07-24
permalink: /posts/2025/07/HTB_Evaluative_Writeup/
tags:
  - HTB
  - writeup
  - very-easy
  - challenge
---

A writeup of the Hack The Box challenge "Evaluative" with very easy difficulty

# Evaluative (very easy)

CHALLENGE DESCRIPTION

A rogue bot is malfunctioning, generating cryptic sequences that control secure data vaults. Your task? Decode its logic and compute the correct output before the system locks you out!

![image.png](/images/challenges/HTB_Evaluative/image.png)

The task is to evaluate the polyonomy from the:

`a₀ + a₁·x + a₂·x² + a₃·x³ + ... + a₈·x⁸`
And the x value.

In the example:

Input:
Coefficients: `1 -2 3 -4 5 -6 7 -8 9`
X = `5`

The assignment: Write code that evaluates the polyonomy. We can achieve this with a simple loop or with `enumerate`  .

```python
# take in the number
coeffs = list(map(int, input().split()))

# Read value X
x = int(input())

# Calculate the polyonomy value
result = 0
for i, a in enumerate(coeffs):
    result += a * (x ** i)

# Print the result
print(result)
```

Running this code:

![image.png](/images/challenges/HTB_Evaluative/image%201.png)

![image.png](/images/challenges/HTB_Evaluative/image%202.png)