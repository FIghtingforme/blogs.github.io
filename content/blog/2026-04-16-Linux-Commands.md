---

date: 2026-04-16
title: "Linux Common Commands"
categories: ["Linux"]

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



### System Monitoring


#### Bash `ps` Command - Snapshot of Current Processes


#### Using the `ps` Command

- The `ps` command is used to report a snapshot of current processes.

- It's a useful tool for monitoring and managing processes on your system.

- All examples below use a hypothetical process list for demonstration:

```bash

[devops@lb01 ~]$ ps
    PID TTY          TIME CMD
  27583 pts/1    00:00:00 bash
  27638 pts/1    00:00:00 ps

```


#### Understanding the Output

The `ps` command output consists of several columns, each representing different aspects of the system's processes:

- **PID**: Process ID, a unique identifier for each process.
- **TTY**: Terminal type associated with the process.
- **TIME**: Total CPU time used by the process.
- **CMD**: The command that started the process.


#### Basic Usage

- To display a snapshot of current processes, use `ps`:

#### Example: Basic Usage

```bash

ps
PID TTY          TIME CMD
1234 pts/0    00:00:01 bash
5678 pts/1    00:00:02 python
9101 pts/2    00:00:03 node

```


#### Options

The `ps` command has options to change how it works:

- `-e` - Show all processes
- `-f` - Show detailed information
- `-u` - Show processes for a specific user
- `-a` - Show all processes with a terminal
- `-x` - Show processes without a terminal


#### Show All Processes

- The `-e` option allows you to show all processes.

### Example: Show All Processes

```bash

[devops@lb01 ~]$ ps -e
    PID TTY          TIME CMD
      1 ?        00:00:49 systemd
      2 ?        00:00:00 kthreadd
      3 ?        00:00:00 pool_workqueue_
      4 ?        00:00:00 kworker/R-rcu_g
      5 ?        00:00:00 kworker/R-sync_
      6 ?        00:00:00 kworker/R-slub_
      7 ?        00:00:00 kworker/R-netns
      9 ?        00:00:00 kworker/0:0H-events_highpri
     10 ?        00:00:00 kworker/u512:0-events_unbound
     11 ?        00:00:00 kworker/R-mm_pe
     12 ?        00:00:00 kworker/u512:1-netns
     13 ?        00:00:00 rcu_tasks_kthre
     14 ?        00:00:00 rcu_tasks_rude_
     15 ?        00:00:00 rcu_tasks_trace
     16 ?        00:00:05 ksoftirqd/0
     17 ?        00:00:02 rcu_preempt
     18 ?        00:00:00 rcu_exp_par_gp_
     19 ?        00:00:00 rcu_exp_gp_kthr
     20 ?        00:00:00 migration/0
     21 ?        00:00:00 idle_inject/0
     23 ?        00:00:00 cpuhp/0
     24 ?        00:00:00 cpuhp/1
     25 ?        00:00:00 idle_inject/1
     26 ?        00:00:00 migration/1
     27 ?        00:00:00 ksoftirqd/1
     29 ?        00:00:00 kworker/1:0H-events_highpri
     34 ?        00:00:00 kdevtmpfs
     35 ?        00:00:00 kworker/R-inet_
     36 ?        00:00:00 kauditd
     37 ?        00:00:00 khungtaskd
     39 ?        00:00:00 oom_reaper
     40 ?        00:00:00 kworker/R-write
     41 ?        00:00:01 kcompactd0
     42 ?        00:00:00 ksmd
     43 ?        00:00:01 khugepaged
     44 ?        00:00:00 kworker/R-crypt
     45 ?        00:00:00 kworker/R-kinte
     46 ?        00:00:00 kworker/R-kbloc

```


#### Show Detailed Information

- The `-f` option allows you to show detailed information.

#### Example: Show Detailed Information

```bash

[devops@lb01 ~]$ ps -f
UID          PID    PPID  C STIME TTY          TIME CMD
devops     33399   33397  0 15:57 pts/0    00:00:00 -bash
devops     33618   33399  0 16:03 pts/0    00:00:00 ps -f

```


#### Show Processes for a Specific User

- The `-u` option allows you to show processes for a specific user.

#### Example: Show Processes for a Specific User

```bash

[devops@lb01 ~]$ ps -u devops
    PID TTY          TIME CMD
  25607 ?        00:00:00 systemd
  25610 ?        00:00:00 (sd-pam)
  27580 ?        00:00:00 sshd
  27583 pts/1    00:00:00 bash
  33397 ?        00:00:00 sshd
  33399 pts/0    00:00:00 bash
  33430 ?        00:00:00 sshd
  33431 ?        00:00:00 sftp-server
  33653 pts/0    00:00:00 ps

```


#### Show Processes with a Terminal

- The `-a` option allows you to show all processes with a terminal.

#### Example: Show Processes with a Terminal

