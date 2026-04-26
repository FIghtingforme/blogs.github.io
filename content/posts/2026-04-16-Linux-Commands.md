---

date: 2026-04-16
title: "Linux Common Commands"
categories: Linux

---


### Linux Commands in Read-World:


#### Commands:

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



####The `sort` command is used to sort lines of text files.

####It's a handy tool for organizing data in files.

```
###vim  4_17.txt
apples,1
kiwis,3
kiwis,4
oranges,3
banana,10

###############################################
###sort 4_17.txt
[devops@lb01 ~/scripts/shell]$ sort ./4_17.txt
apples,1
banana,10
kiwis,3
kiwis,4
oranges,3


#####################################################
###reverse:
[devops@lb01 ~/scripts/shell]$ sort -r ./4_17.txt
oranges,3
kiwis,4
kiwis,3
banana,10
apples,1

######################################################
###number:

[devops@lb01 ~/scripts/shell]$ sort -r -t "," -k 2 -n ./4_17.txt
banana,10
kiwis,4
oranges,3
kiwis,3
apples,1

######################################################

```


####man ls (for more help.)


####Alian:To create an alias, use the syntax `alias name='command'`, where `name` is the shortcut you want to use, and `command` is the full command you want to run.

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

###In this example, `ll` lists all files in long format.

###################################################
###git status

$ alias gs="git status"

wang@DESKTOP-IE4JJEG MINGW64 /d/Git/cloud-native-devops-platform (main)
$ gs
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean

############################################
```

####To copy a file to a remote host, use `scp file user@hostname:/path`:

```
[devops@lb01 ~/scripts/shell]$ scp ./4_17.sh root@10.0.0.6:/
root@10.0.0.6's password:
4_17.sh                                       100%  247   115.1KB/s   00:00

###################################################
#In other machine to detective the file: 
[root@web01 ~]#ip a | grep eth0
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    inet 10.0.0.6/24 brd 10.0.0.255 scope global noprefixroute eth0
[root@web01 ~]#ls
anaconda-ks.cfg  DingDing_py  python.py  ZipFile.py
[root@web01 ~]#ls / | grep 4
4_17.sh
lib64

#Congratuate,it's successful!!!
```



####Clear the file contents:

```
[devops@lb01 ~/scripts/shell]$  > ./4_17.txt
[devops@lb01 ~/scripts/shell]$
```

---


####The `awk` command is used for pattern scanning and processing language.

####It's useful for handling text files and used for data extraction and reporting.

```
#Edit the file:
id,Created,Amount,Currency,Description,Customer
1,2024-11-01,100,USD,Payment,John Doe
2,2024-11-02,200,EUR,Refund,Jane Smith
3,2024-11-03,150,USD,Purchase,Emily Davis
4,2024-11-04,175,GBP,Subscription,Michael Brown

###############################################
#output:
[devops@lb01 ~/scripts/shell]$ less ./4_17.txt | awk -F"," '{print $1}'
id
1
2
3
4

```

---

###Sed
####The `sed` command is a stream editor used to perform basic text transformations on an input stream (a file or input from a pipeline).

####It's a powerful tool for making quick edits to files or streams of data.


```
#edit the file:
id,Created,Amount,Currency,Description,Customer
1,2024-11-01,100,USD,Payment,John Doe
2,2024-11-02,200,EUR,Refund,Jane Smith
3,2024-11-03,150,USD,Purchase,Emily Davis
4,2024-11-04,175,GBP,Subscription,Michael Brown

####################################################
#replace "2026"
[devops@lb01 ~/scripts/shell]$ sed 's/2024/2026/' ./4_17.txt
id,Created,Amount,Currency,Description,Customer
1,2026-11-01,100,USD,Payment,John Doe
2,2026-11-02,200,EUR,Refund,Jane Smith
3,2026-11-03,150,USD,Purchase,Emily Davis
4,2026-11-04,175,GBP,Subscription,Michael Brown


#################################################

