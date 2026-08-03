---
layout: post
title: "Let me C - Types, Operators, Expressions and Control Flow"
date: 2026-08-02
categories: C-PROGRAMMING
tags: embedded-programming c-programming
image:
    path: assets/img/post/let-me-c/control-flow/control_flow_bullet_thumbnail.svg
---

## Variable names
The conventions (not enforced, but expected)
* Underscore-leading names are conventionally avoided in your own code. Names starting with `_` are often used by library implementations for internal names (like `_iob` in old standard library headers).
* Lowercase for variables, and reserve all-uppercase for symbolic constants (things defined via #define)

```c
#define MAXLINE 1000
int lineCount;
```
## Data Type and Sizes

| Type | Meaning |
|------|---------|
| `char` | a single byte, holds one character in the local character set |
| `int` | an integer, typically reflects the "natural" size of integers on the host machine |
| `float` | single-precision floating point |
| `double` | double-precision floating point |

**Qualifiers**

You can modify int (and in older code, sometimes implicitly drop the word int itself) with:
```c
short int x;      // often written just "short x;"
long int count;   // often written just "long count;"
unsigned int u;
unsigned long bignum;
```
**Example**
```c
#include <stdio.h>

int main(void)
{
    printf("char: %zu bytes\n", sizeof(char));
    printf("short: %zu bytes\n", sizeof(short));
    printf("int: %zu bytes\n", sizeof(int));
    printf("long: %zu bytes\n", sizeof(long));
    printf("float: %zu bytes\n", sizeof(float));
    printf("double: %zu bytes\n", sizeof(double));
    return 0;
}
```
```bash
# Output
char: 1 bytes
short: 2 bytes
int: 4 bytes
long: 8 bytes
float: 4 bytes
double: 8 bytes
```
Note:
| Specifier | Expects |
|-----------|---------|
| `%d` | `int` |
| `%u` | `unsigned int` |
| `%ld` | `long` |
| `%lu` | `unsigned long` |
| `%zu` | `size_t` (unsigned) |

## Constants

Suffixes for type
```c
long int y = 123456789L;   // 'L' or 'l' suffix = long
unsigned int u = 42U;      // 'U' or 'u' suffix = unsigned
unsigned long ul = 42UL;   // both combined
```
Octal
```c
int x = 010;   // this is octal 10 = decimal 8, NOT ten!
```
Hexadecimal
```c
int x = 0x1A;   // hex 1A = decimal 26
```
Character Constant
```c
char c = 'A';   // 'A' is really just the integer 65 (in ASCII)
```

| Escape Sequence | Meaning |
|------------------|---------|
| `\a` | Alert (bell) character |
| `\b` | Backspace |
| `\f` | Formfeed |
| `\n` | Newline |
| `\r` | Carriage return |
| `\t` | Horizontal tab |
| `\v` | Vertical tab |
| `\\` | Backslash |
| `\?` | Question mark |
| `\'` | Single quote |
| `\"` | Double quote |
| `\ooo` | Octal number (1 to 3 digits: 0-7) |
| `\xhh` | Hexadecimal number (one or more digits) |

**Enumeration**

It is a way to declare a list of named integers constants.

```c
/* Automatic values starting from 0 */
enum boolean { NO, YES };

/* Explicit values */
enum escapes { 
    BELL = '\a', 
    BACKSPACE = '\b', 
    TAB = '\t', 
    NEWLINE = '\n' 
};

/* Starting from 1 */
enum months { JAN = 1, FEB, MAR, APR, MAY, JUN,
              JUL, AUG, SEP, OCT, NOV, DEC };
```

Here is a gotcha:
```c
enum colors { RED, GREEN = 5, BLUE };
```
> Note: Value of Red is 0, Green is % and Blue is 6

Use case:
```c
#include <stdio.h>

/* Declare an enumeration for days of the week.
   SUN starts at 0 by default, and each subsequent
   name auto-increments by 1. */
enum weekday { SUN, MON, TUE, WED, THU, FRI, SAT };

int main(void)
{
    enum weekday today;   // declare a variable of type "enum weekday"

    today = WED;          // assign it one of the enum constants

    printf("Today's numeric value is: %d\n", today);

    if (today == SAT || today == SUN)
        printf("It's the weekend!\n");
    else
        printf("It's a weekday.\n");

    return 0;
}
```
```bash
# Output
Today's numeric value is: 3
It's a weekday.
```

## Type Conversion

In C's usual arithmetic conversions, when two operands of different types are used in an expression, the "lower" type gets converted up to the "higher" type before the operation happens — conversion works its way up this ladder.

| Priority | Type |
|----------|------|
| 1 (highest) | `long double` |
| 2 | `double` |
| 3 | `float` |
| 4 | `unsigned long` |
| 5 | `long` |
| 6 | `unsigned int` |
| 7 (lowest) | `int` |

**Explicit Conversion : The Cast Operator**

Let's look at this example:
```c
int sum = 7;
int count = 2;

float average = sum / count;   // What do you expect this to print?
```
Even though average is a float, the expression sum / count is computed entirely in integer arithmetic first (since both operands are int), and integer division truncates any fractional part. So sum / count evaluates to 3 (not 3.5), and then that 3 gets converted to 3.0 when stored into average. The fractional information is already lost by the time the float conversion happens.

> The Fix:

```c
float average = (float) sum / count;
```
Here, (float) sum explicitly converts sum to a float first. Now, since one operand of / is a float, the usual arithmetic conversion rule kicks in and promotes count to float as well — so the division itself happens in floating-point, giving 3.5, and that is what's stored in average.

## Increment and Decrement Operators

* ++n or --n (Prefix) Change happens before the value is used
* n++ or n--(Postfix) Change happens after the value is used

```c
#include <stdio.h>

main() {
    int n = 5;

    printf("n++   = %d\n", n++);   /* prints 5, then n becomes 6 */
    printf("++n   = %d\n", ++n);   /* n becomes 7, then prints 7 */
    printf("n     = %d\n", n);     /* final value of n */
}
```
## Bitwise Operator

| Operator | Name |
|----------|------|
| `&` | bitwise AND |
| `\|` | bitwise OR |
| `^` | bitwise XOR (exclusive or) |
| `<<` | left shift |
| `>>` | right shift |
| `~` | one's complement (bitwise NOT) |


## Assignment Operator

| Operator | Meaning |
|----------|---------|
| `+=` | `x = x + y` |
| `-=` | `x = x - y` |
| `*=` | `x = x * y` |
| `/=` | `x = x / y` |
| `%=` | `x = x % y` |
| `<<=` | `x = x << y` |
| `>>=` | `x = x >> y` |
| `&=` | `x = x & y` |
| `^=` | `x = x ^ y` |
| `\|=` | `x = x \| y` |

## Conditional Operator

```c
/* Ternary Operator */

expr1 ? expr2 : expr3
```
How it works: expr1 is evaluated first as a condition. If it's true (nonzero), the whole expression evaluates to expr2. If it's false (zero), the whole expression evaluates to expr3. Only one of expr2 or expr3 is ever actually evaluated - not both.

**Without conditional Operator**
```c
int a = 10, b;

if (a >= 0)
    b = a;
else
    b = -a;
```
**With Conditional Operator**
```c
int b = (a >= 0) ? a : -a;
```
## Precedence and Order of Evaluation

> When in doubt use paranthesis.

| Operators | Associativity |
|-----------|----------------|
| `() [] -> .` | left to right |
| `! ~ ++ -- + - * (type) sizeof` (unary) | right to left |
| `* / %` | left to right |
| `+ -` | left to right |
| `<< >>` | left to right |
| `< <= > >=` | left to right |
| `== !=` | left to right |
| `&` | left to right |
| `^` | left to right |
| `\|` | left to right |
| `&&` | left to right |
| `\|\|` | left to right |
| `?:` | right to left |
| `= += -= *= /= %= &= ^= \|= <<= >>=` | right to left |
| `,` (comma operator) | left to right |


# Control Flow

## 1. If-Else
```c
if (expression)
    statement1
else
    statement2
```
## 2. Else-If
```c
if (expression1)
    statement1
else if (expression2)
    statement2
else if (expression3)
    statement3
else
    statement4
```
## 3. Swtich
```c
switch (expression) {
    case const1:
        statements
        break;
    case const2:
        statements
        break;
    default:
        statements
}
```
## 4. Loops- While and For
```c
initialization;
while (condition) {
    statement
    increment;
}
```
```c
for (initialization; condition; increment)
    statement
```
Example:
```c
int count = 0;

while (count < 5) {
    printf("%d\n", count);
    count++;
}
```
```c
for (int count = 0; count < 5; count++) {
    printf("%d\n", count);
}
```
## 5. Loop- Do-while

```c
do
    statement
while (expression);
```
Notice the `;` at the end this time - unlike while and for, the do-while loop is terminated with a semicolon, since the loop effectively ends on the while (expression) line.

The key difference: with while and for, the condition is checked before the body runs - so the body might execute zero times. With do-while, the condition is checked after the body runs - so the body always executes at least once, no matter what.

## 6. Break and Continue

```c
for (int i = 0; i < 10; i++) {
    if (i == 5)
        break;          // exit the loop entirely
    printf("%d\n", i);
}
// prints 0 1 2 3 4, then stops completely
```
```c
for (int i = 0; i < 5; i++) {
    if (i == 2)
        continue;       // skip printing for i == 2, but keep looping
    printf("%d\n", i);
}
// prints 0 1 3 4  (2 is skipped, loop still runs to completion)
```
## 7. Goto and Labels
```c
goto label;
...
label: statement
```
Example:
```c
int i = 0;

loop:
    if (i >= 5)
        goto done;
    printf("%d\n", i);
    i++;
    goto loop;

done:
    printf("finished\n");
```
