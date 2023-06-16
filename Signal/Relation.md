

# Four Fourier Representations

$$
Sampling\quad in\quad Time/Frequency\leftrightarrow Period\quad extension\quad in\quad Frequency/Time\\
Sampling\rightarrow\times Inpulse\quad train\Leftrightarrow*Impulse\quad train
$$

CTFS:
$$
\because e^{jk\omega_0t}\rightarrow2\pi\delta(\omega-k\omega_0)\qquad\omega_0=\frac{2\pi}{T}\\
\therefore x(t)=\sum\limits_{k=-\infin}^\infin a_ke^{jk\omega_0t}\rightarrow X(j\omega)=2\pi\sum\limits_{k=-\infin}^\infin a_k\delta(\omega-k\omega_0)
$$
DTFS:
$$
\because e^{jk\omega_0n}\rightarrow2\pi\sum\limits_{m=-\infin}^\infin\delta(\omega-\omega_0-2\pi m)\qquad\omega_0=\frac{2\pi}{N}\\
\therefore x[n]=\sum\limits_{k=<N>}a_ke^{jk\omega_0n}\rightarrow X(e^{j\omega})=2\pi\sum\limits_{k=-\infin}^\infin a_k\delta(\omega-k\omega_0)
$$


# Sampling

![image-20230602105352619](C:\Users\GATL76\AppData\Roaming\Typora\typora-user-images\image-20230602105352619.png)