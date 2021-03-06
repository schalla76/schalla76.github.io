---
layout: post
title: "Clojure Book Notes"
date: 2019-12-31
categories: clojure
---

# Clojure Book Notes

## Project file sample

```clojure
(defproject second-edition "1.0.0"
:description "Example sources for the second edition of JoC"
:dependencies [[org.clojure/clojure "1.5.1"]
[org.clojure/clojurescript "0.0-2138"]
[org.clojure/core.unify "0.5.3"]
[org.clojure/core.logic "0.8.5"]]
:source-paths ["src/clj"]
:aot [joy.gui.DynaFrame]
:plugins [[lein-cljsbuild "0.3.2"]]
:cljsbuild
{:builds
[{:source-paths ["src/cljs"]
:compiler
{:output-to "dev-target/all.js"
:optimizations :whitespace
:pretty-print true}}
{:source-paths ["src/cljs"]
:compiler
{:output-to "prod-target/all.js"
:optimizations :advanced
:externs ["externs.js"]
:pretty-print false}}]})
```

## apply function

- apply function takes the vector and sends it to a function as arguments

Example:

```clojure
(def numbers [1 2 3 4 5 6 7 8 9 10])
(apply + numbers)
```

## Polymorphism

- Its done using protocol extension

Example:

```clojure
(extend-type java.util.List
Concatenatable
(cat [this other]
(concat this other)))
```

## Integer data types

```clojure
[127 0x7F 0177 32r3V 2r01111111]
;=> [127 127 127 127 127]
```

- 0x7F is hexadecimal
- 0177 is octadecimal
- 8r177 is octadecimal
- 32r3V is base 32
- 2r01111111 is binary

## Rational numers

Example:

```clojure
22/7
-7/22
1028798300297636767687409028872/88829897008789478784
-103/4
```

## Characters

Example:

```clojure
\a ; The character lowercase a
\A ; The character uppercase A
\u0042 ; The Unicode character uppercase B
\\ ; The back-slash character \
\u30DE ; The Unicode katakana character ?
```

## Function with multiple number of arguments

```clojure
(defn make-set
([x] #{x})
([x y] #{x y}))
```

- The extra arguments are sent as list

```clojure
(defn arity2+ [first second & more]
(vector first second more))
```

## Blocks

- The scope of the variables is only defined in the block.

```clojure
(do
(def x 5)
(def y 4)
(+ x y)
[x y])
; Output [5 4]
```

## Local

- This is similar to the Block defined about

```clojure
(let [r 5
pi 3.1415
r-squared (* r r)]
(println "radius is" r)
(* pi r-squared))
;; radius is 5
;;=> 78.53750000000001
```

## (pos? x)

- Returns true if number is greater than zero

## recur

- Makes a recursive call, but if its in the loop then go the start of the loop
- recur can only be used in the tail statements

## loop

- loop syntax is similar to the let, having a vector with pairs of variable and its binding value. The loop is also target for recur

## quote ; shortcut '

- Quote stops the evaluation of the expression

## syntax quote ; shortcut `

- Qualifies unqualified symbol

Example:

```clojure
`map
;=> clojure.core/map
```

## unquote ; shorcut ~

- opposite of quote

Example:

```clojure
(let [x 2]
`(1 ~x 3))
;=> (1 2 3)
```

## Accessing Java from clojure

### Static class members

```clojure
(Math/sqrt 9)
```

## Namespaces

### Current name space is represented by _ns_

Example:

```clojure
joy.ch2=> (defn report-ns []
(str "The current namespace is " *ns*))
joy.ch2=> (report-ns)
;=> "The current namespace is joy.ch2"
```

### Call the method from a namespace

name-space/method-name

### Load the namespace use :require

```clojure
(ns joy.req-alias
(:require [clojure.set :as s]))
(s/intersection #{1 2 3} #{3 4 5})
;=> #{3}
```

## Logic

- Everything is clojure is true except false and nil
- Seq returns the nil if the vector is empty

## Scope

The names for the function and symbols are same

## Regular expression

Example:

```clj
#"Test"
```

## Persistence

- The vectors, lists, maps etc are immutable

## Sequences

- The sequential include list, vectors and java arrays
- set and map are not sequential

## Seqence functions

- first
- rest

## Vectorss

- vec function returns vector

```clj
- (vec (range 10))
```

- into function adds more items to vector

```clj
(let [my-vector [:a :b :c]]
(into my-vector (range 10)))
;=> [:a :b :c 0 1 2 3 4 5 6 7 8 9]
```

- vector-of function allow same data-type in the vector

```clj
(into (vector-of :int) [Math/PI 2 1.3])
;=> [3 2 1]
(into (vector-of :char) [100 101 102])
;=> [\d \e \f]
```

- Accessing nth element

```clj
(def a-to-j (vec (map char (range 65 75))))
a-to-j
;;=> [\A \B \C \D \E \F \G \H \I \J]

;nth function
;throws exception if vector empty
;supports not found argument
(nth a-to-j 4)
;;=> \E

;get function
;returns nil if empty vector
;supports not found argument
(get a-to-j 4)
;;=> \E

;vector as function
;throws exception if vector empty
;does not supports not found argument

(a-to-j 4)
;;=> \E
```

- assoc changes the nh element of vector and return new vector

```clj
(assoc a-to-j 4 "no longer E")
;=> [\A \B \C \D "no longer E" \F \G \H \I \J]
```

- replace function replaces multiple items

```clj
(replace {2 :a, 4 :b} [1 2 3 2 3 4])
;=> [1 :a 3 :a 3 :b]
```

- assoc-in, get-in and update-in changes items in nested structures

## Understanding thead macors

[Understanding Clojure's thread macros](https://www.youtube.com/watch?v=qxE5wDbt964&t=236s)

- Use thread last (->>) macro for sequences
- Use thread first (->) macro for objects (getter and setter)
- We can use three (,,,) for the replacement arguments for readability
- Clojure.walk/macroexpand-all expand the macro
- We can use comment reader macro (#\_) to comment part of thread macro
- We can start with thread first and switch to thread last but if you start with thread last then to switch to thread first we need to use anonymous functions.

## Understanding list comprehension in Clojure

[Understanding list comprehension in Clojure](https://www.youtube.com/watch?v=5lvV9ICwaMo&t=1087s)

Examples:

```clj
(defn listcomprehension
  []
  (count (for [tumbler-1 (range 10)
               tumbler-2 (range 10)
               tumbler-3 (range 10)
               :when (and (or (= tumbler-1 4)
                              (= tumbler-2 4))
                          (= tumbler-3 4))]
           [tumbler-1 tumbler-2 tumbler-3])))

(defn ticket-number
  []
  (def blacklisted #{\I \O})
  (def capital-letters (map char (range (int \A) (inc (int \Z)))))

  (for [letter-1 capital-letters
        letter-2 capital-letters
        :when (and (not (blacklisted letter-1))
                   (not (blacklisted letter-2)))]
    (str letter-1 letter-2)))
```
