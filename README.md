# Lisp

## How to install and use

### Windows

[Allegro CL](https://franz.com/downloads/clp/survey)

### Linux

```
sudo apt-get install clisp
clisp work.lisp
```

## Syntax

### Comments

```cl
;;; Basic Comment

;; Comment inside a code block

; Comment after a life of code

#||
Multi
line
Comment
||#
```
### Output


```cl
(print "Hello.")
(format t "Hello_~%_world!")  ; `t` use terminal to print

(format t "Number with commas ~:d ~%" 1000000)
(format t "Pi to 5 characters ~5f ~%" 3.141593)

(terpri)    ; Empty line
```

Change style of printed variable.

```cl
(setq *print-case* :upcase) ; :downcase :capitalize
```

### Input

```cl
(defvar *name* (read)) ; General rule for naming a global variable
```

### Variables

```cl
(defvar *number* 0)

(setf *number 6)
```

### `if`

```cl
(defvar *age* 15)

(if (not (< *age* 18))
    (format t "You can vote~%")
    (format t "You can't vote~%"))

(if (not (< *age* 18))
    (progn  ; progn is used for multiple commands
        (format t "You can vote~%")
        (format t "this year~%")
    )
    (format t "You can't vote~%"))
```

### `case`

```cl
(defvar *age* 5)

(defun get-school (age)
    (case age
        (5 (print "Kindergarted"))
        (6 (print "First grade"))
        (otherwise (print "Something else"))))

(get-school *age*)
```

### `cond`

```cl
(defvar *age* 20)
(defvar *college-ready* nil)

(cond ( (>= *age* 18)
            (setq *college-ready* 'yes)
            (format t "Ready for college~%"))
      ( (< *age* 12)
            (setq *college-ready* 'no)
            (format t "Not ready for college~%"))
      (t (format t "Don't know ~%")))
```

### `loop`

```cl
(loop for x from 1 to 10
    do (print x))
```

Same as :

```cl
(setq x 1)
(loop 
    (format t "~d~%" x)
    (setq x (+ x 1))            ; x++
    (when (> x 10) (return x))  ; exit condition
)
```

```cl
(loop for x in '(Aa Bb Cc) do
    (format t "~s~%" x)
)
```

```cl
(loop for y from 100 to 110 do
    (print y)
)
```

```cl
(dotimes (y 12) ;; 0..11
    (print y))
```

### List

#### Constructing a list

```cl
(cons 'Aa 'Bb)      ; (Aa . Bb)
(cons 'Aa '(Bb))    ; (Aa Bb)
(list 'Aa 'Bb 'Cc)  ; (Aa Bb Cc)
(cons 'Aa '(Bb Cc)) ; (Aa Bb Cc)
```

#### `car` and `cdr`

```cl
(defvar list '(a b c d e f))
(print (car list))          ; a
(print (cdr list))          ; (b c d e f)
(print (cadr list))         ; b
(print (caddr list))        ; c
(print (cadddr list))       ; d
;(print (caddddr list))     ; caddddr is not defined
(print (car (reverse list))); f
```

#### Index based list

```cl
(defparameter *numbers* '(2 4 6 8))
(print (nth 1 *numbers*))   ; 4
(push 0 *numbers*)          ; Adds 0 as first element
(print (nth 1 *numbers*))   ; 2
```

#### Association  list

```cl
(defparameter *dict* (list (list 'one '1 'first)(list 'two '2 'second)(list 'three '3 'third)))

(print (assoc 'two *dict*))         ; (two 2 second)
(print (cadr (assoc 'two *dict*)))  ; 2
```


### Functions

```cl
(defun printing-name (*name*)
    (format t "Hello ~a! ~%" *name*))

(printing-name (read))
```

```cl
(defvar list '(a b c d e f))

(print (listp list))        ; T
(print (member 3 list))     ; NIL
(print (member 'a list))    ; (a b c d e f)
(append '(X) list)
(print list)                ; (a b c d e f)
```

#### Parameters

```cl
(defun get-avg (num-1 num-2)
    (/ (+ num-1 num-2) 2))

(format t "Average of 4 and 6 = ~a ~%" (get-avg 4 6)) ; Average of 4 and 6 = 5
```

##### Optional parameters

```cl

(defun print-list (a b &optional c d)
    (format t "List = ~a ~%" (list a b c d)))

(print-list 1 2 3)  ; List = (1 2 3 NIL) 
```

##### Multiple parameters

```cl
(defun sum (&rest nums) ; rest = receive multiple values
    (dolist (num nums)
        (setf *total* (+ *total* num))
    )
    (format t "Sum = ~a ~%" *total*)
)
(sum 1 2 3 4 5) ; Sum = 15
(sum 1 2 3)     ; Sum = 6
```

##### Keyword parameters

```cl
(defun print-list(&key a b c)
    (format t "List: ~a ~%" (list a b c))
)
(print-list :b 2 :c 3 :a 1)     ; List: (1 2 3) 
```

#### Return value

```cl
(defun difference (num1 num2)
    (return-from difference(- num1 num2)))

(print (difference 10 2))   ; 8
```

#### Oneliner

```cl
(defvar data '(a 2 3 d e f))
(print (mapcar #'numberp data)) ; (NIL T T NIL NIL NIL) 
```

#### Lambda

```cl
(mapcar (lambda (x) (print (* x 2))) '(1 2 3 4 5))
; 2 
; 4 
; 6 
; 8 
; 10 
```

#### Math

```cl
(format t "(/ 5 4) = ~d ~%" (/ 5 4))                ; 5/4
(format t "(/ 5 4.0) = ~d ~%" (/ 5 4.0))            ; 1.25
(format t "(rem 7 5) = ~d ~%" (rem 7 5))            ; 2
(format t "(mod 7 5) = ~d ~%" (mod 7 5.0))          ; 1.9999999 ?

(format t "(expt 4 2) = ~d ~%" (expt 4 2))          ; 16
(format t "(sqrt 81) = ~d ~%" (sqrt 81))            ; 9
(format t "(exp 1) = ~d ~%" (exp 1))                ; 2.7182817
(format t "(log 1000 10) = ~d ~%" (log 1000 10))    ; 3
(format t "(eq 'dog 'dog) = ~d ~%" (eq 'dog 'dog))  ; T
(format t "(floor 5.5) = ~d ~%" (floor 5.5))        ; 5
(format t "(ceiling 5.5) = ~d ~%" (ceiling 5.5))    ; 6
(format t "(max 5 10) = ~d ~%" (max 5 10))          ; 10
(format t "(min 5 10) = ~d ~%" (min 5 10))          ; 5
(format t "(oddp 15) = ~d ~%" (oddp 15))            ; T
(format t "(evenp 15) = ~d ~%" (evenp 15))          ; NIL = False  
(format t "(numberp 2) = ~d ~%" (numberp 2))        ; T
(format t "(null nil) = ~d ~%" (null nil))          ; T

(format t "(equalp 1.0 1) = ~d ~%" (equalp 1.0 1))  ; T
(format t "(equalp \"Derek\" \"derek\") = ~d ~%" (equalp "Derek" "derek")) ; T

;not and or
```

### Classes

```cl
(defclass animal() (what name))

(defparameter *dog* (make-instance 'animal))
(setf (slot-value *dog* 'what) "Dog")
(setf (slot-value *dog* 'name) "Spot")

(format t "~a is called ~a~%" (slot-value *dog* 'what) (slot-value *dog* 'name))
```