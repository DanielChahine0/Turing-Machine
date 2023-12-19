# XOR Turing Machine

## Created a Turing Machine using YAML language to make an XOR string out of two strings w1 and w2

### Template by  [Daniel Chahine](https://github.com/DanielChahine0)
<hr>

This Turing machine computes the `XOR` of w1 and w2 in a string w1#w2 and writes it at the end of the tape in this format: w1#w2$w1XORw2, where w1 and w2 are a string of 0's and 1's.

The Machine could be viewd using [THIS LINK](https://turingmachine.io/?import-gist=8fd5cdb1203abb56729b97f63f26a0ca)

## The machine is split into 5 parts:
> # Start
Goes all the way to the end of the **input string** (w1#w2), write a $ at the **end** and goes back to the first character.
```
Qstart:
    [0,1,'#']: R
    ' ': {write: $, L: state1}
  state1:
    [0,1,'#']: L
    ' ': {R: state2}
  state2:
    1: {write: 'X', R: read1}
    0: {write: 'Y', R: read0}
    '#': {L: restore1}
```
<br>

> # TOP
If the **first read character of w1** is a 0 then check it and compare it with **the first character of w2**. If it's another 0 then write 0, if it's a 1 then write a 1.
```
read0:
    [0,1]: R
    '#': {R: b0}
  b0:
    ['X','Y']: {R}
    1  : {write: 'X', R: read01}
    0  : {write: 'Y', R: read00}
  read01:
    [0,1, "$"]: R
    ' ': {write: 1, L: write01}
  read00:
    [0,1,"$"]: R
    ' ': {write: 0, L: write00}
  write00:
    [0,1]: L
    '$': {L: loop1}
  write01:  
    [0,1]: L
    '$': {L: loop1}
```
<br>

> # BOTTOM
If the **first read character of w1** is a 1 then check it and compare it with the **first character of w2**. If it's another 1 then it writes 0, if it's a 0 then it writes a 1.
  ```
  read1:
    [0,1]: R
    '#': {R: b1}
  b1:
    ['X','Y']: {R}
    1  : {write: 'X', R: read11}
    0  : {write: 'Y', R: read10}
  read11:
    [0,1,"$"]: R
    ' ': {write: 0, L: write11}
  read10:
    [0,1,"$"]: R
    ' ': {write: 1, L: write10}
  write11:
    [0,1]: L
    '$': {L: loop1}
  write10:  
    [0,1]: L
    '$': {L: loop1}
```
<br>

> # Middle
Responsible for looping all the way back to the end of the string after writing the wanted XOR character. **Loops the System** to check again for every letter.
```
 loop1:
    [0,1,'X','Y']: L
    '#': {L: loop2}
  loop2:
    [0,1]: L
    ['X','Y']: {R: state2}
  ```
<br>

> # Restore
After the addition of the w1 XOR w2 is done at the end of the strings, w1 and w2 will be consisted of X's and Y's and not 0's and 1's. This is **necessary** to make sure a value is not checked more than once. This section restores the strings w1 and w2 back to their **binary form**.
```
 restore1:
    'X': {write: 1, L}
    'Y': {write: 0, L}
    ' ': {R: restore2}
  restore2:
    [0,1]: R
    '#': {R: restore3}
  restore3:
    'X': {write: 1, R}
    'Y': {write: 0, R}
```

<br>
<hr>
<hr>

This is not supposed to be the **perfect solution** or the most efficient one, this is just **my way** of doing this challenge, feel free to improve my Turing Machine and play with its parameters.


```
Daniel Chahine
```