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

#### Convert string to list and list to string
```
l = list(s)
s = ''.join(l)
```
If you what to add a string in-between of each item, i.e. convert a list of words to a sentence
```
s = ' '.join(l)  # s = '[space]'.join(l)
```

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



