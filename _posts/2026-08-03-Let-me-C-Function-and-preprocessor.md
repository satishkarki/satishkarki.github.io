---
layout: post
title: "Let me C : Functions and Preprocessor"
date: 2026-08-03
categories: C-PROGRAMMING
tags: embedded-programming c-programming
image:
    path: assets/img/post/let-me-c/function-preprocessor/function-preprocessor.png

---

Here I will introduce ideas like scope, separate compilation, and the preprocessor.

Basic Structure of function:
```c
return-type function-name(parameter declarations)
{
    declarations
    statements
}
```
## Functions returning non-integers
If a function is used before its definition or declaration appears, and nothing tells the compiler otherwise, C (in the old K&R style) assumed it returned `int`. If your function actually returns a `double`, that assumption silently produces garbage - the bit pattern of a `double` gets misinterpreted as an `int`.

Fix:
```c
#include <stdio.h>

double average(double a, double b);   // <-- prototype: promise of what's to come

int main(void)
{
    double result = average(4.0, 7.0);
    printf("Average: %.2f\n", result);
    return 0;
}

double average(double a, double b)    // <-- actual definition
{
    return (a + b) / 2;
}
```
## External Variables
```c
#include <stdio.h>

int counter = 0;   // external variable — declared outside any function

void increment(void)
{
    counter++;      // no need to pass counter in — it's visible here
}

void print_counter(void)
{
    printf("Counter is: %d\n", counter);
}

int main(void)
{
    increment();
    increment();
    increment();
    print_counter();   // prints "Counter is: 3"
    return 0;
}
```
Now lets look at another example here:
```c
int count = 10;   // external variable, count = 10, visible file-wide

void reset(void)
{
    int count = 0;   // LOCAL variable — shadows the external one!
    printf("Inside reset: count = %d\n", count);  // uses the LOCAL count
}

int main(void)
{
    reset();
    printf("Inside main: count = %d\n", count);   // uses the EXTERNAL count
    return 0;
}
```

**Shadowing**: Inside `reset()`, the line `int count = 0;` creates a brand-new local variable that happens to share the same name as the external one. C's scoping rule is: the innermost declaration wins. So inside `reset()`, every reference to count refers to the local `count = 0` - the external count is temporarily "shadowed" and completely inaccessible from within that function.

Once `reset()` returns, that local count is destroyed - it never touched the external count at all. So back in `main()`, count still refers to the external variable, untouched at 10.

## Scope Rules

**Automatic Variables**
```c
int main(void)
{
    int x = 5;         // automatic — scope: within main's braces only
    if (x > 0) {
        int y = 10;     // automatic — scope: within the if-block ONLY
        printf("%d\n", y);
    }
    // y is NOT visible here — it's out of scope
    return 0;
}
```
**External Variables**
```c
int main(void)
{
    // x is NOT visible here yet!
    return 0;
}

int x = 100;   // x's scope begins HERE, extends to end of file
```
**extern**
```c
/* helper.c */
int total = 0;

void add_to_total(int n)
{
    total += n;
}
```
```c
#include <stdio.h>

extern int total;              // declares the variable defined in helper.c
void add_to_total(int n);      // prototype for the function in helper.c

int main(void)
{
    add_to_total(5);
    add_to_total(10);
    printf("Total: %d\n", total);   // prints "Total: 15"
    return 0;
}
```
>Key Concept
* Block scope is strict in C — unlike Python, a variable declared inside an if or for block is genuinely invisible outside it. Braces wall things off.
* Top-to-bottom visibility — an external variable's scope starts at its declaration line, not automatically from the top of the file. Using it earlier fails to compile.
* extern solves the ordering/multi-file problem — it's a declaration (announces type, no storage allocated), not a definition (which actually allocates memory and happens exactly once). This lets you:

    * Use a variable in main() before its real definition appears later in the file.
    * Share a variable across multiple .c files — defined once in one file, declared extern wherever else it's needed.


* Shadowing — a local variable can share a name with an external one; inside that block, the local one wins and the external one is temporarily hidden.

## Header Files
A header file (.h) collects related declarations - extern variables, function prototypes, macros, type definitions - in one place. Any .c file that needs them just does `#include "yourheader.h"`, and the preprocessor literally pastes that file's contents in at that point, before real compilation even begins.

Python analogy: This is loosely like import mymodule - except C's `#include` is far more primitive. It's not a smart module system; it's literally a textual copy-paste performed by the preprocessor. There's no namespacing, no "module object" - just raw text substitution

```c
/* helper.h */
extern int total;              // declaration, not definition
void add_to_total(int n);      // prototype
```
```c
/* helper.c */
#include "helper.h"    // pulls in declarations for consistency-checking

int total = 0;         // the one true DEFINITION

void add_to_total(int n)
{
    total += n;
}
```
```c
/* main.c */
#include <stdio.h>
#include "helper.h"    // same declarations, now shared/reused

int main(void)
{
    add_to_total(5);
    add_to_total(10);
    printf("Total: %d\n", total);
    return 0;
}
```

