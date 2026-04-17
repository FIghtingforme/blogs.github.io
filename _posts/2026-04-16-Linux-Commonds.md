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




### continues...
















```


