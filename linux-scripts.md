## Linux Scripts 

This note provides some practical examples of scripts under Linux. <br/>
To create a script, use any text editor, then make it executable with : chmod +x script-name <br/>
Finally, run with a terminal : ./script-name

<a name="index"></a> 
     
## Table of contents
1. [Basic script](#basic)                                               
2. [Using a function](#function)
3. [Calculations, date, formatting and sleep](#calculations)
4. [Searching an array](#searching-array)                                    
5. [Checking parameters with a regex expression](#check-param-regex)                                                 
6. [Calling other scripts](#call-other-scripts)                                      
7. [Split a string](#split-string)
8. [Trim a string](#trim-string)
9. [Use colors when displaying messages](#use-colors)
10. [List the content of a directory](#list-directory)     
11. [Retrieve named parameters](#named-parameters)
12. [Misc](#misc)

### <a name="basic"></a>Basic script
 
```shell script
#!/bin/bash
echo "hello world"
exit 0
```
<a href="#index">back to index</a>

###  <a name="function"></a>Using a function

```shell script
#!/bin/bash
some_function() {
	echo "the color is $1"
}

echo "begin"
some_function "black"
some_function "yellow"
some_function "red"
echo "end"
exit 0
```
<a href="#index">back to index</a>

### <a name="calculations"></a>Calculations, date, formatting and sleep

```shell script
#/bin/bash
DELAY_SECONDS=3
i=1
filename="`date +%Y%m%d_%H%M%S`"

# file name will be current date and a counter formatted with 3 numbers
echo "${filename}_`printf "%03d" $i`"
sleep $DELAY_SECONDS
i=$((i + 1))
echo "${filename}_`printf "%03d" $i`"
sleep $DELAY_SECONDS
i=$((i + 1))
echo "${filename}_`printf "%03d" $i`"
exit 0
```

<a href="#index">back to index</a>

### <a name="searching-array"></a>Searching an array

The displayed result will be : found green
```shell script
#!/bin/bash
some_array=(red green blue)
case "${some_array[@]}" in  *"green"*) echo "found green" ;; esac
case "${some_array[@]}" in  *"orange"*) echo "found orange" ;; esac
exit 0
```
<a href="#index">back to index</a>

### <a name="check-param-regex"></a>Checking parameters with a regex expression

This script just adds 2 numbers, call it **add**.
```shell script
#!/bin/bash

# expect 2 parameters
if [ $# -ne 2 ] ; then
    echo "incorrect syntax : add number1 number2"
    exit 1
fi
# check that parameters are following a regex pattern
pattern='^[0-9]+$'
if ! [[ $1 =~ $pattern ]] ; then
    echo "$1 is not numeric"
    exit 2
fi
if ! [[ $2 =~ $pattern ]] ; then
    echo "$2 is not numeric"
    exit 3
fi

result=$(($1 + $2)) 
echo "$1 + $2 = $result"
exit 0
```
<a href="#index">back to index</a>

### <a name="call-other-scripts"></a>Calling other scripts

- **the main script** <br/>
**$?** provides the return code of last instruction/command called
```shell script
#!/bin/bash
echo "-- begin --"
result=`sh ./success-script`
echo "result [$?] = $result"

result=`sh ./error-script`
echo "result [$?] = $result"

echo "-- end --"
exit 0
```

- **success-script : a script with standard return code**
```shell script
#!/bin/bash
echo "this is a script with normal exit code"
exit 0
```

- **error-script : a script with error(s)**
```shell script
#!/bin/bash
echo "an error occurred so return code is not zero"
exit 5
```

- **result**
```shell script
-- begin --
result [0] = this is a script with normal exit code
result [5] = an error occurred so return code is not zero
-- end --
```
<a href="#index">back to index</a>

### <a name="split-string"></a>Split a string

```shell script
#!/bin/bash

person_name='bob-razovsky'
# only display lastname
echo "$person_name" | cut -d "-" -f 2
exit 0
```
<a href="#index">back to index</a>

### <a name="trim-string"></a>Trim a string

```shell script
#!/bin/bash

# internal field separator (allow blank spaces in strings)
IFS=$(echo -en "\n\b")

trim() {
  result=`echo $1 | sed -e 's/^[[:space:]]*//' -e 's/[[:space:]]*$//'`
  echo "$result"
}

echo "-- begin --"
str=" hello world "
trimmed=$(trim $str)
echo "trimmed string = '$trimmed'"
echo "-- end --"
exit 0
```
<a href="#index">back to index</a>

### <a name="use-colors"></a>Use colors when displaying messages

```shell script
#!/bin/bash

running=1
result="the project is not running."

# use ansi escape codes for colors
NOCOLOR='\033[0m' 
RED='\033[0;31m'
GREEN='\033[0;32m'
ORANGE='\033[0;33m'
BLUE='\033[0;34m'
MAGENTA='\033[0;35m'
CYAN='\033[0;36m'
WHITE='\033[0;37m'
YELLOW='\033[0;93m'

echo "begin"
case "$result" in  *"not running"*) running=0 ;; esac
if [ $running -eq 0  ]; then
	echo -e "${RED}it is not running.${NOCOLOR}"
else
	echo -e "${GREEN}it is running.${NOCOLOR}"	
fi

echo -e "${RED}some message in red${NOCOLOR}."	
echo -e "${GREEN}some message in green${NOCOLOR}."	
echo -e "${ORANGE}some message in orange${NOCOLOR}."	
echo -e "${BLUE}some message in blue${NOCOLOR}."	
echo -e "${MAGENTA}some message in magenta${NOCOLOR}."	
echo -e "${CYAN}some message in cyan${NOCOLOR}."	
echo -e "${WHITE}some message in white${NOCOLOR}."	
echo -e "${YELLOW}some message in yellow${NOCOLOR}."	
echo "end"
exit 0
```
<a href="#index">back to index</a>

### <a name="list-directory"></a>List the content of a directory

Call the script **view-dir**. <br/>
For every sub-directory, a recursive call to the function will be made.

```shell script
#!/bin/bash 
#
# This script lists every file and directory from a directory passed as parameter.
# The directories will be displayed in green, files in blue.
# For every file, its size will be displayed.
#

# use ansi escape codes for colors
NOCOLOR='\033[0m' 
GREEN='\033[0;32m'
BLUE='\033[0;34m'
CYAN='\033[0;36m'

# internal field separator (for folders with blank spaces)
IFS=$(echo -en "\n\b")

view_directory() {
  for dir_entry in `ls $1`
  do
    if [ -f "$1/$dir_entry" ] ; then
      # the entry is a file
      file_size=`du -h "$1/$dir_entry" | cut -f1`
      echo -e "${CYAN}$1/$dir_entry (${file_size})${NOCOLOR}"
    fi
    if [ -d "$1/$dir_entry" ] ; then
      # the entry is a directory
      echo -e "${GREEN}$1/$dir_entry${NOCOLOR}"
      view_directory "$1/$dir_entry"
    fi
  done
}

if [ $# -ne 1 ] ; then
    echo "incorrect syntax : view-dir path"
    exit 1
fi

view_directory "$1"
exit 0
```
<a href="#index">back to index</a>

### <a name="named-parameters"></a>Retrieve named parameters

```shell script
#/bin/bash
#
# usage : ./named-param --foo=one --bar=two
#
foo="abc"
bar="def"

for arg in "$@"
do
    key=$(echo $arg | cut -f1 -d=)
    value=$(echo $arg | cut -f2 -d=)   

    case "$key" in
            --foo)	foo=${value} ;;
            --bar)	bar=${value} ;;     
            *)   
    esac    
done

echo "foo = $foo"
echo "bar = $bar"
exit 0
```

<a href="#index">back to index</a>

<a name="misc"></a>
## Misc

```shell script
# remove line breaks in a file  
tr -d '\n' < input_file.txt > output_file.txt

# clear file (3 different ways)  
cat /dev/null > ~/.bash_history 
echo '' > file.log
dd if=/dev/null of=file.log

# take first 80 characters of every file, and show distinct 4th element (elements being separated by '+')
cut -c-80 * | cut -d'+' -f4 | sort | uniq
```
<a href="#index">back to index</a>