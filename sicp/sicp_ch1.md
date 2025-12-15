# Building Abstraction with Procedures



This book does not talk about computer science in the usual way.
Sometimes, it feels like it is talking about magic.

As programmers, we use a magical language to cast spells.
These spells conjure a kind of spirit inside the machine — a spirit
that does exactly what we tell it to do.

In reality, a computer is a non-living device.
It is just hardware: millions or billions of circuits,
metal and silicon arranged in a very precise way.
By itself, it is lifeless.

What makes a computer feel alive is not the hardware,
but the computational processes running inside it.
Adding two numbers, repeating a task, making a decision —
these are all processes.

A process is like the spirit of the computer.
And we, as programmers, **conjure that spirit by writing spells**.
A program does not just tell the computer *what it is*,
but *what it should do* and *how it should behave*.

From this point of view, programming feels less like engineering
and more like controlled magic.



This perspective may sound funny, **but it completely changed
how I think about programs and processes.**



---

## 1.1 The Elements of Programming

the writer start this section be telling me that programming language isn't just a tool to write instructions and the computer executes **NO**, he says , programming language is much more than that it's **framework** of thinking. 

it shapes how we describe processes, how we break ideas and how we combine them into bigger ones.

and it gives us the **elements** of a programming language make it powerful:

- **primitive expressions** - the simplest things the language can talk about.

- **ways to combine things** - so simple ideas can grow into complex ones.

- **ways to abstractions** - so we can name complexity and treat it as a single unit.

these are the building blocks we have in a powerful language. 



Another interesting idea is the distinction between data and procedures.
Data is the “stuff” we want to manipulate.
Procedures are the rules that describe how to manipulate that stuff.



At this point in the book, they look like two different worlds.
But the authors hint that this **separation is not as real as it seems.**



### 1.1.1 Expressions

The first thing SICP does in surprisingly simple:

it puts you in front of LISP interpreter and asks you to type something.

you type an expression.

the interpreter evaluates it.

then it prints the result. 

that's it. 

no "print" command , no ceremony. just an expression and its value.

typing a number like `486` returns `486` a number is a valid expression. it doesn't do anything - it simply evaluates to itself.



things get more interesting when expressions are combined. when writing:

```scheme
(+ 132 356)
```

i'm no longer dealing with a number , but with a combination of expressions , the interpreter evaluate each part then applies the procedure of the operator to produce the results



the notation of combination by making operator first called **prefix notation** it fell unnatural first but there's no ambiguity the operator come always in the first and everything else belongs inside the parentheses this make it easy to extend the expression be make it nested 

```scheme
(+ (* 3 5) (- 10 4))
```

there's no limit for this nesting and the interpreter has no problem with that , it's we humans who get confused. 



we can use pretty-printing formatting by use indentations in a smart way to aid us understanding the combination. 



no matter how complex the expression is the interpreter always follows the same rhythm: 

1. read an expression

2. evaluate it

3. print the result 

4. repeat

this simple loop called - read, eval, print loop 



### 1.1.2 Naming and Environment

**giving names to things** is our next ability in programming 

a name allows us to refer to a computational object without having to remember how it was constructed. when we give something a name , we are creating a **variable** whose value is that object.

we use define to did this naming - `(define size 2)` , after this line the interpreter associates the name `size` with the value `2` so from this point `size` isn't just a normal word for interpreter it mean something , it mean the value `2` . 

we can use it in expressions - `(* 5 size)` , this sound trivial but it introduces **abstraction**



`define` is simplest abstraction mechanism in the language . instead of repeating compuation every time .we can give it a name and treat it as a **unit**.



```scheme
(define pi 3.14159)
(define radius 10)

(* pi (* radius radius))
;; 314.159


; compare with this 
(* 3.14159 (* 10 10))


;first version communicates meaning, second just numbers


; we can go one step further 
(define circumference (* 2 pi radius))

circumference
;; 62.8318

; here the details of computation is hidden behind single name
; we not just hide numbers , we hide compound expression
```

this abstraction in action : **hiding complexity behind a name** 





for names to work , the interpreter must remember them, that memory is called the **environment** . 

the environment stores: **name -> value associations**

When we type `size`, the interpreter looks it up in the environment

Without the environment, symbols, names and even built-in operators like `+` would have no meaning.



abstraction isn't for styling and convenience, it is a way of thinking , using names or any other abstaction strategy is for moving from just writing instruction to a machine to more like **building a conceptual system**



### 1.1.3 Evaluating Combinations

even interpreter itself follow a procedure when evaluating something.

imagine combination like this 

```scheme

(* (+ 2 (* 4 6))
   (+ 3 5 7))
```

- first, evaluate each part of combination 
  
  - `*` -> the procedure of multiplication in the language
  
  - `(+ 2 (* 4 6))` , which itself another expression need to evaluate , so here evaluate is recursive , **to evaluate bigger expression we need evaluate small one first**.
  
  - `(+ 3 5 7)` same here 

- second, apply the operator to the operands (arguments)
  
  - `(* 26 15)` 



evaluating recursion reach its base cases : 

- numbers -> evaluate to themselves

- built-in operators -> evaluate to the sequence of machine instructions that do the work (**in environment too**)

- names/variable -> evaluate to whatever object they are bound to in the environment 



Not everything in parentheses is combination follow same procedure , there's **special forms** like : `(define x 3)`  this isn't evaluate like `(+ x 1)`

define doesn't take arguments in the usual way; its purpose is to **bind a name to a value** 

**each special form has its own evaluate rules.**



### 1.1.4 Compound Procedures

After introduce some elements of any powerful language

we have more powerful technique in abstraction , as we can abstract some compound expressions under some name , we can define a procedure and give it a name to deal with it as a unit. 

for first example , we can express the idea of **squaring** which says , to square something multiply it by itself. which can expressed as:

```scheme
(define (square x) (* x x))


```

here we created a compound procedure called `square` and `x` is a local name stand for whatever number the user will pass. and the body of a procedure which is a compound expression multiplies `x` by itself. 



now we can use this procedure with no need to remember its details each time, because its packaged neatly in a name



compound procedures can be used as a building blocks for other procedures

```scheme
(define (sum-of-squares x y)
    (+ (square x) (square y)))
```



> for sections 1.1.5 - 1.1.6 i think it's better to read them from the book 



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

observe that the problem of computing square root how we breaks it up into a number of **sub-problems** : 

- how to tell the guess is good-enough

- how to improve the guess, and so on .. 

the decomposition of a big problem not a random splitting , we split it to have sub-task which each task is **identifiable** and do one thing well and can be reused in other procedures

when we define some procedure in terms of another one we able to regard this other procedure as a **black-box** and what i mean by that is to not care at this point how this compute its result , but rather care of what it compute .. **we suppressed details to considered at a later time and that called procedure abstraction it a very strong idea when thinking**



in previous example we need to build a `sqrt` procedure and we find it consist of cluster of other procedures so we define each of them . but the issue here is the only procedure for actual user using is `sqrt` others is just helpers but they clutter the global namespace , this will brings a trouble if there's another procedure use same helper names with different implementation details , they will conflict .



the solution we can hide the procedure of helpers inside `sqrt` these internal procedures only visible inside `sqrt` procedure not outside 



This is called **block structure**: defining functions inside other functions.

in this situation we **don't have to pass** `x` to internal procedures definitions while now its a free variable in sqrt scope can be accessed 

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




