---
layout: post
title: "Let me C : Pointers And Arrays"
date: 2026-08-03
categories: C-PROGRAMMING
tags: embedded-programming c-programming pointers
image:
    path: assets/img/post/let-me-c/pointer-array/pointer-arrays.png

---


A pointer is just a variable whose value is the address of another variable.

Let's introduce two variables:

* `&` (address of): give me the address of this variable
* `*` (dereference, in a declaration or expression context): give me the value stored at this address

```c
int x=1;
int *ip; //ip is declared as pointer to int
ip=&x; //ip now holds the address of x
printf("%d\n", *ip); //deference, prints 1
```
<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

> Note the dual role of `*` here - in the declaration `int *ip;`, it means "ip is a pointer to int." In the expression `*ip`, it means "dereference ip." Same symbol, different job depending on context.
{: .prompt-info }

<!-- markdownlint-restore -->


Example:

```c
int y = 42;
int *p = &y;
*p=100;
printf("%d\n", y);

// Output
100 //Why?
```
Why? - That's the core insight of pointers: `*p = 100` doesn't change what `p` points to, it changes the value at that location. Since `p` points to `y`, you're editing `y` "through the back door."

## Pointers and Function Arguments

```c
#include <stdio.h>

void swap(int *pa, int *pb)
{
    int temp;
    temp=*pa;
    *pa=*pb;
    *pb=temp;
}
int main()
{
    int a=5,b=10;
    printf("The value of a is %d and b is %d before the swap\n", a,b);
    swap(&a,&b);
    printf("The value of a is %d and b is %d after the swap\n", a,b);
    return 0;
}
```
## Pointers and Arrays
```c
int a[10];
int *pa;
pa=a; //equivalent to pa=&a[0];
```

<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

> Core Fact: The name of an array, when used in an expression, "decays" into a pointer to its first element.
{: .prompt-tip }

<!-- markdownlint-restore -->


So that means, these two statements are equal:
```c
a[i]     ==  *(a + i)
pa[i]    ==  *(pa + i)
```
Example:
* pa[2] → indexing syntax, reads as "element 2 of pa"

* (pa + 2) → pointer arithmetic, reads as "go 2 ints past where pa points, then dereference"

Now lets look at this example and see how we can use pointers instead:
```c
//Regular way
int a[5] = {10, 20, 30, 40, 50};
int sum = 0;
int i;
for (i = 0; i < 5; i++)
    sum += a[i];
```
```c
//Using the pointer
int a[5] = {10, 20, 30, 40, 50};
int sum = 0;
int *p;
for (p = a; p < a + 5; p++)
    sum += *p;
```
One Important difference
```c
int a[5] = {10, 20, 30, 40, 50};
int *pa = a;

pa++;      // legal - pa is a variable, can be reassigned
a++;       // ILLEGAL - a is not a variable, it's a fixed label for the array's address
```
## Address Arithmetic


<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

> Important concept : `i * sizeof(the pointed-to type)`
{: .prompt-warning }

<!-- markdownlint-restore -->


Suppose int is 4 bytes on your system, and a starts at memory address 1000.
```c
int a[5] = {10, 20, 30, 40, 50};
```
Memory layout will look like this:
```bash
address:  1000   1004   1008   1012   1016
value:    a[0]   a[1]   a[2]   a[3]   a[4]
          10     20     30     40     50
```
Each `int` takes 4 bytes, so consecutive elements are 4 bytes apart, not 1 byte apart.

Now, `a + 2` doesn't mean "address 1002" (that would land you inside `a[0]`, garbage). It means:
```c
a + 2  =  1000 + (2 * sizeof(int))  =  1000 + 8  =  1008
```
...which correctly lands you at `a[2]`. The compiler knows `a` is `int *`, so it automatically multiplies your offset by `sizeof(int)` before adding it to the raw address.

> Legal Pointer Operations
1. Add or subtract an integer to/from a pointer: `p + n`, `p - n`
2. Subtract one pointer from another (only if both point into the same array): `p2 - p1`
3. Compare two pointers with `<`, `<=`, `>`, `>=`, `==`, `!=` (only meaningful if both point into the same array)

```c
int a[10];
int *p1 = &a[2];
int *p2 = &a[7];

int n = p2 - p1;   // n == 5
```
## Character Pointers and Functions

```c
char *pmessage;
pmessage = "now is the time";
```

<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

> Note: C has no built-in string type. A "string" is just a convention - an array of `chars` terminated by a special sentinel value, `'\0'` (the null character, value `0`).
{: .prompt-info }

<!-- markdownlint-restore -->

Here, `"now is the time"` is a string constant - stored somewhere in memory as a sequence of characters plus a trailing `'\0'`. The name `pmessage` doesn't hold the characters themselves; it holds the address of the first character (`'n'`). This is exactly the array-decay behavior from `pointer and array` topic above.

```c
char amessage[] = "now is the time";   // an array - stores the actual characters
char *pmessage   = "now is the time";  // a pointer - points to the string constant
```
* `amessage` is an array; the characters live inside `amessage`'s own memory. You can modify individual characters: `amessage[0] = 'N';` is legal.
* `pmessage` is a pointer; it points at a string literal, which many compilers place in read-only memory. Attempting `pmessage[0] = 'N';` is undefined behavior - it might crash, or silently corrupt something, depending on the platform.

