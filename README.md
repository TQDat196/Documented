# Linux-document

## Filesystem Layout
* Executables in **/bin, /usr/bin**
* System executables under **/sbin, /usr/sbin**
* Device nodes under **/dev**
*  Config files under **/etc**
* Home directories under **/home, also /root**
* Temporary files under **/tmp**. Often wiped at reboot.
> * Magic dirs under **/proc, /sys**
* Libraries under **/lib, /usr/lib**, sometimes **lib64** too
* Boot files under **/boot**
* User and secondary files under **/usr**. Historically only files needed for boot were directly under **/**, stuff that can be shared over network (or stored on a second drive if your first drive was too small) would be under **/usr**
* Commercial software / pre-packaged software under **/opt**
* Server storage and other data **/srv, /run, /var** programs store data
* Default places to mount media (memory keys, CD-Roms, etc.) **/media, /mnt**
* If the disk checker finds lost files when fixing a disk after unclean shutdown it may put the files in **/lost+found**

## Regular Expressions 
* Summary:
    * Regular expressions are a set of characters used to check patterns in strings.
    * They are also called 'regexp' and 'regex'.
    * It is important to learn regular expressions for writing scripts.
    * Some basic regular expressions are:

| Symbol | Descriptions           |
| :----: | :--------------------- |
|   .    | Replaces any character |
|   ^    | Matches start of tring |
|   $    | Matches end of string  |
* Some extended regular expressiongs:

| Expression | Descriptions                                             |
| :--------: | :------------------------------------------------------- |
|     \+     | Matches one or more occurrence of the previous character |
|     \?     | Matches zero or one occurrence of the previous character |

## Shortcut
| Key                                                                                         | Description                                                                                                       |
| :------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------- |
| `find <path> -name "<filename>"`                                                            | Search for a specific file name.                                                                                  |
| ` grep "<keywork>" <dir>`                                                                   | Search keywork in specific directory.                                                                             |
| `ps > tempfile` <br> `ps >> tempfile`                                                       | Show all process running and save to tempfile <br>Show all process running and append to tempfile                 |
| `kill <PID>`                                                                                | Kill process with PID ( Process ID).                                                                              |
| `Less <file_name>`                                                                          | Display file’s content in terminal.                                                                               |
| `wget -c <link>`                                                                            | Download file from link.                                                                                          |
| `rmdir`                                                                                     | Remove directory.                                                                                                 |
| `top`                                                                                       | Real-time view what's going on in a system.                                                                       |
| `exit` <br> `logout` <br> `Ctrl+D`                                                          | Log out of the machine.                                                                                           |
| `df -h ` <br>` du`                                                                          | Show disk space.                                                                                                  |
| `man <command>`                                                                             | Show ducmentation for a command.                                                                                  |
| `dmesg`                                                                                     | print system and boot messages.                                                                                   |
| `sort `                                                                                     | arranges lines of text alphabetically or numerically.                                                             |
| `fg`                                                                                        | Move job to foreground.                                                                                           |
| `echo $PATH`<br> `export PATH=$PATH:/usr/local/bin` <br> `export PATH=/usr/local/bin:$PATH` | Print PATH out <br>Add /usr/local/bin dir to the end of PATH <br>Add /usr/local/bin dir to the start of PATH <br> |
| `scp -P xxx /path/of/your/local/files` <br> `(username@)remote_ip:/path/to/your/remote/dir` | Copy local files to remote server with port.                                                                      |

## Software
**sFTP**
* Secure File Transfer Protocol | SSH File Transfer Protocol
* Syntax: `sftp -oPort=<custom_port> <serverip>` *(custom_port_defaut:22)*

## Shell scripts
**Terminology**
* **Shell :** 
  * A shell is a program that provides the traditional, text-only user interface for Linux and other Unix-like operating systems. Its primary function is to read commands that are typed into a console (i.e., an all-text display mode) or terminal window (an all-text window) in a GUI (graphical user interface) and then execute (i.e., run) them.
  * The term shell derives its name from the fact that it is an outer layer of an operating system. A shell is an interface between the user and the internal parts of the operating system (at the very core of which is the kernel).
  * Ex: sh, bash (advanced sh), ash (light ver), csh (C shell), ksh, tcsh, zsh, ...
* **Kernel :**
   
   * The kernel is a computer program at the core of a computer's operating system with complete control over everything in the system.
   * It facilitates interactions between hardware and software components.
