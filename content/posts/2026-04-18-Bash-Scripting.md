---

date: 2026-04-18
title:  "Bash Scripting Documetry"
categories: ["Linux"]

---


## Bash Scripting  


### Bash: `Bourne Again Shell`  

---



### Bash Syntax for Scripting

```

Bash scripts are sequences of commands executed by the Bash shell. 
They automate tasks and can be used to perform complex operations. Understanding Bash syntax is crucial for writing effective scripts.

```

### Basic Syntax
- Here are some basic rules for using Bash in scripts:

```

Comments: Comments start with a # and Bash ignores them.
Command Order: Commands run one after the other, from top to bottom.
Semicolons: Use ; to run multiple commands on the same line.
Let's go through them one by one with examples.

```

### This script prints a greeting message

```

  [devops@lb01 ~]$ # This script prints a greeting message!
  [devops@lb01 ~]$ echo "hello!!"
  echo "hello# This script prints a greeting message!"
  hello# This script prints a greeting message!

```




### Best Practices for Writing Scripts
- Here are some tips for writing clean and efficient scripts:

```

  Use comments to explain your code.
  Choose meaningful variable names.
  Test your scripts thoroughly before using them in production.

```





### Introduction to Bash Scripting  
  
- Bash scripts are files containing commands that you run in the terminal.   
- They automate tasks and make your work more efficient.  


### Creating a Bash Script  

- To create a script, start with the shebang `#!` followed by the path to Bash, usually `/bin/bash`. Make sure your script has execute permissions.  

#### Example: Simple Bash Script  

```

#!/bin/bash
# This script prints a greeting message
echo "Hello, World!"

```


#### Using Variables in Scripts

- Variables store data that your script can use. Assign values using the `=` sign without spaces.

#### Example: Using Variables

```

[devops@lb01 ~]$ name="dslfaj"
[devops@lb01 ~]$ echo "hello,$name"
hello,dslfaj

======================================
#!/bin/bash
# Assign a value to a variable 
name="World" 
echo "Hello, $name!"

```


### Bash Variables


#### Understanding Variables in Bash  

Variables in Bash are used to store data that can be used and manipulated throughout your script or command-line session. 
Bash variables are untyped, meaning they can hold any type of data.


### Declaring Variables

Variables are declared by simply assigning a value to a name. There should be no spaces around the equal sign:

- `variable_name=value`
- To access the value of a variable, prefix it with a dollar sign: `$variable_name`

### Example

```

name="John Doe"
echo "Hello, $name!"
number=42
echo "The number is $number"

```


### Environment Variables

Environment variables are special variables that affect the way processes run on your system. They are often used to store system-wide values like the location of executables or the default editor.

### Example: Using Environment Variables

```
# shell script:
# Display the PATH environment variable
echo "Your PATH is $PATH"

[devops@lb01 ~]$ vim ./shell.sh
[devops@lb01 ~]$ bash ./shell.sh
your PATH is /home/devops/.local/bin:/home/devops/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin

```

### Local vs. Global Variables

Local variables are only available within the block of code in which they are defined, such as within a function. Global variables are accessible from anywhere in the script.

### Example: Local Variable

```
# shell script
# Define a local variable in a function
my_function() {
  local local_var="I am local variable"
  echo $local_var
}
my_function


[devops@lb01 ~]$ vim ./shell.sh
[devops@lb01 ~]$ bash ./shell.sh
I am local variable

```


### Common Variable Operations  

Variables can be used in various operations, such as concatenation and arithmetic.  

- **Concatenation:** Combine strings using variables.  
- **Arithmetic:** Perform calculations using variables.  

### Example: Variable Operations  

```
# shell script:
#!/usr/bin/bash

# Concatenation
greeting="hello"
name="world"
echo "$greeting,$name!!!"

# Arithmetic
num1=1
num2=2
sum=$((num1+num2))
echo "The sum is $sum"

======================================

[devops@lb01 ~]$ bash ./shell.sh
hello,world!!!
The sum is 3

```




### Bash Data Types


### Understanding Bash Data Types

This section introduces the different data types available in Bash scripting.


### Strings

Strings are sequences of characters used to store text. 
They can be manipulated using various string operations such as concatenation and substring extraction.

### Example: Strings

```

#!/usr/bin/bash
# string example:
greeting="hello,world!"
name="Alice"
full_greeting="$greeting,$name"
echo $full_greeting


[devops@lb01 ~]$ bash ./shell.sh
hello,world!,Alice

```


