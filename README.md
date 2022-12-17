# Number Theory
This repository for essential number theory laws and methods

## Modular Arithmetic 

- x % n = r
  - 0 <= r <= n-1
- x = q * n + r
- ((a - b) % c + c) % c &rarr; **This is genral formula for C++**
- (a + b) % c = (a % c + b % c) % c
- (a * b) % c = (a % c * b % c) % c
- (a - b) % c = (a % c - b % c + c) % c
- (a / b) % c = (a % b * b<sup>-1</sup> % c) % c

---

## Binary Exponentiation (Fast power)

### The Basic method for solving **x<sup>n</sup>** &rarr; O(N)

```
def power(x,n):
    ans = 1

    for i in range(n):
        ans *= x
    
    return ans
```

### The Fastest method for solving **x<sup>n</sup>** &rarr; O(Log N)

```
def fast_power(x,n):
    ans = 1

    while n:
        if n % 2:
            ans *= x
        x *= x
        n //= 2
    
    return ans
```
---
## Fermat’s little theorem & Primality testing

Fermat’s little theorem : if **P** is prime number and **a** is any positive integer not divisible by **P** then: <br> $a^{P - 1}$ = 1 (mod P) <br> for all values of **a** between 1 and **P-1**

```
import random
def fermat(n: int,k:int = 128) -> bool:
    for _ in range(k):
        x = random.randint(1,n-1)
        if pow(x,n-1,n) != 1:
            return False
    return True
```

---
## Factorization 

**Factorization means to find all factors (divisors) of a number**

### Basic method to get all factors &rarr; O(N)

```
def factorization(n):
    factors = []

    for i in range(1,n+1):
        if n % i == 0:
            factors.append(i)
            
    return factors
```

### Fastest method to get all factors &rarr; O(Sqrt(N))

```
def factorization(n):
    factors = []

    i = 1
    while i * i <= n:
        if n % i == 0:
            if n // i == i:
                factors.append(i)
            else:
                factors.append(i)
                factors.append(n // i)
        i += 1
    
    return factors
```
---
## Prime Factorization

```
def prime_factorization(n):
    prime_factors = []

    i = 2
    end = n

    while i*i <= end:
        while n % i == 0:
            prime_factors.append(i)
            n //= i
        i += 1
    if n != 1:
        prime_factors.append(n)
    
    return prime_factors
```
---
## Euclidean Algorithm Proof
Note:
- gcd &rarr; Greatest Common Divisor
- co-primes &rarr; have no common divisors

Assume that gcd(r<sub>0</sub>,r<sub>1</sub>) &rarr; g and r<sub>0</sub> > r<sub>1</sub><br>Ex: gcd(20,15)

r<sub>0</sub> = g * x <br> r<sub>1</sub> = g * y <br> where x > y, and x and y are co-primes
<br> by subtracting r<sub>1</sub> from r<sub>0</sub><br> 
r<sub>0</sub> - r<sub>1</sub> = (g * x) - (g * y) = g * (x - y) <br>it's ovious that (x - y) and y are also co-primes<br>
and by comparing the two equations:<br>
- x = q * n + r &rarr; r = x - q * n
- r<sub>0</sub> - r<sub>1</sub> = (g * x) - (g * y)

we found that r in the first equation is exactly r<sub>0</sub> - r<sub>0</sub> in the secod on<br>
we can refer to r<sub>0</sub> - r<sub>1</sub> with r<sub>2</sub>

 then we can say that gcd(r<sub>0</sub>,r<sub>1</sub>) &rarr; gcd(r<sub>1</sub>,r<sub>0</sub> - r<sub>1</sub>) &rarr; gcd(r<sub>1</sub>,r<sub>2</sub>) &rarr; gcd(g * y , g * (x - y))

As we can repeat this procces as much as we want<br>
we repeat it until it becomes gcd(r<sub>n</sub>,0)<br>
and as you can observe that we get the result of gcd(r<sub>0</sub>,r<sub>1</sub>) &rarr; r<sub>n</sub>.

And as you observe, we can replace the subtraction with modulus (%).

---
## Greatest Common Divisor GCD (Euclidean Algorithm)

```
def GCD(n: int, m: int) -> int:
    if m == 0:
        return n
    return GCD(m, n % m)
```

Note: When GCD(n,m) = 1 &rarr; called Co-Prime

---
## Least Common Multiple LCM

```
def LCD(n: int, m: int) -> int:
    return (n*m) // GCD(n,m)
```
---
## Extended Euclidean Algorithm

