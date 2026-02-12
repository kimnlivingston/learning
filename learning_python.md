**Python for Data Analysis**
===

# Introduction

- Object oriented.
- Interpreted (line by line, not all at once).
- Portable language (can run on all OS).

# Packages

Packages are a collection of modules and are fundamental. [The Python Package Index (PyPI)](https://pypi.org/) is a repository of packages for Python that contains documentation, release notes, and reference links for each.

### Common Packages

- [NumPy](https://pypi.org/project/numpy/): scientific computing.
- [Matplotlib](https://pypi.org/project/matplotlib/): create static, animated, and interactive visualizations.
- [Pandas](https://pypi.org/project/pandas/): datastructures for data analysis, time series, and statistics.
- [SciPy](https://pypi.org/project/scipy/): algorithms for scientific computing.

### Sample Usage

[camelcase](https://pypi.org/project/camelcase/): Converts a string to Camel Case with the ability to include words to skip over.

``` python
from camelcase import
CamelCase c = CamelCase()
s = 'this is a sentence that needs CamelCasing!'
print c.hump(s)
```
-
  ``` python
  'This is a Sentence That Needs CamelCasing!'
  ```

# Data Types

- `int` : number w/o decimals
- `float` : number w/ decimals
- `str` : string
- `bool` : boolean

# User Input

```python
balance = float(input("What is your account balance? "))
```
- Prompts `What is your account balance?` and saves user input as a `float` to `balance`

# Data Structures

| Feature    | List | Tuple | Set | Dictionary  |
| ---------- | ---- | ----- | --- | ----------- |
| nested     | yes  | yes   | ?   | yes         |
| indexing   | yes  | yes   | no  | yes         |
| modified   | yes  | no    | no  | values only |
| ordered    | yes  | yes   | no  | >= 3.7 yes  |
| duplicates | yes  | yes   | no  | ?           |

## Lists

``` python
x = [1, 2, 3, 4, 5]
```

### Retrieving Data

| INPUT        | OUTPUT            |
| ------------ | ----------------- |
| `type(x)`    | `list`            |
| `len(x)`     | `4`               |
| `sum(x)`     | `15`              |
| `max(x)`     | `15`              |
| `x[1]`       | `2`               |
| `x[1:3]`     | `[2, 3]`          |
| `x[:]`       | `[1, 2, 3, 4, 5]` |
| `x[3:]`      | `[4, 5]`          |
| `x[:3]`      | `[1, 2, 3]`       |
| `x[-1]`      | `5`               |
| `x[:-2]`     | `[1, 2, 3]`       |
| `x[-3:]`     | `[3, 4, 5]`       |
| `x.index(4)` | `3`               |
| `2 in x`     | `True`            |
| `2 not in x` | `False`           |
| `x.count(1)` | `1`               |

### Manipulating Data

| INPUT              | OUTPUT                  |
| ------------------ | ----------------------- |
| `x.append(6)`      | `[1, 2, 3, 4, 5]`       |
| `x.pop()`          | `[1, 2, 3, 4]`          |
| `x.remove(2)`      | `[1, 3, 4, 5]`          |
| `x.insert(1, 1.5)` | `[1, 1.5, 2, 3, 4, 5]`  |
| `x.extend([6, 7])` | `[1, 2, 3, 4, 5, 6, 7]` |
| `x.clear()`        | `[]`                    |
| `del.x`            | `NameError`             |
| `x.reserve()`      | `[5, 4, 3, 2, 1]`       |

### Aliasing

``` python
y = x
y.pop()
```

| INPUT | OUTPUT   |
| ----- | -------- |
| `x`   | `[1, 2]` |
| `y`   | `[1, 2]` |

### Cloning

``` python
y = list(x)
y.pop()

```

| INPUT | OUTPUT      |
| ----- | ----------- |
| `x`   | `[1, 2, 3]` |
| `y`   | `[1, 2]`    |

## Tuples

``` python
product = ('banana', 1.25, [10, 15])
```

| INPUT                  | OUTPUT                       |
| ---------------------- | ---------------------------- |
| `list(product)`        | `['banana', 1.25, [10, 15]]` |
| `tuple(list(product))` | `('banana', 1.25, [10, 15])` |
| `product[2][1] = 20`   | `('banana', 1.25, [10, 20])` |

### Unpacking

``` python
(name, price, stock) = product
(food, *info) = product
```

| INPUT   | OUTPUT             |
| ------- | ------------------ |
| `name`  | `'banana'`         |
| `price` | `1.25`             |
| `stock` | `[10, 15]`         |
| `food`  | `'banana'`         |
| `info`  | `[1.25, [10, 15]]` |

## Sets

``` python
coffee = {'Cappuccino', 'Espresso', 'Latte'}
```

### Manipulate

| INPUT                                         | OUTPUT                                              |
| --------------------------------------------- | --------------------------------------------------- |
| `coffee.discard('Cappuccino')`                | `{'Espresso', 'Latte'}`                             |
| `coffee.discard('Flat White')`                | `{'Cappuccino', 'Espresso', 'Latte'}`               |
| `coffee.remove('Cappuccino')`                 | `{'Espresso', 'Latte'}`                             |
| `coffee.remove('Flat White')`                 | `KeyError`                                          |
| `coffee.add('Cappuccino')`                    | `{'Cappuccino', 'Espresso', 'Latte'}`               |
| `coffee.add('Flat White')`                    | `{'Cappuccino', 'Espresso', 'Flat White', 'Latte'}` |
| `coffee.update({'Cappuccino', 'Flat White'})` | `{'Cappuccino', 'Espresso', 'Flat White', 'Latte'}` |

### Logic Operations

``` python
a = {10, 11, 12, 13, 14, 15}
b = {14, 15, 16, 17, 18, 19, 20}
x = {1, 2, 3}
y = {1, 2, 3, 4, 5}
```

| INPUT               | OUTPUT                                         |
| ------------------- | ---------------------------------------------- |
| `a.intersection(b)` | `{14, 15}`                                     |
| `a.union(b)`        | `{10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}` |
| `a.difference(b)`   | `{10, 11, 12, 13}`                             |
| `b.difference(a)`   | `{16, 17, 18, 19, 20}`                         |
| `x.issubset(y)`     | `True`                                         |
| `y.issuperset(x)`   | `True`                                         |

## Dictionaries

``` python
bag = {
  'orange': 2
  , 'mango': 10
  , 'banana': 20
}
basket = {
  'lemon': {'price': 4.5, 'quantity': 5}
  , 'banana': {'price': .99, 'quantity': 9}
}
```

### Retrieving Data

| INPUT                                 | OUTPUT                                                      |
| ------------------------------------- | ----------------------------------------------------------- |
| `bag['orange']`                       | `2`                                                         |
| `bag.get('orange')`                   | `2`                                                         |
| `bag.get('peach')`                    |                                                             |
| `bag.get('peach', 'Does not exist.')` | `'Does not exist.'`                                         |
| `bag.values()`                        | `[2, 10, 20]`                                               |
| `bag.keys()`                          | `dict_keys(['orange', 'mango', 'banana'])`                  |
| `sorted(bag.keys())`                  | `['banana', 'mango', 'orange']`                             |
| `bag.items()`                         | `dict_keys([('orange', 2), ('mango', 10), ('banana', 20)])` |
| `'orange' in bag`                     | `True`                                                      |
| `basket['lemon']['price']`            | `4.5`                                                       |

### Manipulating Data

| INPUT                                 | OUTPUT                                                             |
| ------------------------------------- | ------------------------------------------------------------------ |
| `bag['orange'] = 10`                  | `{'orange': 10, 'mango': 10, 'banana': 20}`                        |
| `bag.update({'orange': 10})`          | `{'orange': 10, 'mango': 10, 'banana': 20}`                        |
| `bag['peach'] = 4`                    | `{'orange': 10, 'mango': 10, 'banana': 20, 'peach': 4}`            |
| `bag.update({'peach': 4})`            | `{'orange': 10, 'mango': 10, 'banana': 20, 'peach': 4}`            |
| `bag.update({'peach': 4, 'kiwi': 7})` | `{'orange': 10, 'mango': 10, 'banana': 20, 'peach': 4, 'kiwi': 7}` |
| `bag.pop('orange')`                   | `2` <br> `{'mango': 10, 'banana': 20}`                              |
| `del bag['mango']`                    | `{'orange': 10, 'banana': 20}`                                     |
| `bag.popitem()` (>= 3.7)              | `{'orange': 10, 'mango': 10}`                                      |
| `bag.clear()`                         | `{}`                                                               |
| `supply = bag.copy()`                 | `{'orange': 10, 'mango': 10, 'banana': 20}`                        |

# Conditional Statements

``` python
if expression:
  code
elif expression:
  code
else:
  code
```

| INPUT          | OUTPUT  |
| -------------- | ------- |
| `bool(None)`   | `False` |
| `bool(0)`      | `False` |
| `bool([])`     | `False` |
| `bool({})`     | `False` |
| `bool(())`     | `False` |
| `bool(3)`      | `True`  |
| `bool([1, 3])` | `True`  |

```python
balance = float(input("What is your account balance? "))

if balance:
  print(f"Your balance is {balance}")
```
- ```
  What is your account balance? 20.2
  Your balance is 20.3
  ```
- ```
  What is your account balance? 0
  ```

# Loops

## While

``` python
while true:
  do this
```

## For

``` python
for value in list:
  do this
```

``` python
for key, value in list.items():
  do this
```

``` python
numbers = list(range(5))
print(numbers)
```
- ``` python
  [0, 1, 2, 3, 4]
  ```

``` python
for value in range(5):
  print(value)
```
- ``` python
  0
  1
  2
  3
  4
  ```

## For/Else Statement

``` python
for item in grocery_list:
  if item == "bread":
    print("Bread is on the list!")
    break
else:
  print("Bread is not on the list.")

```

# Functions

``` python
def say_hello():
  print("Hello")
```

``` python
say_hello()
```
- ``` python
  Hello
  ```

# Objects

``` python
class BankAccount:
  def __init__(self, name, number, balance):
    self.name = name
    self.number = number
    self.balance = balance

  def deposit(self, amount):
    self.balance += amount

  def withdraw(self, amount):
    if self.balance >= amount:
      self.balance -= amount
    else:
      print ("Insufficient Funds")

  def info(self):
    return f"{self.name} {self.number} {self.balance}"
```