```


####Options

The `sed` command has options to change how it works:

- `-i` - Edit files directly without needing to save separately
- `-e` - Add the script to the commands to be executed
- `-n` - Don't automatically print lines
- `-r` - Use extended regular expressions
- `-f` - Add script from a file
- `-l` - Specify line length for `l` command




####Suppress Printing  

- The `-n` option suppresses automatic printing of pattern space.

- By default, `sed` prints each line of input to the output. Using `-n` allows you to control which lines are printed, typically with the `p` command.

```

[devops@lb01 ~/test]$ sed -n 's/sa/saaaa/p' ./test1.txt
saaaafj
saaaalj

#less test1.txt
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


####Extended Regular Expressions

- The `-r` option allows the use of extended regular expressions, which provide more powerful pattern matching capabilities than basic regular expressions.

- Without this option, `sed` uses basic regular expressions.

```

#text
safj
salj
aslkj

#######################################
[devops@lb01 ~/test]$ sed -r 's/(safj|salj)/aaaa/g' test1.txt
aaaa
aaaa
aslkj

```


####Script from a File

- The `-f` option allows you to add a script from a file, which is useful for executing complex or multiple `sed` commands.

- Without this option, you must specify the script directly in the command line.



```

#Content of `script.sed` file:  
s/salj/saaalj/g


#text
safj
salj
aslkj

################################################
#sed -f ./script.sed ./test1.txt
[devops@lb01 ~/test]$ sed -f ./script.sed ./test1.txt
safj
saaalj
aslkj

```

####Specify Line Length

- The `-l` option specifies the line length for the `l` command, which prints lines with non-printable characters.

- This option is useful for formatting output when dealing with long lines.

```

#text:
alfjioaj#!$ 5345 $%#joi540238709@%%9 345%$$( lkj435 %$%

#sed -l 10 'l' ./test1.txt
[devops@lb01 ~/test]$ sed -l 10 'l' ./test1.txt
alfjioaj#\
!$ 5345 $\
%#joi540\
238709@%%\
9 345%$$(\
 lkj435 %\
$%$
alfjioaj#!$ 5345 $%#joi540238709@%%9 345%$$( lkj435 %$%

```

- This option appends a `$` at the end of each line to indicate the end of the line.
- 默认 `$` 换行,即使指定宽度 -1 N 也要遵守。


######Redirect Output to a File

- To save the changes made by `sed` to a file, you can redirect the output to a new file. This is useful when you don't want to overwrite the `original file` .


```

#sed 's/a/aaaaaa/g' ./test1.txt > ./test2.txt

[devops@lb01 ~/test]$ sed 's/a/aaaaaa/g' ./test1.txt > ./test2.txt
[devops@lb01 ~/test]$ less ./test2.txt
aaaaaalfjioaaaaaaj#!$ 5345 $%#joi540238709@%%9 345%$$( lkj435 %$%

```


####Using `sed` for Advanced Text Processing

- Sed can perform advanced text processing tasks. For example, `sed 's/^/Prefix: /' example_text.txt` adds a prefix to each line.


```

#text:
alfjioaj#!$ 5345 $%#joi540238709@%%9 345%$$( lkj435 %$%
safdsafsd
arewg
fdsbsfdg


[devops@lb01 ~/test]$ sed 's/^/prefix: /g' ./test1.txt
prefix: alfjioaj#!$ 5345 $%#joi540238709@%%9 345%$$( lkj435 %$%
prefix: safdsafsd
prefix: arewg
prefix: fdsbsfdg

```

####Common Errors and Troubleshooting

####When using `sed`, you might encounter errors such as:

- `"sed: command garbled" `- Check your command syntax.
- `"sed: can't read file"` - Ensure the file path is correct and accessible.

- Debugging tips include using `echo` to print intermediate results and verify command logic.


###rmdir

```
 `rmdir` is a command-line utility used to remove empty directories. It's a straightforward tool that helps in cleaning up directory structures by deleting directories that no longer contain any files or subdirectories. The command will return an error if the directory is not empty.
```