### Numbers

Numbers in Bash can be used for arithmetic operations. 
Bash supports integer arithmetic natively, such as addition, subtraction, multiplication, and division.

### Example: Numbers

```

# shell script:
#!/usr/bin/bash
# Number example:
num1=1
num2=2
sum=$((num1 + num2))
difference=$((num2 - num1))
product=$((num1 * num2))
quotient=$((num1 / num2))
echo "Sum: $sum; Difference: $difference; Product: $product; Quotient: $quotient
"


[devops@lb01 ~]$ bash ./shell.sh
Sum: 3; Difference: 1; Product: 2; Quotient: 0

```

### Arrays

Arrays are used to store multiple values in a single variable. 
Each element in an array is accessed using an index. You can iterate over arrays and modify elements.

### Example: Arrays

```

#!/usr/bin/bash
# Array example
fruits=("apple" "banana" "orange")
for fruit in "${fruits[@]}"; do
  echo $fruit
done


[devops@lb01 ~]$ bash ./shell.sh
apple
banana
orange

```


### Associative Arrays

Associative arrays allow you to use named keys to access values.
They are similar to dictionaries in other programming languages. 
You can add or remove keys and values.

### Example: Associative Arrays

```

#!/usr/bin/bash
# Associative array example
declare -A colors
colors[apple]="red"
colors[banana]="yellow"
colors[grape]="purple"
unset colors[banana]
echo ${colors[apple]}  #red
echo ${colors[grape]}  #purple
echo ${colors[banana]}



[devops@lb01 ~]$ bash ./shell.sh
red
purple
# `nothing`
=============================================

```

### Data Type Limitations

Bash does not support floating-point arithmetic natively. 
For such operations, consider using external tools like `bc` or `awk`.



### Bash Operators


### Understanding Bash Operators

This section provides an overview of operators used in Bash scripting, including comparison, string, arithmetic, logical, and file test operators.


### Comparison Operators

- `-eq`: Equal to
- `-ne`: Not equal to
- `-lt`: Less than
- `-le`: Less than or equal to
- `-gt`: Greater than
- `-ge`: Greater than or equal to


### String Comparison Operators

- `=`: Equal to
- `!=`: Not equal to
- `<`: Less than, in ASCII alphabetical order
- `>`: Greater than, in ASCII alphabetical order


### Arithmetic Operators

- `+`: Addition
- `-`: Subtraction
- `*`: Multiplication
- `/`: Division
- `%`: Modulus (remainder of division)
- For exponentiation, use external tools like `bc` or `awk`.


### Logical Operators

- `&&`: Logical AND
- `||`: Logical OR
- `!`: Logical NOT



### File Test Operators

- `-e`: Checks if a file exists
- `-d`: Checks if a directory exists
- `-f`: Checks if a file is a regular file
- `-s`: Checks if a file is not empty




### Bash If...Else


### Using If...Else Statements in Bash

This section explains how to use conditional statements in Bash scripting.


### If Statements

If statements allow you to execute code based on a condition. 
If the condition is true, the code block will run.

The condition is enclosed in square brackets `[ ]` and the statement ends with `fi`, which is `if` spelled backward, marking the end of the if block.

### Example: If Statement

```

# shell script:
# Basic if statement
#!/usr/bin/bash
num=5
if [ $num -gt 1 ]; then
  echo "Number is greater than 1"
fi



[devops@lb01 ~]$ bash ./shell.sh
Number is greater than 1

```



### If...Else Statements

If...else statements provide a way to execute one block of code if a condition is true and another block if it is false.

The `else` keyword introduces the alternative block, and the statement ends with `fi`.

### Example: If...Else Statement

```

#!/usr/bin/bash
num=10
if [ $num -gt 100 ]; then
  echo "Number is greater than 1"
else
  echo "Number is less than 1"
fi


[devops@lb01 ~]$ bash ./shell.sh
Number is less than 1


```

### Elif Statements

Elif statements allow you to check multiple conditions in sequence. 
If the first condition is false, the next one is checked.

- The `elif` keyword stands for "else if," and the statement still ends with `fi`.

### Example: Elif Statement

