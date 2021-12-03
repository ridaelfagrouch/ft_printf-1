# Quick explanation of the original `printf`

## The syntax for a format placeholder

```text
%[parameter][flags][width][.precision][length]type
```

## Parameter field

_This is a POSIX extension and not in C99_

The Parameter field takes as input the index of the parameter _aka_ the `n`th parameter _(or 'the `n`th **argument**', whatever you want to call it)_ followed by a `$` Dollar sign. After that we can apply some flags just as we would do normally.

**\*Note** that the index should always start from `1` and not `0`\*.

Once you specify a parameter field for one parameter, you should do the same with all the other parameters, i.e. you can't just have 5 parameters, apply the "Parameter field" _(aka `n$`)_ to, e.g., the _4th_ element, leave the others without indexing, and expect `printf()` to just magically figure out which flag should point to which parameter, it's either **All of them** or **None of them**.

- **Example** of a good use-case

```c
#include <stdio.h>

int main(void)
{
    printf("%3$d; %1$d; %4$d; %2$d",16, 17, 18, 19)
    return (0);
}
```

```text
Output:

18; 16; 19; 17
```

- **Example** that'll produce an error

```c
#include <stdio.h>

int main(void)
{
    printf("%3$d; %1$d; %d; %2$d",16, 17, 18, 19)
    return (0);
}
```

```text
Output:

file.c:5:26: error: missing $ operand number in format
5   |     printf(%3$d; %1$d; %d; %2$d",16, 17, 18, 19)
    |            ^~~~~~~~~~~~~~~~~~~~
```

## Flags field

### The `-` flag

- When used, the content becomes Left-Justified _(instead of the default Right-Justification)_.

- **_Example:_**

```c
#include <stdio.h>

int main(void)
{
    printf("\"%12s\"\n", "Hello");
    printf("\"%-12s\"\n", "Hello");
    return (0);
}
```

```text
Output:

"       Hello"
"Hello       "
```

_We will later see what the number `12` means in this case._

### The `+` flag

- When used _(with integers)_, it'll prepend a `+` sign if the integer is positive, or a `-` sign if it's negative; As oppose to the default behaviour where the `+` sign is omitted in case the integer is positive.

- **_Example:_**

```c
#include <stdio.h>

int main(void)
{
    printf("Without the '+' sign:\n");
    printf("%d\n", 69);
    printf("%d\n", -69);
    printf("With the '+' sign:\n");
    printf("%+d\n", 69);
    printf("%+d\n", -69);
    return (0);
}
```

```text
Output:

Without the '+' sign:
69
-69
With the '+' sign:
+69
-69
```

### The `` _(space)_ flag

- When used _(with integers)_, it'll prepend a `` *(space character)* if the integer is positive, or a `-` sign if it's negative. It's behaviour is quite similar to the `+` flag, exept that it prepends a `` instead of a `+`.

- **_Example:_**

```c
#include <stdio.h>

int main(void)
{
    printf("Without the '+' sign:\n");
    printf("%d\n", 420);
    printf("%d\n", -420);
    printf("With the '+' sign:\n");
    printf("% d\n", 420);
    printf("% d\n", -420);
    return (0);
}
```

```text
Output:

Without the ' ' (space character):
420
-420
With the ' ' (space character):
 420
-420
```

### The `0` flag

- **_Example:_**

```c

```

### The `#` flag

- **_Example:_**

```c

```

### The `.` flag

- **_Example:_**

```c

```

### Precision field

## Mandatory Flags _(`%c`, `%s`, `%p`, `%d`, `%i`, `%u`, `%x`, `%X`, `%%`)_

### The `%c` flag _(character)_

The `%c` flag indicates that the argument will be a single character _aka_ a `char`, it's really that simple.

### The `%s` flag _(string of characters)_

The `%s` flag indicates that the argument will be a string of characters _aka_ a `char *`.
**\*Note** that the string should be NULL-terminated.\*

### The `%p` flag _(pointer)_

The `%p` flag indicates that the argument will be a pointer _aka_ a `void *` _(pointer to any data type)_, the output should be the address to which that pointer points to, printed .

### The `%d` flag _(decimal number aka **base10 integer**)_

The `%d` flag indicates that the argument will be of type `signed int` _(positive AND negative integers)_ and with a **base10** _(0, 1, 2, ..., 9)_

- **Example**

```c
#include <stdio.h>

int main(void)
{
    int num = 9;

    printf("%d\n", num);
    return (0);
}
```

```text
Output:
9

```

### The `%i` flag

The `%i` flag is similar to the `%d` flag when it's used in `printf()`, but they do differ when used in the `scanf()` function, let's look at these differences:

- The `%d` always assumes that the input is in **base10**.
- The `%i` automatically detects the **base** of the integer, and converts it's value to _decimal_ aka **base10**.

**\*Remember** we are not trying to recode `scanf()`, so don't mix up the use cases, of the above-mentioned flags, for each function\*.

### The `%u` flag

### The `%x` flag

### The `%X` flag

### The `%%` flag

## Bonus Flags _(`-`, `0`, `.`, `#`, ``space,`+`)_

### Some more flags _(`%f`, `%o`, `%`)_

# Notes for me

1. Parameters should be indexed, starting with 1 _(not 0)_. That should pave the way for the "Parameter selecting" —type of flags— implementation, where we put the index of the parameter followed by a `$` sign to indicate that we want to select the `n`th parameter
