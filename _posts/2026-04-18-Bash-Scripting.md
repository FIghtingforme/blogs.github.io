---

date: 2026-04-18
title: Bash Scripting Documetry
layout: post
catagories: Linux

---



## Bash Scripting


### Bash: "Bourne Again Shell"

### Shell programming using other common shells such as sh, csh, tcsh, will also be referenced, as they sometime differ from bash.


### Shell programming can be accomplished by directly executing shell commands at the shell prompt or by storing them in the order of execution, in a text file, called a shell script, and then executing the shell script. To execute, simply write the shell script file name, once the file has execute permission (chmod +x filename).

### The first line of the shell script file begins with a "sha-bang" (#!) which is not read as a comment, followed by the full path where the shell interpreter is located. This path, tells the operating system that this file is a set of commands to be fed into the interpreter indicated. Note that if the path given at the "sha-bang" is incorrect, then an error message e.g. "Command not found.", may be the result of the script execution. It is common to name the shell script with the ".sh" extension. The first line may look like this:


```

#!/bin/bash
# Adding comments: any text following the "#" is considered a comment

```


#### To find out what is currently active shell, and what is its path, type the highlighted command at the shell prompt (sample responses follow):

```
[devops@lb01 ~]$ ps | grep $$
 185152 pts/1    00:00:00 bash
 
 # This response shows that the shell you are using is of type 'bash'. 
```

#### next find out the full path of the shell interpreter

```
[devops@lb01 ~]$ which bash
/usr/bin/bash

# This response shows the full execution path of the shell interpreter. 
```

#### Make sure that the "sha-bang" (#!) line at the beginning of your script, matches this same execution path:

```

[devops@lb01 ~/scripts/shell]$ ls
111  first.sh  server_performance_stats
[devops@lb01 ~/scripts/shell]$   >  111
[devops@lb01 ~/scripts/shell]$   >  first.sh
-bash: first.sh: Permission denied
# It's not available to clear the bash file 

```

- to edit the first.sh

```
#!/usr/bin/bash
echo "hello world"
```

- output:

```
[devops@lb01 ~/scripts/shell]$ less  ./first.sh
[devops@lb01 ~/scripts/shell]$ sh ./first.sh
hello world
```


---

### ==Variables==

#### Shell variables are created once they are assigned a value. A variable can contain a number, a character or a string of characters. Variable name is case sensitive and can consist of a combination of letters and the underscore "_". Value assignment is done using the "=" sign. Note that no space permitted on either side of = sign when initializing variables.

```
[devops@lb01 ~/scripts/shell]$ PRICE_PER_APPLE=5
[devops@lb01 ~/scripts/shell]$ MyFirstLetters=ABC
[devops@lb01 ~/scripts/shell]$ greeting='hello          world!'

# Referencing the variables
```

#### A backslash `\` is used to escape special character meaning

```
[devops@lb01 ~/scripts/shell]$ PRICE_PER_APPLE=5
[devops@lb01 ~/scripts/shell]$ echo "The price of an Apple today is: \$HK $PRICE_PER_APPLE"
The price of an Apple today is: $HK 5
```


#### Encapsulating the variable name with ${} is used to avoid ambiguity


```
[devops@lb01 ~/scripts/shell]$ MyFirstLetters=ABC
[devops@lb01 ~/scripts/shell]$ echo "The first 10 letters in the alphabet are: ${MyFirstLetters}DEFGHIJKL"
The first 10 letters in the alphabet are: ABCDEFGHIJKL
```

#### Encapsulating the variable name with "" will preserve any white space values

```
[devops@lb01 ~/scripts/shell]$ greeting="hello    world"
[devops@lb01 ~/scripts/shell]$ echo $greeting" now with spaces: $greeting"
hello world now with spaces: hello    world
```

#### Variables can be assigned with the value of a command output. This is referred to as substitution. Substitution can be done by encapsulating the command with `` (known as back-ticks) or with $()

```
[devops@lb01 ~/scripts/shell]$ FileList=$(ls)
[devops@lb01 ~/scripts/shell]$ $(ls)
-bash: 111: command not found
[devops@lb01 ~/scripts/shell]$ FileWithTimeStamp=/tmp/my-dir/file_$(/bin/date +^C
[devops@lb01 ~/scripts/shell]$ which date
/usr/bin/date

[devops@lb01 ~/scripts/shell]$ which date
/usr/bin/date
[devops@lb01 ~/scripts/shell]$ FileWithTimeStamp=/tmp/my-dir/file_$(/usr/bin/date +%Y-%m-%d).txt

# Note that when the script runs, it will run the command inside the $() parenthesis and capture its output.
```

#### Exercise

##### The target of this exercise is to create a string, an integer, and a complex variable using command substitution. The string should be named BIRTHDATE and should contain the text "Jan 1, 2000". The integer should be named Presents and should contain the number 10. The complex variable should be named BIRTHDAY and should contain the full weekday name of the day matching the date in variable BIRTHDATE e.g. Saturday. 

- Note that the 'date' command can be used to convert a date format into a different date format. For example, to convert date value, $date1, to day of the week of date1, use:

```
date -d "$date1" +%A
```

- You can use Shell Check to detective your codes.

