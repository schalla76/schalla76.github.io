# Clojure Notes

## Some of useful links

[Solving Problems Declaratively - Mark Engelberg](https://www.youtube.com/watch?v=TA9DBG8x-ys)

[Mark Engelberg - Github repos](https://github.com/Engelberg?tab=repositories)

# Clojure course

[Source: Clojure: The Complete Beginner's Guide](https://www.udemy.com/course/clojureprogramming/)

## Setup Dev Environment

Tools needed:

1. [Java](https://www.java.com/en/)
2. [Java SDK](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html)
3. [Leiningmen](https://leiningen.org/)
4. [IntelliJ Idea](https://www.jetbrains.com/idea/)
5. [Cursive](https://cursive-ide.com/)
6. [Calva](https://marketplace.visualstudio.com/items?itemName=betterthantomorrow.calva)

### Using Leiningen

#### Create new app

```
lein new app app-name
```

#### Run app

```
lein run
```

### Using REPL

```
lein repl
```

## Basics

- Clojure is complied language
- It complies into JVM i.e into .class files
- Clojure everything is a list
  - ex: (verb param1 param2...)
- Everything has a return value

## Functions

### Function Example

```clojure
(ns example.functions
    (:gen-class))

(defn -main
    "Example function"
    []
    (println "This first function")
    (+ 2, 3))
```

### Anonymous Function

Starts with # and takes arguments has %

Example

```clojure
(#(println "This is my" % "anonymous" %) "first" "function")
```

- defn creates a function
- def assigns name to the function

Example

```clojure
(def increment (fn [x] (+ x 1 )))

(defn increment_set
    [x]
    (map increment x))
```

## Data Types

- Clojure inherits the data types from Java
- Some of the data types supported are:
  - Integer
  - Float
  - Hex
  - Null
  - Boolean
  - String
  - Keyword

Example:

```clojure
(defn datatypes
    []
    (def a 1)
    (def b 1.5)
    (def c 0xabc12)
    (def d nil)
    (def e true)
    (def f "Hello World")
    (def g 'important)

    (println a)
    (println b)
    (println c)
    (println d)
    (println e)
    (println f)
    (println g))
```

### Other datatypes

- Integer
  - Short
  - Long
  - Octal
  - Hexadecimal
- Atoms
- Java data types
  - Java.Lang.Byte
  - Java.Lang.Short
  - Java.Lang.Integer
  - Java.Lang.Long
  - Java.Lang.Float
  - Java.Lang.Double

## Variables

- Variables are immutable, means we can't change the value in them
- Variables can't start with number. They can have numbers, letters and underscore
- Variables are case sensitive

## Operators

### Arithmetic Operators

```clojure
(+ 1 2)
(- 1 2)
(* 1 2)
(/ 1 2)
(INC 5)
(DEC 5)
(MAX 10 5)
(MIN 10 5)
(REM 10 5)
```

### Relational Operators

```clojure
(= 2 2)
(NOT= 2 4)
(< 2 4)
(<= 2 4)
(> 2 4)
(>= 2 4)
```

### Logical Operators

```clojure
(AND True False)
(OR True False)
(NOT False)
```

### Bitwise Operators

```clojure
(BIT-AND 0110 1100)
(BIT-OR 0110 1100)
(BIT-XOR 0110 1100)
(BIT-NOT 1100)
```

### Operator Precedence

- All the expression are prefix notation and in parenthesis, so there is no need for operator precedence

## Composite Data Types

### Set

- Set of different data types
- Immutable
- Efficient

Example:

```clojure
#{}
#{1 2 3.4 "Dog" 'Help}
```

### Map

- Key value pairs
- Immutable
- Efficient

Example:

```clojure
#{:Key1 "Value"}
#{1 42, 2 1.5, "Pet" 'Dog}
```

### Vector

- Array
- Immutable
- Efficient
- Indexed by Position

Example:

```clojure
[]
[1 2 3 4 5]
[1 2 "Hello" "World"]
```

### List

- Make up the code
- Immutable
- Efficient

Example:

```clojure
(1 2 3)
(1 "Two" 'Three (1 2 3 4))
(defn foo [] (println "Hello World"))
(foo)
```

## Conditional Statements

### If condition

Example

- Single statement under the if

```clojure
(if (= 5 5)
    println "Condition is true"
    println "Condition is false"
)
```

- Multiple statements under the if

```clojure
(if (and (= 5 5) (= 7 7) (or( (= 8 8) (not true) )))
    (do (println "Condition is true"))
    (do (println "Condition is false"))
)
```

### Case condition

Example

```clojure
(case "dog"
 "dog" (println "its dog")
 "horse" (println "its horse")
 "tiger" (println "its tiger")
 )
```

### Cond

Example

```clojure
(def amount 99)
(cond amount
    (> amount 100) (println "amount > 100")
    (< amount 100) (println "amount < 100")
    (= amount 100) (println "amount = 100")
    :else (println "from else")
)
```

## Loops

### loop

Example

```clojure
(loop [x 0]
    (when (< x 10>)
        (println x)
        (recur (inc x))))
```

### dotimes

Example

```clojure
(dotimes [x 10]
    (println x)))
```

### while

Example

````clojure
(def x (atom 0))
  (while (< @x 10)
    (do
      (println @x)
      (swap! x inc)))
      ```
````

### doseq

Example

```clojure
(defn seqloop
  "seq loop"
  [seq]
  (doseq [x seq]
    (println x)))

(seqloop [2 3 4 5])
```

## Atoms

- Thread safe way of changing the value in clojure

Example

(def x (atom 0))
(swap! x inc)))

## Sequence

Example
