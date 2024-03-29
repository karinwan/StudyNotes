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

## Bitwise Operators

| Operator |	Example |	Meaning | Compound Operator |
| -------- | ------- | ------- | ----- |
| & |	a & b |	Bitwise AND | a &= b |
| \| |	a \| b	| Bitwise OR | a \|= b |
| ^	| a ^ b |	Bitwise XOR (exclusive OR) | a ^= b |
| ~ |	~a |	Bitwise NOT | |
| << |	a << n |	Bitwise left shift | a <<= n |
| >> |	a >> n |	Bitwise right shift | a >>= n |


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

#### List Comprehension
```
[print(x) for x in list]
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
```

#### Initialize lists using the * operator
```
ZeroList = [0] * 5
```
Be careful when the item being repeated is a list. The list will not be cloned: all the elements will refer to the same list! (The outer list stores references to the same inner list.)
```
x = [0]
y = x * 3     # y = [[0], [0], [0]]
y[0][0] = 1   # y = [[1], [1], [1]]
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
- **key**	(Optional) A Function to execute to decide the order. Default is None. 
- **reverse**	(Optional) A Boolean. False will sort ascending, True will sort descending. 

Sorting dictionary by its values in decreasing order:
```
list = sorted(dict.items(), key=lambda x:x[1], reverse=True)
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

## String

Sentence to list of words
```
list = string.split()
```

#### Min & Max
```
min(list)
max(list)
```

#### Operations

| Methods          | Description |
| ---------------- | --------------- |
| append()	| Adds an element at the end of the list | 
| clear()	| Removes all the elements from the list | 
| copy()	| Returns a copy of the list | 
| count()	| Returns the number of elements with the specified value | 
| extend()	| Add the elements of a list (or any iterable), to the end of the current list | 
| index()	| Returns the index of the first element with the specified value | 
| insert()	| Adds an element at the specified position | 
| pop()	| Removes the element at the specified position | 
| remove()	| Removes the first item with the specified value | 
| reverse()	| Reverses the order of the list | 
| sort()	| Sorts the list | 

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

## heapq - Heap queue algorithm / priority queue algorithm
Heaps are binary trees for which every parent node has a value less than or equal to any of its children. 

| Methods          | Description |
| ---------------- | --------------- |
| heapq.heappush(heap, item) | push the value item onto the heap, maintaining the heap invariant |
| heapq.heappop(heap) | pop and return the smallest item from the heap, maintaining the heap invariant; If the heap is empty, IndexError is raised; To access the smallest item without popping it, use heap[0]. |
| heapq.heappushpop(heap, item) | push item on the heap, then pop and return the smallest item from the heap |
| heapq.heapify(x) | transform list x into a heap, in-place, in linear time |
| heapq.heapreplace(heap, item) | pop and return the smallest item from the heap, and also push the new item; If the heap is empty, IndexError is raised. |
|  |  |
| heapq.merge(*iterables, key=None, reverse=False) | merge multiple sorted inputs into a single sorted output and return an iterator over the sorted values |
| heapq.nlargest(n, iterable, key=None) | return a list with the n largest elements from the dataset defined by iterable |
| heapq.nsmallest(n, iterable, key=None) | return a list with the n smallest elements from the dataset defined by iterable |


## DefaultDict
Defaultdict is a container like dictionaries present in the module collections. Defaultdict is a sub-class of the dictionary class that returns a dictionary-like object. The functionality of both dictionaries and defaultdict are almost same except for the fact that defaultdict never raises a KeyError. It provides a default value for the key that does not exists.
```
defaultdict(default_factory)
```
- **default_factory**	(Required) A function returning the default value for the dictionary defined. If this argument is absent then the dictionary raises a KeyError.