```

#!/usr/bin/bash
num=10
if [ $num -gt 10 ]; then
  echo "$num is greater than 1"
elif [ $num -lt 10 ]; then
  echo "$num is less than 1"
else
  echo "$num is equal to 10"
fi



[devops@lb01 ~]$ bash shell.sh
10 is equal to 10

```



### Nested If Statements

Nested if statements allow you to place an if statement inside another if statement, enabling more complex logic.

Each if block must be closed with its own `fi`.

### Example: Nested If Statement

```bash

#!/usr/bin/bash
num=10
if [ $num -lt 100 ]; then
  if [ $num -gt 5 ]; then
    echo "The num $num is between 5 and 100"
  fi
fi




[devops@lb01 ~]$ bash shell.sh
The num 10 is between 5 and 100

```

### Bash Loops


#### Using Loops in Bash

- This section covers the use of loops in Bash scripting, including for, while, and until loops.


#### For Loops

- For loops allow you to iterate over a list of items or a range of numbers. They are useful for repeating tasks a specific number of times.

- The `for` keyword is followed by a variable name, a range of values, and a `do` keyword, which marks the start of the loop block.

#### Example: For Loop

```

# Text:
#!/usr/bin/bash
for i in {1..5}; do
  echo "hello!!"
done


[devops@lb01 ~/Scripts/shell]$ bash ./first.sh
hello!!
hello!!
hello!!
hello!!
hello!!

```


#### While Loops

- While loops execute a block of code as long as a specified condition is true.

- They are useful for tasks that need to repeat until a certain condition changes.

- The condition is enclosed in square brackets `[ ]`, and the loop ends with `done`.

#### Example: While Loop

```bash

# Text:
#!/usr/bin/bash
count=1
while [ $count -le 5 ]; do
  echo "Count is $count"
  ((count++))
done


[devops@lb01 ~/Scripts/shell]$ bash first.sh
Count is 1
Count is 2
Count is 3
Count is 4
Count is 5


```

#### Until Loops

- Until loops are similar to while loops, but they execute until a specified condition becomes true.

- The condition is enclosed in square brackets `[ ]`, and the loop ends with `done`.

#### Example: Until Loop

```bash

#!/usr/bin/bash
count=1
until [ $count -gt 5 ]; do
  echo "The count is $count"
  ((count++))
done


[devops@lb01 ~/Scripts/shell]$ bash first.sh
The count is 1
The count is 2
The count is 3
The count is 4
The count is 5

```


#### Break and Continue

- Break and continue statements are used to control loop execution. `break` exits the loop, while `continue` skips to the next iteration.

- These statements are typically used inside conditional blocks to alter the flow of the loop.

#### Example: Break and Continue

```bash

#!/usr/bin/bash
for i in {a..z}; do
  if [ "$i" == "f" ]; then
    continue
  fi
  if [ "$i" == "l" ]; then
    break
  fi
  echo "The letter is $i"
done



[devops@lb01 ~/Scripts/shell]$ bash first.sh
The letter is a
The letter is b
The letter is c
The letter is d
The letter is e
The letter is g
The letter is h
The letter is i
The letter is j
The letter is k

```


#### Nested(嵌套) Loops

- Nested loops allow you to place one loop inside another, enabling more complex iteration patterns.

- Each loop must be closed with its own `done`.

#### Example: Nested Loops

```bash

# text：
#!/usr/bin/bash
for i in {1..3}; do
  for j in {1..2}; do
    echo "Outer loop $i;Inner loop is $j"
  done
done


[devops@lb01 ~/Scripts/shell]$ bash !$
bash first.sh
Outer loop 1;Inner loop is 1
Outer loop 1;Inner loop is 2
Outer loop 2;Inner loop is 1
Outer loop 2;Inner loop is 2
Outer loop 3;Inner loop is 1
Outer loop 3;Inner loop is 2

```



---


### Bash Functions


#### Defining Functions

- To define a function in Bash, use the following syntax. The function name is followed by parentheses, and the function body is enclosed in curly braces:

#### Example: Define a Function

```bash

my_function() {
  echo "Hello, World!"
}

```

- It's a good practice to use descriptive names for functions.


#### Calling Functions

- In Bash, execute (or call) a function by using its name.

- Functions can be called multiple times, which helps in reusing code:

#### Example: Call a Function

```bash

my_function

```

#### Advanced Function Features

- Functions can accept arguments, return values, and use local variables. 
- Here's an example of a function that takes an argument and uses a local variable:

#### Example: Advanced Function Features

