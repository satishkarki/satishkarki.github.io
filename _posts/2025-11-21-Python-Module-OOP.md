---
layout: post
title: "Module Mayhem & OOP Odyssey: Python's Power Play"
date: 2025-11-21 10:00:00
categories: Python
tags: python  # TAG names should be lowercase
image:
    path: /assets/img/headers/pythonOOP.webp
---

By now you’ve got the basics of Python down and you’ve managed not to get eaten by the snake, so congrats. Now we’re going deeper into its territory: modules, packages, and that whole object-oriented thing everyone keeps talking about.

## Module
Module is a decomposition process of breaking down a complex problem or a system into smaller, independent components. It is a file containing Python definitions and statements, which can be later imported and used when necessary.

[The Python Standard Library](https://docs.python.org/3/library/index.html) contains built-in modules (written in C) as well as modules written in Python.

### Importing a module
```python
import math
from math import sin, pi
from module import *
import module as alias
from module import name as alias
```
```python
# Example
from math import sin, pi

print(sin(pi / 2))

pi = 3.14

'''redefine the meaning of pi and sin - in effect, they supersede the original (imported) definitions within the code's namespace'''
def sin(x): 
    if 2 * x == pi:
        return 0.99999999
    else:
        return None

# Output:
1.0
0.99999999

```
### Working with standard module
[Python Module Index](https://docs.python.org/3/py-modindex.html)
#### `math`
```python
import math
  
for name in dir(math):
  print(name, end="∖t")

# Output
__doc__ __loader__ __name__ __package__ __spec__ acos acosh asin asinh atan atan2
atan2 atanh ceil copysign cos cosh degrees e erf erfc exp expm1 fabs factorial floor
fmod frexp fsum gamma hypot isfinite isinf isnan ldexp lgamma log log10 log1p
log2 modf pi pow radians sin sinh sqrt tan tanh trunc 
```
#### `random`
If computer is 100% deterministic. How does `random.randint(1,10)` sometimes give 3, sometimes 7?
> It’s fake randomness (called pseudo-random).

```python
import random   # ← At this exact moment, Python secretly does:
                # random.seed() with a value based on the current clock/time

print(random.getstate())    # Huge blob of data = current seed state
                            # Run the script again → the blob will be different!

random.seed(42)          # Force the seed to be 42
print(random.randint(1, 100))  # → always 81
print(random.randint(1, 100))  # → always 14
print(random.randint(1, 100))  # → always 3

# Try running the whole script again → you get 81, 14, 3 again!
```
```python
import random
dir(random)

# Output
['BPF', 'LOG4', 'NV_MAGICCONST', 'RECIP_BPF', 'Random', 'SG_MAGICCONST', 'SystemRandom', 'TWOPI', '_ONE', '_Sequence', '__all__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', '_accumulate', '_acos', '_bisect', '_ceil', '_cos', '_e', '_exp', '_fabs', '_floor', '_index', '_inst', '_isfinite', '_lgamma', '_log', '_log2', '_os', '_pi', '_random', '_repeat', '_sha512', '_sin', '_sqrt', '_test', '_test_generator', '_urandom', '_warn', 'betavariate', 'binomialvariate', 'choice', 'choices', 'expovariate', 'gammavariate', 'gauss', 'getrandbits', 'getstate', 'lognormvariate', 'normalvariate', 'paretovariate', 'randbytes', 'randint', 'random', 'randrange', 'sample', 'seed', 'setstate', 'shuffle', 'triangular', 'uniform', 'vonmisesvariate', 'weibullvariate']
```
#### `platform`
```python
import platform
dir(platform)

# Output
['_Processor', '_WIN32_CLIENT_RELEASES', '_WIN32_SERVER_RELEASES', '__builtins__', '__cached__', '__copyright__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', '__version__', '_comparable_version', '_default_architecture', '_follow_symlinks', '_get_machine_win32', '_java_getprop', '_mac_ver_xml', '_node', '_norm_version', '_os_release_cache', '_os_release_candidates', '_parse_os_release', '_platform', '_platform_cache', '_sys_version', '_sys_version_cache', '_syscmd_file', '_syscmd_ver', '_uname_cache', '_unknown_as_blank', '_ver_stages', '_win32_ver', '_wmi_query', 'architecture', 'collections', 'freedesktop_os_release', 'functools', 'itertools', 'java_ver', 'libc_ver', 'mac_ver', 'machine', 'node', 'os', 'platform', 'processor', 'python_branch', 'python_build', 'python_compiler', 'python_implementation', 'python_revision', 'python_version', 'python_version_tuple', 're', 'release', 'sys', 'system', 'system_alias', 'uname', 'uname_result', 'version', 'win32_edition', 'win32_is_iot', 'win32_ver']
```
### How module works?

![__pycache__](/assets/img/post/pythonOOP/bytecode.png){: width="972" height="589" .w-50 .left}

If you create module, lets say module.py and import it to main.py, a new subfolder appears - `__pycache__`. It has a file called module.cpython-xy.pyc where x and y are digits derived from python version. The last part .pyc comes from the word Python and Compiled. It is a byte code ready to be executed by interpreter.



#### The famous `if __name__ == "__main__":` guard

```python
# module.py
print("I like to be a module.")
print("My __name__ is:", __name__)

# This part runs ONLY when you run module.py directly
if __name__ == "__main__":
    print("I'm being run directly! Let's do some tests:")
    print("Testing my own functions...")
else:
    print("I was imported by another module")
```
```python
# Output
I like to be a module.
My __name__ is: __main__
I'm being run directly! Let's do some tests:
Testing my own functions...
```
Let's say you import module.py to your main.py and run the main.py script, the output will be:
```python
import module
print(__name__)

# Output
I like to be a module.
My __name__ is: module
I was imported by another module
```
To sum it up, if you run the module directly, `__name__` becomes `"__main__"` and the statements in `if __name__=="__main__":` runs. If it is imported to main.py and you run main.py, `__name__` becomes `"your file name"` and `if __name__=="__main__":` doesn't run.

Real World Example
```python
# calculator.py
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

# These tests run only when you execute this file directly
if __name__ == "__main__":
    print("Testing calculator...")
    print(add(5, 3))      # → 8
    print(subtract(10, 4)) # → 6
```

