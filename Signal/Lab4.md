# Lab4

## Problem1

### (a)

code:

``` matlab
clear;
clc;
h=[2 0 -2];
x=[1 0 1];
ny=-1:3;
y=conv(x,h);
stem(ny,y);
```

graphics:

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\1a.jpg" style="zoom:80%;" />



### (b)

$$
h[n]=\delta[n-a]+\delta[n-b]\qquad a\le n \le b\\
x[n]=\delta[n-c]+\delta[n-d]\qquad c\le n \le d\\
y[n]=h[n]*x[n]=\sum\limits_{=-\infin}^\infin h[m]x[n-m] \qquad a+c \le n \le b+d\\
ny = b + d -(a + c) + 1\\
a = 0,b = N - 1, c = 0, d = M - 1.\\
ny = M - 1
$$



### (c)

code:

```matlab
clear;
clc;

nh=0:14;
h=ones(1,15);
nx=0:24;
x=((1/2).^(nx-2)).*[zeros(1,2) ones(1,23)];
ny=0:38;
y=conv(x,h);
stem(ny,y);
```

graphics:

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\1c.jpg" style="zoom:80%;" />

​	For x[n], the part of n >= 2 is valid; the part of n < 2 is not valid.

​	For h[n], the part of n >= -2 is valid; the part of n < -2 is not valid.

​	For y[n], the part of 2 <= n <= 23 is valid, and others isn't valid.

### (d)

code:

```matlab
clear;
clc;

n=[0:99];
x=cos(n.^2).*sin((2/5)*pi.*n);
h=((0.9).^n).*[ones(1,10) zeros(1,90)];
y=conv(h,x);
stem(n,y(1:100));
```

graphics:

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\1d.jpg" style="zoom:80%;" />

### (e)

code:

```matlab
clear;
clc;
n=0:99;
n0=0:49;
n1=50:99;
x0=cos(n0.^2).*sin((2/5)*pi.*n0);
x1=cos(n1.^2).*sin((2/5)*pi.*n1);
h=((0.9).^n).*[ones(1,10) zeros(1,90)];
y0=conv(h,x0);
y1=conv(h,x1);
y=[y0(1:50) y1(1:50)];
stem(n,y);
```

graphics:

![](E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\1e.jpg)

## Problem2

### (a)

code:

```matlab
clear;
clc;
wc = 0.4;
n1 = 10;
n2 = 4;
n3 = 12;
[b1, a1] = butter(n1, wc)
a2 = 1
b2 = remez(n2, [0 wc-0.04 wc+0.04 1], [1 1 0 0])
a3 = 1
b3 = remez(n3, [0 wc-0.04 wc+0.04 1], [1 1 0 0])
```

graphics:

![](E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2a-result.jpg)



### (b)

code:

```matlab
clear;
clc;

wc=0.4;
n1=10;
[b1,a1]=butter(n1,wc);
n2=4;
[b2,a2]=butter(n2,wc);
n3=12;
[b3,a3]=butter(n3,wc);

figure;
freqz(b1,a1);
figure;
freqz(b2,a2);
figure;
freqz(b3,a3);
```

graphics:

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2b1.jpg" style="zoom: 80%;" />

<center>fliter1</center>

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2b2.jpg" style="zoom:80%;" />

<center>fliter2</center>

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2b3.jpg" style="zoom:80%;" />

<center>fliter3</center>



​	Compare to others, the second fliter have the linear phase.

### (c)

code:

```matlab
![2c1](E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2c1.jpg)clear;
clc;

wc=0.4;
n1=10;
n2=4;
n3=12;
n=0:20;
x=ones(1,length(n));

[b1,a1]=butter(n1,wc);
[b2,a2]=butter(n2,wc);
[b3,a3]=butter(n3,wc);
h1=filter(b1,a1,x);
h2=filter(b2,a2,x);
h3=filter(b3,a3,x);
figure;
plot(h1);
figure;
plot(h2);
figure;
plot(h3);
```

graphics:

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2c1.jpg" style="zoom:80%;" />

<center>h1</center>

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2c2.jpg" style="zoom:80%;" />

<center>h2</center>

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2c3.jpg" style="zoom:80%;" />

<center>h3</center>

### (d)

code:

```matlab
clear;
clc;
load plus;
x16=x(:,16);

wc=0.4;
n1=10;

[b1,a1]=butter(n1,wc);

y16_1=filter(b1,a1,[x16; zeros(n1/2,1)]);
stem(x16);
hold on
stem(y16_1(n1/2+1:end));
```

