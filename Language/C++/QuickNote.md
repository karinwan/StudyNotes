# C++

## Main

    int main(int argc, char *argv[])

### argc

- Argument count
- Number of strings pointed to by argv. 
- 1 + # of arguments (1: name of the program)

### argv

- Argument vector
- program name, arg1, arg2, ..., NULL

```
myprog arg1 arg2 arg3

argc -> 4
argv -> myprog, arg1, arg2, arg3, NULL
```

## String

### strlen

    size_t strlen ( const char * str );

- Returns the length of the C string str
- Without the terminating null-character

``` 
#include <stdio.h>
#include <string.h>

char mystr[100]="test";

sizeof(mystr) -> 100
strlen(mystr) -> 4
```

## Print

| Format Specifier | Description |
| ----------- | ----------- |
| %c | single character |
| %s | character string |
| %d / %i | signed integer to decimal representation |
| %u | unsigned integer to decimal representation |
| %o | unsigned integer to octal representation |
| %x | unsigned integer to hexadecimal representation |
| %f | floating-point number to the decimal representation |
| %p | printing the value of a pointer  |
| .number | For s: the maximum number of characters to be printed |
| % |  |

```
printf("Print string: %.4s", "abcdefg")

-> abcd
```


### cout

    #include<stdio.h>
    
    std::cout << "Hello! \n"; 
    
### printf

    int printf(const char* str, ...); 

- print character stream of data on stdout console

``` 
#include<stdio.h>

printf("Hello! \n");

-> Hello!

```

### sprintf

    int sprintf(char *str, const char *string,...); 

- String print function 
- Instead of printing on console, store it on char buffer(str) which are specified in sprintf

```
#include<stdio.h>

char buffer[50];

sprintf(buffer, "This is buffer. ");
```

The string "This is buffer. " is stored into buffer instead of printing on stdout. 

```
printf("%s", buffer);

-> This is buffer. 
```

### fprintf

    int fprintf(FILE *fptr, const char *str, ...);

- Print the string content in file but not on stdout console

#### [ref](https://www.geeksforgeeks.org/difference-printf-sprintf-fprintf/)

## Memory

### memset

    void * memset ( void * ptr, int value, size_t num );

- Fill block of memory
- Sets the first num bytes of the block of memory pointed by ptr to the specified value (interpreted as an unsigned char).

```
#include <stdio.h>
#include <string.h>

char str[] = "almost every programmer should know memset!";

memset (str,'-',6);

puts (str);

-> ------ every programmer should know memset!
```

## File

### fgetc

    int fgetc(FILE *stream)

- gets the next character (an unsigned char) from the specified stream

## regex

    #include <regex.h>

### regcomp

    int regcomp(regex_t *preg, const char *pattern, int cflags);

- Compiles the source regular expression pointed to by pattern, 
- into an executable version,
- stores it in the location pointed to by preg.

### regexec

    int regexec(const regex_t *preg, 
                const char *string, 
                size_t nmatch, 
                regmatch_t *pmatch, 
                int eflags);

- Compares the null-ended string against the compiled regular expression preg to find a match between the two.
- Return Value: 
    - 0: a match is found
    - REG_NOMATCH: no match is found
    - nonzero value: error
