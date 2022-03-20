# Format String Exploitation
### Uttam Rao (COSC 69) Final Project

## What is a format string?

A format string is a string which contains format specifiers and is used in format functions. In this writeup we look at format functions in C, but the same kind of functions exist in many other programming languages. Below is an example

```
printf(“The name of this class is %s.\n”, class_name);
```

If the variable class_name is set to the string “COSC 69” the above statement will print 

```
The name of this class is COSC 69.
```

%s indicates that the data is a pointer to the string. Besides %s there are several other format specifiers. They are summarized in the table below

| Specifier | Format | Meaning | Passed as |
| - | - | - | - |
| %s | string ((const) (unsigned) char *) | data should be pointer to a string | reference |
| %d | decimal (int) | signed integers in decimal | value |
| %u | unsigned decimal (unsigned int) | unsigned integer in decimal| value |
| %x | hexadecimal (unsigned int) | unsigned integer in hex| value |
| %n | number of bytes written so far, (* int) | numbe | reference |

## The role of the stack

printf() takes a variable number of arguments and scans the format string for format specifier to determine the number of arguments. Where are these arguments located? The calling conventions are different based on the system being used but at least some arguments are always taken from the stack. 

## How can format strings be exploited

Format function can be exploited in a number of ways when an adversary is given direct control over the format string fed into the function.