gcd(r<sub>0</sub>,r<sub>1</sub>) = s * r<sub>0</sub> + t * r<sub>1</sub><br>
our goal now is to find the gcd(r<sub>0</sub>,r<sub>1</sub>) and the values of s and t.

As we know earlier<br>


| i | gcd(r<sub>i-2</sub>,r<sub>i-1</sub>) | r<sub>i-2</sub> = r<sub>i-1</sub> * q<sub>i-1</sub> + r<sub>i</sub> | r<sub>i</sub> = [s<sub>i</sub>] * r<sub>0</sub> + [t<sub>i</sub>] * r<sub>1</sub> |
|-----| -------- | --------| ------ |
| 2 | gcd(973,301) | 973 = 3 * 301 + 70 | 70 = [1] * 973 + [-3] * 301<br>70 = [1] * r<sub>0</sub> + [-3] * r<sub>1</sub>
| 3 | gcd(301,70)  | 301 = 4 * 70 + 21 | 21 = 301 + (-4) * 70<br> 21 = 301 + (-4) * (973 + (-3) * 301)<br>21 = [-4] * 973 + [13] * 301<br>21 = [-4] * r<sub>0</sub> + [13] * r<sub>1</sub>
| 4 | gcd(70,21) | 70 = 3 * 21 + 7 | 7 = 70 + (-3) * 21<br>7 = r<sub>2</sub> + (-3) * r<sub>3</sub><br>7 = ( [1] * r<sub>0</sub> + [-3] * r<sub>1</sub> ) + (-3) * ( [-4] * r<sub>0</sub> + [13] * r<sub>1</sub> )<br>7 = [13] * r<sub>0</sub> + [-42] * r<sub>1</sub>|
| _ | gcd(21,7) | 21 = 3 * 7 + 0 | _ |

As you observe that we go through this process untill the r<sub>i</sub> becomes 0.

The final formula for r<sub>i</sub> , s<sub>i</sub> and t<sub>i</sub> is :
- r<sub>i</sub> = r<sub>i-2</sub> + (-q<sub>i-1</sub>) * r<sub>i-1</sub>
- s<sub>i</sub> = s<sub>i-2</sub> + (-q<sub>i-1</sub>) * s<sub>i-1</sub>
- t<sub>i</sub> = t<sub>i-2</sub> + (-q<sub>i-1</sub>) * t<sub>i-1</sub>

After all of these equations we know that :
- s<sub>0</sub> = 1 , s<sub>1</sub> = 0
- t<sub>0</sub> = 0 , t<sub>1</sub> = 1

```
def EEA(r0,r1):
    s0 , s1 = 1 , 0
    t0 , t1 = 0 , 1
    # swap the two numbers if the second one is greater than the first one.
    if r1 > r0:
        r0 , r1 = r1 , r0

    while r1:
        q = (r0 // r1) # compute the quotient
        # put r1 in r0 and compute the next r and put it in r1
        r0 , r1 = r1 , r0 - q * r1
        # put s1 in s0 and compute the next s and put it in s1
        s0 , s1 = s1 , s0 - q * s1
        # put t1 in t0 and compute the next t and put it in t1
        t0 , t1 = t1 , t0 - q * t1

    # r0 -> GCD
    # s0 -> s
    # t0 -> t
    return r0 , s0 , t0      # return t0 % the initial r0 to get positive value (for Modular Multiplication Inverse).
```

---
## Modular Multiplicative Inverse

The problem is a<sup>-1</sup> = ? (mod n)<br>a * a<sup>-1</sup> = 1 (mod n)

First before computing the a<sup>-1</sup> we should know that to have a modular multiplicative inverse of number a :
- gcd(n,a) = 1 &rarr; co-primes.
- a<sup>-1</sup> will be withen range of (0,n-1).

### Proof of modular multiplication inverse existance when gcd(n,a) = 1

as we do in Extend Euclidean Algorithm.<br>gcd(n,a) = n * s + a * t<br>1 = n * s + a * t (mod n)<br>1 = 0 + (a * t) mod n<br>then there is a modular multiplicative inverse of a.

### Brute force method &rarr; O(N)

```
def modular_multiplicative_inverse(a,n):
    a %= n
    for i in range(1,n):
        if (a * i) % n == 1:
            return i
    return None
```

### Extend Euclidean Algorithm method &rarr; O(Log N)
Apply the Extend Euclidean Algorithm and get the value of [t] this will be the modular multiplicative inverse.

```
def modular_multiplicative_inverse(a,n):
    return EEA(n,a)[2]
```
