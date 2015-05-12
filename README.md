
    UNDER CONSTRUCTION

    Small guide for those transitioning from a functional programming language to Python. We use Scala methods and provide their equivalent in Python.

# Table of Contents

  1. [Iterator, generator, iterable and list](#iterator,-generator,-iterable-and-list)
  1. [map](#map)
  1. [filter and filterNot](#filter-and-filternot)
  1. [find](#find)
  1. [contains](#contains)
  1. [exists](#exists)
  1. [forall](#flatten)
  1. [flatMap](#flatmap)
  1. [reduce](#reduce)
  1. [zipWithIndex](#zipwithindex)
  1. [takeWhile](#takewhile)
  1. [dropwhile](#dropwhile)
  1. [zip](#zip)
  1. [groupby](#groupby)
  1. [reverse](#reverse)
  1. [head and headOption](#head-and-headoption)
  1. [last and lastOption](#head-and-lastoption)
  1. [getOrElse](#getorelse)
  1. [min and minBy](#min-and-minby)
  1. [max and maxBy](#max-and-maxby)
  1. [sort and sortBy](#sort-and-sortBy)
  1. [sum](#sum)
  1. [union](#union)
  1. [set-operations](#set-operations)
  1. [dictionary-operations](#dictionary-operations)
  1. [toIterator](#toiterator)

### Iterator, generator, iterable and list
An iterator is a stateful helper object that will return the next value when calling the method `next()`. A generator is a special kind of iterator that
one can write using the `yield` keyword or using a generator comprehension.

For instance the function `next_pow2` returns the next power of 2:
```python
def next_pow2():
   pow = 1
   while True:
       yield pow
       pow *= 2

> it = next_pow2()
> it
<generator object next_pow2 at 0x101e19b90>
> it.next()
1
> it.next()
2
```

You can also create a generator using a generator comprehension:
```python
> import sys
> it = (2 ** i for i in xrange(sys.maxint))
> it
<generator object <genexpr> at 0x101f25280>
> it.next()
1
> it.next()
2
```

The generator creates the next value lazily whenever the `next` method is called.

An iterable is an object that has a reference to an iterator (through the method `__iter__`). An iterator is itself an iterable and has the method `__iter__` returning
itself.  A list (e.g., `[1, 2, 3]`) and a tuple (e.g., `(1, 2, 3)`) are also iterables. You can convert an iterable to iterator using the `iter` function:
```python
> l = [1, 2, 3]
> iter(l) # same as t.__iter__()
<listiterator at 0x1082afe10>
> t = (1, 2, 3)
> iter(t)
<tupleiterator at 0x1082afb10>
```

Moreover since list and tuples are called sequence since we can access any of their element directly using their index (e.g., `l[0]`).

### map
The map function applies a function to each element of an iterable

You can use the `imap()` function:
```python
def double(x):
    return x * 2

> from itertools import imap
> res = imap(double, [1, 2])
> list(res)
[2, 4]

> res = imap(lambda x: x * 2, [1, 2])
> list(res)
[2, 4]
```

Note: There is also a `map` function but it returns a list and not a generator

Or more readable using a list comprehension:
```python
# create a list
> [x * 2 for x in [1, 2]]
[2, 4]

# create a generator
> (x * 2 for x in [1, 2])
<generator object <genexpr> at 0x10379ee10>
```

### filter and filterNot
The filter function keeps elements of an iterable that matches a condition. filterNot keeps elements of the iterable that does not match a condition

You can use the `ifilter()` and `ifilterfalse` functions:
```python
def is_even(x):
    return x % 2 == 0
    
from itertools import ifilter
from itertools import ifilterfalse
> res = ifilter(is_even, [1, 2])
# res is a generator so we print the result using list(res)
> list(res)
[1]

> res = ifilter(lambda x: x % 2 == 0, [1, 2])
> list(res)
[1]

> res = ifilterfalse(lambda x: x % 2 == 0, [1, 2])
> list(res)
[2]
```

Note: there is a `filter()` method but it returns a `list` and not a generator.

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
The find method returns the first occurence of an iterator that satisfies a condition

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

### contains
The contains method returns `True` if an element is present in the iterable.

In Python you can use the `in` operator:
```python
> 1 in [1, 2, 3]
True
```

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
> [elem for s_elem in source for elem in create_range(s_elem)]
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

### takeWhile

The function `takeWhile` returns an iterable that contains elements starting from the first one until they don't satisfy a condition.

You can use the function `takewhile` from the module `itertools`:

```python
> from itertools import takewhile
> source = [1, 2, 3, 4, 5, 6]
> res_iterator = takewhile(lambda x: x < 3, source)
> list(res_iterator)
[1, 2]
```

### dropWhile

The function `dropWhile` returns an iterable that strips all the elements starting from the start until they don't satisfy a condition.

You can use the function `dropwhile` from the module `itertools`:
```python
> from itertools import dropwhile
> source = [1, 2, 3, 4, 5, 6]
> res_iterator = dropwhile(lambda x: x < 3, source)
> list(res_iterator)
[3, 4, 5, 6]
```

### zip

The function `zip` combines 2 iterable

### groupby

### reverse
The reverse function reverse a sequence.

In Python, you can use the method `reversed`:
```python
> reverse([1, 2, 3])
<listreverseiterator at 0x101f204d0>
> reverse((1, 2, 3))
<reversed at 0x101f20790>
```

### head and headOption
The method head returns the first element of an iterable and headOption returns an option of the first element or None if the iterable is empty.

Python does not have Option but we can have something that returns None if the iterable is empty. We can use the method `next`:
```
> l = [1, 2, 3]
> next(iter(l), None)
1
> l = []
> next(iter(l), None)
None
```

Or an if statement::
```
> l = [1, 2, 3]
> l[0] if l else None
1
> l = []
> l[0] if l else None
None
```

### last and lastOption
The method last returns the last element of an iterable and lastOption returns an option of the last element or None if the iterable is empty.
```
> l = [1, 2, 3]
> next(reversed(l), None)
3
> l = []
> next(reversed(l), None)
None
```

This method will only work for sequences (since reversed only work for sequences). You can also use if statement instead if you use sequences:
```python
> l = [1, 2, 3]
> l[-1] if l else None
3
> l = []
> l[-1] if l else None
None
```

### getOrElse

### min and minBy

### max and maxBy

### sort and sortBy

### sum

### union

### Set Operations

### Dictionary Operations

##### Transforming a dictionary into another dictionary
	
You can iterate on a dictionary using the `items()` method
```python
# good, works on Python 2.7+ but does not work on Python 2.6
reverse_dictionary = {v: k for k, v in d.items()}

# works on all versions of Python
reverse_dictionary = dict((v, k) for k, v in d.items())
```

You can also use `iteritems()` instead of `items()` if the dictionary is large.

### toIterator
Convert an iterable to an iterator
