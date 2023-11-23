# Introduction
- From the point of view of theoretical computer science, **data** can be thought of as *representations of objects*, and **algorithms** can be (crudely) thought of as *operations on data*
## Objects
- How objects are represented can make a major difference on how they are operated on
  - e.g. Consider how numbers can be represented - Roman Numerals vs. Place Value System (XLII vs. 42)
    - Expressing the distance to the moon in Roman numerals would take a *very* large amount of pages to do, whereas in the place value system it would take simply 7 digits
    - This type of difference in data representation can radically change the efficiency of computation on such objects
## Algorithms
- The kind of algorithm used to operate on data is also important, especially in the context of efficiency
  - e.g. Multiplication algorithms
    - One algorithm for multiplication is to perform *repeated addition*
      -     Input: X, Y -> Output: X*Y
              Result <- 0
              For i = 0, ..., Y - 1:
                Result = Result + X
              Return Result
        - This approach is obviously very inefficient, especially as *Y* becomes larger - the time complexity is $O(Y)$, which means that it depends on the *magnitude* of the input (and not the number of digits in the input as with grade-school multiplication)
    - Another, more commonly taught algorithm for multiplication is *grade-school multiplication*
      -     Input: X: X1, ..., Xn (digits of X); Y: Y1, ..., Yn (digits of Y) 
              Result <- 0
              For i = 0, ..., n - 1:
                For j = 0, ..., n - 1:
                  Result <- Result + 10^(i+j)Xi*Yj
      - This approach is much more efficient than the repeated addition, as it has a time complexity of $O(n^2)$, where *n* is the number of *digits* of the inputs
    - An even better algorithm for multiplication was discovered by Karatsuba in 1960 (**Karatsuba's Algorithm**), which has a time complexity of $O(n^{1.6})$ - to modern day, an algorithm with time complexity of $O(n\log n)$ has been found for multiplication
## What can be computed?
- Beyond data and algorithms, a fundamental question of theoretical computer science is determining *what can be computed* and, by extension, *what cannot be computed* - problems with no computational answer
  - This is a solution to the question: *Can a computer solve problem P?*
    - Showing that it can is usually straightforward, as you can simply write a program that solves *P*
    - Showing that it cannot, however, is usually more difficult and may require proof techniques and an understanding of how to model computation
## Representation of Objects
- Different object types, (e.g. *integers, complex numbers, images, etc.*) are simply represented on a device as a *single* sequence of 1's and 0's that are later decoded as the object being considered
  - How exactly these sequences of 1's and 0's are arranged can be understood via set theory
    - $Set \rightarrow A$
      - $A^2 = A \times A = \{(a, b): a \in A, b \in A\}$
      - $A^3 = A \times A \times A = \{(a, b, c): a \in A, b \in A, c \in A \}$
      - $A^* = \emptyset \cup A \cup A^2 \cup A^3 \cup ...$
        - e.g. If $A = \{0, 1\}$, then $A^*$ is all possible sequences of 0 and 1
    - Mapping: $E: O (object) -> \{0, 1\}^*$
      - *E* should be a one-to-one mapping; it is the *encoding*
        - This means that  for $X \neq Y$, $E(X) \neq E(Y)$      
- This unified representation of data lends itself well when modeling computation
- Complex object types are possible due to the fact that representations of objects can be *composed* 
  - If an object of type *T* can be represented, then a collection of object type *T* can also be represented
    - e.g. A collection of images, each of which are just a collection of numbers
- Example: Representing Natural Numbers:
  - $E: N -> \{0, 1\}^*$
    - $N = \{0, 1, 2, ..., \}$
  - Unary: $E(n) = 0 ... 0$ (n + 1 zeroes)
    - e.g. E(0) = 0
    - e.g. E(1) = 00
    - e.g. E(5) = 000000
    - This is a one-to-one mapping, which satisfies the desired properties of the encoding - albeit it is very inefficient
  - Binary:  
    - e.g. E(1) = 1
    - e.g. E(2) = 10
    - e.g. E(3) = 11
    - N to B (Natural Numbers to Binary): $N \rightarrow \{0, 1\}^*$
      - 0 if $n = 0$
      - 1 if $n = 1$
      - N to B($Floor(\frac{n}{2})$) ... (n mod 2)
        - Append (n mod 2), don't multiply
      - This can be proven as a valid encoding map by showing that it can be paired with a decoding map that maps binary sequences to objects: $D: \{0, 1\}^* \rightarrow O(object)$
        - That is, $D(E(X)) = X$
    - Z to B (Real Numbers to Binary): $Z \rightarrow \{0 ,1\}^*$
      - 0 ... NtoB(n) if n >= 0
      - 1 ... NtoB(-n) if n < 0
        - This is essentially just a sign bit representation of real numbers