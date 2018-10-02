#### M202 Differential Equations, Spring 2018

## Lab 5: Runge-Kutta vs Euler


In this lab we will perform a comparison of the different methods we have for numerically solving IVPs.

Consider the IVP 

$$ x'(t)=2x(t)+4t,\quad x(0)= 1$$

The following code implements forward Euler for this IVP

```matlab
h=0.1;			%timestep
n=3; 			%number of iterations

t=0:h:n*h; 		%array of points in the time interval
x=0:h:n*h; 		 %create corresponding array of space points x_i
x(1)=1; 			%initial value x(t=0)=1

for i=1:n
    x(i+1)=x(i)+h*(2*x(i)+4*t(i));
end

x
```
The output is

```
x =  1.0000    1.2000    1.4800    1.8560
```

We're going to look at local truncation errors, so we need the exact solution to the IVP:

$$x(t)=-2t-1+2e^{2t}$$

Which we can put into matlab as 

```
exact_soln=-2*t-1+2*exp(2*t)
```

We can then calculate the difference between the forward-Euler and exact values at each time step

```
exact_soln-x
```

```
ans = 0    0.0428    0.1036    0.1882
```

---
**Question 1**: Use the output of the above to calculate the average local truncation error over the three time steps. Is your answer consistent with the expectation that this error should be of order $$O(h^2)$$? Modify the code to perform the same experiment with timestep 0.01 over 10 iterations. 

---

Add backward-Euler to your code using a different variable:

```matlab
h=0.1;			%timestep
n=3; 			%number of iterations

t=0:h:n*h; 		%array of points in the time interval
x=0:h:n*h; 		 %create corresponding array of space points x_i
x(1)=1; 			%initial value x(t=0)=1
y=0:h:n*h;		% another array for backward-Euler
y(1)=1;

for i=1:n
    x(i+1)=x(i)+h*(2*x(i)+4*t(i));
end

for i=1:n
	y(i+1)=________________ ;
end

```

---

**Question 2**: Calculate the average local truncation error for backward-Euler over the three time steps. Is your answer consistent with the expectation that this error should be of order $$O(h^2)$$? Modify the code to perform the same experiment with timestep 0.01 over 10 iterations. 

---

We saw in lectures that the baby Runge-Kutta method:

$$ 
\begin{align*}
k_1&= hf(t_i,x_i)\\
x_{i+1}&= x_i+h f\left (t_i+\frac{h}{2},x_i+\frac{k_1}{2}\right)
\end{align*}
$$

has absolute local truncation error of order $$O(h^3)$$.

---
**Question 3** Add an implementation of the baby RK method to your code. For the first 3 steps with `h=0.1` you should get 

```
z =1.0000    1.2400    1.5768    2.0317
```
Work out the average local truncation error again - is it $$O(h^3)$$ this time? Repeat the experiment with `h=0.01` over 10 iterations.

---

We also saw the 4-stage Runge-Kutta method (RK4):

$$ 
\begin{align*}
k_1&= hf\left(t_i,x_i \right)\\
k_2&= hf\left(t_i+\tfrac{h}{2},x_i+\tfrac{k_1}{2}\right)\\
k_3&= hf\left(t_i+\tfrac{h}{2},x_i+\tfrac{k_2}{2}\right)\\
k_4&= hf\left(t_i+h,x_i+k_3\right)\\
x_{i+1}&= x_i+\tfrac{1}{6}( k_1+2k_2+2k_3+k_4)
\end{align*}
$$

and it was claimed that this method has $$O(h^5)$$ local truncation error.

---
**Question 4**. Run the same experiments as above using RK4 and confirm that the average local truncation error is $$O(h^5)$$. Plot all your approximations and the exact solution on the same graph. You should have to zoom in A LOT to see the difference between RK4 and the exact solution.

---
You may want to define $$f$$ so you don't have to keep typing it out, eg:

```
function u=f(a,b)
    u=4*a+2*b
end
```
This needs to go at the bottom of your script because of reasons.