```bash

[devops@lb01 ~]$ ps -a
    PID TTY          TIME CMD
  33926 pts/0    00:00:00 ps

```



#### Show Processes without a Terminal

- The `-x` option allows you to show processes without a terminal.

#### Example: Show Processes without a Terminal

```bash

[devops@lb01 ~]$ ps -x
    PID TTY      STAT   TIME COMMAND
  25607 ?        Ss     0:00 /usr/lib/systemd/systemd --user
  25610 ?        S      0:00 (sd-pam)
  27580 ?        S      0:00 sshd: devops@pts/1
  27583 pts/1    Ss+    0:00 -bash
  33397 ?        R      0:00 sshd: devops@pts/0
  33399 pts/0    Ss     0:00 -bash
  33430 ?        S      0:00 sshd: devops@notty
  33431 ?        Ss     0:00 /usr/libexec/openssh/sftp-server
  33935 pts/0    R+     0:00 ps -x

```



#### Combining Options

- Options can be combined to provide more detailed output. 
- For example, `ps -ef` shows all processes with detailed information.

#### Example: Combine Options

```bash

[devops@lb01 ~]$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 May02 ?        00:00:50 /usr/lib/systemd/systemd --s
root           2       0  0 May02 ?        00:00:00 [kthreadd]
root           3       2  0 May02 ?        00:00:00 [pool_workqueue_]
root           4       2  0 May02 ?        00:00:00 [kworker/R-rcu_g]
root           5       2  0 May02 ?        00:00:00 [kworker/R-sync_]
root           6       2  0 May02 ?        00:00:00 [kworker/R-slub_]
root           7       2  0 May02 ?        00:00:00 [kworker/R-netns]
root           9       2  0 May02 ?        00:00:00 [kworker/0:0H-events_highpri
root          10       2  0 May02 ?        00:00:00 [kworker/u512:0-events_unbou
root          11       2  0 May02 ?        00:00:00 [kworker/R-mm_pe]
root          12       2  0 May02 ?        00:00:00 [kworker/u512:1-netns]
root          13       2  0 May02 ?        00:00:00 [rcu_tasks_kthre]
root          14       2  0 May02 ?        00:00:00 [rcu_tasks_rude_]
root          15       2  0 May02 ?        00:00:00 [rcu_tasks_trace]
root          16       2  0 May02 ?        00:00:05 [ksoftirqd/0]
root          17       2  0 May02 ?        00:00:02 [rcu_preempt]
root          18       2  0 May02 ?        00:00:00 [rcu_exp_par_gp_]

```



---


### Bash `top` Command - Display Linux Tasks


#### Using the `top` Command

- The `top` command is used to display Linux tasks.

- It's a powerful tool for monitoring system performance in real-time.

- Examples:

```bash

top - 16:25:37 up 20:42,  2 users,  load average: 0.03, 0.03, 0.05
Tasks: 166 total,   1 running, 165 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.3 sy,  0.0 ni, 99.0 id,  0.2 wa,  0.3 hi,  0.2 si,  0.0 st
MiB Mem :   1969.6 total,   1080.4 free,    499.7 used,    579.8 buff/cache
MiB Swap:    512.0 total,    512.0 free,      0.0 used.   1470.0 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
  33504 root      20   0       0      0      0 I   0.7   0.0   0:01.15 kworker+
  34082 devops    20   0  225844   4204   3416 R   0.3   0.2   0:00.09 top
      1 root      20   0  105892  13836  10244 S   0.0   0.7   0:50.39 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.05 kthreadd
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_wo+
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      9 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     10 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
     11 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     12 root      20   0       0      0      0 I   0.0   0.0   0:00.03 kworker+
     13 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tas+
     14 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tas+
     
```


#### Understanding the Output

The `top` command output consists of several columns, each representing different aspects of the system's processes:

- **PID**: Process ID, a unique identifier for each process.
- **USER**: The user account that owns the process.
- **PR**: Priority of the process.
- **NI**: Nice value, which affects scheduling priority.
- **VIRT**: Virtual memory size used by the process.
- **RES**: Resident memory size, the non-swapped physical memory the process uses.
- **SHR**: Shared memory size.
- **S**: Process status (e.g., S for sleeping, R for running).
- **%CPU**: CPU usage percentage.
- **%MEM**: Memory usage percentage.
- **TIME+**: Total CPU time the process has used since it started.
- **COMMAND**: The command that started the process.


#### Basic Usage

- To display Linux tasks, use `top`:

#### Example: Basic Usage

