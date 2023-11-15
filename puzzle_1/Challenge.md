# The Challenge

In the challenge we are given 256 messages $(m_1,\ldots,m_{256})$ and those messages' signatures $(s_1,\ldots,s_{256})$
signed by some unknown private key $sk$ which its corresponding public-key is given as well $pk$.

Each message is signed using the BLS signature scheme where messages are mapped to group elements by first employing a blake2s hash and then using pedersen hash on its output.
It's important to mention that the output of blake2s is 256-bits wide. The group elements $g_1,g_2,\ldots, g_{256}$ are picked arbitrarily.
Notice that the prime size of the group in our challenge is $r=0x73eda753299d7d483339d80809a1d80553bda402fffe5bfeffffffff00000001$.
Therefore the private key is a number in $\mathbb{Z}_r$ between 0 and $r-1$.
We are told that these signatures were published and someone managed to sign some previously unsigned message, and we're asked how can this be done?

