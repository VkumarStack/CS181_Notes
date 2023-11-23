# Lecture 12
## Turing Machines
- Turing Machines can be intuitively thought of as DFAs with read/write memory and a two-way moveable head
- For convenience, a Turing Machine can have symbols other than `0` and `1` - any number of symbols is fine as long as it is finite
- Anatomy of a Turing Machine:
  - $\Delta$: Start of tape
  - $\phi$: Empty cell
  - $k$ States
  - $\Sigma \supseteq \{0, 1, \Delta, \phi \}$
  - Transition Function: $\delta: \{0, 1, ..., k - 1 \} \times \Sigma \rightarrow \{0, 1, ..., k - 1\} \times \Sigma \times \{L, R, S, H\}$
    - $\delta$(state_i, a) = (state_j, b, "Move head a certain way")
- Computation Process:
  -     Start with head at x[0]
        Current_state = "0" (default start state)
        Repeat:
          1. (new_state, new_symbol, A) <- delta(current_state, tape[head])
          2. current_state <- new_state
          3. Tape[Head] <- new_symbol
          4. Take Action:
            If A == L: Head = max(0, Head - 1)
            If A == R: Head = Head + 1
            If A == S: Head = Head
            If A = H: STOP
- On an input x:
  - If M halts on input x: M(x) = T[0]Tape[1]...Tape[Head]
  - If M does not halt: M(x) = $\perp$
- Example: $k = 1$, $\Sigma = \{0, 1, \Delta, \phi \}$
  - $S_M(0, 0,) = (0, 1, R)$
  - $S_M(0, 1) = (0, 0, R)$
  - $S_M(0, \phi) = (0, \phi, H)$
  - This Turing Machine *flips* the input bits
- A Turing Machine $M: \{0, 1\}^* \rightarrow \Sigma^* \cup \{\perp\}$
  - M(x) = The tape contents until the head is halted
- A function $f: \{0, 1\}^* \rightarrow \{0, 1\}^*$ is computed by a Turing Machine if $\exists$ a Turing Machine $M$, $f(x) = M(x)$ for all $x$
- Example: $MAJ: \{0, 1\}^* \rightarrow \{0, 1\}$
  - Try to "match" `0`'s to `1`'s 
    - If there is a `0` that cannot be matched, then there are more `0`'s than `1`'s
  - Pseudocode:
    -     "Scan" to the right until you find a 0
          If no 0 is found:
            "Cleanup the tape" and return 1
          If a 0 is found:
            Mark the 0 as seen
            Go to the start of the input 
            Scan the tape to the right to find a 1
              If 1 not found:
                "Cleanup the tape" and return 0
              If 1 is found:
                Mark 1 as seen
                Go to start and look for 0 again
  - $\Sigma =\{0, 1, \Delta, \phi, "a" \}$
    - Start State: "qFind0"
      - $\delta(qFind0, 0) = (qGoStart1, a, L)$
      - $\delta(qFind0, 1) = (qFind0, 1, R)$
      - $\delta(qFind0, \phi) = (qAccept, \phi, L)$
      - $\delta(qFind0, a) = (qFind0, a, R)$
    - Cleanup:
      - $\delta(qAccept, 1) = (qAccept, \phi, L)$
      - $\delta(qAccept, "a") = (qAccept, \phi, L)$
      - $\delta(qAccept, 0) = (qAccept, \phi, L)$
      - $\delta(qAccept, \Delta) = (qWrite1, 1, H)$
- Example: Palindrome
  - Pseudocode:
    -     Read first bit, remember it in the state
            Overwrite bit as empty (or seen)
          Go to end and look to match bit
            If no match, cleanup tape and return 0
            If yes:
              Ovewrite bit as empty (or seen)
              Go to first non-empty bit and repeat
