# Python Core Concepts — Placement Notes

> **Target:** SDE, Data Science, ML, AI/GenAI, Data Engineering, Cloud and MLOps interviews.
> **Priority:** ⭐⭐⭐⭐⭐ unless marked otherwise.
> **Python baseline:** Python 3.10+

Python uses a dynamic, object-based data model. Every value is an object with identity, type, and value; names are bound to objects. The official language reference and tutorial are the primary sources for these notes. 

## 1. Names, Objects and References ⭐⭐⭐⭐⭐

Mental model:

    name ─────► object
                ├─ identity
                ├─ type
                └─ value

Example:

    x = [1, 2, 3]
    y = x
    y.append(4)

    # x and y now both refer to [1, 2, 3, 4]
    x is y        # True
    x == y        # True

Key distinction:

- == checks equality/value.
- is checks identity.
- id(x) exposes an identity value.
- Use is None and is not None for None checks.

The identity of an object does not change during its lifetime. 

## 2. Dynamic Typing ⭐⭐⭐⭐⭐

Python names do not require a fixed declared type:

    x = 10
    x = "hello"

The current object determines the runtime type.

Type annotations improve readability and tooling:

    age: int = 21
    name: str = "A"

Annotations do not by themselves enforce runtime type checking.

## 3. Core Data Types ⭐⭐⭐⭐⭐

Common built-ins:

| Type | Mutable? | Typical use |
|---|---|---|
| int | No | integers |
| float | No | real numbers |
| bool | No | truth values |
| complex | No | complex numbers |
| str | No | text |
| list | Yes | ordered collection |
| tuple | No | fixed/immutable sequence |
| set | Yes | unique elements |
| frozenset | No | immutable set |
| dict | Yes | key-value mapping |
| NoneType | No | absence of value |

### Integer division

    7 / 2      # 3.5
    7 // 2     # 3
    -7 // 2    # -4

// is floor division, not truncation toward zero.

### bool and int

    True == 1
    False == 0

bool is a subclass of int.

## 4. None ⭐⭐⭐⭐⭐

None represents absence of a value.

    result = None

    if result is None:
        print("No result")

Prefer identity checks for None.

## 5. Strings ⭐⭐⭐⭐⭐

Strings are immutable sequences.

    s = "abcdef"
    s[0]       # a
    s[-1]      # f
    s[1:4]     # bcd
    s[::-1]    # fedcba

Common operations:

    lower()
    upper()
    strip()
    split()
    replace()
    startswith()
    endswith()
    find()
    count()

Joining:

    words = ["I", "love", "Python"]
    sentence = " ".join(words)

Because strings are immutable, character assignment is invalid.

## 6. Lists ⭐⭐⭐⭐⭐

Lists are ordered and mutable.

    arr = [10, 20, 30]

Important operations:

    append(x)
    extend(iterable)
    insert(i, x)
    pop()
    pop(i)
    remove(x)
    reverse()
    sort()

Typical complexity:

| Operation | Complexity |
|---|---:|
| Index access | O(1) |
| Append | O(1) amortized |
| Pop from end | O(1) |
| Search | O(n) |
| Insert/delete near beginning | O(n) |
| Sort | O(n log n) |

Use collections.deque for frequent operations at both ends.

## 7. Tuples ⭐⭐⭐⭐⭐

Tuples are ordered and immutable.

    point = (10, 20)

A one-element tuple requires a comma:

    (10,)     # tuple
    (10)      # int

Unpacking:

    x, y = point
    first, *middle, last = [1, 2, 3, 4, 5]

Tuples are useful for fixed records and multiple-return-value patterns.

## 8. Sets ⭐⭐⭐⭐⭐

Sets contain unique hashable elements.

    s = {1, 2, 3}
    s.add(4)
    s.remove(2)
    s.discard(2)

Set operations:

    a | b       # union
    a & b       # intersection
    a - b       # difference
    a ^ b       # symmetric difference

Membership is typically O(1) average.

Set elements must be hashable; list, dict and set objects cannot be elements.

## 9. Dictionaries ⭐⭐⭐⭐⭐

Dictionary = mapping from hashable keys to values.

    student = {"name": "A", "age": 21}

Access:

    student["name"]
    student.get("name")
    student.get("marks", 0)

Difference:

- d[key] raises KeyError if absent.
- d.get(key) returns None by default or a supplied default.

Useful methods:

    keys()
    values()
    items()
    update()
    pop()
    setdefault()

Typical average complexity:

| Operation | Average |
|---|---:|
| Lookup | O(1) |
| Insert | O(1) |
| Delete | O(1) |

Python dictionaries preserve insertion order as part of the language behavior.

