'''
## Exercise 1. 
Define the random variable $X_{ij} = 1$ denoting that $h(x_i) = h(x_j)$, otherwise,  $X_{ij} = 0$. Then the total number of collisions $X$ after hashing $x_1, ..., x_n$ would be: $X = \sum_{i<j} X_{ij}$. Firstly, let's bound the expectation of $X$.
$$\begin{aligned}
E[X] &= E[\sum_{i<j} X_{ij}] \\
&= \sum_{i<j} E[X_{ij}]\ (Lineariy\ of\ expectation)\\
&= \sum_{i<j} Pr[X_{ij} = 1] \\
&\leq \sum_{i<j} \frac{c}{m}\ (Property\ of\ c-universal) \\
&\leq \binom{n}{2} \frac{c}{m} \\
&\leq \frac{cn^2}{2cn^2} = \frac{1}{2}
\end{aligned}$$
So the expected number of collisions is at most $\frac{1}{2}$. Our goal is to bound $Pr[X = 0] = Pr[X < 1] = 1 - Pr[X \geq 1]$, so we can bound $Pr[X \geq 1]$ instead.
According to the Markov's inequality, we have:
$$\begin{aligned}
Pr[X \geq 1] &\leq \frac{E[X]}{1} \leq \frac{1}{2}
\end{aligned}$$
Hence, $Pr[X = 0] = Pr[X < 1] = 1 - Pr[X \geq 1] \geq \frac{1}{2}$, which means that with probability at least 1/2, there are no pairs $x_i, x_j, with\ i \ne j$, such that $h(x_i) = h(x_j)$.
***
## Exercise 2
Consider the following construction:
Based on the analysis of exercise 1, with probability at least $\frac{1}{2}$, there are no collisions at all. So we can just use a large array of size $O(n^2)$ to store keys with a c-universal hash function $h$. When storing the keys, if unfortunately we do have an collision, i.e. $h(x_i) = h(x_j)\ for\ i \ne j$, then we just drop the whole structure we built and start over a new construction with another hash function $h'$. Repeat the process until there are no collision. In the end, we will have exactly $1$ lookup time, $1$ evaluation time and space is $O(n^2)$.
Now let's analysize the expected construction time:
The probability of starting over is exactly upper bounded by $Pr[X \geq 1] \leq \frac{1}{2}$, so the probability of doing the construction $i$ times is bounded by $(\frac{1}{2})^{i-1}$, hence the expected construction time is upper bounded by:
$$
\sum_{i = 1}^{+\infty} (\frac {1}{2}) ^ {i-1} O(n^2) = O(n^2)
$$

***
## Exercise 3
We will continue the analysis based on exercise 1, basiclly the analysis almost holds but the constraint on $m$ is a slightly different. Still want to bound the expected number of collisions:
$$\begin{aligned}
E[X] &= E[\sum_{i<j} X_{ij}] \\
&= \sum_{i<j} E[X_{ij}]\ (Lineariy\ of\ expectation)\\
&= \sum_{i<j} Pr[X_{ij} = 1] \\
&\leq \sum_{i<j} \frac{c}{m}\ (Property\ of\ c-universal) \\
&\leq \binom{n}{2} \frac{c}{m} \\
&\leq \frac{cn^2}{4cn} = \frac{n}{4}
\end{aligned}$$
Then our goal is to show that $Pr[X \leq \frac{n}{2}] = 1 - Pr[X \geq \frac{n}{2}] \geq \frac{1}{2}$, so we only need to bound that $Pr[X > \frac{n}{2}] \leq \frac{1}{2}$. Again, with Markov's inequality, we have:
$$\begin{aligned}
&\ \ \ \ \ \ Pr[X \geq \frac{n}{2}] \leq \frac{E[X]}{\frac{n}{2}} \leq \frac{\frac{n}{4}}{\frac{n}{2}} = \frac{1}{2}\\
&\Rightarrow  Pr[X \leq \frac{n}{2}] = 1 - Pr[X \geq \frac{n}{2}] \geq \frac{1}{2}
\end{aligned}$$
So it basiclly says that with probability at least 1/2, there are no more than $n \over 2$ pairs $x_i, x_j$, with $i ,j$, such that $h(x_i) = h(x_j)$.
***
## Exercise 4
Define the r.v. $n_i$ as the number of keys hashing to position $i$. For instance, there is no collision if only one key mapped to that position, there would be 1 collision if two were in that bucket, 3 if 3,..., so for given number of elements $n_i$ in the bucket $i$, the number of collision of that bucket is exactly $\binom{n_i}{2}$. Then the total number of collisions $X$ can be expressed as:
$$
X = \sum_{i=0}^{m-1} \binom{n_i}{2} = \sum_{i=0}^{m-1} \frac{n_i(n_i -1)}{2}
$$
Based on the analysis of exercise 3, we conclude that there are no more than $n \over 2$ pairs collisions with probability at least $1 \over 2$, which means:
$$\begin{aligned}
Pr[X \leq \frac{n}{2}] = Pr[\sum_{i=0}^{m-1} \frac{n_i(n_i -1)}{2}\leq \frac{n}{2}] = Pr[\sum_{i=0}^{m-1} n_i(n_i-1) \leq n]
\end{aligned}$$
Now we want to say somthing about $n_i(n_i -1)$ and $n_{i}^{2} \over 4$. Define a function $f(x) = x(x-1) - \frac{x^2}{4}$ with $x \in N$, it's easy to prove that $f(x) < 0$ if $x \in [0,\frac{4}{3})$, and $f(x) > 0$ if $x \in (\frac{4}{3}, +\infty)$. 
So for $n_i = 0,\ n_i(n_i -1) = \frac{n_{i}^{2}}{4}$ and for $n_i \geq 2$, we have $n_i(n_i -1) \geq \frac{n_{i}^{2}}{4}$. However,  it can be a little tricky when it comes to $n_i = 1$, because $\frac{n_{i}^{2}}{4} = \frac{1}{4} > n_i(n_i - 1) = 0$.
Now consider the number of $n_i$ that are equal to $1$, note that for $n_i \geq 2$, the difference between $n_i(n_i-1)$ and $\frac{n_{i}^{2}}{4}$ is clearly greater than $\frac{1}{4}$, which means $\sum_{i=0}^{m-1} n_i(n_i-1)$ is still greater than $\sum_{i=0}^{m-1} \frac{n_{i}^{2}}{4}$ when the number of $n_i$ that is equal to $1$ is less than a half. 
When it is more than a half, then it's hard to analyze the relation between those  two. So in this case, let's consider the most extreme case such that all the $n_i = 1$, then $\sum_{i=0}^{m-1} \frac{n_{i}^{2}}{4} = \frac{m}{4}$, if we set $m = 4n$, then $\sum_{i=0}^{m-1} \frac{n_{i}^{2}}{4} \leq n$.
***
## Exercise 5

'''