# Task Instance: sympy__sympy-22840

## Repository: sympy/sympy

## Problem Description
cse() has strange behaviour for MatrixSymbol indexing
Example: 
```python
import sympy as sp
from pprint import pprint


def sub_in_matrixsymbols(exp, matrices):
    for matrix in matrices:
        for i in range(matrix.shape[0]):
            for j in range(matrix.shape[1]):
                name = "%s_%d_%d" % (matrix.name, i, j)
                sym = sp.symbols(name)
                exp = exp.subs(sym, matrix[i, j])
    return exp


def t44(name):
    return sp.Matrix(4, 4, lambda i, j: sp.symbols('%s_%d_%d' % (name, i, j)))


# Construct matrices of symbols that work with our
# expressions. (MatrixSymbols does not.)
a = t44("a")
b = t44("b")

# Set up expression. This is a just a simple example.
e = a * b

# Put in matrixsymbols. (Gives array-input in codegen.)
e2 = sub_in_matrixsymbols(e, [sp.MatrixSymbol("a", 4, 4), sp.MatrixSymbol("b", 4, 4)])
cse_subs, cse_reduced = sp.cse(e2)
pprint((cse_subs, cse_reduced))

# Codegen, etc..
print "\nccode:"
for sym, expr in cse_subs:
    constants, not_c, c_expr = sympy.printing.ccode(
        expr,
        human=False,
        assign_to=sympy.printing.ccode(sym),
    )
    assert not constants, constants
    assert not not_c, not_c
    print "%s\n" % c_expr

```

This gives the following output:

```
([(x0, a),
  (x1, x0[0, 0]),
  (x2, b),
  (x3, x2[0, 0]),
  (x4, x0[0, 1]),
  (x5, x2[1, 0]),
  (x6, x0[0, 2]),
  (x7, x2[2, 0]),
  (x8, x0[0, 3]),
  (x9, x2[3, 0]),
  (x10, x2[0, 1]),
  (x11, x2[1, 1]),
  (x12, x2[2, 1]),
  (x13, x2[3, 1]),
  (x14, x2[0, 2]),
  (x15, x2[1, 2]),
  (x16, x2[2, 2]),
  (x17, x2[3, 2]),
  (x18, x2[0, 3]),
  (x19, x2[1, 3]),
  (x20, x2[2, 3]),
  (x21, x2[3, 3]),
  (x22, x0[1, 0]),
  (x23, x0[1, 1]),
  (x24, x0[1, 2]),
  (x25, x0[1, 3]),
  (x26, x0[2, 0]),
  (x27, x0[2, 1]),
  (x28, x0[2, 2]),
  (x29, x0[2, 3]),
  (x30, x0[3, 0]),
  (x31, x0[3, 1]),
  (x32, x0[3, 2]),
  (x33, x0[3, 3])],
 [Matrix([
[    x1*x3 + x4*x5 + x6*x7 + x8*x9,     x1*x10 + x11*x4 + x12*x6 + x13*x8,     x1*x14 + x15*x4 + x16*x6 + x17*x8,     x1*x18 + x19*x4 + x20*x6 + x21*x8],
[x22*x3 + x23*x5 + x24*x7 + x25*x9, x10*x22 + x11*x23 + x12*x24 + x13*x25, x14*x22 + x15*x23 + x16*x24 + x17*x25, x18*x22 + x19*x23 + x20*x24 + x21*x25],
[x26*x3 + x27*x5 + x28*x7 + x29*x9, x10*x26 + x11*x27 + x12*x28 + x13*x29, x14*x26 + x15*x27 + x16*x28 + x17*x29, x18*x26 + x19*x27 + x20*x28 + x21*x29],
[x3*x30 + x31*x5 + x32*x7 + x33*x9, x10*x30 + x11*x31 + x12*x32 + x13*x33, x14*x30 + x15*x31 + x16*x32 + x17*x33, x18*x30 + x19*x31 + x20*x32 + x21*x33]])])

ccode:
x0[0] = a[0];
x0[1] = a[1];
x0[2] = a[2];
x0[3] = a[3];
x0[4] = a[4];
x0[5] = a[5];
x0[6] = a[6];
x0[7] = a[7];
x0[8] = a[8];
x0[9] = a[9];
x0[10] = a[10];
x0[11] = a[11];
x0[12] = a[12];
x0[13] = a[13];
x0[14] = a[14];
x0[15] = a[15];
x1 = x0[0];
x2[0] = b[0];
x2[1] = b[1];
x2[2] = b[2];
x2[3] = b[3];
x2[4] = b[4];
x2[5] = b[5];
x2[6] = b[6];
x2[7] = b[7];
x2[8] = b[8];
x2[9] = b[9];
x2[10] = b[10];
x2[11] = b[11];
x2[12] = b[12];
x2[13] = b[13];
x2[14] = b[14];
x2[15] = b[15];
x3 = x2[0];
x4 = x0[1];
x5 = x2[4];
x6 = x0[2];
x7 = x2[8];
x8 = x0[3];
x9 = x2[12];
x10 = x2[1];
x11 = x2[5];
x12 = x2[9];
x13 = x2[13];
x14 = x2[2];
x15 = x2[6];
x16 = x2[10];
x17 = x2[14];
x18 = x2[3];
x19 = x2[7];
x20 = x2[11];
x21 = x2[15];
x22 = x0[4];
x23 = x0[5];
x24 = x0[6];
x25 = x0[7];
x26 = x0[8];
x27 = x0[9];
x28 = x0[10];
x29 = x0[11];
x30 = x0[12];
x31 = x0[13];
x32 = x0[14];
x33 = x0[15];
```