## Static Variables

1. static on a variable inside a function - persistence across calls.
    ```c
    #include <stdio.h>

    void counter(void)
    {
        static int calls = 0;   // initialized ONCE, ever
        calls++;
        printf("Called %d times\n", calls);
    }

    int main(void)
    {
        counter();   // "Called 1 times"
        counter();   // "Called 2 times"
        counter();   // "Called 3 times"
        return 0;
    }
    ```
    Normally, automatic (local) variables are destroyed when the function returns and recreated fresh next time. `static` changes that: the variable is initialized once, and then retains its value between function calls - while still only being visible inside that function (scope unchanged).

2. static on an external variable or function - restricting visibility to one file.
    ```c
    /* helper.c */
    static int secret = 42;   // ONLY visible within helper.c

    static void internal_helper(void)   // ONLY callable within helper.c
    {
        // ...
    }
    ```
    Here `static` means something almost opposite: instead of persistence, it's about hiding something from other files. Normally, external variables/functions are visible to any file that `extern`-declares them. Marking one `static` at file scope makes it private to that `.c` file only — no other file can link to it, even with `extern`.

## Register Variables
`register` is a storage class, just like `static` and `extern`, but its purpose is different: it's a hint to the compiler that a variable will be used very heavily (e.g., a loop counter accessed thousands of times), so it should - if possible - be kept in a CPU register instead of regular memory, for faster access.

```c
int sum_squares(int n)
{
    register int i;      // hint: keep 'i' in a fast CPU register
    int total = 0;

    for (i = 1; i <= n; i++) {
        total += i * i;
    }
    return total;
}
```
Only automatic variables (and function parameters) can be declared register - it makes no sense for external/static variables, since those need a fixed, addressable memory location anyway.

	

<!-- markdownlint-capture -->
<!-- markdownlint-disable -->
>register is considered largely a historical/legacy keyword in modern C
{: .prompt-info }
<!-- markdownlint-restore -->

## Block Structure
```c
#include <stdio.h>

int main(void)
{
    int x = 1;                 // outer block: x = 1

    if (x > 0) {
        int x = 2;              // inner block: NEW x, shadows outer x
        printf("Inner x = %d\n", x);   // prints 2
    }

    printf("Outer x = %d\n", x);       // prints 1 - untouched!
    return 0;
}
```

<!-- markdownlint-capture -->
<!-- markdownlint-disable -->
> C does not allow nested function definitions
{: .prompt-warning }
<!-- markdownlint-restore -->

The inner `x` is a completely separate variable from the outer `x` - same name, different storage, different scope. Once the if block ends, the inner `x` is gone, and `x` reverts to referring to the outer one, exactly as if the inner block never happened.

## Initialization

* Rule 1: External and static variables default to zero
    ```
    int global_count;         // external, no initializer -> automatically 0
    static int cache_size;    // static, no initializer -> automatically 0
    ```
    This happens because external/static storage is set up once, before main() even runs, and the C runtime guarantees that memory starts zeroed out.

* Rule 2: Automatic and register variables have NO default value - they contain garbage.
    ```c
    void demo(void)
    {
        int x;              // automatic, uninitialized -> UNDEFINED, garbage value
        printf("%d\n", x);   // could print anything - reads whatever bits were left in memory
    }
    ```
    Automatic storage is just reused stack memory from whatever ran before - the compiler makes zero promises about its contents.

	

<!-- markdownlint-capture -->
<!-- markdownlint-disable -->
> C's silent "zero for globals, garbage for locals" behavior is a common source of bugs for people coming from higher-level languages, so it's worth internalizing this rule carefully.
{: .prompt-warning }
<!-- markdownlint-restore -->

## Recursion

Recursion is a function solving a problem by calling itself on a smaller version of the same problem, until it hits a base case simple enough to answer directly without recursing further.

```c
#include <stdio.h>

int factorial(int n)
{
    if (n <= 1) {          // base case: stops the recursion
        return 1;
    }
    return n * factorial(n - 1);   // recursive case: smaller subproblem
}

int main(void)
{
    printf("5! = %d\n", factorial(5));
    return 0;
}
```
Let's trace it mentally:
```shell
factorial(5) = 5 * factorial(4)
factorial(4) = 4 * factorial(3)
factorial(3) = 3 * factorial(2)
factorial(2) = 2 * factorial(1)
factorial(1) = 1              <- base case reached, unwinding begins
```
Each call is suspended, waiting on the result of the next, until `factorial(1)` returns `1`, and then the multiplications happen in reverse order as the stack unwinds: `1 -> 2*1=2 -> 3*2=6 -> 4*6=24 -> 5*24=120`.

## The C Preprocessor

