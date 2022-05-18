# C++

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
