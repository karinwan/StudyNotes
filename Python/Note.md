# Arithmatic

```
# Floor division
A // B

# Sqare root
sqrt(A)

# Square
A ** 2
```

## minimum and maximum
```
min(0, 1, 2)
max(list)
```

## absolute value
```
abs()
```

# Loop

#### range

```
range(start, stop, step)
```

- **start**	(optional) An integer number specifying at which position to start. Default is 0
- **stop**	(required) An integer number specifying at which position to stop (not included).
- **step**	(optional) An integer number specifying the incrementation. Default is 1

#### Enumerate
Iterating through list with index
```
for idx, x in enumerate(list):
```

## Generator
Generator functions allow you to declare a function that behaves like an iterator, i.e. it can be used in a for loop.

```
 # Using the generator pattern (an iterable)
class first_n(object):

    def __init__(self, n):
        self.n = n
        self.num = 0

    def __iter__(self):
        return self

    # Python 3 compatibility
    def __next__(self):
        return self.next()

    def next(self):
        if self.num < self.n:
            cur, self.num = self.num, self.num+1
            return cur
        raise StopIteration()

sum_of_first_n = sum(first_n(1000000))
```

# Data Structure

## List

```
EmptyList = []
NumberList = [1, 2, 3, 4, 5]
ZeroList = [0] * 5
```

#### Sum
```
sum(list)
```

#### Length
```
len(list)
```

#### Slicing
```
# remove last element
list = list[:-1]
```

#### Converter
Sting -> List
```
l = list(s)
```

List -> String

```
s = ''.join(l)
```
If you what to add a string in-between of each item, i.e. convert a list of words to a sentence
```
s = ' '.join(l)  # s = '[space]'.join(l)
```

#### Sort
```
sorted(iterable, key=key, reverse=reverse)
```
- **iterable**	(Required) The sequence to sort, list, dictionary, tuple etc.
- **key**	(Optional) A Function to execute to decide the order. Default is None
- **reverse**	(Optional) A Boolean. False will sort ascending, True will sort d

## Set

```
items = set()
items.add(item)
if A in items:
  ...
```

#### Converter
List -> Set
```
setA = set(listA)
```

#### Intersection
```
setC = setA.intersection(setB)
setC = setA & setB
```

## Tuple

- ordered
- unchangeable
- allow duplicates
```
EmptyTuple = ()
FruitTuple = ("apple", "banana", "peach", "kiwi")
```

## Dictionary
- ordered
- changeable
- do not allow duplicates
```
EmptyDict = {}
InfoDict = { "name" : "Alice", "age" : 20, "country" : "CA"}
```

#### Constructor
```
ColorDict = dict(yellow = "energy", blue = "sadge", green = "natural")
```

#### Counter
A Counter is a dict subclass for counting hashable objects. It is a collection where elements are stored as dictionary keys and their counts are stored as dictionary values. Counts are allowed to be any integer value including zero or negative counts.
```
Counter("aabbbccccd")
# {a:2, b:3, c:4, d:1}
```

## String

Sentence to list of words
```
list = string.split()
```

# Collections Module

## Deque (Doubly Ended Queue)
Deque is preferred over a list in the cases where we need quicker append and pop operations from both the ends of the container, as deque provides an O(1) time complexity for append and pop operations as compared to a list that provides O(n) time complexity.
```
from collections import deque
 
# initializing deque
de = deque([1, 2, 3, 4, 5, 6])

de.pop()
```

#### Operations

| Methods          | Time Complexity | Auxiliary Space |
| ---------------- | --------------- | --------------- |
| append()         | O(1) | O(1) |
| appendleft() | O(1) | O(1) |
| pop() | O(1) | O(1) | 
| popleft() | O(1) | O(1) |
| index(ele, beg, end) | O(N) | O(1) |
| insert(i, a) | O(N) | O(1) |
| remove() | O(N) | O(1) |
| count() | O(N) | O(1) |
| len(dequeue) | O(1) | O(1) |
| Deque[0] | O(1) | O(1) |
| Deque[-1] | O(1) | O(1) |
| extend(iterable) | O(K) | O(1) |
| extendleft(iterable) | O(K) | O(1) |
| reverse() | O(N) | O(1) |
| rotate() | O(K) | O(1) |

## DefaultDict
Defaultdict is a container like dictionaries present in the module collections. Defaultdict is a sub-class of the dictionary class that returns a dictionary-like object. The functionality of both dictionaries and defaultdict are almost same except for the fact that defaultdict never raises a KeyError. It provides a default value for the key that does not exists.
```
defaultdict(default_factory)
```
- **default_factory**	(Required) A function returning the default value for the dictionary defined. If this argument is absent then the dictionary raises a KeyError.

