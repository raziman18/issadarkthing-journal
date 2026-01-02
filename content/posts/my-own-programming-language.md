---
title: "My Programming Language (Part 1)"
date: 2022-04-02T18:52:53+08:00
tags: ["programming-language", "project-showcase"]
---

It is always a dream for me to implement my own programming language. Borrowing
ideas from other programming languages out there and add it to my language by my
own desire. For example clojure's persistent data structure helps programmer to
think in functional way without sacrificing memory. Haskell's lazy data
structure enables programmers to create infinite data and also Go's
parallelism using goroutines.

My programming language is called [Spirit](https://github.com/issadarkthing/spirit).
It is a clojure inspired language implemented in go. This project is actually is
a fork of [Sabre](https://github.com/spy16/sabre), I built my language on top of
this project.

```clojure
(defn program []
  (print "hello world")

(program)
```

## Language features
There are few main differences between my language with the language I am
inspired from. Below are the top features I've implemented in Spirit.

### Function hoisting for `defn` and `defmacro`
One of the feature I miss from javascript land is function hoisting. Function
hoisting is when the order of the function definition is not important. The
function can be declared at the bottom of the file and the program still can 
run. This helps me to not have to think of the order of the declaration.
```clojure
(hello "jiman" 5)

(defn hello [x i]
  (when (not (zero? i))
    (do 
      (print x) 
      (hello x (dec i)))))
```

### Function apply in clojure is equivalent to <> in spirit
`apply` is similar to array destructing when passing an array to a variadic
function in javascript. This concept is similar to clojure but I want to make it
as a keyword (sort of) instead. Fewer keystrokes to type :)

When applying values inside of `Vector` into `+` throws an error.
```clojure
(print (+ [1 2 3])) ; ArgumentError: wrong number of args (1) to '<>'
```

But `<>` can be used to fix the error.
```clojure
(print (<> + [1 2 3])) ; 6
```

### All functions that acts on Seq returns the same concrete type Seq. 
For example, applying `map` function on `Vector` returns `Vector` instead of
`List`.  This gives more predictable API as we can ensure that the function
returns the same type.

```clojure
(map #(+ 1 %1) '(1 2 3)) ; (2 3 4)
(map #(+ 1 %1) [1 2 3]) ; [2 3 4]
```

### Keyword is used instead of String as key when parsing JSON object.
In clojure, when parsing JSON object the keys in the JSON object are converted
to `String` instead. This is inefficient because `Object` in Spirit uses
`Keyword` to index. If `String` is used to index `Object`, it will have to be
coerced into `Keyword` first, that's just unnecessary work.

```clojure
(parse-json "{ \"name\": \"jiman\" }") ; {:name "jiman"}
```

### Object Oriented system
`OOP` is a one of the well known programming language that is popularized by
Java and C++. This language feature is a way to manage your code. It makes the
abstraction easier as data and function are coupled together.

Defining class in Spirit
```clojure
(defclass Car
  {:name "toyota"
   :mileage 1000}
  
  (defmethod add-mileage [self mile]
    (assoc self :mileage (+ self.mileage mile)))
  
  (defstatic kind []
    "transportation"))
```

Inherit from base class
```clojure
(defclass Human
  {:age 0
   :name ""
   :state "alive"}

  (defstatic code-name []
    "Homo Sapien")
  
  (defmethod aging [self]
    (assoc self :age (inc self.age)))
  
  (defmethod get-age [self]
    self.age)
  
  (defmethod add-age [self age]
    (+ (self.get-age) age)))


(defclass Student <- Human
  {:id ""
   :car (Car {})}
  
  (defmethod get-id [self]
    self.id)
  
  (defmethod get-factorial [self value]
    (if (= 1 value)
      value
      (* value (self.get-factorial (dec value))))))
```

Instantiate a class

```clojure
(def student (Student {:id "10" :age 20}))
```

And few other examples
```clojure
; accessing member
(assert (= student.id "10"))

; invoking method
(assert (= (student.get-id) "10"))

; inherit method and member
(assert (= (student.get-age) 20))
(assert (= student.age 20))

; use default value if member is not initialized
(assert (= "" student.name))

; access other method inside a method
(assert (= (student.add-age 10) 30))

; access nested member
(assert (= "toyota" student.car.name))

; access static method
(assert (= "transportation" (Car.kind)))

; inherit static method
(assert "inherit static method" 
        (= "Homo Sapien" (Student.code-name)))

(assert "recursive method" 
        (= 3628800 (student.get-factorial 10)))
```
