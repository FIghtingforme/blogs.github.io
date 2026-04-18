---

date: 2026-04-16
title: Linux_Commonds
layout: post
catagories: Linux

---


### Linux Commands in Read-World:


### Commands:

```

ls  cp  find  |  xargs  cat  |  grep  |  sort  |  uniq  >  >>

```

串联命令：

```

find  =>  xargs  ==>  cat

```


查找文件 到  处理文件内容   

```

find . -type f -name `"*.txt"`

mkdir -p ./test

ls .

cp ./test ./test.backup

ls ./

cd ./test

touch test{1..3}.txt

ls .



#### The `sort` command is used to sort lines of text files.

#### It's a handy tool for organizing data in files.

```
### vim  4_17.txt
apples,1
kiwis,3
kiwis,4
oranges,3
banana,10

###############################################
### sort 4_17.txt
[devops@lb01 ~/scripts/shell]$ sort ./4_17.txt
apples,1
banana,10
kiwis,3
kiwis,4
oranges,3


#####################################################
### reverse:
[devops@lb01 ~/scripts/shell]$ sort -r ./4_17.txt
oranges,3
kiwis,4
kiwis,3
banana,10
apples,1

######################################################
### number:

[devops@lb01 ~/scripts/shell]$ sort -r -t "," -k 2 -n ./4_17.txt
banana,10
kiwis,4
oranges,3
kiwis,3
apples,1

######################################################

```


#### man ls (for more help.)


#### Alian:To create an alias, use the syntax `alias name='command'`, where `name` is the shortcut you want to use, and `command` is the full command you want to run.

```

[devops@lb01 ~/scripts/shell]$ alias ll='ls -la'
[devops@lb01 ~/scripts/shell]$ ll
total 28
drwxr-xr-x 2 devops devops   122 Apr 15 03:28 .
drwxr-xr-x 3 devops devops    19 Apr 10 17:56 ..
-rw-r--r-- 1 devops devops   247 Apr 15 00:41 4_17.sh
-rw-r--r-- 1 devops devops    45 Apr 15 03:20 4_17.txt
-rw-r--r-- 1 devops devops    36 Apr 14 21:07 first.sh
-rw-r--r-- 1 devops devops    45 Apr 14 06:46 server_performance_stats
-rw-r--r-- 1 devops devops 12288 Apr 14 06:47 .server_performance_stats.swp

### In this example, `ll` lists all files in long format.

###################################################
### git status

$ alias gs="git status"

wang@DESKTOP-IE4JJEG MINGW64 /d/Git/cloud-native-devops-platform (main)
$ gs
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean

############################################
```

#### To copy a file to a remote host, use `scp file user@hostname:/path`:

```
[devops@lb01 ~/scripts/shell]$ scp ./4_17.sh root@10.0.0.6:/
root@10.0.0.6's password:
4_17.sh                                       100%  247   115.1KB/s   00:00

###################################################
# In other machine to detective the file: 
[root@web01 ~]# ip a | grep eth0
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    inet 10.0.0.6/24 brd 10.0.0.255 scope global noprefixroute eth0
[root@web01 ~]# ls
anaconda-ks.cfg  DingDing_py  python.py  ZipFile.py
[root@web01 ~]# ls / | grep 4
4_17.sh
lib64

# Congratuate,it's successful!!!
```



#### Clear the file contents:

```
[devops@lb01 ~/scripts/shell]$  > ./4_17.txt
[devops@lb01 ~/scripts/shell]$
```

---


#### The `awk` command is used for pattern scanning and processing language.

#### It's useful for handling text files and used for data extraction and reporting.

```
# Edit the file:
id,Created,Amount,Currency,Description,Customer
1,2024-11-01,100,USD,Payment,John Doe
2,2024-11-02,200,EUR,Refund,Jane Smith
3,2024-11-03,150,USD,Purchase,Emily Davis
4,2024-11-04,175,GBP,Subscription,Michael Brown

###############################################
# output:
[devops@lb01 ~/scripts/shell]$ less ./4_17.txt | awk -F"," '{print $1}'
id
1
2
3
4

```

---

### Sed
#### The `sed` command is a stream editor used to perform basic text transformations on an input stream (a file or input from a pipeline).

#### It's a powerful tool for making quick edits to files or streams of data.


```
# edit the file:
id,Created,Amount,Currency,Description,Customer
1,2024-11-01,100,USD,Payment,John Doe
2,2024-11-02,200,EUR,Refund,Jane Smith
3,2024-11-03,150,USD,Purchase,Emily Davis
4,2024-11-04,175,GBP,Subscription,Michael Brown