graphics:

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2d1.jpg" style="zoom:80%;" />

### (e)

code:

```matlab
clear;
clc;
load plus;
x16=x(:,16);

wc=0.4;
n1=10;
n2=4;
n3=12;

[b2,a2]=butter(n2,wc);
[b3,a3]=butter(n3,wc);

y16_2=filter(b2,a2,[x16; zeros(n2/2,1)]);
y16_3=filter(b3,a3,[x16; zeros(n3/2,1)]);
figure;
hold on
stem(x16);
stem(y16_2(n2/2+1:end));
hold off
figure;
hold on
stem(x16);
stem(y16_3(n3/2+1:end));
hold off
```

graphics:

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2e1.jpg" style="zoom:80%;" />

<center>fliter2</center>

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2e2.jpg" style="zoom:80%;" />

<center>fliter3</center>

### (f)

code:

```matlab
function y = filt2d(b,a,d,x)
%d = n/2;n is the order of the 1D filter
%a b is the coefficients of the 1Dfilter
 %size fo x y z is N*N
 [m,n] = size(x);

 for i = 1:1:n
     z(:,i) = filter(b,a,[x(:,i);zeros(d,1)]);
 end
 z = z(d+1:end,:);
 for j = 1:1:m
     y(j,:) = filter(b,a,[z(j,:) zeros(1,d)]);
 end
 y = y(:,d+1:end);
     
end
```

### (g)

code:

```matlab
clear;
clc;
load plus;
wc=0.4;
n1=10;
n2=4;
n3=12;

[b1,a1]=butter(n1,wc);
[b2,a2]=butter(n2,wc);
[b3,a3]=butter(n3,wc);
y1=filt2d(b1,a1,n1/2,x);
figure;
colormap(gray);
image(64*y1);

y2=filt2d(b2,a2,n2/2,x);
figure;
colormap(gray);
image(64*y2);

y3=filt2d(b3,a3,n3/2,x);
figure;
colormap(gray);
image(64*y3);
```

graphics:

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2g1.jpg" style="zoom:80%;" />

<center>fliter1</center>

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2g2.jpg" style="zoom:80%;" />

<center>fliter2</center>

<img src="E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\2g3.jpg" style="zoom: 80%;" />

<center>fliter3</center>

​	Compare the three images, the fliter3 leads to more distoration in the shape of the original image.

## Problem3

### (a)

​	Accoring to the table of page 221, we can see that the real signal's fourier series coefficients satisfy:
$$
a_k=a_{-k}^*
$$
so the signal is purely real.



### (b)

​	The signal's period is N = 5. So:
$$
a_1=a_{-4}=2e^{-j\pi/3};a_3=a_{-2}=e^{-j\pi/4}
$$

### (c)

code:

```matlab
clear;
clc;
a=[1 2*exp(-1i*pi/3) exp(1i*pi/4) exp(-1i*pi/4) 2*exp(1i*pi/3)];
x=[];
m=0:4;
for n=0:4
    for k=0:4
        s=a(k+1).*exp(1i.*k.*2.*pi./5.*n);
    end
    x = [x s];
end

figure;
stem(m,x);
```

graphics:

![](E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\3c.jpg)

### (d)

code:

```matlab
clear;
clc;
n=0:63;
x1p=[];
x2p=[];
x3p=[];
x1=ones(1,8);
N1=8;
for i = 1: length(n) / N1
    x1p=[x1p x1];
end

x2=[ones(1,8) zeros(1,8)];
N2=16;
for i=1:length(n)/N2
    x2p=[x2p x2];
end

x3=[ones(1,8) zeros(1,24)];
N3=32;
for i=1:length(n)/N3
    x3p=[x3p x3];
end

figure;
stem(n,x1p);
figure;
stem(n,x2p);
figure;
stem(n,x3p);
```

graphics:

![](E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\3d1.jpg)

<center>x1[n]</center>

![](E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\3d2.jpg)

<center>x2[n]</center>

![](E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\3d3.jpg)

<center>x3[n]</center>



### (e)

code:

```matlab
clear;
clc;
n=0:63;
x1p=[];
x2p=[];
x3p=[];
x1=ones(1,8);
N1=8;
for i = 1: length(n) / N1
    x1p=[x1p x1];
end

x2=[ones(1,8) zeros(1,8)];
N2=16;
for i=1:length(n)/N2
    x2p=[x2p x2];
end

x3=[ones(1,8) zeros(1,24)];
N3=32;
for i=1:length(n)/N3
    x3p=[x3p x3];
end

a1=1/N1*fft(x1);
a2=1/N2*fft(x2);
a3=1/N3*fft(x3);

figure;
stem(0:N1-1,abs(a1));
figure;
stem(0:N2-1,abs(a2));
figure;
stem(0:N3-1,abs(a3));
```

graphics:

![](E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\3e1.jpg)

<center>a1</center>

![](E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\3e2.jpg)

<center>a2</center>

![](E:\Job\2023SpringAndSummerSemester\Signal\HomeWork\Lab4\graphics\3e3.jpg)

<center>a3</center>

​	From the three images, we find that the predicted values match those obtanied with MATLAB.



### (f)

code:

```matlab
clear;
clc;
n=0:31;
x3=[ones(1,8) zeros(1,24)];
N3=32;
a3=1/N3*fft(x3);

figure;
x32=[];
for m=0:length(n)
    for k=-2:2
        if(k<=0)
            temp=conj(a3(length(a3)+k)).*exp(1i.*k.*pi./16.*n);
        else
            temp=a3(k).*exp(1i.*k.*pi./16.*n);
        end
        s=sum(temp,"all");
    end
    x32=[x32 s];
end

x38=[];
for m=0:length(n)
    for k=-8:8
        if(k<=0)
            temp=conj(a3(length(a3)+k)).*exp(1i.*k.*pi./16.*n);
        else
            temp=a3(k).*exp(1i.*k.*pi./16.*n);
        end
        s=sum(temp,"all");
    end
    x38=[x38 s];
end

x312=[];
for m=0:length(n)
    for k=-12:12
        if(k<=0)
            temp=conj(a3(length(a3)+k)).*exp(1i.*k.*pi./16.*n);
        else
            temp=a3(k).*exp(1i.*k.*pi./16.*n);
        end
        s=sum(temp,"all");
    end
    x312=[x312 s];
end

x3all=[];
for m=0:length(n)
    for k=-15:16
        if(k<=0)
            temp=conj(a3(length(a3)+k)).*exp(1i.*k.*pi./16.*n);
        else
            temp=a3(k).*exp(1i.*k.*pi./16.*n);
        end
        s=sum(temp,"all");
    end
    x3all=[x3all s];
end
x32
x38
x312
x3all
```



### (g)

​	
$$
x_{3_all}[n]\quad must\quad be\quad a\quad real\quad signal.
$$


### (h)

code:

```matlab
clear;
clc;
n=0:31;
x3=[ones(1,8) zeros(1,24)];
N3=32;
a3=1/N3*fft(x3);

figure;
x32=[];
for m=0:length(n)
    for k=-2:2
        if(k<=0)
            temp=conj(a3(length(a3)+k)).*exp(1i.*k.*pi./16.*n);
        else
            temp=a3(k).*exp(1i.*k.*pi./16.*n);
        end
        s=sum(temp,"all");
    end
    x32=[x32 s];
end
stem(n,x32(1:32));

figure;
x38=[];
for m=0:length(n)
    for k=-8:8
        if(k<=0)
            temp=conj(a3(length(a3)+k)).*exp(1i.*k.*pi./16.*n);
        else
            temp=a3(k).*exp(1i.*k.*pi./16.*n);
        end
        s=sum(temp,"all");
    end
    x38=[x38 s];
end
stem(n,x38(1:32));

figure;
x312=[];
for m=0:length(n)
    for k=-12:12
        if(k<=0)
            temp=conj(a3(length(a3)+k)).*exp(1i.*k.*pi./16.*n);
        else
            temp=a3(k).*exp(1i.*k.*pi./16.*n);
        end
        s=sum(temp,"all");
    end
    x312=[x312 s];
end
stem(n,x312(1:32));

figure;
x3all=[];
for m=0:length(n)
    for k=-15:16
        if(k<=0)
            temp=conj(a3(length(a3)+k)).*exp(1i.*k.*pi./16.*n);
        else
            temp=a3(k).*exp(1i.*k.*pi./16.*n);
        end
        s=sum(temp,"all");
    end
    x3all=[x3all s];
end
stem(n,x3all(1:32));
```