```bash

top - 16:32:27 up 20:49,  2 users,  load average: 0.80, 0.32, 0.15
Tasks: 166 total,   1 running, 165 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.3 sy,  0.0 ni, 99.2 id,  0.0 wa,  0.3 hi,  0.2 si,  0.0 st
MiB Mem :   1969.6 total,   1074.8 free,    505.2 used,    579.9 buff/cache
MiB Swap:    512.0 total,    512.0 free,      0.0 used.   1464.4 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
  34095 root      20   0       0      0      0 I   0.7   0.0   0:00.53 kworker+
    864 root      20   0 1795648  50320  32456 S   0.3   2.5   1:27.98 contain+
      1 root      20   0  105892  13836  10244 S   0.0   0.7   0:50.68 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.05 kthreadd
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_wo+
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      9 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     10 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
     11 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     12 root      20   0       0      0      0 I   0.0   0.0   0:00.03 kworker+
     13 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tas

```

#### Options

The `top` command has options to change how it works:

- `-d` - Set the time between updates
- `-p` - Monitor specific PIDs
- `-u` - Show tasks for a specific user
- `-n` - Set the number of iterations
- `-b` - Batch mode operation



#### Set Update Interval

- The `-d` option allows you to set the time between updates.

#### Example: Set Update Interval

```bash

[devops@lb01 ~]$ top -d 10
top - 16:34:24 up 20:51,  2 users,  load average: 0.97, 0.54, 0.25
Tasks: 167 total,   1 running, 166 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.3 sy,  0.0 ni, 99.1 id,  0.1 wa,  0.3 hi,  0.2 si,  0.0 st
MiB Mem :   1969.6 total,   1066.8 free,    513.2 used,    579.8 buff/cache
MiB Swap:    512.0 total,    512.0 free,      0.0 used.   1456.4 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
  33504 root      20   0       0      0      0 D   0.5   0.0   0:02.17 kworker+
      1 root      20   0  105892  13836  10244 S   0.1   0.7   0:50.79 systemd
    864 root      20   0 1795648  50324  32456 S   0.1   2.5   1:28.12 contain+
  34095 root      20   0       0      0      0 I   0.1   0.0   0:00.77 kworker+
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.05 kthreadd
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_wo+

```


#### Monitor Specific PIDs

- The `-p` option allows you to monitor specific PIDs.

#### Example: Monitor Specific PIDs

```bash

[devops@lb01 ~]$ ps -ef | grep tail
devops     34435   34345  0 16:35 pts/2    00:00:00 tail -f /etc/passwd
devops     34439   33399  0 16:35 pts/0    00:00:00 grep --color=auto tail


[devops@lb01 ~]$ top -p 34435
top - 16:36:26 up 20:53,  3 users,  load average: 0.98, 0.69, 0.35
Tasks:   1 total,   0 running,   1 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.3 sy,  0.0 ni, 99.2 id,  0.0 wa,  0.3 hi,  0.2 si,  0.0 st
MiB Mem :   1969.6 total,   1075.4 free,    504.5 used,    579.9 buff/cache
MiB Swap:    512.0 total,    512.0 free,      0.0 used.   1465.1 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
  34435 devops    20   0  220996   1984   1872 S   0.0   0.1   0:00.00 tail

```


#### Show Tasks for a Specific User

- The `-u` option allows you to show tasks for a specific user.

#### Example: Show Tasks for a Specific User

```bash

[devops@lb01 ~]$ top -u devops
top - 16:37:59 up 20:54,  3 users,  load average: 0.85, 0.74, 0.40
Tasks: 172 total,   1 running, 171 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  3.0 sy,  0.0 ni, 97.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   1969.6 total,   1063.4 free,    516.5 used,    579.9 buff/cache
MiB Swap:    512.0 total,    512.0 free,      0.0 used.   1453.1 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
  25607 devops    20   0   22384  11932   9984 S   0.0   0.6   0:00.17 systemd
  25610 devops    20   0  107152   5680   1960 S   0.0   0.3   0:00.00 (sd-pam)
  27580 devops    20   0   21244   7696   5420 S   0.0   0.4   0:00.03 sshd
  27583 devops    20   0  223852   5200   3624 S   0.0   0.3   0:00.00 bash
  33397 devops    20   0   21248   7780   5388 S   0.0   0.4   0:00.53 sshd
  33399 devops    20   0  223852   5344   3668 S   0.0   0.3   0:00.15 bash
  33430 devops    20   0   21092   7528   5344 S   0.0   0.4   0:00.00 sshd
  33431 devops    20   0   10064   6432   5404 S   0.0   0.3   0:00.00 sftp-se+
  34343 devops    20   0   21248   7668   5356 S   0.0   0.4   0:00.03 sshd
  34345 devops    20   0  223852   5348   3676 S   0.0   0.3   0:00.02 bash
  34376 devops    20   0   21092   7532   5344 S   0.0   0.4   0:00.01 sshd
  34377 devops    20   0   10064   6380   5352 S   0.0   0.3   0:00.01 sftp-se+
  34503 devops    20   0  225844   4244   3456 R   0.0   0.2   0:00.01 top

```


#### Set Number of Iterations

- The `-n` option allows you to set the number of iterations before `top` exits.

#### Example: Set Number of Iterations

