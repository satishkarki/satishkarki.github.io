---
layout: post
title: "Try-Except or Die Trying: Python for the Exceptionally Clueless"
date: 2025-10-24 10:00:00
categories: Python
tags: python  # TAG names should be lowercase
image:
    path: /assets/img/headers/memepython.webp
---
If you just looked at the thumbnail of the post and jumped right into the contain, I'm sorry to say you are in the wrong place. I clickbaited you. That's how everything works these days. But don't worry-here is a very interesting video about [python 101](https://youtu.be/uns8vUQNxpc?si=nMDPZQu23vGXCXN3) on NatGeo. Now, back to the unalive python. I have summarized the content of python course available on [NetCad](https://www.netacad.com/dashboard). It is free and offered by Python Institute for the PCEP- Certified Entry-Level Python Programmer Certification Exam. Let's catch this python.

## Function
A list of all [built-in Functions](https://docs.python.org/3.12/library/functions.html)
```python
print("Bye World!")
```
A function may have an effect or result. There is also a thrid very important function component- the **argument(s)**. 
* Functions strongly demands a pair of parentheses
* If you want to deliver one or more arguments to a function, you place them inside the parentheses. If you're going to use a function which doesn't take any argument, you still have to have the parentheses.
* String as the print() function's argument- The string is delimited with quotes- **"Bye World!"** or **'Bye World!'**

```python
print("Hello\nWorld") # New Line
print ("\") # It will throw error
print("\\") # It will print \
```
* The backslash( \ ) when used inside a string is called the escape character- kind of like an nnouncement
* The letter **n** placed after the backslash comes from the word newline

```python
print("Max Verstappen", "Carlos Sainz", "Lewis Hamilton")
```
* a print() function invoked with more than one argument outputs them all on one line
* the print() function puts a space between the outputted arguments on its own initiative.
  
```python
print("Hello", "World", sep="---", end="!!!")
print("Next")
#Output
Hello---World!!!Next

print(sep="&", "Max", "Verstappen")
#Output
SyntaxError: positional argument follows keyword argument

# Under the hood
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```

Keyword arguments are the ones whose meaning is not dictated by their location, but by a special word (keyword) used to identify them.
* sep="---": Keyword argument that changes the separator between objects from a space to ---.
* end="!!!": Keyword argument that ends the output with !!! instead of a newline.
* `Remember`: Keyword arguments should be passed after any required positional arguments.

## Python Literals
A literal is data whose values are determined by the literal itself.

The characteristic of the numeric value which determines its kind, range, and application, is called the type.

### Integers

* can be represented as `111111111` or `111_111_111`
* Python 3.6 has introduced underscores in numeric literals, allowing for the placement of single underscores between digits and after base specifiers for improved readability. This feature is not available in older versions of Python.
* Negative Number can be representes as `-111111111` or `-111_111_111`
### Octal and hexadecimal numbers
* If an integer number is preceded by an `0O` or `0o` prefix (zero-o), it will be treated as an octal value. This means that the number must contain digits taken from the [0..7] range only.
* Hexadecimal number should be preceded by the prefix `0x` or `0X` (zero-X)

### Floats

```python
.4 # 0.4
4. # 4.0
3E8 # 3*10^8
6.62607E-34 # 6.62607*10^-34

print(0.00000000000000000000001)
# output
1e-22 #
```
* The letter E (you can also use the lower-case letter e ‒ it comes from the word exponent) is a concise record of the phrase times ten to the power of.
* the exponent (the value after the E) has to be an integer
* the base (the value in front of the E) may be either an integer or a float
* Python always chooses the more economical form of the number's presentation

### Strings
```python
print("I like \"F1\"")
print('I like "F1"')
```
Strings need quotes the way floats need points.

#### Boolean values
```python
print(True > False)
print(True < False)
# Output
True
False

print(True + True)
# Output
2
```
* Python in a binary reptile, True=1 and False=0
* True and False are case-sensitive- insecure about lowercase

### None
```python
a = None
b = None
print(a is b)  # Output: True (same object)

print(type(None))  # Output: <class 'NoneType'>

if not None:
    print("None is falsy")  # Output: None is falsy
print(None == False)  # Output: False (None is not False)

value = None
if some_condition:
    value = 42
print(value)  # Output: None (if some_condition is False)
```
None is commonly used to indicate that a variable or expression has no meaningful value, is undefined, or has not been assigned yet.

## Operators- data manipulation tools
```python
# Exponentiation
print(2**3) # Output 8
print(2**3.) # Output 8.0

# Division
print(6 / 3) # Output 2.0
print(6 / 3.) # Output 2.0

# Integer division (floor division)
print(6 // 3) # Output 2
print(6 // 3.) # Output 2.0
print(6 // 4) # Output 1
print(-6 // 4) # Output -2

# Remainder (modulo)
print(14 % 4) # Output 2
print(12 % 4.5) # Output is 3
```
* The result produced by the division operator is always a float
* Floor Division round up always goes to the lesser integer
* The result of modulo operator is a remainder left after the integer division.
### Operators bindings

```python
print(9 % 6 % 2)  # Output 1

print(2 ** 2 ** 3) # Output 256

print(-3 ** 2) # Output -9

print(-(3 ** 2)) # Output -9

print((-3) ** 2)  # Output 9

print(2 * 3 % 5) # Output 1
```

* The binding of the operator determines the order of computations performed by some operators with equal priority, put side by side in one expression.
* Most of Python's operators have left-sided binding, which means that the calculation of the expression is conducted from left to right.
* from left to right: first 9 % 6 gives 3, and then 3 % 2 gives 1;
* from right to left: first 6 % 2 gives 0, and then 9 % 0 causes a fatal error.
* the exponentiation operator uses right-sided binding.
* The exponentiation operator (**) has higher precedence than the unary negation operator (-)
* Both operators (* and %) have the same priority, so the result can be guessed only when you know the binding direction
* subexpressions in parentheses are always calculated first

## Variables

### Rules
* the name of the variable must be composed of upper-case or lower-case letters, digits, and the character _ (underscore)
* the name of the variable must begin with a letter;
* the underscore character is a letter;
* upper- and lower-case letters are treated as different;
* the name of the variable must not be any of Python's reserved words
* No spaces

The [PEP 8 -- Style Guide for Python Code](https://peps.python.org/pep-0008/) recommends the following naming convention for variables and functions in Python:

*  variable names should be lowercase, with words separated by underscores to improve readability (e.g., var, my_variable)
* function names follow the same convention as variable names (e.g., fun, my_function)
* it's also possible to use mixed case (e.g., myVariable), but only in contexts where that's already the prevailing style, to retain backward compatibility with the adopted convention.

### Reserved Keywords
```python
False    await    else     import   pass
None     break    except   in       raise
True     class    finally  is       return
and      continue for      lambda   try
as       def      from     nonlocal while
assert   del      global   not      with
async    elif     if       or       yield
```

### How to use variables
```python
var = "3.8.5"
print("Python version: " + var)

# output
Python version: 3.8.5

x *= 2 # x = x * 2
sheep += 1 # sheep = sheep + 1
i +=2 *j # i=i +2 *j
var /= 2 # var=var/2
rem %= 10 # rem=rem % 10
x **= 2 # x= x ** 2


a = 6
b = 3
a /= 2 * b
print(a)

# Output
1.0 # This expression is same as a = a / (2 * b)
```

## Comments
A remark inserted into the program, which is omitted at runtime, is called a comment.
If you'd like to quickly comment or uncomment multiple lines of code, select the line(s) you wish to modify and use the following keyboard shortcut: CTRL + / (Windows) or CMD + / (Mac OS). 

## Interaction with the user
```python
anything=input()
anything =input("Tell me about F1")

# Typecasting
anything = float(input("Enter a number: "))
anything = int(input("Enter a number: "))
```
* The result of input() function is a string

### String Operators
```python
name=input("Your name")
team=input("Enter your F1 team")
print("\n Your name is "+name+" "+" and your Team is "+team+" .")
```

### Replication
```python
print("Oscar" * 3)
print(3 * "F1")
print(5 * "2")

# Output
OscarOscarOscar
F1F1F1
22222
```
## Logics in Python
### Comparison: Equality Operator

```python
a==b
2==2 # Output True
2==2. # Output True

a!=b
2!=2 # Output False
2!=1 # Output True
```
* Equality Operator is a binary operator with left-sided binding. It needs two arguments and checks if they are equal.

*  The != (not equal to) operator compares the values of two operands, too. Here is the difference: if they are equal, the result of the comparison is False. If they are not equal, the result of the comparison is True.

### Conditions and conditional execution
```python
if true_or_false_condition:
    perform_if_condition_true
else:
    perform_if_condition_false

# Nested if-else statements
if the_weather_is_good:
    if nice_restaurant_is_found:
        have_lunch()
    else:
        eat_a_sandwich()
else:
    if tickets_are_available:
        go_to_the_theater()
    else:
        go_shopping()

# The elif statement
if the_weather_is_good:
    go_for_a_walk()
elif tickets_are_available:
    go_to_the_theater()
elif table_is_available:
    go_for_lunch()
else:
    play_chess_at_home()
# Note: else is an optional part of the cascade, and may be omitted;

# More Comprehensive form
if number1 > number2: larger_number = number1
else: larger_number = number2 
```
### Loops in Python
`if` performs its statements only once; `while` repeats the execution as long as the condition evaluates to `True`.
```python
while conditional_expression:
    instruction_one
    instruction_two 

# Example
counter = 5
while counter != 0:
    print("Inside the loop.", counter)
    counter -= 1
print("Outside the loop.", counter) 
```
#### Looping your code with `for`
The `for` loop is designed to do more complicated tasks – it can "browse" large collections of data item by item.
```python
 for i in range(100):
    # do_something()
    pass 

# Example
 for i in range(2, 8):
    print("The value of i is currently", i) 

# Example
for i in range(2, 8, 3):
    print("The value of i is currently", i)
# Output
The value of i is currently 2
The value of i is currently 5 
```

#### the `break` and `continue` statements
```python
# break - example

print("The break instruction:")
for i in range(1, 6):
    if i == 3:
        break
    print("Inside the loop.", i)
print("Outside the loop.")


# continue - example

print("\nThe continue instruction:")
for i in range(1, 6):
    if i == 3:
        continue
    print("Inside the loop.", i)
print("Outside the loop.")


# Output
The break instruction:

Inside the loop. 1

Inside the loop. 2

Outside the loop.

The continue instruction:

Inside the loop. 1

Inside the loop. 2

Inside the loop. 4

Inside the loop. 5

Outside the loop.
```
* `break` – exits the loop immediately, and unconditionally ends the loop's operation; the program begins to execute the nearest instruction after the loop's body;
* `continue` – behaves as if the program has suddenly reached the end of the body; the next turn is started and the condition expression is tested immediately.

### Logic and bit operations

#### Logical Operator
```python
counter > 0 and value==100 # and operator
counter > 0 or value==100 # or operator

# not operator
not (p and q) == (not p) or (not q)
not (p or q) == (not p) and (not q) 
```
#### Bitwise operators
There are four operators that allow you to manipulate single bits of data. They are called bitwise operators.

    & (ampersand) ‒ bitwise conjunction;
    | (bar) ‒ bitwise disjunction;
    ~ (tilde) ‒ bitwise negation;
    ^ (caret) ‒ bitwise exclusive or (xor).
    <<(Left Shift) - Shifts all bits of a number to the left by a specified number of positions, filling the right with 0s.
    >> (Right Shift) - Shifts all bits to the right by a specified number of positions, filling the left with the sign bit (for signed integers, typically 0 for positive numbers).

* The arguements of these operators must be integers
* the logical operators do not penetrate into the bit level of its argument. They're only interested in the final integer value.
* Bitwise operators are stricter: they deal with every bit separately. 

```python
# Example
i = 15 # Assume 32 bit representation 00000000000000000000000000001111 
j= 22 # Assume 32 bit representation 00000000000000000000000000010110

log= i and j # Output: True
bit= i & j # Output: 00000000000000000000000000000110
```
## Lists
Elements in a list are always numbered starting from zero.

```python
numbers = [10, 5, 7, 2, 1] 

# Indexing a list
numbers[0] = 111  
print("New list contents: ", numbers) # Output: New list contents: [111, 5, 7, 2, 1] 

# Accessing the list's first element. 
print(numbers[0]) # output: 111

# Length of list
print("\nList length:", len(numbers)) # Output: List length: 5

# Removing elements from a list
del numbers[1]

# Negative Indices are legal
numbers = [111, 7, 2, 1]
print(numbers[-1]) # Output: 1
```
Brief refresher on Method vs Function
```python
result = function(arg) 
result = data.method(arg) 
```
A method is owned by the data it works for, while a function is owned by the whole code.

```python
list.append(value) # Add at the end of list
list.insert(location, value) # Insert at any place in the list

#Example 1
my_list = []  # Creating an empty list.

for i in range(5):
    my_list.append(i + 1)

print(my_list)

# Output: [1, 2, 3, 4, 5]

# Example 2
my_list = []  # Creating an empty list.
 
for i in range(5):
    my_list.insert(0, i + 1)
 
print(my_list) 

# Output [5, 4, 3, 2, 1]
```
### Intresting use case of `for` in list
```python
# Conventional way
my_list = [10, 1, 8, 3, 5]
total = 0

for i in range(len(my_list)):
    total += my_list[i]

print(total)

# Output 27

# Direct Iteration Over Elements
 my_list = [10, 1, 8, 3, 5]
total = 0
 
for i in my_list:
    total += i
 
print(total) 

# Output 27
# The loop iterates directly over the elements of my_list (10, 1, 8, 3, 5).
```
### Lists in action
```python
variable_1 = 1
variable_2 = 2
 
auxiliary = variable_1
variable_1 = variable_2
variable_2 = auxiliary  

# Enters Python and says -"Hold my beer!!"
variable_1 = 1
variable_2 = 2
 
variable_1, variable_2 = variable_2, variable_1 

# Example
my_list = [10, 1, 8, 3, 5]
 
my_list[0], my_list[4] = my_list[4], my_list[0]
my_list[1], my_list[3] = my_list[3], my_list[1]
 
print(my_list) 

# Output [5, 3, 8, 1, 10] 

# Python way
for i in range(length // 2):
    my_list[i], my_list[length - i - 1] = my_list[length - i - 1], my_list[i]

print(my_list)
```

#### Bubble sort




