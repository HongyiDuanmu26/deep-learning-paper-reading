# [mHC: Manifold-Constrained Hyper-Connections](https://arxiv.org/abs/2512.24880)


<img width="1258" height="860" alt="image" src="https://github.com/user-attachments/assets/d352efc9-a020-49cb-951f-620728555ab8" />
As shown above, (a) is the Residual Connection, (b) is the Hyper-Connection (HC), (c) is the newly proposed manifold-constrained Hyper-Connection (mHC).

The equation for the Residual Connection is:
<img width="760" height="50" alt="image" src="https://github.com/user-attachments/assets/4d6ae337-8c66-4edd-9c73-7e396fffcf77" />

So, when we have several residual connections across multiple layers:
<img width="796" height="116" alt="image" src="https://github.com/user-attachments/assets/442b43cc-561a-440b-8d86-0facf1197224" />

The good characteristic of it is that the original input $x_l$ is still $x_l$ without any amplification or compression.

The equation for the Hyper-Connection is:
<img width="884" height="76" alt="image" src="https://github.com/user-attachments/assets/acda7946-8647-4919-a082-12f8bd61d774" />

So, when we have several Hyper-Connections across multiple layers:
<img width="1004" height="132" alt="image" src="https://github.com/user-attachments/assets/15c70c46-97d9-4ac5-a12d-585772b0b44f" />

The original input $x_l$ is amplified or compressed, making the gradient exploded or vanished, making training not stable, as shown below. 
<img width="1252" height="518" alt="image" src="https://github.com/user-attachments/assets/617dc991-8855-4f1a-9b1a-266b00344a56" />

So, to make the HC have similar characteristic as the original residual connection, authors proposed mHC, making $H_{res}$ a doubly stochastic matrices, 
whose all row sums are one, all column sums are one, and all elements are non-negative. The multiply result of two doubly stochastic matrices is another doubly stochastic matrices.

So mHC is:
<img width="870" height="200" alt="image" src="https://github.com/user-attachments/assets/ac175ca8-1a31-4ad5-822a-c3fd8ecf64be" />
<img width="874" height="156" alt="image" src="https://github.com/user-attachments/assets/5d6abee6-0108-4073-892c-43369934032b" />

Sinkhorn-Knopp is making a matrix into a doubly stochastic matrix by firstly making all
elements to be positive via an exponent operator and then conducts iterative normalization
process that alternately rescales rows and columns to sum to 1. They choose $t_{max} = 20$. 
