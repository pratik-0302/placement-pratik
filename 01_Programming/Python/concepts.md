# Python Core Concepts

## 1. Data Types

### Immutable vs Mutable

| Immutable | Mutable |
|---|---|
| int, float, bool, str, tuple, frozenset | list, dict, set, bytearray |

```python
x = 10
y = x
y += 1
print(x, y)  # 10 11

a = [1, 2, 3]
b = a
b.append(4)
print(a)  # [1, 2, 3, 4]
