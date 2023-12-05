# serotonin
will be a compiler for a high level language. I did a similar one in common lisp: https://github.com/jobhdez/zettapy but I want to make one that is better.

## Example 1

So far, as of 10/4/23, you can parse programs like this:

```racket
pyparser.rkt> (define p3 (open-input-string "x = 4 y = 5 z = 4 if True then z + x else z+y"))
pyparser.rkt> (the-parser (lambda () (the-lexer/tokens p3)))
(py-module
 (list
  (py-assign (py-id 'x) (py-num 4))
  (py-assign (py-id 'y) (py-num 5))
  (py-assign (py-id 'z) (py-num 4))
  (py-if-exp (py-bool 'True) (py-plus (py-id 'z) (py-id 'x)) (py-plus (py-id 'z) (py-id 'y)))))
```

## Example 2

```racket
toANormal.rkt> (define p1 (open-input-string "x = 10 + -50 y = 4 + -60 z = 4 + 5 k = 5 + -3"))
toANormal.rkt> (define ast (the-parser (lambda () (the-lexer/tokens p1))))
toANormal.rkt> ast
(py-module
 (list
  (py-assign (py-id 'x) (py-plus (py-num 10) (py-neg 50)))
  (py-assign (py-id 'y) (py-plus (py-num 4) (py-neg 60)))
  (py-assign (py-id 'z) (py-plus (py-num 4) (py-num 5)))
  (py-assign (py-id 'k) (py-plus (py-num 5) (py-neg 3)))))
toANormal.rkt> (program-to-a-normal-form ast)
(list
 (atomic-assignment (atomic (py-id "temp_0")) (py-neg 50))
 (atomic-assignment (atomic (py-id 'x)) (atomic-plus (atomic (py-id "temp_0")) (atomic (py-num 10))))
 (atomic-assignment (atomic (py-id "temp_1")) (py-neg 60))
 (atomic-assignment (atomic (py-id 'y)) (atomic-plus (atomic (py-id "temp_1")) (atomic (py-num 4))))
 (atomic-assignment (atomic (py-id 'z)) (atomic-plus (atomic (py-num 4)) (atomic (py-num 5))))
 (atomic-assignment (atomic (py-id "temp_2")) (py-neg 3))
 (atomic-assignment (atomic (py-id 'k)) (atomic-plus (atomic (py-id "temp_2")) (atomic (py-num 5)))))
toANormal.rkt>
```
