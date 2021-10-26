---
marp: true
theme: gaia
_class: invert
paginate: true
size: 16:9
---

# The Ultimate Pandas Bootcamp: Advanced Python Data Analysis

---

## Introduction

---

### Why Pandas?

1. fast, flexible, powerful data manipulation
2. one of the most popular packages in data science
3. complex data manipulation with little code, in record time

---

## Series at a Glance

---

### What is a series?

- Series are one-dimensional labeled arrays of any data type
- in other words...a sequence of values with associated labels

```python
mixed = [True, 'say', {'my_mood': 100}]
pd.Series(mixed)
```

---

### Parameters vs Arguments

```python
pd.Series(data=students)
```

- `data` is the *parameter*
- `students` is the *argument*

---

### Instantiation Methods

- `list` argument
  - `pd.Series(data=['this', 'is', 'fun'])`

- `dict` argument
  - `pd.Series(data={0: 'this', 1: 'is', 2: 'fun'})`

- also valid series
  - `pd.Series(data=0)`
  - `pd.Series(data='weather')`

---

### Index and RangeIndex

- automatic indexing (`RangeIndex`): 0 ... N - 1
  - can edit `start`, `stop`, and `step`
  - immutable object
- manual indexing:
  - `pd.Series(data=books_list, index=['label 1', 'label 2', 'label 3'])`

---

### The `.head()` and `.tail()` methods

- default `n = 5`
- `series.head(n=3)`
- `series.tail(n=7)`

---

### Extracting by Index Position

```python
from string import ascii_lowercase
alphabet = pd.Series(list(ascii_lowercase))
alphabet[0]    # first letter
alphabet[10]   # eleventh letter
alphabet[:3]   # first three letters
alphabet[6:11] # sixth through tenth letters
alphabet[-6:]  # last six letters
```