## 10. Mutable vs Immutable ⭐⭐⭐⭐⭐

Common immutable objects:

- int
- float
- bool
- str
- tuple
- frozenset
- None

Common mutable objects:

- list
- dict
- set
- bytearray

Aliasing:

    a = [1, 2]
    b = a
    b.append(3)

    # a is also [1, 2, 3]

Mutation changes the shared object.

## 11. Shallow vs Deep Copy ⭐⭐⭐⭐⭐

Assignment creates another reference:

    b = a

Shallow copy:

    b = a.copy()
    # or
    b = a[:]

The outer container is copied, but nested objects can remain shared.

Deep copy:

    import copy
    b = copy.deepcopy(a)

Interview answer: shallow copy copies the outer object; deep copy recursively copies nested referenced objects according to the copy module's rules.

## 12. Operators ⭐⭐⭐⭐⭐

Arithmetic:

    +  -  *  /  //  %  **

Comparison:

    ==  !=  <  <=  >  >=

Identity:

    is  is not

Membership:

    in  not in

Logical:

    and  or  not

Bitwise:

    &  |  ^  ~  <<  >>

Use parentheses when precedence would otherwise make an expression difficult to read.

## 13. Truthiness ⭐⭐⭐⭐⭐

Common falsy values:

    False
    None
    0
    0.0
    ""
    []
    ()
    {}
    set()

Most other objects are truthy.

Therefore:

    if items:
        process(items)

is usually preferable to:

    if len(items) > 0:
        process(items)

## 14. Short-Circuit Evaluation ⭐⭐⭐⭐⭐

and and or stop evaluation as soon as the result is determined.

    0 or 10           # 10
    "hello" and 5     # 5

Important: and/or return operands, not necessarily True/False.

## 15. Control Flow ⭐⭐⭐⭐⭐

Basic forms:

    if condition:
        ...

    for x in iterable:
        ...

    while condition:
        ...

    break
    continue
    pass

range uses start, stop, step:

    range(2, 10, 2)

The stop value is excluded.

### Loop else

The else block of a loop executes when the loop finishes normally, not when it exits through break.

    for x in nums:
        if x == target:
            break
    else:
        print("Not found")

## 16. Functions ⭐⭐⭐⭐⭐

    def add(a, b):
        return a + b

Functions are objects and can be assigned, stored and passed around.

    def square(x):
        return x * x

    f = square
    f(5)

Variable-length arguments:

    def total(*args):
        return sum(args)

    def show(**kwargs):
        return kwargs

- *args collects positional arguments into a tuple.
- **kwargs collects keyword arguments into a dictionary.

Unpacking:

    nums = [1, 2, 3]
    print(*nums)

## 17. Parameter Kinds ⭐⭐⭐⭐

Python supports positional-only and keyword-only parameters.

    def f(a, /, b, *, c):
        ...

Here:

- a is positional-only.
- b is positional-or-keyword.
- c is keyword-only.

This is useful for designing explicit APIs.

## 18. Mutable Default Argument Trap ⭐⭐⭐⭐⭐

Avoid:

    def add_item(item, items=[]):
        items.append(item)
        return items

The default object is created when the function definition is evaluated and can persist across calls.

Use:

    def add_item(item, items=None):
        if items is None:
            items = []
        items.append(item)
        return items

This is a classic interview question.

## 19. Comprehensions ⭐⭐⭐⭐⭐

List:

    squares = [x * x for x in range(10)]

With condition:

    even = [x for x in range(10) if x % 2 == 0]

Dictionary:

    squares = {x: x * x for x in range(5)}

Set:

    unique = {x % 3 for x in range(10)}

Prefer readability over deeply nested comprehensions.

## 20. Lambda, map, filter and reduce ⭐⭐⭐⭐

Lambda:

    square = lambda x: x * x

map:

    result = list(map(lambda x: x * 2, nums))

filter:

    result = list(filter(lambda x: x % 2 == 0, nums))

reduce:

    from functools import reduce
    result = reduce(lambda a, b: a + b, nums)

For many interview solutions, comprehensions are easier to read.

## 21. enumerate and zip ⭐⭐⭐⭐⭐

enumerate avoids manual index management:

    for i, value in enumerate(arr):
        print(i, value)

zip traverses iterables together:

    for name, score in zip(names, scores):
        print(name, score)

Both are essential for clean coding-round solutions.

## 22. Sorting ⭐⭐⭐⭐⭐

sorted returns a new list:

    b = sorted(a)

sort modifies a list in place:

    a.sort()

Custom key:

    students.sort(key=lambda x: x[1], reverse=True)

