build-lists: true

# A Tale of Two Sorted Lists:
## Comparing Inheritance and Composition

> by Brian Schiller

- Devetry.com, let us help you convert to Python 3

---

> Favor Composition over Inheritance

---

## What's Composition? What's Inheritance?

- Inheritance: _**is-a**_ relationships
  - A `PoissonRandomNumberGenerator` _**is-a**_ (specific case of) `RandomNumberGenerator`
- Composition: _**has-a**_ relationship
  - A `PoissonRandomNumberGenerator` _**has-a**_ (makes use of) `RandomNumberGenerator`

---

### Building a sorted list
```

```

---
### Building a sorted list

```python
from bisect import bisect, insort






```

---

### Building a sorted list

```python
from bisect import bisect, insort
keep_me_sorted = []





```

---

### Building a sorted list

```python
from bisect import bisect, insort
keep_me_sorted = []

insort(keep_me_sorted, 4)
insort(keep_me_sorted, 10)


```

---

### Building a sorted list

```python
from bisect import bisect, insort
keep_me_sorted = []

insort(keep_me_sorted, 4)
insort(keep_me_sorted, 10)

include_fav_numbers(keep_me_sorted)
```

---

### Building a sorted list

```python
from bisect import bisect, insort
keep_me_sorted = []

insort(keep_me_sorted, 4)
insort(keep_me_sorted, 10)

include_fav_numbers(keep_me_sorted) # ⚠️
```

---

### Building a `SortedList`

- with inheritance!
- An `InheritanceSortedList` _**is-a**_ `list`

---

### Building `InheritanceSortedList`

```python
class InheritanceSortedList(list):
  def insert(self, elem):


  def __contains__(self, elem):


```

---

### Building `InheritanceSortedList`

```python
class InheritanceSortedList(list):
  def insert(self, elem):
    insort(self, elem)

  def __contains__(self, elem):


```

---

### Building `InheritanceSortedList`

```python
class InheritanceSortedList(list):
  def insert(self, elem):
    insort(self, elem)

  def __contains__(self, elem):
    ix = bisect(self, elem)
    return ix != len(self) and self[ix] == elem
```

---

### Building `InheritanceSortedList`

```python
class InheritanceSortedList(list):
  def insert(self, elem):
    insort(self, elem)

  def __contains__(self, elem):
    ix = bisect(self, elem)
    return ix != len(self) and self[ix] == elem
```

... are we done?

---

### Building a `SortedList`

- with composition!
- A `CompositionSortedList` _**has-a**_ `list`

---

### Building `CompositionSortedList`

```python
class CompositionSortedList:
  def __init__(self):
    self._lst = []

  def insert(self, elem):
    insort(self._lst, elem)

  def __contains__(self, elem):
    ix = bisect(self._lst, elem)
    return ix != len(self._lst) and self._lst[ix] == elem
```

---

### A problem with `CompositionSortedList`

```python
sl = CompositionSortedList()
for elem in (12, 4, 15, 2):
    sl.insert(elem)




```

---

### A problem with `CompositionSortedList`

```python
sl = CompositionSortedList()
for elem in (12, 4, 15, 2):
    sl.insert(elem)
print(4 in sl) # True
print(18 in sl) # False


```

---

### A problem with `CompositionSortedList`

```python
sl = CompositionSortedList()
for elem in (12, 4, 15, 2):
    sl.insert(elem)
print(4 in sl) # True
print(18 in sl) # False
print(', '.join(elem for elem in sl))

```

---

### A problem with `CompositionSortedList`

```python
sl = CompositionSortedList()
for elem in (12, 4, 15, 2):
    sl.insert(elem)
print(4 in sl) # True
print(18 in sl) # False
print(', '.join(elem for elem in sl))
# 💥 TypeError: iteration over non-sequence
```

---

### Allow iteration over `CompositionSortedList`

```python
class CompositionSortedList:
  # ...
  def __iter__(self):
    return iter(self._lst)
```

"To iterate over a `CompositionSortedList`, iterate over the hidden `_lst` bit"

---

### `InheritanceSortedList` breaks down

```python
lst = InheritanceSortedList()

lst.append(4)
lst.append(5)

```

---

### `InheritanceSortedList` breaks down

```python
lst = InheritanceSortedList()

lst.append(4)
lst.append(5)
print(5 in lst)
# prints 'False'
```

---

### `InheritanceSortedList` breaks down

```python


```
---

### `InheritanceSortedList` breaks down

```python
lst = InheritanceSortedList([5, 3, 2])
print(2 in lst)
# prints 'False'
```

---

### `InheritanceSortedList` breaks down

What happened?

- `__init__` and `append` were inherited from `list`.
- The right thing for a `list` is the wrong thing for a `SortedList`.

---

### Fixing `InheritanceSortedList`

```python
class InheritanceSortedList(list):
    def __init__(self, iterable=()):
        super().__init__(self)
        for elem in iterable:
            self.insert(elem)
    def append(self, elem):
        raise RuntimeError("...")
    def extend(self, iterable):
        raise RuntimeError("...")
```

---

### Fixing `InheritanceSortedList`

```python
class InheritanceSortedList(list):
    def __init__(self, iterable=()):
        super().__init__(self)
        for elem in iterable:
            self.insert(elem)
    def append(self, elem):
        raise RuntimeError("...")
    def extend(self, iterable):
        raise RuntimeError("...")
```

... are there others?

---

### `CompositionSortedList` builds up

```python
class CompositionSortedList:
    # ...
    def __iter__(self):
        return self._lst.__iter__()
    def __len__(self):
        return self._lst.__len__()
    def __getitem__(self, ix):
        return self._lst.__getitem__(ix)
    def __reversed__(self):
        return self._lst.__reversed__()
```

---

### Extra Credit: An even better delegation pattern

- `pip install superdelegate`

```python
from superdelegate import SuperDelegate, delegate_to

class CompositionSortedList(SuperDelegate):
  # ...
  __iter__ = __len__ = __getitem__ = __reversed__ = delegate_to('_lst')
```

---

### So... when do I use inheritance?

- When you want to accept _all_ behavior of parent class.
- When it's the way a framework wants you to customize behavior. You're "slotting-in" custom logic.
  - Django's Class-based Views are a good example.
- When there's some weird metaclass stuff going on
  - SqlAlchemy's DeclarativeBase, or WTForms classes

---
[.build-lists: false]

### References

- https://code.activestate.com/recipes/577197-sortedcollection/
- https://docs.python.org/3.7/library/bisect.html
- https://github.com/bgschiller/superdelegate
- https://brianschiller.com/blog/

---
# [fit] Thank you!