```bash

[devops@lb01 ~]$ top -n 5
top - 16:40:01 up 20:56,  3 users,  load average: 0.20, 0.51, 0.35
Tasks: 177 total,   2 running, 175 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.5 us,  1.2 sy,  0.0 ni, 97.8 id,  0.0 wa,  0.3 hi,  0.2 si,  0.0 st
MiB Mem :   1969.6 total,   1058.3 free,    521.7 used,    579.9 buff/cache
MiB Swap:    512.0 total,    512.0 free,      0.0 used.   1447.9 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
      1 root      20   0  105892  13836  10244 D   0.3   0.7   0:51.03 systemd
    752 root      20   0   20912   9932   8304 S   0.3   0.5   0:04.84 systemd+
    864 root      20   0 1795648  50324  32456 S   0.3   2.5   1:28.50 contain+
  34332 root      20   0       0      0      0 I   0.3   0.0   0:01.01 kworker+
  34549 root      20   0    3996   2492   2316 R   0.3   0.1   0:00.01 unix_ch+
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.05 kthreadd
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_wo+
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      9 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     10 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
     11 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     12 root      20   0       0      0      0 I   0.0   0.0   0:00.03 kworker+
[devops@lb01 ~]$

```


#### Batch Mode Operation

- The `-b` option allows `top` to run in batch mode, suitable for sending output to other programs or files.

#### Example: Batch Mode Operation

```bash

[devops@lb01 ~]$ top -b -n 1
top - 16:46:45 up 21:03,  3 users,  load average: 0.03, 0.16, 0.23
Tasks: 170 total,   2 running, 168 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.9 us,  2.9 sy,  0.0 ni, 94.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   1969.6 total,   1061.6 free,    518.2 used,    580.0 buff/cache
MiB Swap:    512.0 total,    512.0 free,      0.0 used.   1451.4 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
      1 root      20   0  105892  13836  10244 S   0.0   0.7   0:51.29 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.05 kthreadd
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_wo+
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      9 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+

```


#### Combining Options

- Options can be combined to provide more detailed output. For example, `top -b -n 1` runs `top` in batch mode for one iteration.

#### Example: Combine Options

```bash

[devops@lb01 ~]$ top -bn 1 | tee aaa.txt
top - 16:54:20 up 21:11,  3 users,  load average: 0.04, 0.06, 0.15
Tasks: 174 total,   1 running, 173 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   1969.6 total,   1068.6 free,    511.1 used,    580.1 buff/cache
MiB Swap:    512.0 total,    512.0 free,      0.0 used.   1458.5 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
      1 root      20   0  105892  13836  10244 S   0.0   0.7   0:51.60 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.05 kthreadd
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_wo+
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      9 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     10 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
     11 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+

```



---



### Bash `df` Command - File System Disk Space Usage


#### Using the `df` Command

- The `df` command is used to report file system disk space usage.

- It's a useful tool for checking available storage on your system.

- All examples below use a hypothetical output for demonstration:

```bash

[devops@lb01 ~]$ df
Filesystem           1K-blocks    Used Available Use% Mounted on
devtmpfs                  4096       0      4096   0% /dev
tmpfs                  1008444       0   1008444   0% /dev/shm
tmpfs                   403380   36708    366672  10% /run
/dev/mapper/rlm-root  19329024 4935180  14393844  26% /
/dev/nvme0n1p2          996780  327360    600608  36% /boot
tmpfs                   201688       0    201688   0% /run/user/1002

```


#### Understanding the Output

The `df` command output consists of several columns, each representing different aspects of the file system's disk usage:

- **Filesystem**: The name of the file system.
- **1K-blocks**: Total size of the file system in 1K blocks.
- **Used**: Amount of space used.
- **Available**: Amount of space available for use.
- **Use%**: Percentage of space used.
- **Mounted on**: Directory where the file system is mounted.


#### Basic Usage

- To display disk space usage, use `df`:

#### Example: Basic Usage

```bash

[devops@lb01 ~]$ df -h
Filesystem            Size  Used Avail Use% Mounted on
devtmpfs              4.0M     0  4.0M   0% /dev
tmpfs                 985M     0  985M   0% /dev/shm
tmpfs                 394M   36M  359M  10% /run
/dev/mapper/rlm-root   19G  4.8G   14G  26% /
/dev/nvme0n1p2        974M  320M  587M  36% /boot
tmpfs                 197M     0  197M   0% /run/user/1002

```


#### Options

The `df` command has options to change how it works:

- `-h` - Show sizes in human-readable format (e.g., KB, MB)
- `-a` - Show all file systems, even empty ones
- `-T` - Show the type of file system
- `-i` - Show inode usage
- `-P` - Use POSIX output format


#### Show Sizes in Human-Readable Format

- The `-h` option allows you to show sizes in human-readable format.

#### Example: Show Sizes in Human-Readable Format

