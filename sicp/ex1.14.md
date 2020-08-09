Exercise 1.14.  Draw the tree illustrating the process generated by the count-change procedure of section 1.2.2 in making change for 11 cents.
What are the orders of growth of the space and number of steps used by this process as the amount to be changed increases?

The program for reference:
```
(define (count-change amount)
  (cc amount 5))
(define (cc amount kinds-of-coins)
  (cond ((= amount 0) 1)
        ((or (< amount 0) (= kinds-of-coins 0)) 0)
        (else (+ (cc amount
                     (- kinds-of-coins 1))
                 (cc (- amount
                        (first-denomination kinds-of-coins))
                     kinds-of-coins)))))
(define (first-denomination kinds-of-coins)
  (cond ((= kinds-of-coins 1) 1)
        ((= kinds-of-coins 2) 5)
        ((= kinds-of-coins 3) 10)
        ((= kinds-of-coins 4) 25)
        ((= kinds-of-coins 5) 50)))
```

From the graph it can be clearly seen that the order of growth of space is
proportinal to the maximum depth of the tree as the maximum space is used
when we're at the leaves and have to remeber the result of the previous computations
to calculate our answer recursively


The order of growth of the number of steps is dependent on the number of kinds of coins.
We will use big-oh notation rather than theta to get an upper bound on the growth of the number of steps as the analysis is simpler
If the number of kinds of coins is 1, then T(n, k) (the total number of steps done for an input of n with k kinds of coins) is:
T(n, 1) <= T(n-1, 1) + c
where T(n-1, 1) represents the recursive call to `cc` with the first denomination of 1 kind of coin (i.e. 1) subtracted from the amount and `c` is some
constant which represents the number of operations done at this step
Expanding T(n-1, 1), we get
T(n, 1) <= T(n-2, 1) + 2c
Expanding till the base case,
T(n, 1) <= n c
Thus, the order of grown of steps is proportional linearly to n, so T(n, 1) = big-oh(n)

Now, if the number of coins is 2, the first denomination (d) is 5, and for ease of analysis
we will let n = b.d where b is some natural number
T(n, 2) <= T(n, 1) + T(n-d, 2) + c
T(n, 2) <= n c + T(n-d, 1) + T(n-2d, 2) + 2c
T(n, 2) <= n c + (n-d)c + T(n-2d, 1) + T(n-3d, 2) + 3c
....
T(n, 2) <= n c + (n-d)c + ... + (n-(b-1)d) c + [(n-bd)c] + b.c
T(n, 2) <= c [(b+1).n - ((b)(b+1)d/2) + b]
T(n, 2) <= c [(b+1).n - (n(b+1)/2) + b]
T(n, 2) <= c [n(b+1)/2] + b]
T(n, 2) <= c [n(n+d)/2d] + (n/d)]
T(n, 2) <= c [(n^2+ nd)/2d] + (n/d)]
T(n, 2) <= (c.n^2 + (c.d + 2)n)/2d
T(n, 2) <= (c.n^2 + (c.5 + 2)n)/10
Thus, using big-oh notation and surpressing constants and lower order terms, we can see that
T(n, 2) = O(n^2)

T(n, 2) <= T(n, 1) + T(n-d, 2) + c
T(n, 2) <= T(n, 1) + T(n-d, 1) + T(n-2d, 2) + 2c
T(n, 2) <= T(n, 1) + T(n-d, 1) + T(n-2d, 1) + ... + T(n-bd,1) + bc
T(n, 2) <= n + (n - d) + (n - 2d) + ... + (n - bd) + bc
T(n, 2) <= n(b+1) - b(b+1)d/2 + bc
T(n, 2) <= n(b+1) - n(b+1)/2 + bc
T(n, 2) <= n(n+d)/2d + nc/d
T(n, 2) <= n^2/2d + nd/2d + nc/d
Thus O(n^2)



Now, for 3 kinds-of-coins, d = 10, and we will let n = b.d where b is some natural number (different from the above case of 2 kinds of coins)
T(n, 3) <= T(n, 2) + T(n-d, 3) + c
T(n, 3) <= T(n, 2) + T(n-d, 2) + T(n-2d, 2) + 2c
...
T(n, 3) <= T(n, 2) + T(n-d, 2) + T(n-2d, 2) + .... + T(n-bd, 2) + bc
T(n, 3) <= c.n^2 + (c.5 + 2)n + c.(n-d)^2 + (c.5 + 2)(n-d) + .... + c.(n-bd)^2 + (c.5 + 2)(n-bd) + bc
T(n, 3) <= c.n^2 + (c.5 + 2)n + c.(n-d)^2 + (c.5 + 2)(n-d) + .... + c.(n-bd)^2 + (c.5 + 2)(n-bd) + bc
(c.5 + 2)[n(b+1)-(b)(b+1)d/2]
(c.5 + 2)[n(n/d+1)-n(n/d+1)/2]
(c.5 + 2)[(n^2/d)+n)/2] + 10c


T(n, 3) <= T(n, 2) + T(n-d, 2) + T(n-2d, 2) + .... + T(n-bd, 2) + bc
T(n, 3) <= n^2 + (n-d)^2 + (n-2d)^2 + .... + (n-bd)^2 + bc
Now for (n-c)^2 where c is some constant, the largest term with regards to n will be n^2, hence
(if you question as to why we don't bother with the negative `n` terms in the expansion)
T(n, 3) <= n^2 + n^2 + n^2 + .... + n^2 + bc
T(n, 3) <= n^2(b+1) + bc
T(n, 3) <= n^2(n+d)/d + nc/d
T(n, 3) <= n^3/d + n^2d/d + nc/d
O(n^3)