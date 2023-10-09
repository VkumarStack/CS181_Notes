# Binary Representations and Operations
## Binary Representations
- Example: R to B (Rational Numbers to Binary):
  - e.g. $\frac{1}{2}$, $\frac{2}{3}$, $\frac{4}{7}$
    - Simply defining the encoding to be the numerator followed by the denominator would not work because of potential ambiguity - it cannot be determined easily when the numerator ends and the denominator starts
  - A modified version of the previously discussed encoding to go from integers to strings can be used
    - For the numerator and the denominator each, compute ZtoB for each and then *duplicate each bit*
    - To signify a separator or an *end of input*, append `01`
      - Since each bit is duplicated, `01` cannot naturally occur 
    - e.g. `pNtoB(5) = 11 00 11 01`
  - Thus, rational numbers can be represented as a *pair* of integers, as it can be determined easily when the first integer ends (thus representing the numerator) as well as when the second integer ends (thus representing the denominator)
- The type of encoding strategy discussed in the previous example is known as **prefix-free encoding**
  - $E: O \rightarrow \{0, 1\}^*$ is a prefix-free encoding if for all $x \neq y$ in O, $E(x)$ is *not* a prefix of $E(y)$
  - The aforementioned binary encoding of real numbers (N to B) is *not* necessarily prefix-free
    - `NtoB(4) = 10`
    - `NtoB(5) = 101`
    - The prefix of `NtoB(5)` is exactly `NtoB(4)`
  - Any non-prefix-free encoding can be converted to a prefix-free encoding by employing the aforementioned duplication strategy
    - This intuitively makes sense because `01` cannot occur in any encoding of an object, so it must only represent some sort of separator or end marker
  - Prefix-free encodings are important for creating encodings that contain *multiple* of an object
  - **Theorem**: Suppose we have a prefix-free encoding $pE: O \rightarrow \{ 0, 1\}^*$, then $\bar{pE}: O^* \rightarrow \{0, 1\}^*$
    - $\bar{pE}([x_0, x_1, ... x_k]) = pE(x_0) ○ pE(x_1) ○ ... ○ pE(x_k)$
      - This is simply concatenating the prefix-free representations of each $x_i$ together
    - Proof: A Decoding Algorithm:
      - Input: y - A binary string representing the encoding $\bar{pE}([x_0, x_1, ..., x_k])$
      -     i = 0, j = 0
            While i < length(y)
              Check if y[i], y[i + 1], ... y[j] is a valid encoding under pE
              If Yes:
                // Decoding for the base input
                Decode y[i]...y[j]
                Result = Result + Decode(y[i]...y[j])
                i = j + 1
                j = j + 1
              If No:
                j = j + 1
  - Properties:
    - If $pE$ was a prefix-free encoding, then $\bar{pE}$ is *not* a prefix-free encoding
      - $\bar{pE}([x_0]) = pE(x_0)$
      - $\bar{pE}([x_0, x_1]) = pE(x_0) ○ pE(x_1)$
        - The first is a prefix of the second
- Given an encoding $E: O \rightarrow \{0, 1\}^*$, we can build a new encoding $pE: O \rightarrow \{0, 1\}*$ that is prefix-free
  - Algorithm:
    - Compute $E(x)$
    - Duplicate each bit
    - Add `01` at the end
  - To get a list of an encoding, then:
    - Start with the normal method of encoding
    - Convert that encoding to prefix-free
    - Concatenate this prefix-encoding to get a list of the object
    - This list can then be converted to prefix-free again and concatenated again to other prefix-free lists to get a list of list of the object
  - In terms of efficiency, this approach of converting it to prefix-free effectively *doubles* the length of the encoding
    - $pE(x) = 2 * E(x) + 2$
- There is no encoding that converts *real numbers* to binary strings, and this was proven by Canton in 1876
  - This intuitively makes sense due to the existence of irrational numbers such as pi, e, and sqrt(2)
- Aside from the aforementioned issues with real numbers, all inputs can be viewed as binary strings when constructing a model of computation
## Algorithms
- An algorithm can be thought of as a series of steps to solve a problem
  - The notion of a *problem* can be formalized as effectively transforming an input to some desired output
    - Specification: $f: \{0 ,1\}^* \rightarrow \{0, 1\}^*$
      - i.e. `Mult(3, 5) = 15`
        - The input and output can be expressed as binary strings
    - Imagine some sort of "truth table" between inputs and desired outputs that the algorithm is able to map properly
  - The notion of *steps* can be formalized using the model of boolean circuits
    - In this model, the set of simple operations allowed are `AND` ($\land$), `OR` ($\lor$), and `NOT` ($\neg$)
    - Example: A circuit that outputs the majority bits in a binary string input