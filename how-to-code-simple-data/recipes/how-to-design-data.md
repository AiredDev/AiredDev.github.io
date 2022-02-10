## Prerequisite information
A data definition establishes the represent/interpret relationship between data: 
- Information in the program domain is represented by data
- Data in the program can be interpreted as information in the program's domain.

A data definition describes how to form data that satisfies the data definition, as well as how to tell whether it does. It must also describe how to **represent** information as data, and how to **interpret** data as information.

## Recipe
The HtDD recipe consists of 4-5 steps.

Before beginning the recipe, we need to identify the structure of the information in the problem domain.

For example, in a program, we may want to represent two separate pieces of information with the set of natural numbers: one number for the speed of a ball, and another for the altitude of an aeroplane. If we were to come across a natural number without context in this program, there is ambiguity as to what it represents. Data definitions clarify what we're talkikng about.

The first step of the data definition recipe is to identify the structure of the information.

| Structure                      |Definition type|
|:------------------------------:|:-------------:|
| Atomic                         | Atomic        |
| Numbers within a certain range | Interval      |
| Fixed number of distinct items | Enumeration   |
| Two or more subclasses, at least one of which is **not** a distinct item | Itemization |
| Two or more items that naturally belong together | Compound data |
| Naturally composed of different parts | Reference to other defined type|
| Of arbitrary or unknown size  | Self-referential or mutually referential |

Now that we know the structure of our data, we can begin the recipe proper.
1. If the data is compound, naturally composed of different parts, or of arbitrary or unknown size, create a **structure definition**.
2. Write a **type comment** that  defines a new type name and describes how to form data of that type.
3. Write an **interpretation** that describes the correspondence between information in the problem domain and data in the program.
4. Create one or more **examples** of the data.
5. Write the correct **template** for a one-argument function that operates on this type, using the table in Appendix A.

Use a block of code like the following to create a data definition:

```lisp
;; <DataTypeName> is <PrimitiveDataType>
;; interp. <correspondence between information in problem domain and data in program>

;; <examples of how the data represents information>

#;
(define (fn-for-data-type-name <parameter>)
  <body>)

;; Template rules used:
;;  - <rule>: <type-of-data>
;;  ...
```

Example (atomic data):

```lisp
;; Time is Natural
;; interp. number of clock ticks since start of game

(define START-TIME 0)
(define OLD-TIME 1000)

#;
(define (fn-for-time t)
  (... t))

;; Template rules used:
;;  - atomic non-distinct: Natural
```

Example ()

