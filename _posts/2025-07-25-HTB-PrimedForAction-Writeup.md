---
title: 'HTB PrimedForAction Writeup (challenge)'
date: 2025-07-25
permalink: /posts/2025/07/HTB_PrimedForAction_Writeup/
tags:
  - HTB
  - writeup
  - very-easy
  - challenge
---

A writeup of the Hack The Box challenge "PrimedForAction" with very easy difficulty

# Primed for Action (very easy)

CHALLENGE DESCRIPTION

Intelligence units have intercepted a list of numbers. They seem to be used in a peculiar way: the adversary seems to be sending a list of numbers, most of which are garbage, but two of which are prime. These 2 prime numbers appear to form a key, which is obtained by multiplying the two. Your answer is the product of the two prime numbers. Find the key and help us solve the case.

So we need to write code:

The input rule needs to contain multiple numbers, we filter the 2 prime numbers out;

The key is the product of the 2 prime numbers;

Print it out;

The code that works:

```python
# take in the number line, e.g. "10 17 22 13 5 8"
numbers = input().split()

# convert to integers
numbers = list(map(int, numbers))

# function to check if the number is prime
def is_prime(x):
    if x < 2:
        return False
    if x == 2:
        return True
    if x % 2 == 0:
        return False
    for i in range(3, int(x**0.5) + 1, 2):
        if x % i == 0:
            return False
    return True

# filter the prime numbers
primes = [num for num in numbers if is_prime(num)]

# There are 2 prime numbers
if len(primes) != 2:
    print("")
else:
    key = primes[0] * primes[1]
    print(key)
```

![image.png](/images/challenges/HTB_PrimedForAction/image.png)