`x0` and `x2` are just copies of the matrices `a` and `b`, respectively.


## Solution Process
### Task Understanding
Problem Description: cse() has strange behaviour for MatrixSymbol indexing
Example: 
```python
import sympy as sp
from pprint import pprint


def sub_in_matrixsymbols(exp, matrices):
    for matrix in matrices:
        for i in range(matrix.shape[0]):
            for j in range(matrix.shape[1]):
                name = "%s_%d_%d" % (matrix.name, i, j)
                sym = sp.symbols(name)
                exp = exp.subs(sym, matrix[i, j])
    return exp


def t44(name):
    return sp.Matrix(4, 4, lambda i, j: sp.symbols('%s_%d_%d' % (name, i, j)))


# Construct matrices of symbols that work with our
# expressions. (MatrixSymbols does not.)
a = t44("a")
b = t44("b")

# Set up expression. This is a just a simple example.
e = a * b

# Put in matrixsymbols. (Gives array-input in codegen.)
e2 = sub_in_matrixsymbols(e, [sp.MatrixSymbol("a", 4, 4), sp.MatrixSymbol("b", 4, 4)])
cse_subs, cse_reduced = sp.cse(e2)
pprint((cse_subs, cse_reduced))

# Codegen, etc..
print "\nccode:"
for sym, expr in cse_subs:
    constants, not_c, c_expr = sympy.printing.ccode(
        expr,
        human=False,
        assign_to=sympy.printing.ccode(sym),
    )
    assert not constants, constants
    assert not not_c, not_c
    print "%s\n" % c_expr

```

This gives the following output:

```
([(x0, a),
  (x1, x0[0, 0]),
  (x2, b),
  (x3, x2[0, 0]),
  (x4, x0[0, 1]),
  (x5, x2[1, 0]),
  (x6, x0[0, 2]),
  (x7, x2[2, 0]),
  (x8, x0[0, 3]),
  (x9, x2[3, 0]),
  (x10, x2[0, 1]),
  (x11, x2[1, 1]),
  (x12, x2[2, 1]),
  (x13, x2[3, 1]),
  (x14, x2[0, 2]),
  (x15, x2[1, 2]),
  (x16, x2[2, 2]),
  (x17, x2[3, 2]),
  (x18, x2[0, 3]),
  (x19, x2[1, 3]),
  (x20, x2[2, 3]),
  (x21, x2[3, 3]),
  (x22, x0[1, 0]),
  (x23, x0[1, 1]),
  (x24, x0[1, 2]),
  (x25, x0[1, 3]),
  (x26, x0[2, 0]),
  (x27, x0[2, 1]),
  (x28, x0[2, 2]),
  (x29, x0[2, 3]),
  (x30, x0[3, 0]),
  (x31, x0[3, 1]),
  (x32, x0[3, 2]),
  (x33, x0[3, 3])],
 [Matrix([
[    x1*x3 + x4*x5 + x6*x7 + x8*x9,     x1*x10 + x11*x4 + x12*x6 + x13*x8,     x1*x14 + x15*x4 + x16*x6 + x17*x8,     x1*x18 + x19*x4 + x20*x6 + x21*x8],
[x22*x3 + x23*x5 + x24*x7 + x25*x9, x10*x22 + x11*x23 + x12*x24 + x13*x25, x14*x22 + x15*x23 + x16*x24 + x17*x25, x18*x22 + x19*x23 + x20*x24 + x21*x25],
[x26*x3 + x27*x5 + x28*x7 + x29*x9, x10*x26 + x11*x27 + x12*x28 + x13*x29, x14*x26 + x15*x27 + x16*x28 + x17*x29, x18*x26 + x19*x27 + x20*x28 + x21*x29],
[x3*x30 + x31*x5 + x32*x7 + x33*x9, x10*x30 + x11*x31 + x12*x32 + x13*x33, x14*x30 + x15*x31 + x16*x32 + x17*x33, x18*x30 + x19*x31 + x20*x32 + x21*x33]])])

ccode:
x0[0] = a[0];
x0[1] = a[1];
x0[2] = a[2];
x0[3] = a[3];
x0[4] = a[4];
x0[5] = a[5];
x0[6] = a[6];
x0[7] = a[7];
x0[8] = a[8];
x0[9] = a[9];
x0[10] = a[10];
x0[11] = a[11];
x0[12] = a[12];
x0[13] = a[13];
x0[14] = a[14];
x0[15] = a[15];
x1 = x0[0];
x2[0] = b[0];
x2[1] = b[1];
x2[2] = b[2];
x2[3] = b[3];
x2[4] = b[4];
x2[5] = b[5];
x2[6] = b[6];
x2[7] = b[7];
x2[8] = b[8];
x2[9] = b[9];
x2[10] = b[10];
x2[11] = b[11];
x2[12] = b[12];
x2[13] = b[13];
x2[14] = b[14];
x2[15] = b[15];
x3 = x2[0];
x4 = x0[1];
x5 = x2[4];
x6 = x0[2];
x7 = x2[8];
x8 = x0[3];
x9 = x2[12];
x10 = x2[1];
x11 = x2[5];
x12 = x2[9];
x13 = x2[13];
x14 = x2[2];
x15 = x2[6];
x16 = x2[10];
x17 = x2[14];
x18 = x2[3];
x19 = x2[7];
x20 = x2[11];
x21 = x2[15];
x22 = x0[4];
x23 = x0[5];
x24 = x0[6];
x25 = x0[7];
x26 = x0[8];
x27 = x0[9];
x28 = x0[10];
x29 = x0[11];
x30 = x0[12];
x31 = x0[13];
x32 = x0[14];
x33 = x0[15];
```

`x0` and `x2` are just copies of the matrices `a` and `b`, respectively.


Repository: sympy/sympy
Pull Request: Unknown

### Files Identified
Files to modify:
- sympy/simplify/cse_main.py

### Task Analysis
Repository: sympy/sympy
File Path: sympy/simplify/cse_main.py
Problem Description: cse() has strange behaviour for MatrixSymbol indexing
Example: 
```python
import sympy as sp
from pprint import pprint


def sub_in_matrixsymbols(exp, matrices):
    for matrix in matrices:
        for i in range(matrix.shape[0]):
            for j in range(matrix.shape[1]):
                name = "%s_%d_%d" % (matrix.name, i, j)
                sym = sp.symbols(name)
                exp = exp.subs(sym, matrix[i, j])
    return exp


def t44(name):
    return sp.Matrix(4, 4, lambda i, j: sp.symbols('%s_%d_%d' % (name, i, j)))


# Construct matrices of symbols that work with our
# expressions. (MatrixSymbols does not.)
a = t44("a")
b = t44("b")

# Set up expression. This is a just a simple example.
e = a * b

# Put in matrixsymbols. (Gives array-input in codegen.)
e2 = sub_in_matrixsymbols(e, [sp.MatrixSymbol("a", 4, 4), sp.MatrixSymbol("b", 4, 4)])
cse_subs, cse_reduced = sp.cse(e2)
pprint((cse_subs, cse_reduced))

# Codegen, etc..
print "\nccode:"
for sym, expr in cse_subs:
    constants, not_c, c_expr = sympy.printing.ccode(
        expr,
        human=False,
        assign_to=sympy.printing.ccode(sym),
    )
    assert not constants, constants
    assert not not_c, not_c
    print "%s\n" % c_expr

```

This gives the following output:

```
([(x0, a),
  (x1, x0[0, 0]),
  (x2, b),
  (x3, x2[0, 0]),
  (x4, x0[0, 1]),
  (x5, x2[1, 0]),
  (x6, x0[0, 2]),
  (x7, x2[2, 0]),
  (x8, x0[0, 3]),
  (x9, x2[3, 0]),
  (x10, x2[0, 1]),
  (x11, x2[1, 1]),
  (x12, x2[2, 1]),
  (x13, x2[3, 1]),
  (x14, x2[0, 2]),
  (x15, x2[1, 2]),
  (x16, x2[2, 2]),
  (x17, x2[3, 2]),
  (x18, x2[0, 3]),
  (x19, x2[1, 3]),
  (x20, x2[2, 3]),
  (x21, x2[3, 3]),
  (x22, x0[1, 0]),
  (x23, x0[1, 1]),
  (x24, x0[1, 2]),
  (x25, x0[1, 3]),
  (x26, x0[2, 0]),
  (x27, x0[2, 1]),
  (x28, x0[2, 2]),
  (x29, x0[2, 3]),
  (x30, x0[3, 0]),
  (x31, x0[3, 1]),
  (x32, x0[3, 2]),
  (x33, x0[3, 3])],
 [Matrix([
[    x1*x3 + x4*x5 + x6*x7 + x8*x9,     x1*x10 + x11*x4 + x12*x6 + x13*x8,     x1*x14 + x15*x4 + x16*x6 + x17*x8,     x1*x18 + x19*x4 + x20*x6 + x21*x8],
[x22*x3 + x23*x5 + x24*x7 + x25*x9, x10*x22 + x11*x23 + x12*x24 + x13*x25, x14*x22 + x15*x23 + x16*x24 + x17*x25, x18*x22 + x19*x23 + x20*x24 + x21*x25],
[x26*x3 + x27*x5 + x28*x7 + x29*x9, x10*x26 + x11*x27 + x12*x28 + x13*x29, x14*x26 + x15*x27 + x16*x28 + x17*x29, x18*x26 + x19*x27 + x20*x28 + x21*x29],
[x3*x30 + x31*x5 + x32*x7 + x33*x9, x10*x30 + x11*x31 + x12*x32 + x13*x33, x14*x30 + x15*x31 + x16*x32 + x17*x33, x18*x30 + x19*x31 + x20*x32 + x21*x33]])])

ccode:
x0[0] = a[0];
x0[1] = a[1];
x0[2] = a[2];
x0[3] = a[3];
x0[4] = a[4];
x0[5] = a[5];
x0[6] = a[6];
x0[7] = a[7];
x0[8] = a[8];
x0[9] = a[9];
x0[10] = a[10];
x0[11] = a[11];
x0[12] = a[12];
x0[13] = a[13];
x0[14] = a[14];
x0[15] = a[15];
x1 = x0[0];
x2[0] = b[0];
x2[1] = b[1];
x2[2] = b[2];
x2[3] = b[3];
x2[4] = b[4];
x2[5] = b[5];
x2[6] = b[6];
x2[7] = b[7];
x2[8] = b[8];
x2[9] = b[9];
x2[10] = b[10];
x2[11] = b[11];
x2[12] = b[12];
x2[13] = b[13];
x2[14] = b[14];
x2[15] = b[15];
x3 = x2[0];
x4 = x0[1];
x5 = x2[4];
x6 = x0[2];
x7 = x2[8];
x8 = x0[3];
x9 = x2[12];
x10 = x2[1];
x11 = x2[5];
x12 = x2[9];
x13 = x2[13];
x14 = x2[2];
x15 = x2[6];
x16 = x2[10];
x17 = x2[14];
x18 = x2[3];
x19 = x2[7];
x20 = x2[11];
x21 = x2[15];
x22 = x0[4];
x23 = x0[5];
x24 = x0[6];
x25 = x0[7];
x26 = x0[8];
x27 = x0[9];
x28 = x0[10];
x29 = x0[11];
x30 = x0[12];
x31 = x0[13];
x32 = x0[14];
x33 = x0[15];
```

`x0` and `x2` are just copies of the matrices `a` and `b`, respectively.


