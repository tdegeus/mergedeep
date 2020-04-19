# [mergedeep](https://mergedeep.readthedocs.io/en/latest/)

[![PyPi release](https://img.shields.io/pypi/v/mergedeep.svg)](https://pypi.org/project/mergedeep/)
[![PyPi versions](https://img.shields.io/pypi/pyversions/mergedeep.svg)](https://pypi.org/project/mergedeep/)
[![Downloads](https://pepy.tech/badge/mergedeep)](https://pepy.tech/project/mergedeep)
[![Documentation Status](https://readthedocs.org/projects/mergedeep/badge/?version=latest)](https://mergedeep.readthedocs.io/en/latest/?badge=latest)

A deep merge function for 🐍.

[Check out the mergedeep docs](https://mergedeep.readthedocs.io/en/latest/)

## Installation

```bash
$ pip install mergedeep
```

## Usage

```text
merge(destination: Map[KT, VT], *sources: Map[KT, VT], strategy: Strategy = Strategy.REPLACE) -> Map[KT, VT]
```

Deep merge without mutating the source dicts.

```python3
from mergedeep import merge

a = {"keyA": 1}
b = {"keyB": {"sub1": 10}}
c = {"keyB": {"sub2": 20}}

merged = merge({}, a, b, c) 

print(merged)
# {"keyA": 1, "keyB": {"sub1": 10, "sub2": 20}}
```

Deep merge into an existing dict.
```python3
from mergedeep import merge

a = {"keyA": 1}
b = {"keyB": {"sub1": 10}}
c = {"keyB": {"sub2": 20}}

merge(a, b, c) 

print(a)
# {"keyA": 1, "keyB": {"sub1": 10, "sub2": 20}}
```

### Merge strategies:

1. Replace (*default*)

> `Strategy.REPLACE`

```python3
# When `destination` and `source` values are the same, replace the `destination` value with one from `source` (default).

# Note: with multiple sources, the `last` (i.e. rightmost) source value will be what appears in the merged result. 

from mergedeep import merge, Strategy

dst = {"key": [1, 2]}
src = {"key": [3, 4]}

merge(dst, src, strategy=Strategy.REPLACE) 
# same as: merge(dst, src)

print(dst)
# {"key": [3, 4]}
```

2. Additive

> `Strategy.ADDITIVE`

```python3
# When `destination` and `source` values are both the same additive collection type, extend `destination` by adding values from `source`.
# Additive collection types include: `list`, `tuple`, `set`, and `Counter`

# Note: if the values are not additive collections of the same type, then fallback to a `REPLACE` merge.

from mergedeep import merge, Strategy

dst = {"key": [1, 2]}
src = {"key": [3, 4]}

merge(dst, src, strategy=Strategy.ADDITIVE) 

print(dst)
# {"key": [1, 2, 3, 4]}
```

3. Typesafe replace

> `Strategy.TYPESAFE_REPLACE` or `Strategy.TYPESAFE`

```python3
# When `destination` and `source` values are of different types, raise `TypeError`. Otherwise, perform a `REPLACE` merge.

from mergedeep import merge, Strategy

dst = {"key": [1, 2]}
src = {"key": {3, 4}}

merge(dst, src, strategy=Strategy.TYPESAFE_REPLACE) # same as: `Strategy.TYPESAFE`  
# TypeError: destination type: <class 'list'> differs from source type: <class 'set'> for key: "key"
```

4. Typesafe additive

> `Strategy.TYPESAFE_ADDITIVE`

```python3
# When `destination` and `source` values are of different types, raise `TypeError`. Otherwise, perform a `ADDITIVE` merge.

from mergedeep import merge, Strategy

dst = {"key": [1, 2]}
src = {"key": {3, 4}}

merge(dst, src, strategy=Strategy.TYPESAFE_ADDITIVE) 
# TypeError: destination type: <class 'list'> differs from source type: <class 'set'> for key: "key"
```

## License

MIT &copy; [**Travis Clarke**](https://blog.travismclarke.com/)
