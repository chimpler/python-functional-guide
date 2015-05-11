
    UNDER CONSTRUCTION

    Small guide for those transitioning from a functional programming language to Python

# Table of Contents

  1. [map](#map)
  1. [filter](#filter)
  1. [find](#find)
  1. [exists](#exists)
  1. [forall](#flatten)
  1. [flatMap](#flatMap)
  1. [reduce](#reduce)
  1. [zipWithIndex](#zipWithIndex)
  
### map
The map function applies a function to each element of an iterable

You can use the `map()` function:
```python
def double(x):
    return x * 2

> map(double, [1, 2])
[2, 4]

> map(lambda x: x * 2, [1, 2])
[2, 4]
```

Or more readable using a list comprehension:
```python
# create a list
> [x * 2 for x in [1, 2]]
[2, 4]

# create a generator
> (x * 2 for x in [1, 2])
<generator object <genexpr> at 0x10379ee10>
```

### filter
The filter function keeps elements of a list that matches a condition.

You can use the `filter()` function:
```python
def is_even(x):
    return x % 2 == 0
    
> filter(is_even, [1, 2])
[1]

> filter(lambda x: x * 2, [1, 2])
[1]
```

Or more readable using a list comprehension:
```python
# create a list
> [x for x in [1, 2] if x % 2 == 0]
[2, 4]

# create a generator
> (x for x in [1, 2] if x % 2 == 0)
<generator object <genexpr> at 0x10379ee10>
```

### find
The find method returns the first occurence of a generator that satisfies a condition

You can use the `next()` method combined with a map or list comprehension:
```python
def is_even(x):
    return x % 2 == 0
    
# using filter    
> even_num_it = filter(is_even, [1, 2, 3, 4])  
> next(even_num_it, None)
2

# using list comprehension
> even_num_it = (x for x in [1, 2, 3, 4] if x % 2 == 0)
> next(even_num_it, None)
2
```

Note that the generator is evaluated lazily so next would not evaluate elements after the element is found. The second parameter of the
next function is the value to return if the iterator is empty 

### exists
The exists method returns `True` if an element satisfies a condition.
 
You can use the `any()` method combined with a map or list comprehension:
```python
> even_num_it = (x % 2 == 0 for x in [1, 2, 3, 4])
> any(even_num_it)
True
```
any will return `True` as soon as it finds a `True` value from an iterable.
 
### forall
The forall method returns `True` if all the elements of the iterable satisfy the condition.

You can use the `all` method combined with a map or list comprehension: 
```python
> even_num_it = (x % 2 == 0 for x in [1, 2, 3, 4])
> all(even_num_it)
False
```

### flatten
The flatten method converts an iterable of iterable of elements into an iterable of elements.

You can use a list comprehension:
```python
> source = [[1, 2], [3, 4]]
> [element for sub_list in source for element in sub_list]
[1, 2, 3, 4]
```

### flatMap
The flatMap applies map and flatten.
You can use a list comprehension:
```python
> def create_range(x):
    return range(x, x + 10)

# We want to do something like source.flatMap(create_range)
> source = [10, 20, 30]
> [elem for s_elem in source for elem in [create_range(s_elem)]]
[[10, 11, 12, 13, 14, 15, 16, 17, 18, 19],
 [20, 21, 22, 23, 24, 25, 26, 27, 28, 29],
 [30, 31, 32, 33, 34, 35, 36, 37, 38, 39]]
```

### reduce
The reduce function aggregates all the elements of a list into one.
  
You can use the `reduce` function:  
```python
> source = [1, 2, 3]
> reduce(lambda x, y: x + y, source)
6
```

The first parameter of the `reduce` function is a function that accepts 2 parameters and returns the reduction of the 2 parameters.
In our case we simply implemented the `sum` function.

### zipWithIndex

The function `zipWithIndex` applied to an iterable return an iterator of tuples `(index, element)`.
 
You can use the `enumerate` function:
```python
> source = ['a', 'b', 'c']
> for index, element in enumerate(source):
    print('%d: %s' % (index, element))
0: a
1: b
2: c    
```

### takewhile

### dropwhile

### zip

### groupby

# Dictionaries

### Initialization
- When initializing a dictionary, try to set all elements in one call.

```python
# bad
d = {}	
d['a'] = 1
d['b'] = 2	

# good
d = {
    'a': 1,
    'b': 2,
}
```
	
### List comprehension
	
```python
# good, works on Python 2.7+ but does not work on Python 2.6
reverse_dictionary = {v: k for k, v in d.items()}

# works on all versions of Python
reverse_dictionary = dict((v, k) for k, v in d.items())
```

You can also use `iteritems()` instead of `items()` if the dictionary is large.

### Exceptions
