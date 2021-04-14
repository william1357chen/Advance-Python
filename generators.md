# Generators
Generators are Iterators that yields one value at a time. Generators do not hold the entire result in memory. It yeild it one at a time

```python 
def square_numbers(*args):
    for i in nums:
        yield (i*i)

my_nums = square_numbers(1,2,3,4,5)

print(next(my_nums))

```

Alternately, we could use list comprehension

```python 
my_nums = [x*x for x in [1,2,3,4,5]]

```
But we don't what my_nums as a list since it is O(n) space. We could use generator conprehension

```python 
my_nums = (x*x for x in [1,2,3,4,5])
# returns a generator
```

### Changing generator into iterables
```python 
list(my_nums)
```

## Advantage
Clearly, the space complexity performance of generators are much better than having lists. O(1) vs O(n)

Additionally, if we don't run the for loop on the generator, time complexity is O(1) for squaring numbers in a list. 

However, for list, just building the list costs O(n). 
