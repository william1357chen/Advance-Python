## Iterators 

### Iterable 
Anything that can be looped over, specifically, an object with the `__iter__()` special method 

The `__iter__()` method will return a iterator.

## What exactly is a Iterator
An iterator is an object with a state sp that it remembers where it is during iteration. Iterators also know how to get their next value with the ``__next__()` special method. 

Therefore, a list is iterable, but it isn't an iterator. 

Ex:
```python 
num = [1,2,3]
i_nums = nums.__iter__()
# or iter(nums)

print(type(i_nums))
print(dir(i_nums))

# Output 
# <class 'list_iterator'>
```

## Build our own Iterator
```python
class MyRange:
    def __init__(self, start, end):
        self.value = start
        self.end = end
    # iterators also have to be iterable
    def __iter__(self):
        return self
    def __next__(self):
        if self.value >= self.end:
            raise StopIteration
        current = self.value 
        self.value += 1
        return current 

nums = MyRange(1,10)

```
Note: Iterators can continue forever