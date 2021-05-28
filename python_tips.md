# [Python Tips](https://www.youtube.com/watch?v=C-gEQdGVXbk)

## Ternary Conditionals
Instead of 
```python 
condition = False
if condition:
    x = 1
else:
    x = 0
print(x)
```
We can use a ternary conditional for make it one line 
```python 
condition = True

x = 1 if condition else 0

print(x)
```

## Automated Testing with `assert`
`assert` statments gives us an `AssertionError` if the speficied condition is not true.
```python
assert 1 + 1 == 2
assert 1 + 1 == 2, "1 + 1 should be equal to 2 but didn't
```
We can add an optional message to print if assertion fails. 

## Underscore Placeholders
To make numbers easier to read
```python 
num1 = 100000000
num2 = 10000000
```
We can add underscores without affecting the code
```python 
num1 = 10_000_000_000
num2 = 100_000_000
```

## Context Manager
Rather than needing to close the file everything manually, which you might forget
```python 
f = open(filename, 'r')
files_contents = f.read()
f.close()

word = files_contens.split(' ')
word_count = len(words)
print(word_count)
```
We can use the Context Manager
```python 
with open(filename, 'r') as f:
    files_contents = f.read()

word = files_contens.split(' ')
word_count = len(words)
print(word_count)
```
`Everything that requires you to manage resources can be made easier with Context Manager`

## Enumerate 
Use enumerators to track index of a list. Instead of 
```python 
names = ["tom", "tim", "chris", "dave"]

index = 0
for name in names:
    print(index,name)
    index += 1
```
Use the enumerator 
```python 
names = ["tom", "tim", "chris", "dave"]

for index, name in enumerate(names, start=1):
    print(index, name)
```

## Zip 
Looping over two lists without using index
```python 
names = ["tom", "tim", "chris", "dave"]
names1 = ["tom", "tim", "chris", "dave"]
for name, name1 in zip(names, names1):
    print(f"{name} is actually {hero}")
```
`Note that in this case, the lists have the same length, or else the loop will stop when the shorter list is finished iterated`

## Unpacking 
```python 
# Normal
item = (1,2)

# Unpacking 
a, b = (1,2)

# To ignore a variable, for example we dont need the b variable, we use an underscore

a, _ = (1,2)

a, b, c = (1,2,3,4,5) # this is an error

# If we want c to store 3,4,5
a, b, *c = (1,2,3,4,5)

a, b, *c, d = (1,2,3,4,5)
# d will be the last element
# c will be elements between the second and last element

```

## Getting and Setting Attributes
```python
person = Person()

person_info = {'first': 'William', 'last': 'Chen'}

for key, value in person_info.items():
    setattr(person,key,value)
    # Sets an attribute for the person object with the name of what the string key is and with the attribute defined as value
    # Basically doing person.key = value
for key in person_info.keys():
    print(getattr(person,key))
    # print(person.key)
```

## Inputing Secret Information (GetPass)
### The wrong way 
```python 
username = input('Username: ')
password = input('Password: ')

print('Logging In')
```
### Output - Shows the Password, which is not ideal
```python 
Username: William
Password: omgchen1357
Logging In
```

### The Correct Way (GetPass)
```python 
from getpass import getpass
username = input('Username: ')
password = getpass('Password: ')

print('Logging In')
```

## Learning about a Module
```python 
import sys
help(sys)
```

List all attributes and methods of a module
```python 
import os
dir(os)
```