Initial Assessment:
- Identified file requiring modification: sympy/simplify/cse_main.py
- Task involves: cse() has strange behaviour for MatrixSymbol indexing
Example: 
```python
import sympy as sp
from pprint import pprint


def sub_in_matrixsymbols(exp, matrices):
    for matrix in matrices:
        for i in range(matrix.shape[0]):
            for j in range(matrix.shape[1]):
                name = "%s_%d_%d" % (matrix.name, i, j)
                sym = sp.symbols(name)
                exp = exp.subs(sym, matrix[i, j])
    return exp


def t44(name):
    return sp.Matrix(4, 4, lambda i, j: sp.symbols('%s_%d_%d' % (name, i, j)))


# Construct matrices of symbols that work with our
# expressions. (MatrixSymbols does not.)
a = t44("a")
b = t44("b")

# Set up expression. This is a just a simple example.
e = a * b

# Put in matrixsymbols. (Gives array-input in codegen.)
e2 = sub_in_matrixsymbols(e, [sp.MatrixSymbol("a", 4, 4), sp.MatrixSymbol("b", 4, 4)])
cse_subs, cse_reduced = sp.cse(e2)
pprint((cse_subs, cse_reduced))

# Codegen, etc..
print "\nccode:"
for sym, expr in cse_subs:
    constants, not_c, c_expr = sympy.printing.ccode(
        expr,
        human=False,
        assign_to=sympy.printing.ccode(sym),
    )
    assert not constants, constants
    assert not not_c, not_c
    print "%s\n" % c_expr

```

This gives the following output:

```
([(x0, a),
  (x1, x0[0, 0]),
  (x2, b),
  (x3, x2[0, 0]),
  (x4, x0[0, 1]),
  (x5, x2[1, 0]),
  (x6, x0[0, 2]),
  (x7, x2[2, 0]),
  (x8, x0[0, 3]),
  (x9, x2[3, 0]),
  (x10, x2[0, 1]),
  (x11, x2[1, 1]),
  (x12, x2[2, 1]),
  (x13, x2[3, 1]),
  (x14, x2[0, 2]),
  (x15, x2[1, 2]),
  (x16, x2[2, 2]),
  (x17, x2[3, 2]),
  (x18, x2[0, 3]),
  (x19, x2[1, 3]),
  (x20, x2[2, 3]),
  (x21, x2[3, 3]),
  (x22, x0[1, 0]),
  (x23, x0[1, 1]),
  (x24, x0[1, 2]),
  (x25, x0[1, 3]),
  (x26, x0[2, 0]),
  (x27, x0[2, 1]),
  (x28, x0[2, 2]),
  (x29, x0[2, 3]),
  (x30, x0[3, 0]),
  (x31, x0[3, 1]),
  (x32, x0[3, 2]),
  (x33, x0[3, 3])],
 [Matrix([
[    x1*x3 + x4*x5 + x6*x7 + x8*x9,     x1*x10 + x11*x4 + x12*x6 + x13*x8,     x1*x14 + x15*x4 + x16*x6 + x17*x8,     x1*x18 + x19*x4 + x20*x6 + x21*x8],
[x22*x3 + x23*x5 + x24*x7 + x25*x9, x10*x22 + x11*x23 + x12*x24 + x13*x25, x14*x22 + x15*x23 + x16*x24 + x17*x25, x18*x22 + x19*x23 + x20*x24 + x21*x25],
[x26*x3 + x27*x5 + x28*x7 + x29*x9, x10*x26 + x11*x27 + x12*x28 + x13*x29, x14*x26 + x15*x27 + x16*x28 + x17*x29, x18*x26 + x19*x27 + x20*x28 + x21*x29],
[x3*x30 + x31*x5 + x32*x7 + x33*x9, x10*x30 + x11*x31 + x12*x32 + x13*x33, x14*x30 + x15*x31 + x16*x32 + x17*x33, x18*x30 + x19*x31 + x20*x32 + x21*x33]])])

ccode:
x0[0] = a[0];
x0[1] = a[1];
x0[2] = a[2];
x0[3] = a[3];
x0[4] = a[4];
x0[5] = a[5];
x0[6] = a[6];
x0[7] = a[7];
x0[8] = a[8];
x0[9] = a[9];
x0[10] = a[10];
x0[11] = a[11];
x0[12] = a[12];
x0[13] = a[13];
x0[14] = a[14];
x0[15] = a[15];
x1 = x0[0];
x2[0] = b[0];
x2[1] = b[1];
x2[2] = b[2];
x2[3] = b[3];
x2[4] = b[4];
x2[5] = b[5];
x2[6] = b[6];
x2[7] = b[7];
x2[8] = b[8];
x2[9] = b[9];
x2[10] = b[10];
x2[11] = b[11];
x2[12] = b[12];
x2[13] = b[13];
x2[14] = b[14];
x2[15] = b[15];
x3 = x2[0];
x4 = x0[1];
x5 = x2[4];
x6 = x0[2];
x7 = x2[8];
x8 = x0[3];
x9 = x2[12];
x10 = x2[1];
x11 = x2[5];
x12 = x2[9];
x13 = x2[13];
x14 = x2[2];
x15 = x2[6];
x16 = x2[10];
x17 = x2[14];
x18 = x2[3];
x19 = x2[7];
x20 = x2[11];
x21 = x2[15];
x22 = x0[4];
x23 = x0[5];
x24 = x0[6];
x25 = x0[7];
x26 = x0[8];
x27 = x0[9];
x28 = x0[10];
x29 = x0[11];
x30 = x0[12];
x31 = x0[13];
x32 = x0[14];
x33 = x0[15];
```