```bash

[devops@lb01 ~]$ df -h
Filesystem            Size  Used Avail Use% Mounted on
devtmpfs              4.0M     0  4.0M   0% /dev
tmpfs                 985M     0  985M   0% /dev/shm
tmpfs                 394M   36M  359M  10% /run
/dev/mapper/rlm-root   19G  4.8G   14G  26% /
/dev/nvme0n1p2        974M  320M  587M  36% /boot
tmpfs                 197M     0  197M   0% /run/user/1002

```


#### Show All File Systems

- The `-a` option allows you to show all file systems, even empty ones.

#### Example: Show All File Systems

```bash

[devops@lb01 ~]$ df -a
Filesystem           1K-blocks    Used Available Use% Mounted on
proc                         0       0         0    - /proc
sysfs                        0       0         0    - /sys
devtmpfs                  4096       0      4096   0% /dev
securityfs                   0       0         0    - /sys/kernel/security
tmpfs                  1008444       0   1008444   0% /dev/shm
devpts                       0       0         0    - /dev/pts
tmpfs                   403380   36704    366676  10% /run
cgroup2                      0       0         0    - /sys/fs/cgroup
pstore                       0       0         0    - /sys/fs/pstore
bpf                          0       0         0    - /sys/fs/bpf
configfs                     0       0         0    - /sys/kernel/config
/dev/mapper/rlm-root  19329024 4935312  14393712  26% /
systemd-1                    0       0         0    - /proc/sys/fs/binfmt_misc
mqueue                       0       0         0    - /dev/mqueue
hugetlbfs                    0       0         0    - /dev/hugepages
debugfs                      0       0         0    - /sys/kernel/debug
tracefs                      0       0         0    - /sys/kernel/tracing
fusectl                      0       0         0    - /sys/fs/fuse/connections
none                         0       0         0    - /run/credentials/systemd-sysctl.service
none                         0       0         0    - /run/credentials/systemd-tmpfiles-setup-dev.service
/dev/nvme0n1p2          996780  327360    600608  36% /boot
none                         0       0         0    - /run/credentials/systemd-tmpfiles-setup.service
tmpfs                   201688       0    201688   0% /run/user/1002

```


#### Show File System Type

- The `-T` option allows you to show the type of file system.

#### Example: Show File System Type

```bash

[devops@lb01 ~]$ df -Th
Filesystem           Type      Size  Used Avail Use% Mounted on
devtmpfs             devtmpfs  4.0M     0  4.0M   0% /dev
tmpfs                tmpfs     985M     0  985M   0% /dev/shm
tmpfs                tmpfs     394M   36M  359M  10% /run
/dev/mapper/rlm-root xfs        19G  4.8G   14G  26% /
/dev/nvme0n1p2       ext4      974M  320M  587M  36% /boot
tmpfs                tmpfs     197M     0  197M   0% /run/user/1002

```

- **File System Type:** This indicates the format and structure used to store and organize data on a disk.

- Common file system types include `ext4`, `ntfs`, and `vfat`.

- Each type has its own features, limitations, and compatibility with operating systems.


#### Show Inode Usage

- The `-i` option allows you to show inode usage.

#### Example: Show Inode Usage

```bash

[devops@lb01 ~]$ df -i
Filesystem            Inodes IUsed   IFree IUse% Mounted on
devtmpfs              246240   419  245821    1% /dev
tmpfs                 252111     1  252110    1% /dev/shm
tmpfs                 819200   734  818466    1% /run
/dev/mapper/rlm-root 9697280 93667 9603613    1% /
/dev/nvme0n1p2         65536   376   65160    1% /boot
tmpfs                  50422    14   50408    1% /run/user/1002

```

#### **Inodes:** Inodes are data structures used by many file systems to store information about files and directories, such as their size, owner, permissions, and timestamps.

- Each file or directory has a unique inode. The `df -i` command shows inode usage, which can be important for systems with many small files.


#### Use POSIX Output Format

- The `-P` option allows you to use the POSIX output format.

#### Example: Use POSIX Output Format

```bash

[devops@lb01 ~]$ df -P
Filesystem           1024-blocks    Used Available Capacity Mounted on
devtmpfs                    4096       0      4096       0% /dev
tmpfs                    1008444       0   1008444       0% /dev/shm
tmpfs                     403380   36712    366668      10% /run
/dev/mapper/rlm-root    19329024 4935456  14393568      26% /
/dev/nvme0n1p2            996780  327360    600608      36% /boot
tmpfs                     201688       0    201688       0% /run/user/1002
tmpfs                     201688       0    201688       0% /run/user/0

```

- **POSIX:** POSIX (Portable Operating System Interface) is a set of standards specified by the IEEE for maintaining compatibility between operating systems.

- The `df -P` option provides output in a POSIX-compliant format, ensuring consistency across different environments and systems.


#### Combining Options

- Options can be combined to provide more detailed output.

- For example, `df -hT` shows sizes in human-readable format along with the file system type.

#### Example: Combine Options

