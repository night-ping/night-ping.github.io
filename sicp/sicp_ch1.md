# Building Abstraction with Procedures

computer is just a device consist of some metal components , it seems not a alive creature .. but we can conjure it's spirit .

computer do a computational process all the time and that what make it special device or more clearly that what make it alive

computational process is like abstract beings inhbit computers, it perform a specific task, by did a specific instructions in some code (our spells) using our magical language (any programming language e.g LISP)

in short , **we conjure the spirit of computer using our spells.**

> lisp come in many dialects , in SICP course we use Scheme LISP

---

## 1.1 The Elements of Programming

a powerful programming language isn't just a means for instructing a computer to perform tasks .. it also serve as a framework within we organize our ideas about prcesses.  Thus, we should pay attention to what the language provide us to combining our ideas to form complex ideas .

every powerful language has three mechanisms for accomplishing this: 

- **primitive expressions** (what the simplest entities in the language)

- **means of combination** (how to combine simple elements to complex ones)

- **means of abstraction** (how to deal with compound elements as units)

### 1.1.1 Expressions

LISP is interpreter language .. you type **expression** , and the interpreter respond by displaying the result of its **evaluate** of that expression (no print command it print the result naturally after evaluate the expression)

```scheme
123      ;print 123

(/ 10 5) ; compound expression using (), it result 2
         ; it apply the procedure of operator on the operands
         ; this called combination use the prefix notation
```

### 1.1.2 Naming and the Environment

we can use a name to refer to computational objects , we say that the name identifies a variable whose value is the object 

```scheme
(define size 2) ; assign value of object 2 to the variable size


(define pi 3.14159)
(define radius 10)
; it can refer to compound operations such that
(define circumference (* 2 pi radius)
```

`define` is the simplest way to abstraction it hide the compound operations under a simple name we can use 

this associating need some sort of memory to keep it the variable/value pairs and in LISP this is what called **environment** 

### 1.1.3 Evaluating Combinations

simply to evaluate a combination: 

- Evaluate the subexpressions of the combination

- apply the procedure of the operator to the operands in the expression

> evaluating is recursive in nature (evaluate expression need to evaluate sub expression first)

- evaluate numbers -> numbers 

- built-in operators i.e `+` -> procedure of that operator **saved in environment too**

- names -> value of that names in the environment

> without the environment any symbol or name will has no context in the code. 

- some expressions not evaluating normally called **special forms** like `define` 



### 1.1.4 Compound Procedures

procedure defintino is much more powerfil abstraction technique , you can write a procedure and give it a name refer to it as a **unit**scheme

```scheme
; to square something , multiply it by itself.
(define (square x) (* x x))
```

- evaluate such definition , create compound procedure and associates it with name square.

- we can build another definition from old definition 

```scheme
(define (sum-of-squares x y)
    (+ (square x) (square y)))
```



> 1.1.5 - 1.1.6 i think it's better to read them from the book 



### 1.1.7 Example: Square Roots by Newton's Method

it's good to know that computer science is concerned with imperative knowledge **how to** not declarative **what is** and this reflects on the way we write procedures , we define them in terms of **how to**



example: square root procedure :

- what is ? -> 

$$
\sqrt{x} = \{ y \mid y \ge 0,\quad y^2 = x\} 
$$



is describes a perfectly legitimate mathematical function. On the other hand, the definition does not describe a procedure.



- how to ? -> 
  
  the most common way is to use Newton’s method of successive approximations, which says that whenever we have a guess $y$ for the value of the square root of a number $x$ , we can perform a simple manipulation to get a beer guess (one closer to the actual square root) by averaging $y$ with $x/y$.
  
  

Now let’s formalize the process in terms of procedures. We start with a value for the **radicand** (the number whose square root we are trying to compute) and a value for the guess. If the guess is good enough for our purposes, we are done; if not, we must repeat the process with an improved guess.

```scheme
(define (sqrt-iter guess x)
    (if (good-enough? guess x)
        guess
        (sqrt-iter (improve guess x) x)))

(define (improve guess x)
    (average guess (/ x guess)))


(define (average x y)
    (/ (+ x y) 2))


(define (good-enough? guess x)
    (< (abs (- (square guess) x)) 0.0001))


(define (sqrt x)
    (sqrt-iter 1.0 x))
```



### 1.1.8 Procedures as Black-Box Abstractions

observe that the problem of computing square root breaks up into a number of **sub-problems** : 

- how to tell the guess is good-enough

- how to improve the guess, and so on .. 

each of these tasks accomplished by a separate procedure , the entire `sqrt` program can be viewed as a cluster of procedures this mirrors the decomposition of the problem into sub-problems 

- its not a random decomposition , it's crucial that each proceduers accomplishes an **identifiable task** that can be used a module in defining other procedures . 

- when we define `good-enough?` in terms of `square` , we are able to regard  `square` procedure as a **"black-box"** ,we are not at this moment concerned with **how** the procedure computed the result, only concerned with the fact it computes the square of a number , the details of how the square is computed can be **suppressed to be considered at a later time** 

- this called **procedure abstraction**   

- we can make a block structure 

```scheme
(define (sqrt x)
    (define (good-enough? guess)
        (< (abs (- (square guess) x)) 0.001))
    (define (improve guess)
        (average guess (/ x guess)))
    (define (sqrt-iter guess)
        (if (good-enough? guess)
            guess
            (sqrt-iter (improve guess))))
(sqrt-iter 1.0))
```

---



## 1.2 Procedures and the Processes They Generate

A procedure is a pattern for the **local evolution** of a computational process. It specifies how each stage of the process is built upon the previous stage. We would like to be able to make statements about the **overall** ,or global behavior of a process whose local evolution has been specified by a procedure. we try to see the **shapes** of processes and also see the rates of consume important resources like **time and space** 



### 1.2.1 Factorial using Linear Recursion and Iteration



```scheme
; linear recursive
(define (factorial n)
    (if (= n 1)
        1
        (* n (factorial (- n 1))))) ; * n is a deferred operation

; Iteration can save the state of each step using state variables
; make no deferred operations
(define (factorial n)
    (fact-iter 1 1 n))
(define (fact-iter product counter max-count)
    (if (> counter max-count)
        product
        (fact-iter (* counter product)
                   (+ counter 1)
                   max-count)))
```

### 1.2.2 Tree Recursion

```scheme
; recursive fibonacci
(define (fib n)
    (cond ((= n 0) 0)
          ((= n 1) 1)
          (else (+ (fib (- n 1)
                   (fib (- n 2))))))

; iterative fibonacci
(define (fib n)
    (fib-iter 1 0 n))
(define (fib-iter a b count)
    (if (= count 0)
        b
        (fib-iter (+ a b) a (- count 1))))
```



in general 

- linear iteration :
  
  - space : O(1)
  
  - time : O(n)

- linear recursive :
  
  - space : O(n)
  
  - time : O(n)

> 1.2.3 - 1.2.6 read from book





---

## 1.3 Formulating Abstraction with Higher-Order Procedures