```
#!/bin/bash

set -euo pipefail

BIRTHDATE="Jan 1, 2000"

Presents=10

BIRTHDAY=$(date -d "$BIRTHDATE" +%A)


# BIRTHDATE:
if [ "$BIRTHDATE" == "Jan 1, 2000" ]; then
  echo "BIRTHDATE is correct,it is $BIRTHDATE"
else
  echo "BIRTHDATE is incorrect,try again!"
fi

# Presents:
if [ "$Presents" == 10 ]; then
  echo "I have received $Presents presents"
else
  echo "Presents is incorrect,plz try again!"
fi

# BIRTHDAY:
if [ "$BIRTHDAY" == "Saturday" ]; then
  echo "I was born on a $BIRTHDAY"
else
  echo "BIRTHDAY is incorrect,plz try again!"
fi


##############################################
# output:
[devops@lb01 ~/scripts/shell]$ sh ./Birthday.sh
BIRTHDATE is correct,it is Jan 1, 2000
I have received 10 presents
I was born on a Saturday

```


---

### ==Passing Arguments to the Script==


#### Arguments can be passed to the script when it is executed, by writing them as a space-delimited list following the script file name.

#### Inside the script, the $1 variable references the first argument in the command line, $2 the second argument and so forth. The variable $0 references to the current script. In the following example, the script name is followed by 6 arguments.

#### Example

- 4_17.sh file contains below code

```
#!/bin/bash
echo "File name is $0"
echo $3
Data=$5
echo "A $Data costs just $6."
echo $#
echo $@

############################################
# The variable $# holds the number of arguments passed to the script

# The variable $@ holds a space delimited string of all arguments passed to the script

# output:
[devops@lb01 ~/scripts/shell]$ sh ./4_17.sh apple 5 banana 8 "Fruit Basket" 15
File name is ./4_17.sh
banana
A Fruit Basket costs just 15.
6
apple 5 banana 8 Fruit Basket 15

```

#### Exercise

- Pass "Shell is fun" (3 arguments) to the script(prog.sh) as an arguments and print the length of the arguments.

```
#!/usr/bin/bash

function File {
  echo $#
}

if [ ! $# -lt 1 ]; then
  File "$@"
  exit 0
fi

#####################################################
# output:
3


```



---


### Arrays

#### An array can hold several values under one name. Array naming is the same as variables naming. An array is initialized by assign space-delimited values enclosed in ()

```

[devops@lb01 ~/scripts/shell]$ my_array=(apple banana "a pear")
[devops@lb01 ~/scripts/shell]$ new_array[2]=apricot

```

- Array members need not be consecutive or contiguous. Some members of the array can be left uninitialized.

- The total number of elements in the array is referenced by ${#arrayname[@]}

```bash
my_array=(apple banana "Fruit Basket" orange)
echo  ${#my_array[@]}                   # 4
```

- The array elements can be accessed with their numeric index. The index of the first element is 0.
```
[devops@lb01 ~/scripts/shell]$ my_array=(apple banana "Fruit Basket" orange)
[devops@lb01 ~/scripts/shell]$ echo $(my_array[2])
-bash: my_array[2]: command not found

[devops@lb01 ~/scripts/shell]$ echo ${#my_array}
5
[devops@lb01 ~/scripts/shell]$ echo ${#my_array[@]}
4
[devops@lb01 ~/scripts/shell]$
[devops@lb01 ~/scripts/shell]$ echo ${my_array[3]}
orange
[devops@lb01 ~/scripts/shell]$ my_array[4]="carrot"
[devops@lb01 ~/scripts/shell]$ echo $(#my_array[@]}
> ^C
[devops@lb01 ~/scripts/shell]$ echo ${#my_array[@]}
5
[devops@lb01 ~/scripts/shell]$ echo ${my_array[${#my_array[@]}-1]}
carrot

```


#### Exercise

- In this exercise, you will need to add numbers and strings to the correct arrays. You must add the numbers 1,2, and 3 to the "numbers" array, and the words 'hello' and 'world' to the strings array.

- You will also have to correct the values of the variable NumberOfNames and the variable second_name. NumberOfNames should hold the total number of names in the NAMES array, using the $# special variable. Variable second_name should hold the second name in the NAMES array, using the brackets operator [ ]. Note that the index is zero-based, so if you want to access the second item in the list, its index will be 1.

```

#!/bin/bash
numbers=(1 2 3)
string=("hello" "world")
NumberOfNames=${#string[@]}

NAMES=("wangyang" "xiaohong" "xiaoli")
second_name=${NAMES[1]}
echo "numbers: ${#numbers[@]}"
echo "second_name: $second_name"
echo "Numberofnames: $NumberOfNames"

################################################
# output:
[devops@lb01 ~/scripts/shell]$ sh ./4_17.sh
numbers: 3
second_name: xiaohong
Numberofnames: 2

```


---


### Basic Operators

- Arithmetic Operators
- Simple arithmetics on variables can be done using the **arithmetic expression**: $((expression))

```
[devops@lb01 ~/scripts/shell]$ A=3
[devops@lb01 ~/scripts/shell]$ B=$((100 * $A / 5))
[devops@lb01 ~/scripts/shell]$ echo $B
60
```


The basic operators are:

a + b addition (a plus b)

a - b substraction (a minus b)

a * b multiplication (a times b)

a / b division (integer) (a divided by b)

a % b modulo (the integer remainder of a divided by b)

a ** b exponentiation (a to the power of b)

#### Exercise
- In this exercise, you will need to calculate to total cost (variable TOTAL) of a fruit basket, which contains 1 pineapple, 2 bananas and 3 watermelons. Don't forget to include the cost of the basket....

```
#!/bin/bash

COST_PINEAPPLE=50

COST_BANANA=4

COST_WATERMELON=23

COST_BASKET=1

TOTAL=$(($COST_PINEAPPLE + $COST_BANANA + $COST_WATERMELON + $COST_BASKET))

echo "Total Cost is $TOTAL"

#########################################################
# BASH && OUTPUT:
Total Cost is 78




---


### Basic String Operations


