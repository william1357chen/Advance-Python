# Type Annotations
Python is a *dynamically typed* language. That means it doesn't care about the types of objects we use, as long as we use them in valid ways. 
```python
def add(a, b):
    return a + b

assert add(10, 5) == 15, "+ is valid for numbers"
assert add([1, 2], [3]) == [1, 2, 3], "+ is valid for lists"
assert add("hi ", "there") == "hi there", "+ is valid for strings"
try:
    add(10, "five")
except TypeError:
    print("cannot add an int to a string")
```
whereas in a *statically typed language* our functions and objects would have
specific types:
```python 
def add(a: int, b: int) -> int:
    return a + b
    
add(10, 5) # you'd like this to be OK
add("hi ", "there") # you'd like this to be not OK
```
In fact, recent versions of Python do (sort of) have this functionality. The
preceding version of `add` with the `int` type annotations is valid Python 3.6. 

However, these type annotations don’t actually *do* anything. You can still
use the annotated `add` function to add strings, and the call to `add(10,
"five")` will still raise the exact same `TypeError`.

That said, there are still (at least) four good reasons to use type annotations
in your Python code:
* Types are an important form of documentation.
* There are external tools (the most popular is `mypy`) that will read
your code, inspect the type annotations, and let you know about
type errors *before you ever run your code*.
* Having to think about the types in your code forces you to design
cleaner functions and interfaces
* Using types allows your editor to help you with things like
autocomplete and to get angry at type errors.

## How to Write Type Annotations

### Support input of different types
```python
from typing import Union

def ex_function(value: int, 
                  operation: Union[str, int, float, bool]) -> int:
```
Here we have a function whose operation parameter is allowed to
be a `string`, or an `int`, or a `float`, or a `bool`.

### For Container Objects like `list` 
```python
from typing import List # note capital L

def total(xs: List[float]) -> float:
    return sum(total)
```
### Annotation for variables
```python 
# This is how to type-annotate variables when you define them.
# But this is unnecessary; it's "obvious" x is an int.
x: int = 5
```
In such cases we will supply inline type hints:
```python
from typing import Optional

values: List[int] = []
best_so_far: Optional[float] = None # allowed to be either a float or None
```
### Other important types (Dict, Iterable, Tuple)
```python
# the type annotations in this snippet are all unnecessary
from typing import Dict, Iterable, Tuple

# keys are strings, values are ints
counts: Dict[str, int] = {'data': 1, 'science': 2}

# lists and generators are both iterable
if lazy:
    evens: Iterable[int] = (x for x in range(10) if x % 2 == 0)
else:
    evens = [0, 2, 4, 6, 8]

# tuples specify a type for each element
triple: Tuple[int, float, int] = (10, 2.3, 5)
```
### What about Function Objects
```python 
from typing import Callable

# The type hint says that repeater is a function that takes
# two arguments, a string and an int, and returns a string.
def twice(repeater: Callable[[str, int], str], s: str) -> str:
    return repeater(s, 2)

def comma_repeater(s: str, n: int) -> str:
    n_copies = [s for _ in range(n)]
    return ', '.join(n_copies)

assert twice(comma_repeater, "type hints") == "type hints, type hints"
```

As type annotations are just Python objects, we can assign them to variables
to make them easier to refer to:
```python
Number = int
Numbers = List[Number]

def total(xs: Numbers) -> Number:
    return sum(xs)
```