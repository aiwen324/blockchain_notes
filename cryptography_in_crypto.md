# Introduction
This article will introduce 2 different protocol `Elgamal` and `RSA`. It will introduce two different cyclic group used for `Elgamal`. Although I am still not very clear how elliptic curve mapping works. 

# Elgamal

Based on Diffie-Hellman key exchange.

## Base Case Using $\mathbb{Z}_p^*$
**Notice**: **This is a cyclic group of order $p-1$**

Consider the following.

$(p, \alpha)$ are shared knowledge where $p$ is the modulus and $\alpha$ is the generator. Let's say Bob chooses a private key $d\in \{1, \dots, p-2\}$ and computes $\beta = \alpha^d \pmod{p}$. Then sends the public key $(p, \alpha, \beta)$ to Alice.

Then Alice chooses her own private key $i \in \{1, ..., p-2\}$ and computes the $K_E = \alpha^i \pmod{p}$. Then computes the **_shared secret_** $s = \beta^i \pmod{p}$. After this, encrypted the `message` $X$ with $Y = X\cdot s \pmod{p}$.

Alice sends $(Y, K_E)$ back to Bob. Bob could use $K_E$ to generate the ***shared secret*** $s = \beta^i = (\alpha^d)^i = (\alpha^i)^d = K_E^d \pmod{p}$.

Now since we've got the ***shared secret*** $s$, to decrypt the msg, we need $s^{-1}$. By Fermat's Little Theorem, we have $K_E^{p-1} \equiv 1 \pmod{p}$. Thus $K_E^d \cdot K_E^{p-1-d} \equiv 1 \pmod{p}$. Therefore, $s^{-1} \equiv K_E^{p-1-d} \pmod{p}$.

Once we caoculate $s^{-1}$, just use $Y\cdot s^{-1}$, we will get the decrypted msg $X$.


## Generalized Version of Elgamal
Given a cyclic group $G$ of order $q$ with generator $\alpha$. Let $e$ represent the unit element of $G$.

**Notice**: The conclulsion will not conflict with above base case, the order of $\mathbb{Z}_p^*$ is $p-1$ instead of $p$.

Bob chooses $d\in \{1, \dots, q-1\}$ and computes $\beta = \alpha^d$. Then send the public key consists of $(G, q, \alpha, \beta)$ to Alice.

Alice chooses $i\in \{1, \dots, q-1\}$ and computes $K_E = \alpha^i$ and shared secret $s = \beta^i$. Map message $X$ to the group element $x$ using a reversible mapping function. (This was not done in base case though, depends on the implementation, the base case might not need it). Alice encrypts the msg $X$ using $Y = x \cdot s$. Sends $K_E$ and $Y$ to Bob.

Then Bob calculates the shared secret $s = K_E^d$. Then calculate $s^{-1}$ by $s \cdot K_E^{q-d} = K_E^d \cdot K_E{q-d} = K_E^q = (\alpha^i)^q = (\alpha^q)^i = e$. Therefore, $s^{-1} = K_E^{q-d}$.

In the end, Bob can decrypt the msg by $x = Y \cdot s^{-1}$ and then map $x$ back to $X$ to get original msg $X$.

## Using Cyclic Group From Elliptic Curve

# Elliptic Curve
According to the course material, given an elliptic curve in the form $y^2 = x^3 + ax + b$ over certain field $F_p$ and a infinity point $\mathcal{O}$, we can define a cyclic group of order $n$ on it. And the order of n is **asymmetrically** equal to $p-1$.

## Elliptic Curve Elgamal Protocol
As long as it is a cyclic group, just checkout the procedure in the generalized version.

The big problem here is to find a injective function (or semi-injective) to map the msg $m$ onto the curve. 

Check the [answer](https://crypto.stackexchange.com/questions/14955/mapping-of-message-onto-elliptic-curve-and-reverse-it) and the following [paper](https://eprint.iacr.org/2013/373.pdf), they propose a way to use binary msg $m$ to find the x-axis $x$, then pick one of the corresponding $y$ value. 

Also in the link of [answer](https://crypto.stackexchange.com/questions/14955/mapping-of-message-onto-elliptic-curve-and-reverse-it) they mentioned a way without the need of mapping message to the curve. I've forgot those dual ring stuff so I give up.


# RSA
Bob chooses two large primes $(p, q)$. $n=pq, \phi(n)=(p-1)(q-1)$. Next choose private, public key pairs $(d, e)$ such that $de \equiv 1 \pmod{\phi(n)}$ where $gcd(e, \phi(n)) = 1$. Then $gcd(d, \phi(n)) = 1$ is straight forward. Bob then forwards $n$ and public key $e$ to Alice.

Alice takes msg $X$ (assume $X \in \{1, \dots, n\}$) and calculates $Y \equiv X^e \pmod{n}$. Then send $Y$ to Bob.

Bob decrypts the msg by $X \equiv Y^d \pmod{n}$.


## Some Intuitions On the Proof
$Y^d \equiv X^{ed} \equiv X^{k\phi(n)+1} \pmod{n}$. 

- If $gcd(X, n) = 1$, it is straight forward by Euclidean Theorem $X^{\phi(n)} \equiv 1 \pmod{n} \Rightarrow X^{k\phi(n)+1}\equiv X \pmod{n}$

- If $gcd(X, n) \neq 1$, notice $n=pq$, then $X$ can only be either $p$ or $q$. WLOG let's say $X = p$. Then $p^{q-1} \equiv 1 \pmod{q}$ by Fermat's Little Theorem. This implies that $p^{k(q-1)(p-1)} \equiv p^{k\phi(n)} \equiv 1 \pmod{q} \Rightarrow p^{k\phi(n)+1}\equiv p \pmod{q}$. It is straight forward that $p^{k\phi(n)+1} \equiv p \pmod{p}$ (It's just 0). Therefore, since $gcd(p, q) = 1$. We can get $p^{k\phi(n)+1} \equiv p \pmod{pq}$ which as defined $X^{k\phi(n)+1} \equiv X \pmod{n}$