###Bash `cut` Command - Remove Sections from Lines

####The `cut` command is used to remove sections from each line of files.

####It's a useful tool for extracting specific fields of data from a file or output stream.

####All examples below use the `example.txt` file:

```
Kai Refsnes 30,Norway 
Robin Smith 25,Denmark 
Sienna Davis 40,Germany
```


####Basic Usage

- To extract the first field of a file, use `cut -f1 example.txt`:

```
[devops@lb01 ~]$ cut -f1 example.txt
Kai
Robin
Sienna
```

- By default, `cut` uses a tab as the delimiter.

####Options

#####The `cut` command has options to change how it works:

- `-d` - Choose what separates the fields
- `-f` - Select specific fields to display
- `--complement` - Show all fields except the selected ones


####Specify a Delimiter

- The `-d` option allows you to choose what separates the fields.

```
#text:
Kai     Refsnes 30,Norway
Robin   Smith   25,Denmark
Sienna  Davis   40,Germany

[devops@lb01 ~]$ cut -d ',' -f2 example.txt
Norway
Denmark
Germany
```

####Select Specific Fields

- The `-f` option allows you to select specific fields to display.

```
#text:
Kai     Refsnes 30,Norway
Robin   Smith   25,Denmark
Sienna  Davis   40,Germany


[devops@lb01 ~]$ cut -f1-2 example.txt
Kai     Refsnes
Robin   Smith
Sienna  Davis

```


####Show Complement

The `--complement` option allows you to show all fields except the selected ones.

```

#text:
Kai     Refsnes 30,Norway
Robin   Smith   25,Denmark
Sienna  Davis   40,Germany


[devops@lb01 ~]$ cut -f1 --complement example.txt
Refsnes 30,Norway
Smith   25,Denmark
Davis   40,Germany

```



####Advanced Field Extraction

- Cut can perform advanced field extraction tasks.

- For example, `cut -d   -f2-3 example_data.txt` extracts fields 2 through 3 from the file.


```

#text:

Kai     Refsnes 30,Norway
Robin   Smith   25,Denmark
Sienna  Davis   40,Germany

[devops@lb01 ~]$ cut -d ' ' -f2-3 example.txt
        Refsnes 30,Norway
Robin   Smith   25,Denmark
Sienna  Davis   40,Germany

```



####Common Errors and Troubleshooting

#####When using `cut`, you might encounter errors such as:

- "cut: delimiter must be a single character" - Ensure the delimiter is correctly specified.
- "cut: fields and positions are numbered from 1" - Remember that field and position numbering starts at 1.

####Debugging tips include checking the delimiter and field specifications to ensure they match the file's format.


---


###Bash `sort` Command - Sort Lines of Text Files


####Using the `sort` Command

#####The `sort` command is used to sort lines of text files.

#####It's a handy tool for organizing data in files.




### Bash `tail` Command - Display Last Part of Files



- The `tail` command is used to display the last part of files.

- It's particularly useful for viewing the end of log files or any file that is being updated in real-time.

#### Syntax

- The basic syntax of the `tail` command is:

```bash
tail [OPTION]... [FILE]...
```

### Example

```

[devops@lb01 ~]$ tail /etc/passwd
sssd:x:998:997:User for sssd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
chrony:x:997:996:chrony system user:/var/lib/chrony:/sbin/nologin
rsync:x:1000:1000::/home/rsync:/sbin/nologin
ftpuser:x:1001:1001::/home/ftpuser:/bin/bash
devops:x:1002:1002::/home/devops:/bin/bash
deploy_bot:x:1003:1003::/home/deploy_bot:/bin/bash
sync_runner:x:996:994::/opt/alist_sync:/sbin/nologin
nginx:x:995:993:Nginx web server:/var/lib/nginx:/sbin/nologin
sre_share:x:1004:1005::/home/sre_share:/sbin/nologin

```


#### Options

##### The `tail` command has several options to customize its behavior:

- `-n [number]`: Display the last [number] lines of the file.
- `-f`: Follow the file as it grows, useful for monitoring log files.
- `-c [number]`: Display the last [number] bytes of the file.
- `--pid=[pid]`: Terminate after the process with the given PID dies.
- `--retry`: Keep trying to open a file even if it is inaccessible.



#### Option: -n [number]

- The `-n` option allows you to specify the number of lines to display from the end of the file.

- By default, `tail` shows the last 10 lines.

#### Example: Display Last 5 Lines

```

[devops@lb01 ~]$ tail -n 5 /etc/passwd
devops:x:1002:1002::/home/devops:/bin/bash
deploy_bot:x:1003:1003::/home/deploy_bot:/bin/bash
sync_runner:x:996:994::/opt/alist_sync:/sbin/nologin
nginx:x:995:993:Nginx web server:/var/lib/nginx:/sbin/nologin
sre_share:x:1004:1005::/home/sre_share:/sbin/nologin

```

#### Option: -f

The `-f` option is used to follow a file as it grows, which is particularly useful for monitoring log files in real-time.

#### Example: Follow Log File

```


[devops@lb01 ~]$ tail -f /etc/passwd
sssd:x:998:997:User for sssd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
chrony:x:997:996:chrony system user:/var/lib/chrony:/sbin/nologin
rsync:x:1000:1000::/home/rsync:/sbin/nologin
ftpuser:x:1001:1001::/home/ftpuser:/bin/bash
devops:x:1002:1002::/home/devops:/bin/bash
deploy_bot:x:1003:1003::/home/deploy_bot:/bin/bash
sync_runner:x:996:994::/opt/alist_sync:/sbin/nologin
nginx:x:995:993:Nginx web server:/var/lib/nginx:/sbin/nologin
sre_share:x:1004:1005::/home/sre_share:/sbin/nologin

```


#### Option: -c [number]

- The `-c` option allows you to display the last [number] bytes of a file instead of lines.

#### Example: Display Last 20 Bytes

```

[devops@lb01 ~]$ tail -c 20 /etc/passwd
share:/sbin/nologin
[devops@lb01 ~]$ tail -c 200 /etc/passwd
003::/home/deploy_bot:/bin/bash
sync_runner:x:996:994::/opt/alist_sync:/sbin/nologin
nginx:x:995:993:Nginx web server:/var/lib/nginx:/sbin/nologin
sre_share:x:1004:1005::/home/sre_share:/sbin/nologin

```


#### Option: --pid=[pid]

The `--pid` option terminates tailing after the process with the given PID dies. This is useful for stopping the tail operation when a related process ends.

#### Example: Terminate After Process Ends

- 可以在另一个终端用vim新开文件，ps -ef 获取vim的进程号，用他的进程号来taile --pid /etc/passwd，当vim关闭时进程号消失随即tail 不再运行

```

[devops@lb01 ~]$ ps -ef | grep 310599
devops    310599  310521  0 12:33 pts/1    00:00:00 vim ./aaa.txt
devops    310712  305903  0 12:37 pts/0    00:00:00 grep --color=auto 310599
[devops@lb01 ~]$
[devops@lb01 ~]$
[devops@lb01 ~]$
[devops@lb01 ~]$ tail -f --pid=310599 /etc/passwd
sssd:x:998:997:User for sssd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
chrony:x:997:996:chrony system user:/var/lib/chrony:/sbin/nologin
rsync:x:1000:1000::/home/rsync:/sbin/nologin
ftpuser:x:1001:1001::/home/ftpuser:/bin/bash
devops:x:1002:1002::/home/devops:/bin/bash
deploy_bot:x:1003:1003::/home/deploy_bot:/bin/bash
sync_runner:x:996:994::/opt/alist_sync:/sbin/nologin
nginx:x:995:993:Nginx web server:/var/lib/nginx:/sbin/nologin
sre_share:x:1004:1005::/home/sre_share:/sbin/nologin

```

#### Option: --retry

