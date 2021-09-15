---
toc: true
layout: post
description: Short Notes on Python 3.8 new features
categories: [python]
title: New Features in Python 3.8
published: true
comments: true
---

# Introduction

It may be noted that this document is more of an exemplified excerpt of official Python Documentation: [What’s New In Python 3.8](https://docs.python.org/3/whatsnew/3.8.html).  

You are recommended to take a look at the Official Documentation with a view to take full advantage of new features of Python 3.8.

# Features

## 1. Assignment Expressions (to simplify code constructs)

- [Assignment Expression and Walrus Operator](https://blog.teclado.com/python-assignment-expressions/)
- Provide for easier-looking code in certain situation (while loop conditions, efficient list/dictionary/set comprehensions)
- Please see [Python Standard Library](https://www.python.org/dev/peps/pep-0572/#examples) for examples from Python 3.8 standard library

## 2. Positional-Only & Keyword-Only Function Arguments

- **Rules**
  
  - Division operator in function signature `/` - arguments to the left side of the operator are enforced as `positional-only`
  - Multiplication operator in function signature `*` - arguments to the right side of the operator are enforced as `keyword-only`
  - You can intersperse positional or keyword arguments between `/` and `*` in the function signature.
  
- **Examples**

  - ```python
    # A 'normal' function signature
    >>> def incr(x):
            return x + 1
    >>>     
    # The above function can be invoked in two different ways.
    >>> incr(x=1)  
    2
    >>> incr(1)
    2
    ```

  - ```python
    # Convert the above function to accept 'positional-only' argument.
    # Default values for arguments can be provided.
    >>> def incr(x=0, /):
    >>>     return x + 1
    >>>     
    # This works.
    >>> incr(1)
    2
    >>> 
    # This fails with an error.
    >>> incr(x=1)
    Traceback (most recent call last):
      File "<input>", line 1, in <module>
        incr(x=1)
    TypeError: incr() got some positional-only arguments passed as keyword arguments: 'x'
    ```
    
  - ```python
    # Function signature allowing keyword-only argument
    >>> def to_fahrenheit(*, celsius=0):
    >>>     return 32 + 9 * celsius / 5
    >>>     
    # This works.
    >>> to_fahrenheit(celsius=40)
    104.0
    >>> 
    # This won't. 
    >>> to_fahrenheit(40)
      File "<input>", line 1, in <module>
        to_fahrenheit(40)
    TypeError: to_fahrenheit() takes 0 positional arguments but 1 was given
    ```
    
   - ```python
     # Function signature showing a combination of: 
     #    positional only, general, keyword only arguments
     
     >>> def greet(name, /, greeting="Hi there", *, time_of_day="4:30pm"):
     >>>    return f"{greeting} {name}.  It is {time_of_day} now!"
     >>>
     >>> greet("Shashank", greeting="Hello", time_of_day="8:00")
     'Hello Shashank.  It is 8:00 now!'
     >>> greet("Shashank")
     'Hi there Shashank.  It is 4:30pm now!'     
     ```

## 3. Advanced f-strings & debug specifier

- Assignment Expressions can be used inside a `f-string` 
  - ```python
    >>> r = 3.8
    >>> print(f"Diameter {(diam := 2 * r)} gives circumference {math.pi * diam:.2f}")
    ```

- Specifier `=` enables self-documenting output 

  - Prints the name of the variable in along with its value.

  - ```python
    # Here is how it works.
    >>> x = 100
    >>> print(f"{x=})
    'x=100'
    ```

## 4. Improvements to modules

- `asyncio.run()` graduates to a stable API from a provisional one.  
- `math` gets new functions: [`prod()`](https://docs.python.org/3/library/math.html#math.prod), [`isqrt()`](https://docs.python.org/3/library/math.html#math.isqrt), [`dist()`](https://docs.python.org/3/library/math.html#math.dist) [`perm()`](https://docs.python.org/3/library/math.html#math.perm), [`comb()`](https://docs.python.org/3/library/math.html#math.comb)
- `statistics` gets fortified with new functions: [`fmean()`](https://docs.python.org/3/library/statistics.html#statistics.fmean), [`geometric_mean()`](https://docs.python.org/3/library/statistics.html#statistics.geometric_mean), [`multimode()`](https://docs.python.org/3/library/statistics.html#statistics.multimode), [`quantiles()`](https://docs.python.org/3/library/statistics.html#statistics.quantiles), [`NormalDist()`](https://docs.python.org/3/library/statistics.html#statistics.NormalDist)
- And several others...

## 5. A new module: `importlib.metadata`

 Helps with inspection of a package, its metadata, files, requirements, so on. 

- `metadata.version('package')`

  - ```python
    >>> from importlib import metadata
    >>> metadata.version("bpython")
    '0.21'
    ```

- `metadata.metadata('package')`

  - ```python
    >>> from pprint import pprint
    >>> pprint(dict(metadata.metadata("bpython")))
    {'Author': 'Bob Farrell, Andreas Stuehrk, Sebastian Ramacher, Thomas '
               'Ballinger, et al.',
     'Author-email': 'robertanthonyfarrell@gmail.com',
     'Classifier': 'Programming Language :: Python :: 3',
     'Home-page': 'https://www.bpython-interpreter.org/',
     'License': 'MIT/X',
     'Metadata-Version': '2.1',
     'Name': 'bpython',
     'Platform': 'UNKNOWN',
     'Provides-Extra': 'jedi',
     'Requires-Dist': 'pygments',
     'Requires-Python': '>=3.6',
     'Summary': 'Fancy Interface to the Python Interpreter',
     'Version': '0.21'}
    ```

- `metadata.files('package')`=> returns list of `Path()` objects

  - ```python
    >>> len(metadata.files("bpython"))
    153
    ```

- `metadata.requires('package')`

  - ```python
    >>> pprint(metadata.requires("bpython"))
    ['pygments',
     'requests',
     'curtsies (>=0.3.5)',
     'greenlet',
     'cwcwidth',
     'pyxdg',
     "jedi (>=0.16) ; extra == 'jedi'",
     "urwid ; extra == 'urwid'",
     "watchdog ; extra == 'watch'"]
    ```

## 6. `SyntaxWarning` over dubious looking code

- The difference between `is` and `==` can be confusing. The latter checks for equal values, while `is` is `True` only when objects are the same. Python 3.8 will try to warn you about cases when you should use `==` instead of `is`.

  - ```python
    >>> # Python 3.7
    >>> version = "3.7"
    >>> version is "3.7"
    False
    >>>     
    >>> # Python 3.8
    >>> version = "3.8"
    >>> version is "3.8"
    <stdin>:1: SyntaxWarning: "is" with a literal. Did you mean "=="?
    False
    >>>     
    >>> version == "3.8"
    True
    ```

- It is easy to miss a comma when you’re writing out a long list, especially when formatting it vertically. Forgetting a comma in a list of tuples will give a confusing error message about tuples not being callable. Python 3.8 additionally emits a warning that points toward the real issue.

  - ```python
    >>> [
    ...   (1, 2)
    ...   (3, 4)
    ... ]
    <stdin>:2: SyntaxWarning: 'tuple' object is not callable; perhaps
               you missed a comma?
    Traceback (most recent call last):
      File "<stdin>", line 2, in <module>
    TypeError: 'tuple' object is not callable
    ```

## 7. Optimizations

As with every new point release, Python offers several runtime improvements with 3.8 as well.  The salient ones are:

- faster file copying with `shutil` functions
- faster lookup of fields in `namedtuple` objects
- faster `operator.itemgetter` operations
- reduced memory footprint for lists, dictionaries
- improved default performance in `pickle` (enabled by pickle protocol 5; see [PEP 574](https://www.python.org/dev/peps/pep-0574))

The comprehensive list of improvements is available at [Python Official Website](https://docs.python.org/3.8/whatsnew/3.8.html#optimizations).

## 8. More Precise Type Hinting Support 

Type hinting is a formal solution to statically indicate the type of a variable within Python code.  


Type hinting makes for an important subject.  [`Dataclasses`](https://realpython.com/python-data-classes/) in Python Standard Library are built over type hinting.  Several libraries in open source domain are taking advantage of this type hinting to build sophisticated capabilities (e.g., accurate [code auto-completion in IDEs](https://realpython.com/python-type-checking/#pros-and-cons) to improve developer productivity), and tools (data validation libraries e.g., [`Pydantic`](https://betterprogramming.pub/the-beginners-guide-to-pydantic-ba33b26cde89)).  Notable examples include: [`FastAPI`](https://realpython.com/fastapi-python-web-apis/) - a highly scalable web application development framework; [`Typer`](https://typer.tiangolo.com/tutorial/) - a modern command line application development framework.

[PEP 484](https://www.python.org/dev/peps/pep-0484/) specifies Python's type hinting.  Python 3.5 introduced language-level support for type hinting.  

Please see [this article](https://realpython.com/python-type-checking/) to get started with python type hints.

[Python 3.8](https://docs.python.org/3/whatsnew/3.8.html#typing) expands type hinting support to offer more precision, by adding support for the following: 

- Literal Types through [PEP 586](https://www.python.org/dev/peps/pep-0586)
- Typed Dictionaries through [PEP 589](https://www.python.org/dev/peps/pep-0589)
- Final Objects through [PEP 591](https://www.python.org/dev/peps/pep-0591)
- Protocol Definitions through [PEP 544](https://www.python.org/dev/peps/pep-0544)
