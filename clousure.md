# [Closure](https://www.youtube.com/watch?v=swU3c34d2NQ&list=PL-osiE80TeTsnP0Nl1UDY8VZAlHu1m_MQ)

`A closure is a record storing a function together with an environment. The environment is a mapping associating each free variable of the function with the value or reference to which the name was bound when the closure was created. Unlike a plain function, a closure allows the function to access those captured variables through the closure's copies of their values or references, even when the function is invoked outside their scope.`

Free variable - variables that are used locally, but defined in an enclosing scope. In mathematics, it means a dummy variable, a placeholder

**In simple terms: A closure is an inner function that remembers and has access to variables in the local scope in which it was created even after the outer function has finished executing**

## `The inner_func() in these examples is a closure`

## Example 1 
```python
def outer_func():
    message  = "Hi" 
    def inner_func():
        print(message) # free variable, a placeholder for some value defined in the local outer_func() scope

    return inner_func()

outer_func()

```
## Output 
```python
Hi
```

## Example 2
```python
def outer_func():
    message  = "Hi" 
    def inner_func():
        print(message) 

    return inner_func

my_func = outer_func()
# my_func == inner_func
print(my_func.__name__)

my_func()
my_func()
my_func()
# Note that the execution of the outer_func is finished, but inner_func() still remembers the message variable

```
## Output 
```python 
inner_func
Hi
Hi
Hi
```

## Example with Parameters
```python
def outer_func(msg):
    message = msg 
    def inner_func():
        print(message) 

    return inner_func

hi_func = outer_func("Hi")
hello_func = outer_func("Hello")

hi_func()
hello_func()
```
## Output 
```python
Hi
Hello
```
**Note that hi_func() and hello_func() remembers their own parameters**
### `The closures closes over the free variables from their environment`