- The `--retry` option makes `tail` keep trying to open a file even if it is inaccessible. This is useful for files that may be temporarily unavailable.

#### Example: Retry Opening File

```

[devops@lb01 ~]$ tail --retry /etc/passwd
tail: warning: --retry ignored; --retry is useful only when following
sssd:x:998:997:User for sssd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
chrony:x:997:996:chrony system user:/var/lib/chrony:/sbin/nologin
rsync:x:1000:1000::/home/rsync:/sbin/nologin
ftpuser:x:1001:1001::/home/ftpuser:/bin/bash
devops:x:1002:1002::/home/devops:/bin/bash
deploy_bot:x:1003:1003::/home/deploy_bot:/bin/bash
sync_runner:x:996:994::/opt/alist_sync:/sbin/nologin
nginx:x:995:993:Nginx web server:/var/lib/nginx:/sbin/nologin
sre_share:x:1004:1005::/home/sre_share:/sbin/nologin


[devops@lb01 ~]$ tail --retry -f /aaa
tail: warning: --retry only effective for the initial open
tail: cannot open '/aaa' for reading: No such file or directory

```


#### Use Cases

Common scenarios where the `tail` command is beneficial include:

- Monitoring server logs to detect issues in real-time.
- Checking the latest entries in a continuously updated file.
- Debugging applications by reviewing recent log entries.


---


### Bash `head` Command - Display the beginning of a file



#### Using the `head` Command

- The `head` command is used to display the first part of files.

- It's particularly useful for previewing the start of a file to understand its structure.

- All examples below use the `/etc/ansible/ansible.cfg` file:

```

[devops@lb01 ~]$ head /etc/ansible/ansible.cfg
# Since Ansible 2.12 (core):
# To generate an example config file (a "disabled" one with all default settings, commented out):
#               $ ansible-config init --disabled > ansible.cfg
#
# Also you can now have a more complete file by including existing plugins:
# ansible-config init --disabled -t all > ansible.cfg

# For previous versions of Ansible you can check for examples in the 'stable' branches of each version
# Note that this file was always incomplete  and lagging changes to configuration settings


```

#### Basic Usage

- The `head` command displays the first 10 lines of a file by default:

#### Example: Display First 10 Lines

```

passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/

```


#### Options

##### The `head` command has several options used to customize its behavior:

- `-n [number]`: Display the first [number] lines of the file.
- `-c [number]`: Display the first [number] bytes of the file.



#### Option: -n [number]

- The `-n` option allows you to specify the number of lines to display from the start of the file. By default, `head` shows the first 10 lines.

#### Example: Display First 5 Lines

```

[devops@lb01 ~]$ head -n 5 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin

```



#### Option: -c [number]

- The `-c` option allows you to display the first [number] bytes of a file instead of lines.

#### Example: Display First 20 Bytes

```

[devops@lb01 ~]$ head -c 20 /etc/passwd
root:x:0:0:root:/roo

```



#### Option: Multiple Files

- The `head` command can be used to display the beginning of multiple files. By default, it prints the file name as a header before the content of each file.

#### Example: Display First 3 Lines of Multiple Files

```

[devops@lb01 ~]$ head -n 4 ./4_25.txt ./4_25_1.txt
==> ./4_25.txt <==
apple
banana
orange
juice

==> ./4_25_1.txt <==
01
002
0033
444

```



#### Option: -q

- The `-q` option suppresses the printing of headers when multiple files are being processed. This is useful when you want to view the contents of multiple files without the file names being printed.

#### Example: Suppress Headers

```

# 两个文件内容是直接连起来的
[devops@lb01 ~]$ head -q -n 3 ./4_25.txt ./4_25_1.txt
apple
banana
orange
01    # notice start！！
002
0033
[devops@lb01 ~]$

```


#### Common Uses

##### The `head` command is commonly used to:

- Preview the start of a file to understand its structure.
- Quickly check the contents of a file without opening it fully.
- Extract the header information from a data file.


---