```bash

[devops@lb01 ~]$ df -Th
Filesystem           Type      Size  Used Avail Use% Mounted on
devtmpfs             devtmpfs  4.0M     0  4.0M   0% /dev
tmpfs                tmpfs     985M     0  985M   0% /dev/shm
tmpfs                tmpfs     394M   36M  359M  10% /run
/dev/mapper/rlm-root xfs        19G  4.8G   14G  26% /
/dev/nvme0n1p2       ext4      974M  320M  587M  36% /boot
tmpfs                tmpfs     197M     0  197M   0% /run/user/1002
[devops@lb01 ~]$

```



---


### Bash `du` Command - File Space Usage


#### Using the `du` Command

- The `du` command is used to estimate file space usage.

- It's helpful for finding out how much space files and directories take up.

- examples:

```bash

80K     ./Scripts/python/venv/lib/python3.9/site-packages/google_genai-1.47.0.dist-info
53M     ./Scripts/python/venv/lib/python3.9/site-packages
53M     ./Scripts/python/venv/lib/python3.9
53M     ./Scripts/python/venv/lib
52K     ./Scripts/python/venv/bin
53M     ./Scripts/python/venv
53M     ./Scripts/python
53M     ./Scripts
355M    .

```


#### Understanding the Output

The `du` command output consists of two columns:

- **Size**: The amount of disk space used by the file or directory.
- **Path**: The file or directory path.


#### Basic Usage

- To display file space usage, use `du`:

#### Example: Basic Usage

```bash
du
8.0K    ./dir1
12K     ./dir2
20K     .
```


#### Options

The `du` command has options to change how it works:

- `-h` - Show sizes in human-readable format (e.g., KB, MB)
- `-s` - Show only the total size for each item
- `-a` - Show sizes for all files, not just directories
- `-c` - Produce a grand total
- `--max-depth=N` - Limit the depth of directory traversal


#### Show Sizes in Human-Readable Format

- The `-h` option allows you to show sizes in human-readable format.

#### Example: Show Sizes in Human-Readable Format

```bash


du -h

80K     ./Scripts/python/venv/lib/python3.9/site-packages/google_genai-1.47.0.dist-info
53M     ./Scripts/python/venv/lib/python3.9/site-packages
53M     ./Scripts/python/venv/lib/python3.9
53M     ./Scripts/python/venv/lib
52K     ./Scripts/python/venv/bin
53M     ./Scripts/python/venv
53M     ./Scripts/python
53M     ./Scripts
355M    .

```


#### Show Only Total Size

- The `-s` option allows you to show only the total size for each item.

#### Example: Show Only Total Size

```bash

[devops@lb01 ~]$ du -sh
355M    .

```


#### Show Sizes for All Files

- The `-a` option allows you to show sizes for all files, not just directories.

#### Example: Show Sizes for All Files

```bash


du -a

4.0K    ./Scripts/python/venv/bin/activate.csh
4.0K    ./Scripts/python/venv/bin/activate.fish
4.0K    ./Scripts/python/venv/bin/normalizer
4.0K    ./Scripts/python/venv/bin/websockets
4.0K    ./Scripts/python/venv/bin/httpx
52K     ./Scripts/python/venv/bin
4.0K    ./Scripts/python/venv/pyvenv.cfg
53M     ./Scripts/python/venv
53M     ./Scripts/python
53M     ./Scripts
16K     ./aaa.txt
355M    .

```


#### Produce a Grand Total

- The `-c` option allows you to produce a grand total.

#### Example: Produce a Grand Total

```bash

80K     ./Scripts/python/venv/lib/python3.9/site-packages/google_genai-1.47.0.dist-info
53M     ./Scripts/python/venv/lib/python3.9/site-packages
53M     ./Scripts/python/venv/lib/python3.9
53M     ./Scripts/python/venv/lib
52K     ./Scripts/python/venv/bin
53M     ./Scripts/python/venv
53M     ./Scripts/python
53M     ./Scripts
355M    .
355M    total

```


#### Limit Directory Traversal Depth

- The `--max-depth=N` option allows you to limit the depth of directory traversal.

- This can be useful for summarizing disk usage at a specific directory level.

#### Example: Limit Directory Traversal Depth

```bash

[devops@lb01 ~]$ du --max-depth=1
4       ./.config
8       ./.ssh
8       ./.terraform.d
3476    ./Docker
68      ./.docker
0       ./.ansible
4       ./.vim
11812   ./.cache
293084  ./Terraform
54256   ./Scripts
362796  .

```

- In this example, `du --max-depth=1` shows the space used by each directory at the top level, without diving deeper into subdirectories.

#### Example: Limit Directory Traversal Depth to 2

