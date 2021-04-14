# [First-Class Functions](https://www.youtube.com/watch?v=kr0mpwqttM0&list=PL-osiE80TeTsnP0Nl1UDY8VZAlHu1m_MQ&index=2)
`A programming language is said to have first-class functions if it treats functions as first-class citizens `

## First-Class Citizens
A first-class citizen/object in a programming language is an entity which supports all the operations generally avaiable to other entites, including:

* Being Passed as an **Argument**
* **Returned** from a Function
* Assigned to a **Variable**

## Assigned to a Variable
```python
def square(x):
    return x * x 

f = square

print(square)
print(f)
print(f(5))
```

### Output 
```python
<function square at 0x000002561EAAE0D0>
<function square at 0x000002561EAAE0D0> # f and square are the same object
25
```
`Note that putting parenthesis after a function executes the function, not return the function object.`

## Being Passed as an **Argument**
```python
def square(x):
    return x * x 

def my_map(func, arg_lst):
    result = []
    for i in arg_list:
        result.append(func(i))
    return result

squares = my_map(square, [1,2,3,4,5]) # passing in the function object square
print(squares)
```

### Output
```python 
[1, 4, 9, 16, 25]
```

## **Returned** from a Function
```python
def logger(msg):
    def log_message():
        print("Log: " + msg)
    return log_message

log_hi = logger("hi") 
# Now log_hi == log_message
# Note that it remembers the msg argument passed in

log_hi() 
```

### Output 
```python
Log: hi
```