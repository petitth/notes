
# C Language development with Eclipse CDT

This note details the main steps to develop in C Language with Eclipse CDT, using Makefile.

---

## Requirements

1. Make sure GCC is available on your system : gcc --version<br>
   If not present, install it with package manager.
2. Make sure GDB debugger is available too on your system : gdb --version<br>
   If not present, install it with package manager.
3. Install Eclipse CDT, this version is made for C/C++. <br>
   Get this tool from the official website : www.eclipse.org/cdt


## Create a first project

Create a folder containing 2 files :
- a source file : hello.c
```c
#include <stdio.h>

int main() {
    printf("hello world\n");
    return 0;
}
```
- a Makefile file : this file will be used to build, and run the C application
```make
# CC is the convention for C compiler
CC=gcc
# CFLAGS
# ------
# '-g' : allow debugging
# '-Wall' brings compiler warning msgs
# '-pedantic' : force you to respect standards, makes the code easier to compile under other compilers
CFLAGS=-g -W -Wall -pedantic
LDFLAGS=
EXEC=hello

all: $(EXEC) test

hello: hello.o
	$(CC) -o hello hello.o $(LDFLAGS)

hello.o: hello.c 
	$(CC) -o hello.o -c hello.c $(CFLAGS)

test: hello
	./hello

clean:
	rm -f *.o hello
```


## Import project in Eclipse CDT
- Go to menu \ file \ import
- Select C/C++ then existing code as Makefile project
- Select the folder containing the source and the Makefile
- In Toolchain, select Linux GCC

## Project usage
You can build (and execute) the project using menu\project\Build project <br>
To go faster, just use CTRL + B shortcut. <br>
After the build, you can even debug the code, with standard debug icon.<br><br>
In case of issues, it could be usefull to clean project from generated files, 
using menu\Project\Clean