* *PATH*:
   * When you type any command on the command prompt, the shell has to locate the command before it can be executed.
   * The PATH variable specifies the locations in which the shell should look for commands.
Architech::

* **Unix architech :**

<p align="center">
  <img src="./unix_architecture.jpg" width="450" title="hover text">
</p>



**Shell Scripts**
* Variable Types:
  * **Local Variables:** A local variable is a variable that is present within the current instance of the shell. It is not avaiable to programs that are started by the shell. They are set at the command prompt.
  * **Enviroment Variables:** An environment variable is available to any child process of the shell. Some programs need environment variables in order to function correctly. Usaually, a shell script defines only those environment variables that are needed by the programs that it runs.
  * **Shell Variables:** A shell variable is a special variable that is set by the shell and is required by the shell in order to function correctly. Some of these variables are environment variables whereas others are local variables.
* Syntax
```sh
SAMPLE PROGRAM::

#!/bin/sh                                                                     
   #Print string to terminal
echo "What is your name?"

   #Read input from keyboard
read NAME                    

   #Variables
NAME="This is a value"

   #Using variable 
echo "Hello, $PERSON"

   #read only variable (const)
readonly NAME

   #Unset variables | Delete Variable (You can not unset readonly variable)
unset AGE

   #Unset function
unset -f function_name
```
```sh
SPECIAL VARIABLE::

#!/bin/sh
   #Take all file in current dir and do for loop
for TOKEN in $*
do
   echo $TOKEN
      #Print exit status (0: Succesfull, 1: Unsuccesfull, others)
   echo $?
done
```
```sh
ARRAY::

#!/bin/sh
NAME[0]="Zara"
NAME[1]="Qadir"
NAME[2]="Mahnaz"
NAME[3]="Ayan"
NAME[4]="Daisy"

echo "Take all array_First Method: ${NAME[*]}"
echo "Take all array_Second Method: ${NAME[@]}"
echo "Take 1 value:${NAME[0]}"

====================================================

#!/bin/bash
NAME=("Zara" "Qadir" "Mahnaz" "Ayan" "Daisy")

====================================================
#!/bin/ksh

set -A NAME "Zara" "Qadir" "Mahnaz" "Ayan" "Daisy"
```
```sh
OPERATION::

#!/bin/sh
   #expr do mathematic compute.
   #`command`: execute command
val=`expr 2 + 2`
echo "Total value : $val"
```
```sh
IF::

if [ expression 1 ]
then
   Statement(s) to be executed if expression 1 is true
elif [ expression 2 ]
then
   Statement(s) to be executed if expression 2 is true
elif [ expression 3 ]
then
   Statement(s) to be executed if expression 3 is true
else
   Statement(s) to be executed if no expression is true
fi
```
```sh
SWITCH-CASE::

case word in 
 pattern1) 
      Statement(s) to be executed if pattern1 matches
      ;;
   pattern2)
      Statement(s) to be executed if pattern2 matches
      ;;
   pattern3)
      Statement(s) to be executed if pattern3 matches
      ;;
   *)
     Default condition to be executed
     ;;
esac
```
```sh
WHILE::

while command
do
   Statement(s) to be executed if command is true
done
```
```sh
FOR::

for var in word1 word2 ... wordN
do
   Statement(s) to be executed for every word.
done
```
```sh
UNTIL::

until command
do
   Statement(s) to be executed until command is true
done
```
```sh
SELLECT:: Only in ksh and bash

select var in word1 word2 ... wordN
do
   Statement(s) to be executed for every word.
done
----
---
----
HERE DOCUMENTS::

#!/bin/sh

   #Print the following until reach EOF
cat << EOF
This is a simple lookup program 
for good (and bad) restaurants
in Cape Town.
EOF

====================================================

#!/bin/sh

filename=test.txt

vi $filename <<EndOfCommands
i
This file was created automatically from
a shell script
^[
ZZ
EndOfCommands
```
```sh
FUNCTION::

function_name () { 
   list of commands
   return smthing
}
```
## Sed
* SED stands for strem editor
* This stream-oriented editor was created exclusively for executing scripts.
* Thus, all the input you feed into it passes through and gos to STDOUT and it does not change the input file.

