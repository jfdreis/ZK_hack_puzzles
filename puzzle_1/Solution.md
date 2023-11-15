# Solution

We assume that some familiarity on BLS signature and Pedersen Hashes, b will stand for the blake2s hash.

## Hints

Think about a pedersen hash of a 2-bit message space - 01 and 10. Can you deduce another hash? Are tools from linear algebra useful to find a pedersen hash on an arbitrary message without hashing directly? Can you also use the linear structure of BLS signatures on the signatures themselves?

Let $h_1=h(01)=0 \cdot g_1 + 1 \cdot g_2=g_1$ and $h_2=h(10)=1 \cdot g_1 + 0 \cdot g_2= g_1$. 
We deduce that $h(11)=g_1+g_2$.

This suggests that if we are given some pedersen hashes of $n$-bit messages, we can compute the pedersen hash of some linear combination of those $n$-bit messages.

Regarding the signatures. Note that given $g$ and $h$ in a group $G$ with signatures $s_g=sk \cdot g $ and $s_h=sk \cdot h$ one has that the signature of $g+h$ is:

$s_{g+h}=sk \cdot (g+h)=sk \cdot g sk \cdot h = s_g+s_h$.

## Comment to the hints

Thanks to what we did above, given a message $m$, if we can write $b(m)$ as a linear combination of $b(m_i)$ one can determine the signature of $m$.

For $i=1,2,\ldots, 256$ we will write $b(m_i)$ as a column vector:

$b(m_i)=\begin(pmatrix)
b_1(m_i)\\
b_2(m_i)\\
\vdots \\
b_{256}(m_i)\\ \end(pmatrix)