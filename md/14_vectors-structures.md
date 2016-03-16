<!DOCTYPE html>
<html lang="en-GB">
    <!-- scheme notes by NewForester is licensed under a Creative Commons Attribution-ShareAlike 4.0 International Licence. -->
<head>
    <meta charset="UTF-8" />
    <meta name="description" content="Notes on the Yet Another Scheme Introduction tutorial" />
    <meta name="keywords" content="Scheme" />
    <meta name="author" content="NewForester" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="../styles/style-sheet.css" />

    <title>Scheme Notes: Vectors and Structures</title>
</head>

<body>
# Scheme

## Vectors and Structures

Lists and numbers are the most commonly used data types in Scheme but there are others.

This chapter introduces Scheme macros.
These are covered in greater detail in a later chapter.

<hr /><!-- Vectors -->

In Scheme, `vectors` are sets of heterogeneous data indexed by integer.
They are more compact than lists and random access is faster.
However, they are manipulated using side effects.

Literal vectors are represented thus:

```scheme
    '#(1 2 3)                           ; a vector of integers
    '#(a 0 #\a)                         ; a heterogeneous vector comprising a symbol, an integer and a character
```

Vector specific functions are:

```scheme
    (vector? obj)                       ; #t if obj is a vector

    (make-vector k)                     ; create a vector with k elements
    (make-vector k fill)                ; ditto and set each element to the value of (fill)

    (vector obj1 obj2 ...)              ; returns a vector comprising obj1 obj2 ...

    (vector-length vector)              ; the length of the vector (aka number of elements)

    (vector-ref vector k)               ; returns the k-th element of the vector
    (vector-set! vector k obj)          ; sets the k-th element of vector to obj

    (vector-fill! vector fill)          ; sets all the items of vector to (fill)

    (vector->list vector)               ; converts vector to a list
    (list->vector list)                 ; converts list to a vector
```

<hr /><!-- Structures -->

In Scheme, `structures` are composite heterogeneous data indexed by name.
There are generated by Scheme macros that automatically create accessors and setters.

Structures are not defined in R<sub>5</sub>RS but many Scheme implementations implement structures similar to those of Common Lisp.

The implementation of `structure` is based on `vector`.
Each element of the vector is named using a `macro`.

Structures are defined using the `define-structure` function.
One way of doing this is:

```scheme
    (define-structure book title authors publisher year isbn)

    (define bazaar
      (make-book
        "The Cathedral and the Bazaar"
        "Eric S. Raymond"
        "O'Reilly"
        1999
        0596001088))
```

However, this relies on the order of parameters.

The `keyword-constructor` option allows for a clearer constructor:

```scheme
    (define-structure (book keyword-constructor copier)
      title authors publisher year isbn)

    (define bazaar
      (make-book
        'title      "The Cathedral and the Bazaar"
        'authors    "Eric S. Raymond"
        'publisher  "O'Reilly"
        'year       1999
        'isbn       0596001088))
```

The `copier` option creates a copy function for the structure.

The following illustrates the functions created by the macro:

```scheme
    (book? bazaar)                      ; test whether bazaar is a book structure
    (copy-book bazaar)                  ; return a copy of the book bazaar

    (book-title bazaar)                 ; return the title field
    (set-book-year! bazaar 2001)        ; set the year field
```

<hr /><!-- Aside -->

You can compile functions so they execute faster:

```scheme
    (compile-file "mastermind.scm")
    (load "mastermind")
    (mastermind)
```

</body>
</html>