Example:
`strlen` function
```c
int strlen(char *s)
{
    int n;
    for (n = 0; *s != '\0'; s++)
        n++;
    return n;
}
```
## Pointer Arrays: Pointers to Pointers

I tied to follow the example in book but it flew over my head. So I am relying on this [w3schools note](https://www.w3schools.com/c/c_pointer_to_pointer.php) to get acquainted with the pointer concept.

```c
int myNum = 5;
int *ptr = &myNum;
int **pptr = &ptr;

**pptr = 20; // changes myNum

printf("myNum = %d\n", myNum); // prints 20 
```
* A pointer to pointer stores the address of another pointer.
* `*ptr` gives the value of a variable.
* `**pptr` gives the same value by following two levels of indirection.

## Multi-dimensional Arrays

Again, I didn't want to spend much time on it. I rather choose to go through this [Multi-dimensional note](https://www.w3schools.com/c/c_arrays_multi.php) to get the gist of it.

From K&R book: 
* `int m[4][3]` is one contiguous block of 12 ints in memory - not separate rows, not pointers to anything. It's a flat sequence that we interpret as rows and columns.
* Indexing formula to locate any element within in the block
```bash
offset = row * (columns per row) + column
```
***Example***
```c
int a[10][20];      // a true 2D array: ONE block of 200 ints, contiguous
int *b[10];         // an array of 10 pointers: EACH pointer can point anywhere separately
```


## Initialization of Pointer Arrays
```c
char *month_name(int n)
{
    static char *name[] = {
        "Illegal month",
        "January", "February", "March",
        "April", "May", "June",
        "July", "August", "September",
        "October", "November", "December"
    };

    return (n < 1 || n > 12) ? name[0] : name[n];
}
```
Let's break it down:
* The function name `char *month_name(int n)`, how to read it? - month_name is a function that takes integer n input and returns `char *` meaning - returns a pointer to character aka address of the character.

* the `static char *name[]={}` , how to read it? - `name` is an array `[]` of `char *` (array of pointers to char)

* `static` - This means `name` is created once and keeps its values between function calls, rather than being rebuilt from scratch every time `month_name()` runs. Without `static`, a local array like this would normally be re-initialized on every single call - wasteful for a fixed table that never changes.

## Pointers to Functions

Just like variables live at addresses, so does compiled code. A function has an address in memory too - and C lets you store that address in a pointer, then call the function through that pointer.

```c
int (*comp)(void *, void *);
```
* This says: `comp` is a pointer to a function that takes two `void *` arguments and returns an `int`
* Here `void *` means a pointer to some type, unspecified

Note:
```c
int *comp(void *, void *);   // DIFFERENT: a function named comp, returning int*
```

***Important Concept***

Let's recall
```c
int a[5];
int *pa = a;      // no & needed - 'a' already means "address of a[0]"
```
Similarly
```c
int numcmp(int a, int b) { ... }

int (*p)(int, int) = numcmp;    // 'numcmp' by itself already means its own address

int (*p)(int, int) = &numcmp;  // also legal, & is optional here, means the same thing
```
Let's look at the below example and its output to better understand the above concept.
```c
#include <stdio.h>

int numcmp(int a, int b)
{
    return a - b;
}

int main(void)
{
    // 'numcmp' by itself (no parentheses) means "the address of this function"
    // just like an array name 'a' by itself means "the address of a[0]"

    printf("Address of numcmp: %p\n", (void *)numcmp);

    int (*p)(int, int);
    p = numcmp;   // no & needed - numcmp already IS the address

    printf("Address stored in p: %p\n", (void *)p);

    // both addresses printed above will be IDENTICAL

    int result = p(10, 3);   // call the function through the pointer
    printf("p(10, 3) = %d\n", result);

    return 0;
}
```

***Output***
```bash
Address of numcmp: 0x5578a1b3e169
Address stored in p: 0x5578a1b3e169
p(10, 3) = 7
```
### Example
```c
#include <stdio.h>

// Two candidate functions with matching signatures
int add(int a, int b)
{
    return a + b;
}

int multiply(int a, int b)
{
    return a * b;
}

// A function that takes a "pointer to function" as a parameter
int compute(int x, int y, int (*op)(int, int))
{
    return op(x, y);   // call whichever function op points to
}

int main(void)
{
    int (*fp)(int, int);   // fp: pointer to a function taking (int,int), returning int

    fp = add;                          // fp now points at add
    printf("add:      %d\n", fp(3, 4));       // calls add(3,4) -> 7

    fp = multiply;                     // fp now points at multiply
    printf("multiply: %d\n", fp(3, 4));       // calls multiply(3,4) -> 12

    // Passing function pointers directly into another function
    printf("compute(add):      %d\n", compute(5, 6, add));       // 11
    printf("compute(multiply): %d\n", compute(5, 6, multiply));  // 30

    return 0;
}
```
***Output***
```bash
add:      7
multiply: 12
compute(add):      11
compute(multiply): 30
```

## Complicated Declarations

As the name suggests, it is complicated and my brain couldn't grasp the idea completely for now. My brain hurts when I try to comprehend the concepts presented in this section. So, I am leaving this part empty for now. And, I will revisit this later. 

Basically this part sums up the different ways the pointers are bind using the `()` and how to interpret it. More on this later.