* Following is the general syntax for sed
```sh
/pattern/action
```
* Here, pattern is a regular expression, and action is one of the commands given in the following table. If pattern is omitted, action is performed for every line as we have seen above. <br>
* The slash character (/) that surrounds the pattern are required because they are used as delimiters.
* 
| Key                 | Descriptions                                               |
| :------------------ | :--------------------------------------------------------- |
| /patttern/p         | Prints the line                                            |
| /pattern/d          | Deletes the line                                           |
| s/pattern1/pattern2 | Substitutes the first occurrence of pattern1 with pattern2 |



**Example**
```sh
#!/bin/bash

   # Substitutes the first occurrence of pattern1 with pattern2 in <file_name>
sed 's/pattern1/pattern2'  file_name
   # Substitutes all the occurrence of pattern1 with pattern2 in  <file_name>
sed 's/pattern1/pattern2/g'  file_name
   # Substitutes the first occurrence of pattern1 with pattern2 in  <file_name> and change  <file_name>
sed -i 's/pattern1/pattern2'  file_name

   # Ex: Substites all the occurence number(0->9) follow by character (a->z) with **
sed 's/[0-9][a-z]/**/g' file_name
   # Ex: Substites all the occurence number(0->9) or character (a->z) with *
sed 's/[0-9A-Z]/*/g' file_name
sed 's/[0-Z]/*/g' file_name

   # Ex: Add all the occurrence number(0-9) with () arround
sed 's/[0-9]/(&)/g' file_name

   # You can use other divider
   # Ex: Each examples below are the same
sed 's_pattern1_pattern2_g' file_name
sed 's:pattern1:pattern2:g' file_name
sed 's!pattern1!pattern2!g' file_name
```

##  Soft Link & Hard Link:
**Soft Link**
* Just create a shortcut to source file.
* If Source file's removed, current file's data will be lost.
* Syntax : *ln -s <source-file_dir> <current-file_dir>*.
**Hard Link**
* Create physic link to file.
* If any file're removed, the number of hard linked file simply decrease by one.
* If the number of hard linked file decrease to zero, data will be lost.
* Syntax : `ln <source-file_dir> <current-file_dir>`
## Git
* Change commit message with rebase:

| Steps                                                             | Descriptions                                     |
| :---------------------------------------------------------------- | :----------------------------------------------- |
| `git rebase -i HEAD~~`                                            | Show all commit list.                            |
| Replace `pick` with `word` in font of which one you want to edit. | Choose commit that you want to edit.             |
| `git commit --amend`                                              | Edit your commit's details.                      |
| `git rebase --continue`                                           | Do the edit to branch.                           |
| `git push --force`                                                | Push on server and replace the commit's details. |

## 4 thành phần của Embedded Linux
**Toolchain :**
* binutils (công cụ liên quan đến mã nhị phân): GNU Assembler, Linker, etc.
* gcc: GNU C Compiler.
* C library (libc): gồm các file binary cho phép ứng dụng giao tiếp với hệ điều hành.
* gdb: Debugger.

**Bootloader :**
* Chương trình phải được load lên bộ nhớ thông qua bootloader thì mới chạy được.
* CPU chỉ có khả năng load được một đoạn code nhất định và nó chỉ thao tác được với một địa chỉ cố định -> bị giới hạn.
* Các nhiệm vụ chính:
   * Khởi tạo phần cứng.
   * Thiết lập bộ nhớ RAM (DRAM).
   * Thiết lập bộ xử lý.
   * Load hệ điều hành bằng cách đọc thiết bị nhớ, từ mạng, từ serial.
* Các bootloader phổ biến trên Linux: `Das U-boot, Bareboot`.

**Kernel :**
* Là thành phần quan trọng nhất của hệ thống, chứa bộ lập lịch các tiến trình, quản lý bộ nhớ, quản lý thiết bị, ...etc.
* Được khởi động từ Bootloader.
* Board Support Package:
   * Để kernel có thể chạy được trên phần cứng thì phải có BSP.
   * Đóng vai trò trung gian cho hầu hết các lớp khác của Kernel và phần cứng.
* Kernel module
   * Là thành phần phần mềm được load động vào kernel đẻ chạy
   * Ứng dụng phổ biến của module là làm driver cho thiết bị.
    * Dù đa số là mã nguồn mở, nhưng có một số module sử dụng kèm theo thiết bị lại ở dạng mã nguồn đóng, và chúng sẽ không được quản lý trên main stream --> #Cần chú ý khi sử dụng kernel module dạng này trong các sản phẩm thương mại.#
. User-space application

:
