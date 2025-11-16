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

## Bye World
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
```python
my_list = [8, 10, 6, 2, 4]  # list to sort
swapped = True  # It's a little fake, we need it to enter the while loop.

while swapped:
    swapped = False  # no swaps so far
    for i in range(len(my_list) - 1):
        if my_list[i] > my_list[i + 1]:
            swapped = True  # a swap occurred!
            my_list[i], my_list[i + 1] = my_list[i + 1], my_list[i]

print(my_list)

# ready to use tool
my_list = [8, 10, 6, 2, 4]
my_list.sort()
my_reverse_list.reverse()

# Example
 a = "A"
b = "B"
c = "C"
d = " "
 
lst = [a, b, c, d]
lst.reverse()
 
print(lst) # Output: [' ', 'C', 'B', 'A']
```
### Nature of Lists

```python
list_1 = [1]
list_2 = list_1
list_1[0] = 2
print(list_2)
# Output [2]
```
* List is mutable and reference-based. It's like giving two people (list_1 and list_2) directions to the same house—if you repaint the house (modify the list), both people see the new color.
* 

#### The Slicing Syntax: Your Swiss Army Knife
```python
my_list[start:stop:step] # General form
my_list[start:end]
```
* start: Inclusive index to begin (default: 0).
* stop: Exclusive index to end (default: len(list)).
* step: Increment (default: 1; -1 reverses, 2 skips every other).
* Shortcuts: [:] = full copy; [2:] = from 2 to end; [:-1] = all but last; [::-1] = reverse. Negative indices count from the end (-1 = last item).

```python
# Example
cart = ['milk', 'bread', 'eggs', 'coffee', 'bananas', 'tea']

# From start to middle (first 3)
breakfast = cart[0:3]
print(breakfast)          # Output: ['milk', 'bread', 'eggs']

# End chunk (last 3)
snacks = cart[3:]
print(snacks)             # Output: ['coffee', 'bananas', 'tea']

# Every other (step=2, evens vibe)
alternates = cart[::2]
print(alternates)         # Output: ['milk', 'eggs', 'bananas']

# Reverse for "unshopping" list
reverse_cart = cart[::-1]
print(reverse_cart)       # Output: ['tea', 'bananas', 'coffee', 'eggs', 'bread', 'milk']

# Negative: Last 2 (from -2 to end)
final_two = cart[-2:]
print(final_two)          # Output: ['bananas', 'tea']
```
```python
# del() + Slicing

fruits = ['apple', 'banana', 'cherry', 'date', 'elderberry']
print(fruits)  # Output: ['apple', 'banana', 'cherry', 'date', 'elderberry']

del fruits[1:3]  # Delete indices 1 to <3 → bye 'banana' & 'cherry'
print(fruits)    # Output: ['apple', 'date', 'elderberry']  (Original mutated!)

tasks = ['code', 'test', 'debug', 'deploy']
print(tasks)  # ['code', 'test', 'debug', 'deploy']

del tasks[2:]   # From 2 to end → delete 'debug' & 'deploy'
print(tasks)    # ['code', 'test']

del tasks[:]    # Full slice delete → empty list
print(tasks)    # []  (All gone—careful!)

numbers = [1, 2, 3, 4, 5, 6]
print(numbers)  # [1, 2, 3, 4, 5, 6]

del numbers[::2]  # Every other from start → delete 1,3,5
print(numbers)    # [2, 4, 6]

# Reset & reverse delete (last 3, but step -1 doesn't change much for del)
numbers = [1, 2, 3, 4, 5, 6]
del numbers[-3::-1]  # Tricky: from -3 (4) backwards to start (but del slices forward-ish)
print(numbers)       # Actually deletes [4,5,6] effectively → [1,2,3]  (Test it—slices with neg step can flip!)
```
#### The *in* and *not in* operators

```python
cart = ['apple', 'banana', 'cherry']
print('apple' in cart)      # Output: True
print('date' in cart)       # Output: False
print('dragonfruit' not in cart)  # Output: True
```

#### Different style of accessing list by position or value
```python
# Ex1 style
my_list = [5, 12, 3, 18, 7]
largest = my_list[0]  # 5
for i in range(1, len(my_list)):
    if my_list[i] > largest:
        print(f"Update! {my_list[i]} beats {largest} at pos {i}")
        largest = my_list[i]
print(largest)

# Outputs: 
# Update! 12 beats 5 at pos 1
# Update! 18 beats 12 at pos 3
# 18

# Ex2: Same, but i=values, no pos tracking
largest = my_list[0]
for i in my_list:
    if i > largest:
        print(f"Update! {i} beats {largest}")
        largest = i
print(largest)

#Outputs:
# Update! 12 beats 5
# Update! 18 beats 12
# 18

# Ex3: Skips first entirely
largest = my_list[0]
for i in my_list[1:]:
    if i > largest:
        print(f"Update! {i} beats {largest}")
        largest = i
print(largest) 

# Output
# Update! 12 beats 5
# Update! 18 beats 12
# 18
```
### List in advanced applications
 List comprehension allows you to create new lists from existing ones in a concise and elegant way. The syntax of a list comprehension looks as follows:

```python
[expression for element in list if conditional]
 # which is actually an equivalent of the following code:

for element in list:
    if conditional:
        expression 
```

#### List Comprehension

```python
# Loop version: Conditional append
row_chess = []
for i in range(8):
    if i % 2 == 0:
        row_chess.append('WHITE_PAWN')
    else:
        row_chess.append('BLACK_PAWN')
print(row_chess)  # ['WHITE_PAWN', 'BLACK_PAWN', 'WHITE_PAWN', ...] (alternating)

# Comp version: Ternary for condition (fancy!)
row2_chess = ['WHITE_PAWN' if i % 2 == 0 else 'BLACK_PAWN' for i in range(8)]
print(row2_chess)  # Same alternating output—boom, one-liner!

# Bonus: Use i for numbering (no dummy var)
numbered = [f"Pawn {i}" for i in range(8)]
print(numbered)  # ['Pawn 0', 'Pawn 1', ..., 'Pawn 7']
```
#### 2D Arrays
```python
board = []  # Line 1: Empty "board" list (will hold 8 rows)

for i in range(8):  # Line 2: Outer loop—8 rows (i=0 to 7)
    row = ['EMPTY' for i in range(8)]  # Line 3: Inner comp—8 'EMPTY's per row (but 'i' shadows outer i—harmless here, but confusing!)
    board.append(row)  # Line 4: Add this row to board

print(board)  # Line 5: Dump the 2D list

board2 = [['EMPTY' for i in range(8)] for j in range(8)]  # Line 6: Nested comp—outer j for rows, inner i for cols (cleaner vars!)
print(board2)  # Line 7: Print the quick board
```
#### Multi-D Arrays
```python
# Example 1: Loop Build
garage = []  # Empty 3D: list of 2D floors per level

# Outer: 2 
for level in range(2):
    floor_plan = []  # Inner 2D for this level
    # Middle: 2 floors per level
    for floor in range(2):
        spot_row = ['EMPTY' for spot in range(3)]  # Inner: 3 spots per row
        floor_plan.append(spot_row)
    garage.append(floor_plan)  # Add full level to garage

print(garage)  # [[[EMPTY x3], [EMPTY x3]], [[EMPTY x3], [EMPTY x3]]]
print(garage[0][1][2])  # Level 0, floor 1, spot 2: 'EMPTY'



#Example 2 : Loop Comprehension
garage2 = [[['EMPTY' for spot in range(3)] for floor in range(2)] for level in range(2)]
print(garage2)  # Matches the loop output—quick and clean!

# Access & mutate: "Park" a 'CAR' in level 1, floor 0, spot 1
garage2[1][0][1] = 'CAR'
print(garage2[1][0])  # ['EMPTY', 'CAR', 'EMPTY'] (that floor updated)
```
## Functions
```python
def your_function(optional parameters):
    # the body of the function 
```
There are at least four basic types of functions in Python:
* built-in functions which are an integral part of Python (such as the print() function). You can see a complete list of built-in Python functions at https://docs.python.org/3/library/functions.html.
* the ones that come from pre-installed modules
* user-defined functions which are written by users for users ‒ you can write your own functions and use them freely in your code,
* the lambda functions

```python
def hello(name):    # defining a function
    print("Hello,", name)    # body of the function


name = input("Enter your name: ")

hello(name)    # calling the function
```
### Parameterized functions

```python
def fancy_greet(name, mood="happy", emoji="😊"):
    print(f"Hello, {name}! You're feeling {mood} today. {emoji}")

# Pure positional (order matters!)
fancy_greet("Alex", "excited")  # name="Alex", mood="excited", emoji=default 😊
# Output: Hello, Alex! You're feeling excited today. 😊

# Mix: Positional for name, keyword for mood (skips to it)
fancy_greet("Jordan", mood="chill")  # name="Jordan" (pos), mood="chill" (key), emoji=default
# Output: Hello, Jordan! You're feeling chill today. 😊

# Full mix: Positional name, keywords for the rest (order of keywords doesn't matter!)
fancy_greet("Taylor", emoji="🚀", mood="motivated")
# Output: Hello, Taylor! You're feeling motivated today. 🚀

# All keywords (no positionals—totally fine!)
fancy_greet(name="Casey", mood="grumpy", emoji="😴")
# Output: Hello, Casey! You're feeling grumpy today. 😴
```
The Golden Rule for Mixing
* Positional args must come before keyword args in the call. (Python reads left-to-right.)
* No duplicates: Can't specify the same param twice.
* All positional args must match the function's param order up to that point.
* Keywords can be in any order after positionals, as long as they're defined.

Common Pitfalls
* Error Example:`fancy_greet(mood="sad", "Sam")` → SyntaxError! Keywords can't come before positionals. Fix:`fancy_greet("Sam", mood="sad")`.
* Duplicate:`fancy_greet("Pat", mood="joyful", mood="meh")` → SyntaxError. Fix: Pick one!
* Out-of-Order Pos: If you have 3 params, `func(1, 3, 2)` assumes a=1, b=3, c=2—not what you want if order matters.

### Returning a result from a function
```python
def happy_new_year(wishes=True):  # Default: wishes=True
    print("Three...")
    print("Two...")
    print("One...")
    if not wishes:  # If wishes is False, skip the celebration
        return  # Early exit: returns None, skips the print below
    print("Happy New Year!")  # Only runs if wishes is True

# Test calls (mixing that positional/keyword from last time!)
happy_new_year()  # Default: wishes=True
# Output:
# Three...
# Two...
# One...
# Happy New Year!

print("---")  # Separator for clarity

happy_new_year(wishes=False)  # Keyword override: no wishes
# Output:
# Three...
# Two...
# One...
# (No "Happy New Year!"—it bailed early)
```
What Does `return` Do?

* Exits the function immediately: Anything after return in that code path? Skipped!
* Returns a value (optional): Can be a number, string, list, etc.—or nothing (just return alone, which returns None).
* Why use it? Control flow (early outs), compute results for reuse, keep code clean.
* Forgetting return: Function runs but gives None. Always ask: "What should this give back?"

<!-- markdownlint-disable-next-line -->
>  Implicit Return value :
{: .prompt-info }
```python
def strange_function(n):
    if(n % 2 == 0):
        return True
print(strange_function(2)) # Output : True
print(strange_function(1)) # Output : None
```
```python
def is_even(n):
    """
    Returns True if n is even, False otherwise.
    Uses modulo for the check—fast and simple!
    """
    if n % 2 == 0:
        return True
    return False  # Explicit for odds—avoids implicit None

# Or, one-liner style (shorter, same result):
def is_even_short(n):
    return n % 2 == 0  # Returns True/False directly—no if needed!

# Test 'em
print(is_even(2))      # True
print(is_even(1))      # False
print(is_even_short(4)) # True
print(is_even_short(3)) # False
```
### Effects and results: lists and functions
```python
def strange_list_fun(n):
    strange_list = []
    
    for i in range(0, n):
        strange_list.insert(0, i)
    
    return strange_list

print(strange_list_fun(5))
# Output [4, 3, 2, 1, 0]
```
### Functions and scopes
```python
def my_function():
    var = 2
    print("Do I know that variable?", var)
var = 1
my_function()
print(var)

# Outout
# Do I know that variable? 2
# 1 
```
```python
def my_function():
    global var
    var = 2
    print("Do I know that variable?", var)
var = 1
my_function()
print(var)

# Outout
# Do I know that variable? 2
# 2 
```
```python
def my_function(n):
    print("I got", n)
    n += 1
    print("I have", n)
var = 1
my_function(var)
print(var)

# Output
# I got 1
# I have 2
# 1
```
```python
def my_function(my_list_1):
    print("Print #1:", my_list_1)
    print("Print #2:", my_list_2)
    del my_list_1[0]  # Pay attention to this line.
    print("Print #3:", my_list_1)
    print("Print #4:", my_list_2)


my_list_2 = [2, 3]
my_function(my_list_2)
print("Print #5:", my_list_2)

# Output
# Print #1: [2, 3]
# Print #2: [2, 3]
# Print #3: [3]
# Print #4: [3]
# Print #5: [3] 
```
### Recursive Function
When a function calls itself, this situation is known as `recursion`, and the function which calls itself and contains a specified termination condition (i.e., the base case − a condition which doesn't tell the function to make any further calls to that function) is called a `recursive` function.

```python
def factorial(n):
    if n <= 1:  # Base case: 0! or 1! = 1 (stops the chain)
        return 1
    return n * factorial(n - 1)  # Recursive: Call self with smaller n

# Test it
print(factorial(5))  # Output: 120
print(factorial(0))  # Output: 1 (base!)
print(factorial(3))  # Output: 6 (3*2*1)
```
```python
def factorial(n):
    return n * factorial(n - 1)
print(factorial(4))
# Output: RecursionError: maximum recursion depth exceeded
```

```python
def countdown(n):
    if n == 0:  # Base
        print("Blast off!")
        return
    print(f"T minus {n}")
    countdown(n - 1)  # Recursive

# Test
countdown(3)
# Output:
# T minus 3
# T minus 2
# T minus 1
# Blast off!
```
## Tuples
Tuples like lists but immutable (can't change after creation—no appends, no deletes, no drama).
* Tuples are ordered and unchangeable (immutable) collections of data. They can be thought of as immutable lists. They are written in round brackets.
* If you remove the comma, you will tell Python to create a variable, not a tuple.
* 

```python
tuple_1 = (1, 2, 4, 8)
tuple_2 = 1., .5, .25, .125
print(tuple_1)
print(tuple_2)

# Output 
# (1, 2, 4, 8)
# (1.0, 0.5, 0.25, 0.125) 
```


```python
my_tuple = (1, "hello", 3.14, True)  # Mixed types—flexible!
short_tuple = (42,)  # Singleton: Trailing comma!
empty_tuple = ()

print(my_tuple)         # Output: (1, 'hello', 3.14, True)
print(type(my_tuple))   # <class 'tuple'>
print(len(my_tuple))    # 4

print(my_tuple[0])      # 1 (first item)
print(my_tuple[1:3])    # ('hello', 3.14) — slice returns a new tuple!
print(my_tuple[-1])     # True (last)
```
### Unpacking Tuples
```python
coords = (10, 20)  # Point (x, y)
x, y = coords      # Unpack: x=10, y=20
print(f"Point at ({x}, {y})")  # Output: Point at (10, 20)

# Multi-unpack
a, b, *rest = my_tuple  # a=1, b='hello', rest=[3.14, True] (star grabs extras)
print(rest)             # [3.14, True]
```
### Immutability: The Superpower (And Limitation)
```python
my_tuple = (1, 2, 3)

# This fails!
# my_tuple[0] = 99  # TypeError: 'tuple' object does not support item assignment

# Workaround: Reassign the whole tuple
my_tuple = (99, 2, 3)  # New tuple—old one garbage collected
print(my_tuple)        # (99, 2, 3)
```
### Tie to Functions
```python
def get_stats(numbers):  # Say, from a list [1,2,3]
    return (sum(numbers), len(numbers), sum(numbers)/len(numbers))  # Tuple of sum, count, avg

stats = get_stats([1, 2, 3])
total, count, avg = stats  # Unpack!
print(f"Total: {total}, Avg: {avg}")  # Total: 6, Avg: 2.0
```
## Dictionaries

What is a Dictionary?

* Definition: Mutable, hashable-key collection: key: value pairs. Keys must be immutable (strings, ints, tuples—not lists!).
* Syntax:{key1: value1, key2: value2}. Empty: {}.
* Why? O(1) average lookups (faster than list searching). No duplicates keys—last one wins.
* From Tuples/Lists: Like a list of tuples [(k,v)], but dicts handle the mapping magic.
* keys are case-sensitive: 'Suzy' is something different from 'suzy'.
* To remove the last item in a dictionary, you can use the popitem() method
* To check if a given key exists in a dictionary, you can use the in keyword
* You can also insert an item into a dictionary by using the update() method, and remove the last element by using the popitem() method
* You can use the del keyword to remove a specific item, or delete a dictionary. To remove all the dictionary's items, you need to use the clear() method
* To copy a dictionary, use the copy() method
* 
```python
# Creation
person = {"name": "Alice", "age": 30, "city": "Toronto"}  # Strings as keys
scores = {1: "A", 2: "B", 3: "C"}  # Int keys (like indices, but sparse)
mixed = {("x", "y"): [10, 20], "z": 99}  # Tuple key! (Immutable = hashable)

empty_dict = {}
print(type(person))  # <class 'dict'>
print(len(person))   # 3 (number of pairs)
```
### Accessing dictionary
```python
print(person["name"])      # Alice
print(person.get("hobby", "None"))  # None (safe miss)

for k, v in person.items():  # Loop like unpacked tuples
    print(f"{k}: {v}")
# Output:
# name: Alice
# age: 30
# city: Toronto
```
### Modifying Dictionary
Add, update, delete—like lists but by key.
```python
person["job"] = "Coder"  # Add/update
person["age"] += 1       # Update: 31

del person["city"]       # Remove key (or pop("city"))
print(person)            # {'name': 'Alice', 'age': 31, 'job': 'Coder'}

# Merge: Update from another dict
defaults = {"hobby": "Reading"}
person.update(defaults)
print(person)            # Adds hobby
```
### Tie to Functions
```python
def get_person_info(name, age):
    return {"name": name, "age": age, "status": "Active"}  # Dict return!

info = get_person_info("Bob", 25)
name, age = info["name"], info["age"]  # Or unpack: name, age, _ = info.values() (kinda)
print(f"{name} is {age}")  # Bob is 25
```
```python
# Keys() method
""" The method returns an iterable object consisting of all the keys gathered within the dictionary."""

dictionary = {"cat": "chat", "dog": "chien", "horse": "cheval"}

for key in dictionary.keys():
    print(key, "->", dictionary[key])

#Output
# horse -> cheval
# dog -> chien
# cat -> chat
```
```python
# items() method
""" The method returns tuples where each tuple is a key-value pair."""

dictionary = {"cat": "chat", "dog": "chien", "horse": "cheval"}

for english, french in dictionary.items():
    print(english, "->", french)

# Output
# cat -> chat
# dog -> chien
# horse -> cheval
```

## Exceptions

