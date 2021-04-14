# [Decorators](https://www.youtube.com/watch?v=FsAPt_9Bf3U)

`Similar to Closures, a Decorator is just a function that takes another function as an argument, adds some functionality, and returns another function, all of this wwithout altering the source code of the original function`

## Example 
```python
def decorator_function(original_function):
    def wrapper_function():
        return original_function()
    return wrapper_function

def display():
    print('display function ran')

decorated_display = decorator_function(display)

decorated_display()

```
## Output 
```
display function ran
```

## Adding Functionality Wihout Modifying Original Function
```python
def decorator_function(original_function):
    def wrapper_function():
        print('wrapper executed this before {}'.format(original_function.__name__))
        return original_function()
    return wrapper_function

def display():
    print('display function ran')

decorated_display = decorator_function(display)

decorated_display()

```
## Output 
```
wrapper executed this before display
display function ran
```

## Typical Syntax
```python 
def decorator_function(original_function):
    def wrapper_function():
        print('wrapper executed this before {}'.format(original_function.__name__))
        return original_function()
    return wrapper_function

@decorator_function 
# this syntax is equal to having the orignal function display() equal to the function wrapped within the decorator, like
# display = decorator_function(display)

def display():
    print('display function ran')

display()
 
```

## Original Functions with any Parameters
```python 
def decorator_function(original_function):
    # to decorate functions with any combination and number of parameters, wrapper function needs to accept any number of positional and keyword arguments
    def wrapper_function(*args, **kwargs):
        print('wrapper executed this before {}'.format(original_function.__name__))
        return original_function(*args, **kwargs)
    return wrapper_function

@decorator_function 
def display():
    print('display function ran')
@decorator_function
def display_info(name, age):
    print('display_info ran with arguments ({}, {})'.format(name, age))

display_info("John", 25)
display()

```
## Ouput
```python
wrapper executed this before display_info
display_info ran with arguments (John, 25)
wrapper executed this before display
display function ran
```


# Use Classes as Decorators 

Some people like to use classes are decorators. `Different then decorator functions, decorating a function with the decorator class makes the function name a decorator class instance.`

## Example
```python 

class decorator_class:
    def __init__(self, original_function):
        self.original_function = original_function
    
    # class method lets class instances behave like functions and can be called like a function
    # examples ing line 28, 29
    def __call__(self, *args, **kwargs):
        print('call method executed this before {}'.format(self.original_function.__name__))
        return self.original_function(*args, **kwargs)


@decorator_class # == display = decorator_class(display)
# Unlike decorator functions, display is now a decorator_class instance, 
# with a function object attribute display()
def display():
    print('display function ran')
@decorator_class
def display_info(name, age):
    print('display_info ran with arguments ({}, {})'.format(name, age))

print("display is now type" , type(display))
print("display has attribute", display.original_function)
print("display_info is now type" , type(display_info))
print("display_info has attribute", display_info.original_function)
print()

display_info("John", 25) # this equals to display_info.__call__("John", 25)
display() # this equals to display.__call__()
```
## Output 
```python 
display is now type <class '__main__.decorator_class'>
display has attribute <function display at 0x0000023FCB7AD940>
display_info is now type <class '__main__.decorator_class'>
display_info has attribute <function display_info at 0x0000023FCB7ADAF0>

call method executed this before display_info
display_info ran with arguments (John, 25)
call method executed this before display
display function ran

```
# Practical Examples for Decorators 

## Logging - Keeping track of what functions were ran, and what arguments were passed
```python
def my_logger(orig_func):
    import logging
    logging.basicConfig(filename='{}.log'.format(orig_func.__name__), level=logging.INFO)

    def wrapper(*args, **kwargs):
        logging.info(
            'Ran with args: {}, and kwargs: {}'.format(args, kwargs))
        return orig_func(*args, **kwargs)

    return wrapper

@my_logger
def display_info(name, age):
    print('display_info ran with arguments ({}, {})'.format(name, age))
display_info("John", 25)
display_info("Hank", 30)
```
## In display_info.log
```
INFO:root:Ran with args: ('John', 25), and kwargs: {}
INFO:root:Ran with args: ('Hank', 30), and kwargs: {}
```

`This logger decorator enables us to reuse code for any function that we want to log, which prevents code repetition`


## Timing how long a function was ran
```python 
def my_timer(orig_func):
    import time

    
    def wrapper(*args, **kwargs):
        t1 = time.time()
        result = orig_func(*args, **kwargs)
        t2 = time.time() - t1
        print('{} ran in: {} sec'.format(orig_func.__name__, t2))
        return result

    return wrapper
import time 

@my_timer
def display_info(name, age):
    time.sleep(1)
    print('display_info ran with arguments ({}, {})'.format(name, age))
display_info("John", 25)
display_info("Hank", 30)
```
## Output
```python
display_info ran with arguments (John, 25)
display_info ran in: 1.002990484237671 sec
display_info ran with arguments (Hank, 30)
display_info ran in: 1.0050036907196045 sec
```

# Stacking Decorators 
## Incorrect Way
```python 
...

@my_logger
@my_timer
def display_info():
    pass

# This syntax equals to 
display_info = my_logger(my_timer(display_info))
```
` Remember that my_timer will return the wrapper function (the original function with added functionality), so my_logger will get my_logger(wrapper) where the wrapper was returned by the my_timer decorator, which is incorrect.`

To fix this, use the the functool module and wraps decorator 
## Correct Way - decorate every wrapper with the @wraps(orig_func) decorator 
```python 
from functools import wraps


def my_logger(orig_func):
    import logging
    logging.basicConfig(filename='{}.log'.format(orig_func.__name__), level=logging.INFO)

    @wraps(orig_func)
    def wrapper(*args, **kwargs):
        logging.info(
            'Ran with args: {}, and kwargs: {}'.format(args, kwargs))
        return orig_func(*args, **kwargs)

    return wrapper


def my_timer(orig_func):
    import time

    @wraps(orig_func)
    def wrapper(*args, **kwargs):
        t1 = time.time()
        result = orig_func(*args, **kwargs)
        t2 = time.time() - t1
        print('{} ran in: {} sec'.format(orig_func.__name__, t2))
        return result

    return wrapper

import time


@my_logger
@my_timer
def display_info(name, age):
    time.sleep(1)
    print('display_info ran with arguments ({}, {})'.format(name, age))

display_info('Tom', 22)
```