### File Inclusion
```c
#include <stdio.h>   // preprocessor pastes in the full contents of stdio.h here
#include "helper.h"  // preprocessor pastes in the full contents of helper.h here

int main(void)
{
    printf("Hello\n");   // printf's prototype came from that pasted-in stdio.h
    return 0;
}
```
### Macro Substitution
```c
#define name replacement text
```
Another preprocessor directive, #define, lets you give a name to a piece of text, and the preprocessor will substitute that text in, wherever the name appears, before compilation even starts. This is purely textual - no type-checking, no evaluation - just find-and-replace.

```c
#define MAX_SIZE 100

int main(void)
{
    int arr[MAX_SIZE];   // preprocessor replaces this with: int arr[100];
    return 0;
}
```

**Important Concept**
```c
#define getchar() getc(stdin)
```
In many implementations of `<stdio.h>`, `getchar()` and `putchar()` aren't actually plain functions - they're defined as macros, for performance reasons (avoiding function-call overhead for something used so heavily, character by character).

```c
#undef getchar   // remove the macro definition

int getchar(void)   // now this refers to the REAL underlying function
{
    // ...
}
```

`#undef` is the escape hatch. It tells the preprocessor: "forget that macro definition from this point forward in the file."

### Conditional Inclusion

Just like your program has `if/else` for runtime decisions, the preprocessor has its own parallel set of directives for compile-time decisions: `#if`, `#ifdef`, `#ifndef`, `#else`, `#elif`, and `#endif`. These decide which chunks of source code even reach the compiler, based on conditions evaluated during preprocessing.

* Example 1: `#ifdef` / `#ifndef` - "Is this name defined?"
    ```c
    #define DEBUG

    int main(void)
    {
    #ifdef DEBUG
        printf("Debug mode is on\n");   // only included if DEBUG is #defined
    #endif
        printf("Program running\n");
        return 0;
    }
    ```
    Here's the key: the preprocessor scans the file first, sees `#ifdef DEBUG`, checks "has `DEBUG` been `#defined` anywhere above this point?" - yes - so it keeps that `printf` line as-is. Then it hands off a modified source file to the actual compiler, which looks like this:
    ```c
    int main(void)
    {
        printf("Debug mode is on\n");
        printf("Program running\n");
        return 0;
    }
    ```
    The `#ifdef` and `#endif` lines are gone entirely - they were never real C code, just instructions to the preprocessor.

    Now, if you delete `#define DEBUG` from the top of the file, the preprocessor's check fails ("`DEBUG` is NOT defined"), so it deletes that entire `printf` line before the compiler ever sees it. The compiler receives:
    ```c
    int main(void)
    {
        printf("Program running\n");
        return 0;
    }
    ```
 * Example 2: `#ifndef` and `#define` guard
    ```c
    /* helper.h */
    extern int total;
    void add_to_total(int n);
    ```
    ```c
    /* other.h */
    #include "helper.h"   // other.h needs helper.h's declarations too
    ```
    ```c
    /* main.c */
    #include "helper.h"   // included directly...
    #include "other.h"    // ...but other.h ALSO includes helper.h internally!
    ```
    Remember: `#include` is just copy-paste. So when the preprocessor flattens `main.c`, it pastes in `helper.h`'s contents once directly, and again indirectly through `other.h`. The compiler ends up seeing `extern int total;` and `void add_to_total(int n);` twice - which causes duplicate-declaration errors. 

    Now let's trace the guard.

    ```c
    #ifndef HELPER_H
    #define HELPER_H
    extern int total;
    void add_to_total(int n);
    #endif
    ```
    First time helper.h gets pasted in (via main.c's first #include):

    1. `#ifndef HELPER_H` - preprocessor asks: "has the name `HELPER_H` been `#define`d anywhere yet?" Answer: no, this is the very first time. So it proceeds into the block.
    2. `#define HELPER_H` - this immediately marks `HELPER_H` as now defined (it doesn't need a value, it's just a flag/marker, same idea as `DEBUG` from before).
    3. The two declaration lines get pasted in normally.
    4. `#endif` closes the conditional.

    Second time `helper.h` gets pasted in (via `other.h`'s `#include`, later in the same file):

    1. `#ifndef HELPER_H` - preprocessor asks the same question again: "has `HELPER_H` been defined yet?" This time, the answer is yes - step 2 above already defined it, earlier in this same preprocessing pass.
    2. Since the condition (`ifndef` = "if not defined") is now false, the preprocessor skips straight past everything down to `#endif` - the two declaration lines are not pasted in a second time.

    End result: no matter how many places `#include "helper.h"` (directly or indirectly), the actual declarations only get pasted into the final flattened source once

* Example 3 : `#if` with actual conditions (not just defined -checks)
    ```c
    #define VERSION 2

    #if VERSION >= 2
        printf("Using new feature\n");
    #else
        printf("Using legacy behavior\n");
    #endif
    ```