```bash

[devops@lb01 ~]$ du --max-depth=2
0       ./.config/procps
4       ./.config/htop
4       ./.config
8       ./.ssh
8       ./.terraform.d
0       ./Docker/redis
3476    ./Docker/docker-nodejs-sample
0       ./Docker/nginx
3476    ./Docker
68      ./.docker/buildx
68      ./.docker
0       ./.ansible/tmp
0       ./.ansible/cp
0       ./.ansible
4       ./.vim
11812   ./.cache/pip
11812   ./.cache
293080  ./Terraform/aliyun
4       ./Terraform/tailscale
293084  ./Terraform
20      ./Scripts/shell
54236   ./Scripts/python
54256   ./Scripts
362796  .


[devops@lb01 ~]$ tree -L 2
.
├── aaa.txt
├── Docker
│   ├── docker-nodejs-sample
│   ├── nginx
│   └── redis
├── Scripts
│   ├── python
│   └── shell
└── Terraform
    ├── aliyun
    └── tailscale

10 directories, 1 file

```

- Here, `du --max-depth=2` provides a summary of disk usage up to two levels deep, including subdirectories.


#### Combining Options

- Options can be combined to provide more detailed output. For example, `du -h --max-depth=1` shows sizes in human-readable format with limited directory depth.

#### Example: Combine Options

```bash

[devops@lb01 ~]$ du -h --max-depth=2
0       ./.config/procps
4.0K    ./.config/htop
4.0K    ./.config
8.0K    ./.ssh
8.0K    ./.terraform.d
0       ./Docker/redis
3.4M    ./Docker/docker-nodejs-sample
0       ./Docker/nginx
3.4M    ./Docker
68K     ./.docker/buildx
68K     ./.docker
0       ./.ansible/tmp
0       ./.ansible/cp
0       ./.ansible
4.0K    ./.vim
12M     ./.cache/pip
12M     ./.cache
287M    ./Terraform/aliyun
4.0K    ./Terraform/tailscale
287M    ./Terraform
20K     ./Scripts/shell
53M     ./Scripts/python
53M     ./Scripts
355M    .

```




---




### Bash `free` Command - Display Free and Used Memory


#### Using the `free` Command

- The `free` command is used to display the amount of free and used memory in the system.

- It's useful for monitoring memory usage and managing system resources.


#### Basic Usage

- To display memory usage, use `free`:

#### Example

```bash
free -k
              total        used        free      shared  buff/cache   available
Mem:        8176588     1376568     5869188      146532     1051172     6352280
Swap:       2097148           0     2097148
```



#### Options

The `free` command has options to change how it works:

- `-h` - Show memory in human-readable format (e.g., KB, MB, GB)
- `-b` - Show memory in bytes
- `-k` - Show memory in kilobytes (KB)
- `-m` - Show memory in megabytes (MB)
- `-g` - Show memory in gigabytes (GB)
- `-s [interval]` - Continuously display memory usage at specified intervals
- `-t` - Display total memory


#### Show Memory in Human-Readable Format

- The `-h` option allows you to show memory in a human-readable format, such as KB, MB, or GB.

- Without this option, memory is displayed in kilobytes by default.

#### Example: Show Memory in Human-Readable Format

```bash

[devops@lb01 ~]$ free -h
               total        used        free      shared  buff/cache   available
Mem:           1.9Gi       505Mi       1.0Gi        35Mi       585Mi       1.4Gi
Swap:          511Mi          0B       511Mi

```


#### Show Memory in Bytes

- The `-b` option allows you to show memory in bytes, providing a more precise measurement.

- Without this option, memory is displayed in kilobytes by default.

#### Example: Show Memory in Bytes

```bash

[devops@lb01 ~]$ free -b
               total        used        free      shared  buff/cache   available
Mem:      2065297408   534020096  1119330304    37597184   613793792  1531277312
Swap:      536866816           0   536866816

```


#### Show Memory in Kilobytes

- The `-k` option allows you to show memory in kilobytes.

- This is the default behavior of the `free` command.

#### Example: Show Memory in Kilobytes

```bash

[devops@lb01 ~]$ free -k
               total        used        free      shared  buff/cache   available
Mem:         2016892      517516     1097080       36716      599404     1499376
Swap:         524284           0      524284

```


#### Show Memory in Megabytes

- The `-m` option allows you to show memory in megabytes, which can be easier to read for larger memory sizes.

- Without this option, memory is displayed in kilobytes by default.

#### Example: Show Memory in Megabytes

```bash

[devops@lb01 ~]$ free -m
               total        used        free      shared  buff/cache   available
Mem:            1969         500        1076          35         585        1469
Swap:            511           0         511

```


#### Show Memory in Gigabytes

- The `-g` option allows you to show memory in gigabytes, which is useful for systems with large amounts of memory.

- Without this option, memory is displayed in kilobytes by default.

#### Example: Show Memory in Gigabytes

```bash

[devops@lb01 ~]$ free -g
               total        used        free      shared  buff/cache   available
Mem:               1           0           1           0           0           1
Swap:              0           0           0

```


#### Continuously Display Memory Usage

