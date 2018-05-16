## Operator-Driven Programming with Algebraic Effect Handlers

A synoptic overview of an alternative to lambda-calculus (anonymous function)
based languages combined with a novel and ergonomic approach to handling
side-effects safely.  Theoretical and Fun!

---

## What This Talk Really is

Experimenting with programming language design grounded in Combinator Calculus and
leveraging algebraic effects to produce a syntactically clean language with a
robust effect system without the headache of monads

---

## Why do we need an alternative?

### Which languages are shaped by lambda calculus-derived structures?
--

nearly all of them
(with one glorious exception)
--

### What's the alternative?
--

Combinator Calculus
--

### But what even are these things?

Where do programming languages come from?

---

## The underlying matter of formal reasoning
(Warning: There Be Dragons)

- In the course of experience some chains of reasoning appear to share a definite
formal structure e.g., All people are mortal; Socrates is a person; therefore, Socrates is mortal

- Similarly we encounter objects, either in the physical world or in abstraction,
which also appear to share common forml structures, e.g., a triangle or square


The first region of shared structures goes by the name "Logic", and it provides
both an illuminating framework for investigating what is possible within the
realm of formal reasoning as well as establishing normative rules for abstract
cognition.

The second indicates a provisional development of "Mathematics"; it provides a
formal ontology for understanding and manipulating objects

---

## Models of Computation

Within mathematics, it is possible to study the properties of objects,
e.g., that the sum of the angles of a triangle share particular relations with
each other.

But sometimes we care not just about the actual results obtained from a
particular computation but the steps involved in reaching the conclusion.  While
there may be dozens of potential methods for finding the greatest common divisor
of a number, it may be convenient to choose one with the least steps or
computational overhead.

This brings to mind the distinction between the extensional and intensional
properties of a given method.

--

The varying systems used to describe the intensional properties of
computation methods we'll call "models of computation".

---

### A few examples of computational models

- Recursive Functions
- Markov Algorithm
- Finite State Machine
- Combinator Calculus
- Turing Machine
- Lambda Calculus

--

Notice anything familiar?

---

### Lambda Calculus and Combinator Calculus are different Computational Models

In short, programming languages are borne out of computational models since they
are a nice fit for practically computing values or formally manipulating objects

They aren't necessarily creatures of mathematics or logic but inhabit a hazy
space in between.

--

### What differentiates them?

---

### The core of Lambda Calculus

Lambda calculus approaches the problem of computation using function abstraction
and application, i.e. defining functions and then calling them.

by wrapping a bit of code in a function:

```javascript
const name = 'bobby talley';
console.log(name)

function printName(greeting) {
  const name = 'bobby talley';
  console.log(`#{greeting} #{name}`);
}

printName('hey there');
```

I have "abstracted" the previous two lines of code into a function

I can then "apply" the function to zero or more arguments.

---

Here is a slightly more formal way of describing this core

### Term Equality
M → N

―――――

AM → AN

### Application Equality
M → N

―――――

MA → NA

---


### Abstraction Equality
M → N

―――――

λx[M] = λx[N]


#### Beta Reduction

――――

(λx[M])A → M[x := A]

---

### What's the problem?

The seams we use to stitch together our programs become increasingly tied to the
order, type, and use of parameters assigned to particular functions. This
unfortunate property inevitably leads to developing programs that are by nature
nearly impossible to effectively understand.

Several patches or strategies for ameliorating these problems have been proposed

---

## Proposals

- Static Type disciplines, wherein function "signatures" can be automatically
  analyzed before execution to alert the developer if inconsistencies have
  emerged
--

- Increasing layers of abstraction and modularity by which the creeping
  complexity of varying data terms that span across blocks of code, viz., the
  subject and arguments of functions, are further encapsulated
--

- Attempts at writing "clean" code, e.g., method chaining, exotic forms of
  function composition to enhance referential integrity à la clojure, data
  bundling into objects or compound structures to further reduce passing around in
  tightly coupled clumps of variables


:(
---

## To date, these solutions leave much to be desired

```rust
error[E0412]: cannot find type `Error` in this scope
--> src/lib.rs:792:84
|
792 |     ) -> Result<Box<Fn(u64, pid_t) -> Result<Vec<String>,
copy::MemoryCopyError>>, Error> {
|
^^^^^ not found in this scope
help: possible candidates are found in other modules, you can import
them into scope
|
739 |     use failure::Error;
|
739 |     use std::error::Error;
|
739 |     use std::fmt::Error;
|
739 |     use std::io::Error;
}
```

wat
---

### Lawl

java.lang.Object
  extended by
  org.apache.xmlrpc.server. \
  RequestProcessorFactoryFactory.RequestSpecificProcessorFactoryFactory

(the class)
---

```clojure
(ns my.ns
  (:require [cheshire.factory]
            [cheshire.generate :as cheshire]
            [clj-time.coerce :as tc]
            [clj-time.core :as t])
  (:import [org.joda.time.DateTime]
           [com.fasterxml.jackson.core.JsonGenerator]
           [java.text.SimpleDateFormat]
           [java.util SimpleTimeZone]))

(defn cheshire-add-jodatime-encoder! []
  (cheshire/add-encoder
   org.joda.time.DateTime
   (fn [^org.joda.time.DateTime d ^JsonGenerator jg]
     (let [sdf (SimpleDateFormat. cheshire.factory/default-date-format)]
       (.setTimeZone sdf (SimpleTimeZone. 0 "UTC"))
       (.writeString jg (.format sdf (tc/to-date d)))))))

(cheshire-add-jodatime-encoder!)
```

---
### Close, but no cigar

```javascript
new Kitten()
  .setName('Bob')
  .setColor('black')
  .setGender('male')
  .save();
```

This strategy requires returning `this` from every method you
want to chain from which gets wonky

---

### Revisiting a classic

```
Read a file of text, determine the n most frequently used words, and print out a
sorted list of those words along with their frequencies.
```

---

### The Ultimate Answer

```bash
tr -cs A-Za-z '\n' |
tr A-Z a-z |
sort |
uniq -c |
sort -rn |
sed ${1}q
```
~Doug Mcllroy
--


## What makes this so satisfying?
--

# _The suppression of argument passing/variable assignment_

---

## . . . And what is the defining characteristic of Combinatory Logic as opposed to Lambda Calculus?

--

_As far as possible, to reduce the role of bound variables, i.e., make all
functions take at most one argument, and then pass it through a chain of
commands implicitly._

This has come to be known as "point-free" style in some languages.

---
## Compare the `thread` macro in clojure

```clojure
(defn describe-number [n]
  (cond-> []
    (odd? n) (conj "odd")
    (even? n) (conj "even")
    (zero? n) (conj "zero")
    (pos? n) (conj "positive")))

(describe-number 3) ;; => ["odd" "positive"]
(describe-number 4) ;; => ["even" "positive"]
```
---

## Or the "Point-Free" style of Haskell

#### Point free:
```Haskell
foldr f e . map g == foldr (f . g) e
```

#### "Point-ful"
```Haskell
foldr f e . map g == foldr f' e
     where f' a b = f (g a) b
```

---
All functions, propositional and otherwise,
are for Schonfinkel one-place functions,
thanks to the following ingenious
device (which was anticipated by Frege
(1893, § 36)). Where F is what we would
ordinarily call a two-place function,
Schonfinkel reconstrues it by treating
Fxy not as F(x, y) but as (Fx)y. Thus F
becomes a one-place function whose value
is a one-place function. The same trick
applies to functions of three or more
places; thus "Fxyz" is taken as "((Fx)
y)z ". In particular, a dyadic relation, as
an ostensibly two-place propositional
function, thus becomes a one-place function
whose value is a class.
---
digression of an operator

What kind of functions do [operator terms] represent?
. . . In the 1920s when lambda calculus and Combinatory Logic began, logicians
did not automatically think of functions as sets of ordered pairs, with domain
and range given, as mathematicians are trained to do today.  Throughout
mathematical history, right through to computer science, there has run another
concept of function, less precise at first, but strongly influential always;
that of a function as an operation-process (in some sense) which may be applied
to certain objects to produce other objects.  Such a process can be defined giving
a set of rules describing how it acts on an arbitrary input-object. (the rules
need not produce an output for every input).

- function or "map" could easily apply to the set-of-ordered-pairs concept.
