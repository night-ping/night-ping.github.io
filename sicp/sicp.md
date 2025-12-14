# Understanding Recursion

Recursion used to be scary for me, but after reading SICP, I realized it's just a function calling itself!

```scheme
(define (fact n)
  (if (= n 0)
      1
      (* n (fact (- n 1)))))
```