- The `-s` option allows you to continuously display memory usage at specified intervals. This is useful for monitoring memory usage over time.

#### Example: Continuously Display Memory Usage

```bash

free -s 5
               total        used        free      shared  buff/cache   available
Mem:         2016892      521552     1092952       36716      599500     1495340
Swap:         524284           0      524284

               total        used        free      shared  buff/cache   available
Mem:         2016892      521552     1092952       36716      599500     1495340
Swap:         524284           0      524284

               total        used        free      shared  buff/cache   available
Mem:         2016892      506416     1108084       36704      599492     1510476
Swap:         524284           0      524284

               total        used        free      shared  buff/cache   available
Mem:         2016892      510512     1103988       36708      599496     1506380
Swap:         524284           0      524284


```

- This command will update the memory usage every 5 seconds.



#### Display Total Memory

- The `-t` option includes a line showing the total memory (used + free) for both RAM and swap. This provides a quick overview of total memory resources.

#### Example: Display Total Memory

```bash

               total        used        free      shared  buff/cache   available
Mem:         2016892      521684     1092752       36708      599556     1495208
Swap:         524284           0      524284
Total:       2541176      521684     1617036

```



---


### Bash `kill` Command - Terminate Processes


#### Using the `kill` Command

- The `kill` command is used to terminate processes in a Unix-like operating system.

- It's a powerful tool for managing system resources and ensuring that processes do not consume more resources than necessary.


#### Common Uses

The `kill` command is commonly used to:

- Terminate unresponsive processes.
- Manage system resources by stopping unnecessary processes.
- Send specific signals to processes for custom handling.


#### Syntax

- The basic syntax of the `kill` command is:

```bash
kill [OPTION]... ...
```



#### Options

The `kill` command has several options to customize its behavior:

- `-9`: Forcefully terminate a process.
- `-l`: List all signal names.
- `-s [signal]`: Specify a signal to send.
- `-p`: Print the process ID.

#### Forcefully Terminate a Process

- The `-9` option sends the SIGKILL signal to a process, which forcefully terminates it. 
- This is useful when a process does not respond to other signals.

#### Example: Forcefully Kill a Process

```bash

[devops@lb01 ~]$ kill -9 36112


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
Killed

```


#### List All Signal Names

- The `-l` option lists all available signal names. 
- This can help you understand which signals are available for use with the `kill` command.

#### Example: List Signal Names

```bash

[devops@lb01 ~]$ kill -l
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 2) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
3) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
4) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
5) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
6) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
7) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
8) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
9) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
10) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
11) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
12) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
13) SIGRTMAX-1  64) SIGRTMAX

```


#### Specify a Signal to Send

- The `-s` option allows you to specify a signal to send to a process. 
- This provides flexibility in controlling processes.

#### Example: Send a Custom Signal

```bash

[devops@lb01 ~]$ kill -s SIGTERM 36167


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
Terminated

```


#### Print the Process ID

- The `-p` option prints the process ID of the process you are targeting. 
- This is useful for verifying the process you intend to signal.

#### Example: Print Process ID

```bash
kill -p 1234
```



---


### Bash `uptime` Command - System Runtime


#### Using the `uptime` Command

The `uptime` command is used to find out how long the system has been running.

It provides a quick overview of the system's performance, including:

- The current time
- How long the system has been up
- The number of users logged in
- The system load averages for the past 1, 5, and 15 minutes


#### Syntax

- The basic syntax of the `uptime` command is:

```bash
uptime
```

- This command displays information like the current time, uptime duration, number of users, and load averages.


#### Understanding the Output

The output of the `uptime` command shows information like:

- **Current Time:** The time at which the command was run.
- **Uptime Duration:** How long the system has been running since the last reboot.
- **Number of Users:** The number of users currently logged into the system.
- **Load Averages:** The system load averages for the past 1, 5, and 15 minutes.



#### Example Output

- Here's an example of the `uptime` command output:

#### Example: Understanding Uptime Output

```bash

[devops@lb01 ~]$ uptime
 17:52:33 up 22:09,  2 users,  load average: 0.03, 0.07, 0.08

```

In this example:

- The current time is **17:52:33**.
- The system has been up for **22:09**.
- There are **2 users** currently logged in.
- The load averages are **0.03, 0.07, 0.08**, indicating the system load over the last 1, 5, and 15 minutes.


#### Interpreting Load Averages

The load averages provide a snapshot of the system's workload:

- A lower load average indicates a less busy system.
- A higher load average suggests the system is busier.
- Load averages above 1.0 per core may indicate the system is overloaded.

#### Example: Interpreting Load Averages

```bash
load average: 0.75, 0.60, 0.50
```

- In this example, the system load is decreasing over time, indicating that the system is becoming less busy.



#### Common Uses

The `uptime` command is commonly used to:

- Monitor system performance and stability.
- Quickly check how long the system has been running.
- Assess system load and determine if the system is under heavy use.


