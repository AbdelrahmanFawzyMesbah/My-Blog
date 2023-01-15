# Number of integral solutions of the equation $x_1 + x_2 +…. + x_n = k$

given n ,m , k 

find number of solution to $x_1 + x_2 +…. + x_n = k$ where $0\leq x_i \leq m$

$1 ≤ n, m, k ≤ 10^5$

sample input 

```plaintext
2 4 5
```

output

```plaintext
2
```

```plaintext
20 10 50
```

```plaintext
366736536
```

let's take small example and try to generalize it

This problem has versions :

1. $x_1+x_2+x_3+=11$
   
   with ($x_1,x_2,x_3>0$)positive integers for $x_i$
   
   then number of solutions ${n-1 \choose k-1}$
   
   why ? we use stars and bars method to prove it :
   
   `n=11` and `k= 3`
   
   we have n stars that are not distinguishable and want to place them into k distinguishable bars
   
       {* * * * * * * * * *}

if we put our bars between stars 

```
   {* *|* * * * *|* * *}
```

$x_1=2,x_2=5,x_3=3$

we have 10 gaps and 2 bars to put

$n-1=10 , k-1=2$

we have 10 gaps to choose from and 2 bars to puts 

${10 \choose 2}$

2. same problem where $x_1+x_2+x_3+=11$ but with with ($x_1,x_2,x_3\geq0$)non-negative  integers for $x_i$

so $x_i$ can be zero

we will have $x_i$ maybe zero 

so we will have 

${11+3-1 \choose 2}$

3. some more difficult version of the problem 
   
   that $x_1+x_2+x_3+x_4=16$
   
   and $0\leq x_i \leq 6 <16$

we have total ${16+4-1 \choose 3}$

if  any x overshoot $7 \leq x_i\leq 16$

we have ${4 \choose 1}$ choose any of x ,and ${16-7+4-1 \choose 3}$ solution 

so ${4 \choose 1}{16-7+4-1 \choose 3}$

but when we do this we deleted when two of x overshoot ($x_1,x_2=7$)

 so we need to add ${4 \choose 2}{16-7*2+4-1 \choose 3}$

so result = ${16+4-1 \choose 3}$-${4 \choose 1}{16-7+4-1 \choose 3}$+${4 \choose 2}{16-7*2+4-1 \choose 3}$ can't have ${4 \choose 3}$ because this will not give as 16

in general to calculate number of solutions 

$x_1+x_2+x_3+x_4+....+x_n=r$ and $0\leq x_i<l<r$

$\sum_{i=0}^{n}(-1)^i{n \choose i}{n+r-1-i(l+1) \choose n-1}$ (**)

let's back to our problem above how to write code to solve it

we need some helper function to find  (**) 

1. Binary exponentiation
   [Binary Exponentiation - Algorithms for Competitive Programming](https://cp-algorithms.com/algebra/binary-exp.html)
   
   
   ```cpp
   ll power(ll a, ll b, ll M) {
    if (b == 0) return 1;
    int t = power(a, b / 2, M);
    t = t * t % M;
    if (b % 2 == 1) t = t * a % M;
    return t;
   }`
   ```

2. factorial 
   
   ```cpp
   int fact[50000];
   void fac() {
    fact[0] = 1;
    for (int i = 1; i < 50000; ++i)
    {
    fact[i] = fact[i - 1] * i % mod;
    }
   }
   ```

3. nCr 
   to find nCr using factorial function we need `(fact(n)%mod fact(n-r)^-1%mod fact(r)^-1%mod)`
   so need Fermat's little theorem to find inverse give that mod is prime
   
   so $fact()^(m-2)=fact()^{-1}  mod \ m$
   
   ```cpp
   ll ncr(int n , int r ) {
    if (n < r) return 0 ;
    int ans = fact[n];
    ans = ans * power (fact[n - r], mod - 2, mod) % mod;
    ans = ans * power(fact[r], mod - 2, mod) % mod;
    return ans;
   }`
   ```
   
   

4. Sum 
   
   
   ```cpp
   void sum() {
   fac();
   int n, m, k;
   cin >> n >> m >> k;
   ll ans = 0 ;
   for (int i = 0 ; i <= n ; i++)
   {
       int t = ncr(n, i);
       if (n + k - i * m - 1 < n - 1) break;
       t = (t * ncr(n + k - 1 - i * (m + 1), n - 1)) % mod;
       if (t < 0) t += mod;
       if (i % 2 == 0) (ans - t + mod) % mod;
       else
           ans = (ans + t) % mod;
   }
   cout << ans << endl;
   }
   ```
   
   

#### Resources

[combinatorics - Counting bounded integer solutions to $\sum_ia_ix_i\leqq n$ - Mathematics Stack Exchange](https://math.stackexchange.com/questions/910809/counting-bounded-integer-solutions-to-sum-ia-ix-i-leqq-n/910820#910820)[combinatorics - Counting bounded integer solutions to $\sum_ia_ix_i\leqq n$ - Mathematics Stack Exchange](https://math.stackexchange.com/questions/910809/counting-bounded-integer-solutions-to-sum-ia-ix-i-leqq-n/910820#910820)