```bash

# text：

#!/usr/bin/bash
greeting() {
  local name=$1
  echo "hello!!My name is $name."
}

greeting "Machal Json"





[devops@lb01 ~/Scripts/shell]$ bash first.sh
hello!!My name is Machal Json.

```

- You can also return values from functions using `echo` or the `return` statement:

#### Example: Return Values

```bash
add() {
  local sum=$(($1 + $2))
  echo $sum
}
result=$(add 5 3)
echo "The sum is $result"


[devops@lb01 ~/Scripts/shell]$ bash first.sh
The sum is 8


```



---


### Bash Arrays



#### Working with Arrays in Bash

- This section explains how to create and manipulate arrays in Bash scripts.

- Arrays allow you to store multiple values in a single variable, making data management easier.



#### Creating Arrays

- To create an array in Bash, use the following syntax.

- Arrays can store multiple values, and each value is indexed starting from zero:

#### Example: Create an Array

```bash
my_array=("value1" "value2" "value3")
```

- Use descriptive names for arrays to indicate their purpose.



#### Accessing Array Elements

- To access elements in a Bash array, use the index of the element.

- The index is specified in square brackets:

#### Example: Access Array Elements

```bash

[devops@lb01 ~/Scripts/shell]$ my_array=("value1" "value2" "value3")
[devops@lb01 ~/Scripts/shell]$ echo ${my_array[2]}
value3
[devops@lb01 ~/Scripts/shell]$ echo ${my_array[0]}
value1

```

#### Modifying Array Elements

- You can modify elements in a Bash array by specifying the index and assigning a new value:

#### Example: Modify Array Elements

```bash

[devops@lb01 ~/Scripts/shell]$ my_array[1]="apple"
[devops@lb01 ~/Scripts/shell]$ echo ${my_array[1]}
apple

```



---


### Bash `crontab` Command - Schedule Tasks



#### Understanding Cron and Crontab

- The `cron` system is a time-based job scheduler in Unix-like operating systems.

- It automates the execution of tasks (known as cron jobs) at specified intervals.

- While `cron` is the background service that runs these tasks, `crontab` is the command used to manage them.

- There is no direct "cron" command; instead, you use `crontab` to set up and control cron jobs.



#### Using Crontab

- The `crontab` command allows you to define scheduled tasks.

- These tasks are specified in a crontab file, which is a simple text file containing a list of commands meant to be run at specified times.

#### Crontab Syntax

- The basic syntax of the `crontab` command is:

```bash
crontab [options]
```

#### Options

- `-e`: Edit the crontab file for the current user.
- `-l`: List the crontab entries for the current user.
- `-r`: Remove the crontab file for the current user.



#### Setting Up Cron Jobs

- Cron jobs are defined using a specific syntax in the crontab file. Each line in the file represents a task and follows this format:

```bash
* * * * * command_to_execute
```

- **Minute**: 0-59
- **Hour**: 0-23
- **Day of Month**: 1-31
- **Month**: 1-12
- **Day of Week**: 0-7 (0 and 7 are Sunday)

- Each asterisk can be replaced with a specific value or range to schedule the command at specific times.

#### Example: Schedule a Task

- To run a script every day at midnight, you would use:

```bash
0 0 * * * /path/to/script.sh
```

- This entry will execute `/path/to/script.sh` every day at 00:00 (midnight).



#### Common Uses

##### Cron jobs are commonly used to:

- Automate system maintenance tasks, like backups and updates.
- Schedule scripts to run at specific times or intervals.
- Perform regular monitoring and reporting tasks.





```

- Shell programming using other common shells such as sh, csh, tcsh, will also be referenced, as they sometime differ from bash.


- Shell programming can be accomplished by directly executing shell commands at the shell prompt or by storing them in the order of execution, in a text file, called a shell script, and then executing the shell script. To execute, simply write the shell script file name, once the file has execute permission (chmod +x filename).

- The first line of the shell script file begins with a "sha-bang" (#!) which is not read as a comment, followed by the full path where the shell interpreter is located. This path, tells the operating system that this file is a set of commands to be fed into the interpreter indicated. Note that if the path given at the "sha-bang" is incorrect, then an error message e.g. "Command not found.", may be the result of the script execution. It is common to name the shell script with the ".sh" extension. The first line may look like this:

```


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

```

bash
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

```


---


### Basic String Operations



