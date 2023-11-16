# Solution

We assume that some familiarity on BLS signature and Pedersen Hashes, b will stand for the blake2s hash.

## Hints

Think about a pedersen hash of a 2-bit message space - 01 and 10. Can you deduce another hash? Are tools from linear algebra useful to find a pedersen hash on an arbitrary message without hashing directly? Can you also use the linear structure of BLS signatures on the signatures themselves?

Let $h_1=h(01)=0g_1 + 1g_2=g_1$ and $h_2=h(10)=1g_1 + 0g_2= g_1$.

We deduce that $h(11)=g_1+g_2$.

This suggests that if we are given some pedersen hashes of $n$-bit messages, we can compute the pedersen hash of some linear combination of those $n$-bit messages.

Regarding the signatures. Note that given $g$ and $h$ in a group $G$ with signatures $s_g=sk \cdot g$ and $s_h=sk \cdot h$ one has that the signature of $g+h$ is:

$s_{g+h}=sk \cdot (g+h)=sk \cdot g + sk \cdot h = s_g+s_h$.

## Comment to the hints

Thanks to what we did above, given a message $m$, if we can write $b(m)$ as a linear combination of $b(m_i)$ one can determine the signature of $m$.

For $i=1,2,\ldots, 256$ we will write $b(m_i)$ as a column vector $b(m_i) = [b_1(m_i)], [b_2(m_i)], \vdots, [b_{256}(m_i)]$.

Note that we can define sum of these vectors (coordinate wise) and multiplication by a scalar in $\mathbb{Z}_r$.

## Guessing the signature of a message m

Given a message $m$ let us assume that $b(m)$ can be written as a linear combination of $b(m_i)$, with scalars in $\mathbb{Z}_r$.

That is, let $a_1, a_2, \ldots, a_{256}$ in $\mathbb{Z}_r$ and assume $b(m)=\sum\limits_{j=1}^{256}a_jb(m_j)$. Hence, $b_i(m)=\sum\limits_{j=1}^{256}a_jb_i(m_j)$. We claim that $s_m=\sum\limits_{j=1}^{256}a_j s_j$. Indeed: 

The Pedersen hash of $b(m)$ is $\sum\limits_{i=1}^{256} b_i(m) g_i = \sum\limits_{i=1}^{256} \Big(\sum\limits_{j=1}^{256}a_jb_i(m_j)\Big) g_i$.
Note also that $s_j=sk \cdot \sum\limits_{j=1}^{256} b_i(m_j)g_i$.
Hence,
$$
    s_m=sk \cdot \sum\limits_{i=1}^{256} b_i(m) g_i=

    =sk \cdot \sum\limits_{i=1}^{256} \Big(\sum\limits_{j=1}^{256}a_jb_i(m_j)\Big) g_i=

    =sk \cdot \sum\limits_{i=1}^{256} \sum\limits_{j=1}^{256} a_jb_i(m_j) g_i=

    =sk \cdot \sum\limits_{j=1}^{256} \sum\limits_{i=1}^{256} a_jb_i(m_j) g_i=

    =\sum\limits_{j=1}^{256} a_j sk \cdot \Big(\sum\limits_{i=1}^{256} b_i(m_j) g_i\Big)=

    =\sum\limits_{j=1}^{256} a_j s_j$$

as we expected.

So, to solve this challenge, given a message $m$ we just need to find $a_1,\ldots, a_{256}$ in $\mathbb{Z}_r$ such that $b(m)=\sum\limits_{j=1}^{256}a_jb(m_j)$. This reduces to a linear system of 256 equations and 256 variables.
$$
    a_1b_1(m_1)+a_2b_1(m_2)+ \cdots + a_{256}b_1(m_{256})=b_1(m)

    a_1b_2(m_1)+a_2b_2(m_2)+\cdots+ a_{256}b_2(m_{256})=b_2(m)

    \vdots

    a_1b_{256}(m_1)+a_2b_{256}(m_2)+\cdots+ a_{256}b_{256}(m_{256})=b_1(m)$$

So, let $A=[ [b_1(m_1),b_1(m_2), \cdots, b_1(m_{256})] , [b_2(m_1),b_2(m_2), \cdots, b_2(m_{256})] ,\vdots, [b_{256}(m_1), b_{256}(m_2), \cdots, b_{256}(m_{256})] ]$, $x=[[a_1],[a_2],\vdots, [a_{256}]]$ and $b=[[b_1(m)],[b_2(m)],\vdots, [b_{256}(m)]]$.

Consequently, if $A$ is invertible, the solution is: $x=A^{-1}b$.