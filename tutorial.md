# C̆

## Code Page

This is the code page for C̆:

```
 0123456789ABCDEF
0∅¡±°¬¢£¤¥¦¶«»₪ℶ♠
1ÞÐßǷȜſµ⅟↶↷↹¿⚅♭♯⍏
2 !"#$%&'()*+,-./
30123456789:;<=>?
4@ABCDEFGHIJKLMNO
5PQRSTUVWXYZ[\]^_
6`abcdefghijklmno
7pqrstuvwxyz{|}~÷
8∀∃∄∆∇∈∉∎∏∑√∞∧∨∩∪
9∫∴∵≝≠≡≤≥≰≱⊆⊇⊈⊉⋈⊨
A⁰¹²³⁴⁵⁶⁷⁸⁹⁻←↑→↓↺
B₀₁₂₃₄₅₆₇₈₉□◇⟠⊤⊥⊦
C½⌫Ɂ‼†‡•‘’“”⋯⋮⋰⋱⁜
DⅠⅡⅢⅣⅤⅥⅦⅧⅨⅩⅪⅫⅬⅭⅮⅯ
EↃↀↁↂↇↈ↤⇤δΓΘΛΞΦΨΩ
Fℂℍℕℙℚℝℤπℯא℞⊕⊖⊗⊘⊜
```

## Literals

Since C̆ is very type-dependent, there are a lot of different literals, and ways
to define them. However, these literals are defined by special operators rather
than any pre-processed tokenization.

## Arity

Each operation has a fixed arity. If too few arguments are specified, then
arguments are supplied from the input to the program. While each operation's
arity is fixed, some process their arities with special rules. Additionally,
compilation is carried out lazily, so the return type of an operation may not be
known initially.

An informative example is the generic binary operation table program.
This program takes as input a number and a binary operation and
prints that operation's table for all numbers up to and including the input.

In C̆ this is: `IrWD₪`

This program can be read as:

```
I   - (): Input integer
r   - (integer): range 1 .. input
W   - (list): tuple containing two duplicates
D   - (list,list): (list,list, input dyadic function)
₪   - (list,list,dyadic function): matrix of list convolution with function
```

If the operation must instead be specified as a character, the coercion operator
must be applied to it.

## Formalism

To be more specific, each operator can be considered a single input single
output function. However its input and output are tuples, which may themselves
contain tuples, which are the arguments. In the tracing of the above program

## Description of operators

### Format

The format of each operator has its name followed by a short description of what
the operator does. Then it will have subsections for each argument format the
operator can act on, and its unique name in that circumstance. Note that no
argument list can be a prefix of another one.

Each operator's description also contains a formal definition of what it does to
each input shape. In this description certain symbols have special meaning:

```
* - Anything at all, even another tuple or nothing.
_ - Any single type, not another tuple nor nothing.
1..9 - nth input
```

Any string of letters is a type description. Many operators use a custom type to
effect a 'mode change' using a type that has no values, but does have a special
name.

### Begin Function

The _begin function_ operator is represented by `‘`. This operator returns a new
function. This function is defined by the operations that appear following this
operator until the _end function_ operator is encountered. If no such operator
is ever encountered, one is implicitly added to the end of the file.

Together with _end function_ this operator defines a function pseudo-literal.

 - `(*) -> (FUNCTION_MODE,*)`: Switches to function definition mode

### End Function

The _end function_ operator is represented by `’`. This operator ends a single
enclosing function definition. If no previous unclosed _begin function_ operator
is present, this instead forces execution to stop, returning whatever arguments
are provided to this operator.

Together with _begin function_ this operator defines a function pseudo-literal.

```
(*,FUNCTION_MODE,*) -> (function,*)
```

### Convolve

The _convolve_ operator is represented by `₪`. This operator takes 2 arguments.
Depending on the types of the arguments it will attempt to convolve the first
argument using the second.

```
(f:*,m:FUNCTION_MODE,x:*) -> (CONVOLVE_OPERATOR, f, m, x)
(f:function,g:function,x:*) -> (function, x)
(g:function,l..:lists,x:*) -> (matrix, x)
```

For example, if two monadic functions are convolved, it will return a new
function which is the convolution of the two if the result is called on a
number.

If a tuple of lists and a function of equal arity are convolved, it will return a
matrix of each pairwise applications of the list elements via the function.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Modes are emulated by input type. EG: if input is a graph, operation is
graph-mode. Mode switching is done via type changing operations.

Idea for dealing with input to main() needing typing: compile to seperate source
files for each valid input type. The user can then compile whichever one(s) they
want.
