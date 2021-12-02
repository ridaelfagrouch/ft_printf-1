# FT_PRINTF

## Flags

### The `%d` flag

The `%d` flag indicates that the input will be of type `signed int` *(positive AND negative integers)* and with a **base10** *(0, 1, 2, ..., 9)*

- **Example**

```c
#include <stdio.h>

int	main(void)
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
- The `%i` automatically detects the **base** of the integer, and converts it's value to *decimal* aka **base10**.

***Remember** we are not trying to recode `scanf()`, so don't mix up the use cases of the above-mentioned flags*.


