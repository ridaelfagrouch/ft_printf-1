[![mnaimi's 42Project Score](https://badge42.herokuapp.com/api/project/mnaimi/printf)](https://github.com/JaeSeoKim/badge42)

# *42 Network* version of `printf()` 

_**Note:** I won't be covering everything about the original `printf()`, just the stuff we need to know to recode the *42 Network* version._

## The syntax for a format placeholder

```text
%[parameter][flags][width][.precision][length]type
```

## Parameter field

_This is a POSIX extension and not in C99_

The Parameter field takes as input the index of the parameter _aka_ the `n`th parameter _(or 'the `n`th **argument**', whatever you want to call it)_ followed by a `$` Dollar sign. After that we can apply some flags just as we would do normally.

_**Note** that the index should always start from `1` and not `0`_.

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

When used, the content becomes Left-Justified _(instead of the default Right-Justification)_.

- **_Example:_**

```c
#include <stdio.h>

int main(void)
{
	printf("Without the '-' flag:\n");
    printf("\"%12s\"\n\n", "Hello");
    printf("Without the '-' flag:\n");
    printf("\"%-12s\"\n", "Hello");
    return (0);
}
```

```text
Output:

Without the '-' flag:
"       Hello"

With the '-' flag:
"Hello       "
```

_We will later see what the number `12` means in this case._

### The `+` flag

When used _(with signed-numeric types)_, it'll prepend a `+` sign if the integer is positive, or a `-` sign if it's negative; As oppose to the default behaviour where the `+` sign is omitted in case the integer is positive.

- **_Example:_**

```c
#include <stdio.h>

int main(void)
{
    printf("Without the '+' flag:\n");
    printf("%d\n", 69);
    printf("%d\n\n", -69);
    printf("With the '+' flag:\n");
    printf("%+d\n", 69);
    printf("%+d\n", -69);
    return (0);
}
```

```text
Output:

Without the '+' flag:
69
-69

With the '+' flag:
+69
-69
```

### The '&emsp;' _(space character)_ flag

When used _(with signed-numeric types)_, it'll prepend a '&emsp;' *(space character)* if the integer is positive, or a `-` sign if it's negative. It's behaviour is quite similar to the `+` flag, exept that it prepends a '&emsp;' instead of a `+`.

- **_Example:_**

```c
#include <stdio.h>

int main(void)
{
    printf("Without the ' ' (space character) flag:\n");
    printf("%d\n", 420);
    printf("%d\n\n", -420);
    printf("With the ' ' (space character) flag:\n");
    printf("% d\n", 420);
    printf("% d\n", -420);
    return (0);
}
```

```text
Output:

Without the ' ' (space character) flag:
420
-420

With the ' ' (space character) flag:
 420
-420
```
_**Remember:** this flag is ignored if the `+` flag exists._

### The `0` flag

When used with the `%d` _or_ `%i` _or_ `%x` _or_ `%X`, and with the **Width** flag _(we'll get to that later on)_, it replaces the default blank spaces for numeric types with _`0`'s_.

- **_Example:_**

```c
#include <stdio.h>

int main(void)
{
	printf("Without the '0' flag:\n");
    printf("\"%5d\"\n\n", 1);
    printf("With the '0' flag:\n");
    printf("\"%05d\"\n", 1);
    return (0);
}
```

```text
Output:

Without the '0' flag:
"    1"

With the '0' flag:
"00001"
```

_**Remember** the `0` flag is ignored if precision is specified for an integer or if the `-` flag is specified._

### The `#` flag

When used with the `%x` _or_ `%X` formats, it prefixes the output with a `0x` _or a_ `0X` respectively, i.e. it adds either a `0x` at the beginning of the output given by the `%x` formats, or a `0X` at the beginning of the output given by the `%X`.

- **_Example:_**

```c
#include <stdio.h>

int main(void)
{
	printf("Without the '#' flag:\n");
    printf("%x\n", 180);
    printf("%X\n\n", 180);
    printf("With the '#' flag:\n");
    printf("%#x\n", 180);
    printf("%#X\n", 180);
    return (0);
}
```

```text
Output:

Without the '#' flag:
b4
B4

With the '#' flag:
0xb4
0XB4
```
_The `#` flag should not be used with `c` or `d` or `i` or `u` or `s` types._

### Width field

It's basicaly a non-negative decimal number that represents the minimum number of bytes to be printed. If the number of bytes in the output value is less than the specified width, blanks are added on the _Right_ _(or to the Left if the `-` flag is specified)_.

Width never causes a value to be truncated; if the number of bytes in the output value is greater than the specified width, or width is not given, all characters of the value are printed.

The width specification can be an asterisk `*`, in which case an argument —from the argument list— supplies the value. The width argument must precede the value being formatted in the argument list.

- **Example:**

```c
int main(void)
{
	printf("Without the Width:\n");
    printf("\"%d\"\n\n", 69420);
    printf("With the Width:\n");
    printf("\"%10d\"\n\n", 69420);
    printf("With the Width:\n");
    printf("\"%-10d\"\n\n", 69420);
    printf("With the Width:\n");
    printf("\"%5d\"\n\n", 69420);
    printf("With the Width:\n");
    printf("\"%*d\"\n", 15, 69420);
    return (0);
}
```

```text
Output:

Without the Width:
"69420"

With the Width:
"     69420"

With the Width:
"69420     "

With the Width:
"69420"

With the Width:
"          69420"
```

### Precision field

The **Precision field** can be described as the opposite of what the _Width field_ does, while the latter specifies the minimum number of bytes to be printed, the _Precision field_ specifies the maximum number of bytes to be printed; another thing is that  the _Width field_ doesn't truncate the output in any case, the _Precision field_ does in case the original length of the output exceeds the maximum specified length in the _Precision field_.

The Precision specification can be an asterisk `*`, in which case an argument —from the argument list— supplies the value. The Precision argument must precede the value being formatted in the argument list.

In our case, the precision will only be applied to `%s`, `%d` and `%i`. If the specified width exceeds the total length of the string, the string gets printed, and that's all, no spaces, and no `0`'s. If it's applied to a digit _(`%d` or `%i`)_, and the specified length exceeds the total length of the digits in the given number, the rest is filled with `0`'s, but if it's the opposite, and the specified length is smaller than the total length of the number, it doesn't get truncated.

_**Note** that the precision field doesn't work for `%c`, and that it outputs a warning —but does work— when used with `%p`, _
- **Example:**

```c

```
___

## Sources

Wikipedia: https://en.wikipedia.org/wiki/Printf_format_string

IBM's Docs: https://www.ibm.com/docs/en/i/7.4?topic=functions-printf-print-formatted-characters

Other random websites:

- <a href="https://www.lix.polytechnique.fr/~liberti/public/computing/prog/c/C/FUNCTIONS/format.html" target="_blank">https://www.lix.polytechnique.fr/~liberti/public/computing/prog/c/C/FUNCTIONS/format.html</a>

- <a href="https://flylib.com/books/en/2.254.1/using_flags_in_the_printf_format_string.html" target="_blank">https://flylib.com/books/en/2.254.1/using_flags_in_the_printf_format_string.html</a>

- <a href="https://www.codingunit.com/printf-format-specifiers-format-conversions-and-formatted-output" target="_blank">https://www.codingunit.com/printf-format-specifiers-format-conversions-and-formatted-output</a>

- <a href="https://alvinalexander.com/programming/printf-format-cheat-sheet/" target="_blank">https://alvinalexander.com/programming/printf-format-cheat-sheet/</a>

___

