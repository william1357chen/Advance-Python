# Positinal Arguments vs. Keywords Arguments

## Positional Arguments
### Rules
* Must be included in the correct order 
* Same arguments with different order produces different results
* Has to be defined and passed in before any keyword arguments
### Example 
```python
complex(3,5)
complex(5,3)
```
### Output 
```python 
3 + 5j
5 + 3j
```

### Passing a variable number of positional arguments
`The arguments are passed as a tuple and these passed arguments make tuple inside the function with same name as the parameter excluding asterisk *`
#### Syntax
```python 
*args
```
### Example 
```python 
def adder(*num):
    sum = 0
    print(type(num))
    for n in num:
        sum = sum + n

    print("Sum:",sum)

adder(3,5)
adder(4,5,6,7)
adder(1,2,3,5,6)
```
### Output 
```python 
<class 'tuple'>
Sum: 8
<class 'tuple'>
Sum: 22
<class 'tuple'>
Sum: 17
```

## Keywords Arguments
### Rules 
* Arguments are included with a keyword and equals sign
* Order does not matter
* Has to be defined and passed in before any required positional arguments

### Example
```python 
def add(a, b = 10, c):
    return a + b  + c 
# This is an error 
SyntaxError: non-default argument follows default argument
```

### Example
```python 
complex(real=3, imag=5)
complex(imag=5, real=3)
# Order does not matter, since it is a dictionary
```
### Output 
```python 
3+5j
3+5j
```

### Passing a variable number of keyword arguments
`The arguments are passed as a dictionary and these arguments make a dictionary inside function with name same as the parameter excluding double asterisk **`
#### Syntax
```python
**kwargs
```
### Example 
```python 
def intro(**data):
    print("\nData type of argument:",type(data))

    for key, value in data.items():
        print("{} is {}".format(key,value))

intro(Firstname="Sita", Lastname="Sharma", Age=22, Phone=1234567890)
intro(Firstname="John", Lastname="Wood", Email="johnwood@nomail.com", Country="Wakanda", Age=25, Phone=9876543210)
```
### Output 
```python 
Data type of argument: <class 'dict'>
Firstname is Sita
Lastname is Sharma
Age is 22
Phone is 1234567890

Data type of argument: <class 'dict'>
Firstname is John
Lastname is Wood
Email is johnwood@nomail.com
Country is Wakanda
Age is 25
Phone is 9876543210
```