# Default Parameter Values in Python 

When you pass in mutable objects as parameters in a function
```python
def foo(a=[]):
    a.append(5)
    return a
```

You would expect that everytime you run foo() it would be [5]. However,
```python 
>>> foo()
[5]
>>> foo()
[5, 5]
>>> foo()
[5, 5, 5]
>>> foo()
[5, 5, 5, 5]
```
The reason is simple: the function foo keeps using the same object in each call. But why

## Why does this happen 
Default parameters are always and only evaluated once when the `def` statement they belong to is executed (when the interperter defines the function). `def` is like a function call (executable statement), and the default arguments are evaluated in the `def` function call environment (like a local variable in the environment). 

When `def` is called, it creates a new function object, and stores the default arguments like an attribute. This function object will keep on being used by function calls until the same `def` statement is called again.

### Recalling **def** creates new function object and new default mutable objects
```python 
for i in range(5):
    def hi(x=[]):
        x.append(1)
        print(x)
    
    hi()
```
### Output 
```python 
[1]
[1]
[1]
[1]
[1]
```
`Note that this is just a showcase and not what you should do`
## To Fix This
The corerct way to get a default mutable object is to create it at run time, `inside the function block`. 

```python 
def good_append(new_item, a_list=None):
    if a_list is None:
        a_list = []
    a_list.append(new_item)
    return a_list
```
## Valid Uses for Mutable Defaults 
Caching values between function calls
```python
def get_from_cache(name, cache={}):
    if name in cache: return cache[name]
    cache[name] = result = expensive_calculation()
    return result
```
