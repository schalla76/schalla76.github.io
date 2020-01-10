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