Common trap:

    result = a.sort()
    # result is None

Python's sorting algorithm is stable.

## 23. Exceptions ⭐⭐⭐⭐⭐

    try:
        result = operation()
    except ValueError:
        handle_error()
    else:
        use(result)
    finally:
        cleanup()

Roles:

- try: risky operation.
- except: handle selected exception.
- else: executes if no exception occurred.
- finally: cleanup/finalization.
- raise: explicitly raise an exception.

Example:

    if age < 0:
        raise ValueError("age cannot be negative")

Prefer catching specific exceptions instead of using a broad except when possible.

## 24. Context Managers ⭐⭐⭐⭐⭐

Use with for reliable resource management:

    with open("data.txt", "r", encoding="utf-8") as f:
        data = f.read()

Important for files, locks, database connections and other resources that require cleanup.

## 25. File Handling ⭐⭐⭐⭐⭐

Read:

    with open("data.txt", encoding="utf-8") as f:
        data = f.read()

Line-by-line:

    with open("data.txt", encoding="utf-8") as f:
        for line in f:
            print(line.strip())

Write:

    with open("out.txt", "w", encoding="utf-8") as f:
        f.write("hello")

Modes:

| Mode | Meaning |
|---|---|
| r | read |
| w | write/truncate |
| a | append |
| x | exclusive creation |
| b | binary |
| + | update/read + write |

## 26. Modules and Imports ⭐⭐⭐⭐⭐

A module is a Python file containing definitions and statements that can be imported elsewhere.

    import math
    math.sqrt(16)

Or:

    from math import sqrt

Avoid unnecessary wildcard imports.

Main guard:

    if __name__ == "__main__":
        main()

This allows a module to be imported without automatically executing its script entry point.

## 27. Scope and LEGB ⭐⭐⭐⭐⭐

Name lookup is commonly explained as:

    L = Local
    E = Enclosing
    G = Global
    B = Built-in

Example:

    x = "global"

    def outer():
        x = "enclosing"

        def inner():
            x = "local"
            print(x)

        inner()

    outer()

Python's execution model formally describes local, global and free/enclosing name bindings.

## 28. global and nonlocal ⭐⭐⭐⭐

global changes a module-level binding:

    x = 10

    def change():
        global x
        x = 20

nonlocal changes a binding in an enclosing function scope:

    def outer():
        x = 10

        def inner():
            nonlocal x
            x += 1

        inner()
        return x

Use both deliberately; excessive shared state makes programs harder to reason about.

## 29. Iterable vs Iterator ⭐⭐⭐⭐⭐

Iterable: an object from which an iterator can be obtained.

Examples:

- list
- tuple
- string
- set
- dictionary

Iterator: produces values one at a time.

    arr = [1, 2, 3]
    it = iter(arr)

    next(it)   # 1
    next(it)   # 2
    next(it)   # 3

After exhaustion, next raises StopIteration.

Generators provide a convenient iterator mechanism and are covered in advanced_python.md.

## 30. Packing and Unpacking ⭐⭐⭐⭐⭐

Packing:

    values = 1, 2, 3

Unpacking:

    a, b, c = values

Starred unpacking:

    a, *rest = [1, 2, 3, 4]

These patterns appear frequently in coding rounds.

## 31. String Formatting ⭐⭐⭐⭐⭐

Prefer f-strings:

    name = "Partik"
    score = 95
    print(f"{name} scored {score}")

Formatting:

    price = 12.3456
    print(f"{price:.2f}")

## 32. Important Built-ins ⭐⭐⭐⭐⭐

Know these without searching:

    len()
    range()
    enumerate()
    zip()
    sorted()
    sum()
    min()
    max()
    abs()
    round()
    any()
    all()
    reversed()
    type()
    isinstance()

Examples:

    any(x > 10 for x in nums)
    all(x >= 0 for x in nums)

any returns True when at least one element is truthy.
all returns True when every element is truthy.

## 33. isinstance ⭐⭐⭐⭐⭐

For normal runtime type checks:

    isinstance(x, int)

is generally more useful than:

    type(x) is int

because isinstance accounts for subclass relationships.

## 34. Hashability ⭐⭐⭐⭐⭐

Dictionary keys and set elements must be hashable.

Common hashable objects:

- int
- str
- bytes
- tuples whose elements are hashable
- frozenset

Common unhashable objects:

- list
- dict
- set

Example:

    d = {}
    d["name"] = "A"

    # d[[1, 2]] = "x"  # TypeError

## 35. collections for Coding Rounds ⭐⭐⭐⭐⭐

### Counter

    from collections import Counter
    freq = Counter(nums)

### defaultdict

    from collections import defaultdict
    groups = defaultdict(list)
    groups[key].append(value)

### deque

    from collections import deque
    q = deque()
    q.append(x)
    q.appendleft(y)
    q.popleft()
    q.pop()

Use deque for BFS, queues and sliding-window patterns where both-end operations matter.

## 36. Heap / Priority Queue ⭐⭐⭐⭐⭐

heapq implements a min-heap:

    import heapq

    heap = []
    heapq.heappush(heap, 5)
    heapq.heappush(heap, 2)
    smallest = heapq.heappop(heap)

Typical complexities:

- push: O(log n)
- pop minimum: O(log n)
- minimum access: O(1)

A common max-heap pattern is to push negative values.

## 37. Coding-Round Input ⭐⭐⭐⭐⭐

    n = int(input())

    a, b, c = map(int, input().split())

    arr = list(map(int, input().split()))

For very large input:

    import sys
    data = sys.stdin.buffer.read().split()

Use the simplest input method that is fast enough.

## 38. Python Complexity Awareness ⭐⭐⭐⭐⭐

Typical interview assumptions:

| Operation | Typical complexity |
|---|---:|
| list[i] | O(1) |
| list.append(x) | O(1) amortized |
| x in list | O(n) |
| x in set | O(1) average |
| x in dict | O(1) average |
| sorted(list) | O(n log n) |
| deque.popleft() | O(1) |
| heapq.heappush() | O(log n) |
| heapq.heappop() | O(log n) |

Always analyze the actual algorithm; nested loops do not automatically imply O(n²).

## 39. Classic Placement Traps ⭐⭐⭐⭐⭐

### Trap: is vs ==

    a = [1]
    b = [1]

    a == b    # True
    a is b    # False

### Trap: aliasing

    a = [1, 2]
    b = a
    b.append(3)

a changes too.

### Trap: nested list multiplication

Dangerous:

    grid = [[0] * 3] * 3

All rows refer to the same inner list.

Correct:

    grid = [[0] * 3 for _ in range(3)]

### Trap: sort return value

    result = arr.sort()
    # result is None

### Trap: dictionary lookup

    d["missing"]       # KeyError
    d.get("missing")   # None

### Trap: string immutability

    s = "abc"
    # s[0] = "x"       # TypeError

## 40. Placement Priority

### ⭐⭐⭐⭐⭐ MUST KNOW

- Lists, tuples, sets, dictionaries
- Mutable vs immutable
- References and aliasing
- == vs is
- Shallow vs deep copy
- Functions and argument passing
- Mutable default arguments
- Comprehensions
- enumerate, zip, sorted
- Exceptions and with
- LEGB / scope
- Hashability
- Counter, defaultdict, deque
- heapq
- Common operation complexity
- Coding-round input/output

### ⭐⭐⭐⭐ SHOULD KNOW

- Positional-only and keyword-only parameters
- lambda
- map/filter/reduce
- loop else
- global/nonlocal
- type annotations
- modules and packages
- assignment expressions

### ⭐⭐⭐ NICE TO KNOW

- Less-common syntax
- Import-system internals
- Interpreter implementation details

Advanced generators, closures, decorators, memory management, GIL and async programming are intentionally reserved for advanced_python.md.

## 41. Self-Test Before Moving On

You should be able to answer these verbally and code small examples:

1. What is the difference between a name and an object?
2. == vs is?
3. Why is None normally checked with is?
4. Mutable vs immutable?
5. Shallow vs deep copy?
6. Why is [[0] * n] * n dangerous?
7. Why is a mutable default argument dangerous?
8. List vs tuple vs set vs dict?
9. Why must dictionary keys be hashable?
10. What are *args and **kwargs?
11. Explain LEGB.
12. Iterable vs iterator?
13. What does with do?
14. sort() vs sorted()?
15. How does dict.get differ from d[key]?
16. Why is set membership usually faster than list membership?
17. When would you use deque?
18. How do you implement a priority queue?
19. What does enumerate solve?
20. What does zip solve?
21. What do any and all do?
22. What is the average dictionary lookup complexity?
23. What happens if a function has no explicit return?
24. What does finally do?
25. Why use the __main__ guard?

## 42. Authoritative References

- Python Language Reference — data model, execution model, expressions, statements and operator semantics. 
- Python Tutorial — control flow, data structures, functions, modules, errors and classes.
- Python Tutorial — modules and import behavior.

References:
- https://docs.python.org/3/reference/
- https://docs.python.org/3/reference/datamodel.html
- https://docs.python.org/3/reference/executionmodel.html
- https://docs.python.org/3/reference/expressions.html
- https://docs.python.org/3/tutorial/
