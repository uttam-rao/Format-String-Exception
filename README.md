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

printf() takes a variable number of arguments and scans the format string for format specifier to determine the number of arguments. Where are these arguments located? The calling conventions are different based on the systems being used but at least some arguments are always taken from the stack. For this writeup, knowledge of how the stack works is assumed so a detailed explanation of how the stack works is skipped. Generally speaking, when a local variable or a function argument is declared it gets pushed onto the stack. See the simple example below

```
printf(“The value of first is %d, the value of second is %d, the address of third is %08x.”, first, second, &third);
```
![stack_screenshot](./stack_screenshot.png)

## So what's the issue?

Format functions can be exploited in a number of ways when an adversary is given control over the format string fed into the function, for example, when a program asks for user input. We’ll start with the simplest and arguably the least harmful of the exploits: leaking information. Suppose we have the simple program below which asks user for input and prints it. 

```
int main(int argc, char **argv){
    char user_input[100];
    printf("Please enter a string\n");
    //get string from user
    scanf("%s", user_input);
    // the vulnerability
    printf(user_input); 
    return 0;
}
```