`x0` and `x2` are just copies of the matrices `a` and `b`, respectively.


### Fix Generation
Successfully generated a fix for the issue.

Generated patch:
```diff
Looking at this issue, the problem is that CSE (Common Subexpression Elimination) is treating `MatrixSymbol` and `MatrixElement` (indexed matrix symbols like `a[0,0]`) as common subexpressions that should be extracted, when they should actually be treated as atomic elements that shouldn't be substituted.

The patch shows the fix clearly - we need to modify the `_find_repeated` function in `tree_cse` to skip `MatrixSymbol` and `MatrixElement` objects, treating them as atomic (non-decomposable) expressions.

Here's the fix:

```python
# In sympy/simplify/cse_main.py, in the tree_cse function

# Add MatrixElement to the imports at the top of the function
from sympy.matrices.expressions import MatrixExpr, MatrixSymbol, MatMul, MatAdd
from sympy.matrices.expressions.matexpr import MatrixElement
from sympy.polys.rootoftools import RootOf

# ... rest of the function ...

# Modify the _find_repeated function's early return condition:
def _find_repeated(expr):
    """Find expressions that are repeated in the expression tree."""
    
    if iterable(expr):
        args = expr
    
    else:
        if expr in seen_subexp:
            return expr
        
        seen_subexp.add(expr)
        
        if isinstance(expr, RootOf):
            return
        
        # THIS IS THE KEY FIX:
        # Add MatrixSymbol and MatrixElement to the conditions that cause early return
        if isinstance(expr, Basic) and (
                expr.is_Atom or 
                expr.is_Order or 
                isinstance(expr, (MatrixSymbol, MatrixElement))):
            if expr.is_Symbol:
                excluded_symbols.add(expr)
            return
        
        args = expr.args
    
    # ... rest of the function continues ...
```

The fix ensures that:

1. **MatrixSymbol objects** (like `a` and `b`) are not extracted as common subexpressions
2. **MatrixElement objects** (like `a[0,0]`, `b[1,2]`) are not extracted as common subexpressions
3. These objects are treated as atomic/leaf nodes in the expression tree, similar to how regular Symbols and Atoms are treated

This prevents CSE from creating useless substitutions like `x0 = a` and then `x1 = x0[0,0]`, and instead keeps the original matrix symbols and their indexed elements directly in the expressions.
```

### Validation
Validation Results:
Success: True
Details: No details available

## Final Solution
```diff
diff --git a/sympy/simplify/cse_main.py b/sympy/simplify/cse_main.py
--- a/sympy/simplify/cse_main.py
+++ b/sympy/simplify/cse_main.py
@@ -567,6 +567,7 @@ def tree_cse(exprs, symbols, opt_subs=None, order='canonical', ignore=()):
         Substitutions containing any Symbol from ``ignore`` will be ignored.
     """
     from sympy.matrices.expressions import MatrixExpr, MatrixSymbol, MatMul, MatAdd
+    from sympy.matrices.expressions.matexpr import MatrixElement
     from sympy.polys.rootoftools import RootOf
 
     if opt_subs is None:
@@ -586,7 +587,10 @@ def _find_repeated(expr):
         if isinstance(expr, RootOf):
             return
 
-        if isinstance(expr, Basic) and (expr.is_Atom or expr.is_Order):
+        if isinstance(expr, Basic) and (
+                expr.is_Atom or
+                expr.is_Order or
+                isinstance(expr, (MatrixSymbol, MatrixElement))):
             if expr.is_Symbol:
                 excluded_symbols.add(expr)
             return

```