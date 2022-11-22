---
creation-date: 2022-11-22
publish: true
audience: all
completion: .2
tags: type/model/protocol, related/zero-knowledge
---
# The First Zero Knowledge Protocol
Zero Knowledge Proofs are a relatively new phenomenon. They were first introduced in 1989 with a paper written by Shafi Goldwasser, Silvio Micali, and Charles Rackoff called "The Knowledge Complexity of Interactive Proof Systems". If this title seems daunting, you are not alone. Leading cryptographers at the time were also stumped by this paper and rejected the authors’ initial publishing attempts. Eventually, they caught on, and in 2012, Goldwasser and Micali were named Turing Award winners for their contributions in revolutionizing the science of cryptography.

The paper presented an initial, interactive protocol for proving whether a number is a quadratic residue within a certain finite field without revealing the prover’s method of calculating.

## Definitions
**Finite Field**: $\mathbb{F}_n := \{0,1, ..., n-1\}$

**Quadratic Residue**: $y$ is a *quadratic residue* mod $x$ if $y=z^2$ mod $x$ for some $z$. Otherwise, we say $y$ is a *quadratic nonresidue* mod $x$.

**gcd(x,y):** Function to find the greatest common denominator between $x$ and $y$.

$QR = \{(x,y)|y \text{ is a quadratic residue mod }x\}$

$QNR = \{(x,y)|y\text{ is a quadratic nonresidue mod }x\}$

## The Protocol
Let there be a prover $P$ and a verifier $V$ that can send and receive messages. $V$ wants to know if $y$ is an element of $QR$ but doesn’t have the knowledge of how to determine this. $P$ claims to know how to do this but does not want to reveal their knowledge. 

1) $V$ randomly generates two numbers: $b$ and $z$

    a) $b\in_R\{0,1\}$ → a zero or a one

    b) $z\in_R\mathbb{F}_x$ such that $gcd(x,z)=1$ → a number in the finite field that is relatively prime to $x$

2) $V$ sends $w \leftarrow [(z^2y^{b}) \mod x]$ to $P$

    a) The verifier does this because they don’t want the prover to learn the exact number they are curious about

    b) This lets the verifier verify the provers general knowledge on the computation

3) $P$ responds with $c = \left\{        \begin{array}{ll}1, & \text{if }(x,w) \in QNR \\0, & \text{if }(x,w) \in QR\end{array}\right.$

4) Repeat process until $V$ is satisfied ($n$ times)

## Results
- When $(x,y)$ is an element of $QR$ then $(x,w)$ is an element of $QR$
    - $(x,z^2)$ is in $QR$ by definition of quadratic residue, and similarly $(x,z^2y)$ is also in $QR$
- When $(x,y)$ is an element of $QNR$, then $(x,w)$ is an element of $QNR$ when $b=1$
    - when $b = 1$, $y$ is included in the message
- If for all $n$ rounds of the protocol the prover sends $c=0$, then the verifier can be certain with probability $1-2^{-n}$ that $y$ is an element of $QR$
    - Notice that every correct response the prover sends has a $\frac{1}{2}$ chance of being a guess, meaning the prover doesn’t actually know how to calculate a quadratic residue
- If for all $n$ rounds the prover sends $c=b$, then the verifier can be certain with probability $1-2^{-n}$ that $y$ is an element of $QNR$
- Otherwise, the verifier is not convinced that the prover knows how to determine whether $(x,y)$ is an element of $QR$

Notice that no knowledge of how to find this result is ever shared, thus making this a Zero Knowledge Proof. We can further specify that this is a Zero Knowledge Interactive Proof because the verifier needs to directly interact with the prover throughout multiple rounds of the protocol. Later on, you will see how proofs can become non-interactive such as zkSNARKs (Zero Knowledge Succinct Non-interactive ARgument of Knowledge).

---
## Related Pages
*The related pages section is for linking this page other the rest of the graph, press F11 for details. If applicable, replace the following dummy links.*
- primary-topic:: [[Topic-Zero Knowledge]]
- [[Model-The First Zero Knowledge Protocol]]

## External Resources
*The sources section is for recommending resources on other sites*.

## References
- ["The Knowledge Complexity of Interactive Proof Systems"](https://people.csail.mit.edu/silvio/Selected%20Scientific%20Papers/Proof%20Systems/The_Knowledge_Complexity_Of_Interactive_Proof_Systems.pdf) (1989)
