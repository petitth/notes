## Introduction to GCC

The GNU Compiler Collection, GCC, is a set of compilers created by GNU Project.<br>
It is used to compile programming languages, such a C, C++, Objective C and more.<br>
For more info, see the official website of GCC : https://gcc.gnu.org/

## Setup

| Command                  | Detail                            |
| -------------------------|-----------------------------------|
| ```gcc --version```      | current version of gcc            |
| ```which gcc```          | location of gcc executable        |
| ```whereis gcc```        | binary, source and manual page    |

## GCC usage

| Command                           | Detail                                          |
| ----------------------------------|-------------------------------------------------|
| ```gcc hello.c```                 | compile C source, produces out file to execute  |
| ```gcc hello.c -o hello```        | '-o' is used to set output file                 |
| ```gcc -Wall hello.c -o hello```  | '-Wall' enables all compiler's warning messages |
| ```gcc -g hello.c -o hello ```    | allow debugging of code                         |
| ```gcc -D DEBUG tool.c -o tool``` | define a macro to pass to the source            |


## Examples

### A first source to try gcc
```c
#include <stdio.h>

int main() {
    printf("hello world\n");
    return 0;
}
```

#### A source using a DEBUG macro

```c
#include <stdio.h>

int main() {
    #ifdef DEBUG
        printf("running in debug\n");
    #else    
        printf("not running in debug\n");
    #endif
    return 0;
}
```