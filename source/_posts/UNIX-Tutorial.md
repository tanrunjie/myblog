---
title: UNIX_Tutorial
date: 2021-12-27 10:55:55
tags: bash
categories:
- 常用指令
mathjax: true
---

Unix和Linux的基本指令
[http://www.ee.surrey.ac.uk/Teaching/Unix/](http://www.ee.surrey.ac.uk/Teaching/Unix/)

### Introduction

内核(Kernel): 分配时钟和内存给程序和处理文件存储以及系统交互
Shell:用户和内核的命令交互
UNIX中要么时文件，要么是程序

### Tutorial One & Two

``` cpp
ls -a
pwd

// search sci in file.txt
// Method 1
less file.txt
/sci  

// Method 2
grep sci file.txt //-i mean 'ignore up/low case'

wc -l file.txt  // count line in file.txt

clear // clean monitor
```

### Tutorial Three & Four
``` cpp
cat > list1  // output to list1
cat >> list1 // append to list1
cat list1 list2 > biglist // concat list1 and list2 to biglist

sort < biglist > slist // sort biglist and output to slist
command1 | command2  // pipe the output of command1 to the input of command2

ls list*
ls ?list

// getting help
man wc

```

|Command|Meaning|
|----|----|
|command > file| redirect standard output to a file|
|command >> file| append standard output to a file|
|command < file| redirect standard input from a file|
|command1 \| command2| pipe output of command1 to input of command2|
|who|list users currently logged|

### Tutorial Five & Six

``` cpp
ls -l

-rwxrw-r-- 1 ee51ab beng95 2450 Sept29 11:52 file1

// read/write/execute rights in owner/group/everyone
// file_size time file_name

chmod a+x ex // add permission to execute ex to all
```

|Symbol|Meaning|
|----|----|
|u|user|
|g|group|
|o|other|
|a|all|
|r|read|
|w|write and delete|
|x|execute and access directory|
|+|add permission|
|-|take away permission|

#### summary of process control
foreground: terminal suspend and wait
background: terminal can go on

|Command|Meaning|
|----|----|
|ls -lag|list access rights for all files|
|chmod [options] file|change access rights for named file|
|command &| run command in background|
|^C|kill the job running in the forground|
|^Z|suspend the job running in the foreground|
|jobs|list current jobs|
|bg %2|background the suspended job|
|fg %1|foreground job number 1 |
|kill %1|kill job number 1|
|ps|list current processes|
|kill 310|kill process id 310|


``` cpp
df .  // report space left on the file system
ls -lh
gzip
gunzip  // unzip
tar -xvf .tar // extract
diff file1 file2  // < denotes file1, > denotes file2

history // terminal history
!! // recall last command
!-3 // recall third most recent command
!grep // recall last command starting with grep

```


### Tutorial Seven & Eight
Install software:
1. Locate and download the source code(which is usually compressed)
2. Unpack the source code
3. Compile the code(Most difficult)
4. Install the resulting executable
5. Set paths to the installation directory

**make**:manage large programs and only compiling those parts changed
**Makefile**: record the related make rules, contains information on how to compile the software.

Some simplest way to compile a package:
1. **cd** to the directory containing the packages' source code.
2. Type **./configure** to configure the package for your system
3. Type **make** to compile the package
4. Optionally, type **make check** to run any self0tests that come with the package.
5. Type **make install** to install the programs and any data files and documentation.
6. Optionally, type **make clean** to remove the program binaries and object files from the source code directory.


#### UNIX variables
two categories: Environment variables, Shell variables
Shell variables: apply only to the current instance of shell and set short-term working conditions.
Environment variables: system environment with UPPER CASE name

``` cpp
echo $PATH // print system environment
set PATH=($PATH ~/new_path/)  // add path to PATH

```