####################################################
# replace "2026"
[devops@lb01 ~/scripts/shell]$ sed 's/2024/2026/' ./4_17.txt
id,Created,Amount,Currency,Description,Customer
1,2026-11-01,100,USD,Payment,John Doe
2,2026-11-02,200,EUR,Refund,Jane Smith
3,2026-11-03,150,USD,Purchase,Emily Davis
4,2026-11-04,175,GBP,Subscription,Michael Brown


#################################################

```


#### Options

The `sed` command has options to change how it works:

- `-i` - Edit files directly without needing to save separately
- `-e` - Add the script to the commands to be executed
- `-n` - Don't automatically print lines
- `-r` - Use extended regular expressions
- `-f` - Add script from a file
- `-l` - Specify line length for `l` command




#### Suppress Printing  

- The `-n` option suppresses automatic printing of pattern space.

- By default, `sed` prints each line of input to the output. Using `-n` allows you to control which lines are printed, typically with the `p` command.

```
[devops@lb01 ~/test]$ sed -n 's/sa/saaaa/p' ./test1.txt
saaaafj
saaaalj

# less test1.txt
safj
salj
aslkj


[devops@lb01 ~/test]$ sed -n 's/as/aaa/p' ./test1.txt
aaalkj
[devops@lb01 ~/test]$ less ./test1.txt
[devops@lb01 ~/test]$ cat ./test1.txt
safj
salj
aslkj
```


#### Extended Regular Expressions

- The `-r` option allows the use of extended regular expressions, which provide more powerful pattern matching capabilities than basic regular expressions.

- Without this option, `sed` uses basic regular expressions.

```
# text
safj
salj
aslkj

#######################################
[devops@lb01 ~/test]$ sed -r 's/(safj|salj)/aaaa/g' test1.txt
aaaa
aaaa
aslkj
```


#### Script from a File

- The `-f` option allows you to add a script from a file, which is useful for executing complex or multiple `sed` commands.

- Without this option, you must specify the script directly in the command line.



```
# Content of `script.sed` file:  
s/salj/saaalj/g


# text
safj
salj
aslkj

################################################
# sed -f ./script.sed ./test1.txt
[devops@lb01 ~/test]$ sed -f ./script.sed ./test1.txt
safj
saaalj
aslkj
```

#### Specify Line Length

- The `-l` option specifies the line length for the `l` command, which prints lines with non-printable characters.

- This option is useful for formatting output when dealing with long lines.

```
# text:
alfjioaj#!$ 5345 $%# joi540238709@%%9 345%$$( lkj435 %$%

# sed -l 10 'l' ./test1.txt
[devops@lb01 ~/test]$ sed -l 10 'l' ./test1.txt
alfjioaj#\
!$ 5345 $\
%# joi540\
238709@%%\
9 345%$$(\
 lkj435 %\
$%$
alfjioaj#!$ 5345 $%# joi540238709@%%9 345%$$( lkj435 %$%

```

- This option appends a `$` at the end of each line to indicate the end of the line.
- 默认 `$` 换行,即使指定宽度 -1 N 也要遵守。


###### Redirect Output to a File

- To save the changes made by `sed` to a file, you can redirect the output to a new file. This is useful when you don't want to overwrite the `original file` .


```
# sed 's/a/aaaaaa/g' ./test1.txt > ./test2.txt

[devops@lb01 ~/test]$ sed 's/a/aaaaaa/g' ./test1.txt > ./test2.txt
[devops@lb01 ~/test]$ less ./test2.txt
aaaaaalfjioaaaaaaj#!$ 5345 $%# joi540238709@%%9 345%$$( lkj435 %$%
```


#### Using `sed` for Advanced Text Processing

- Sed can perform advanced text processing tasks. For example, `sed 's/^/Prefix: /' example_text.txt` adds a prefix to each line.


```
# text:
alfjioaj#!$ 5345 $%# joi540238709@%%9 345%$$( lkj435 %$%
safdsafsd
arewg
fdsbsfdg


[devops@lb01 ~/test]$ sed 's/^/prefix: /g' ./test1.txt
prefix: alfjioaj#!$ 5345 $%# joi540238709@%%9 345%$$( lkj435 %$%
prefix: safdsafsd
prefix: arewg
prefix: fdsbsfdg

```

#### Common Errors and Troubleshooting

#### When using `sed`, you might encounter errors such as:

- `"sed: command garbled" `- Check your command syntax.
- `"sed: can't read file"` - Ensure the file path is correct and accessible.

- Debugging tips include using `echo` to print intermediate results and verify command logic.


















```


