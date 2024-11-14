[# Pedagogy](https://www.youtube.com/watch?v=zhOl33qIItw&list=PLI-knp71HL3VmW2xLfkql0W82pfuRVcs4&index=10)

ctl+shift+ ‘+/-’: zoom in/out terminal 

Ls -F: show file type, such as file, directory, executable, etc.  *means executable. -sh : human readable file size.

Cat -n <file>: add line number to the output of the file
Tac <file>: show file in reverse order 

Nano -l <file1> <file2> < fill3>: show line number in each file. Alt + ‘.’or’,’ To cycle through each file.



Bash/Shell scripting
&> same as 1> and 2>: send stdout/stderr to the output
#!/bin/bash -x/-n: trace/debug the entire file.
set -n: check syntax error in no execution mode for any code below it. -x: enable execution trace mode
set -x, then section of code, then set +x: Only trace a section of the code. Same as -v, +v for verbose output
${A}:variable expansion
${#A[@]}: length of variable A
${#A[*]}: length of variable A
${#A[<index> or <key>]}: length of a specific item in the index or associative array.


${!A[@]} or ${!A[*]}: Find keys of an array regardless of index array or associative array. They are the same without “” but different with “” see below.
unset  A[<index>]: remove a value in the index array
unset A[<key>]: remove a value in the associative array
unset A : remove everything 
A+=(B C D): append elements for index array
A+=( [B]=“hi” [C]=“hello”): append elements for associative array
${A[@]:<starting index>}: slice array from the starting index.


${A[4]//<b>/<o>}: replace all b with o in the item at the index 4.  // means replace all. / means only replace the first occurrence of b.

${A:4}: skip the first 4 characters of the value of A and print out the rest.
${VAR:OFFSET:LENGTH}
${VAR#today}:today will be removed and print out the rest

`ls -F` or $(ls -la):command expansion
 $?: status code for the last run command. 0: successfully. 1-255 errors. 127 means command not found
Exit <status code>: if status code is not defined, by default exit will return the status code of its previous command.
Test <-f file1>: to test if file1 is file type. -d is directory type.  Test can also use to compare values test < 1 -eq 1>
-a : and 
-o: or
Use [] to replace test command no difference but [] may not support all operators like &&, ||, >, <,  only support -a, -o and wild card(* ? []), so status code may return success whereas the expression should be evaluated false. -n “string” means to check if the string is empty 
[[“string” =~ str]]: ~supports regex, means check if str is the substring of string
[[“string” =~ [^a-z]tring ]]: ^ means the first letter should not from a to z
$(()) doesn’t support float number, if you calculate float number, you need to use bc. don’t use $ if the variable inside $(())
g=$(echo “scale=2;17/3” | bc )
Echo $g
${FUNCNAME[0]}:print the name of the function based on the execution hierarchy. The current execution function will be [0], then calling function [1], etc. the exit code of the last command in the function will be the exit code of the function,
But you can use return <status code> inside the function to define an exit code for the function
Return will terminate the function, and any code after the return will not be executed 
Local var will have highest precedent 

var=“user1”  # global var, without local keyword, all vars are defined as global by default
function xyz()
{
	local var=“userlocal”
	echo ${var}
	var=“sss” # local var will change not global 
	echo ${var} 
}

xyz


set | grep IFS
FILENAME=${1}
IFS=$’\n’ $ is required if escape character is used
I=1

for line in $(cat ${FILENAME})
do
	echo “Line ${i} : ${line}”
	((i++))
done



Pass csv file in the command as arg to the below bash script

#! /bin/bash
FILE=${1}
IFS=$’\n’
((c=-1))
((sum=0))
for line in $(cat ${FILE})
do
	((c++))                 
	if ((c==0))
	then continue          skip the header in the csv file
	fi
	IFS=‘,’
	read name des sal <<<${line}   or read -a data <<<${line}
	((sum+=sal))                               or ((sum+=data[2]))
	cat <<-CLOSE
		name is ${name}          or name is ${data[0]}
		designation is ${des}    or designation is ${data[1]}  
		salary is ${sal}               or salary is ${data[2]}
		===================
CLOSE
done
((avg=sum/c))
echo average is ${avg}




Addgroup <group1>: add regular group1, if you want to create a system group, add “—system”, 
Delgroup <group1> : delete group1 but you can’t delete if it is used as  primary group
Groups <user1>: check user1’s groups
Gpasswd <group1>: set group password for group1
Gpasswd  -r <group1>: remove group password for group1
Gpasswd <group1> -a <user1> : add user1 to group1
Gpasswd <group1> -M <user1>,<user2>,<user3>: -M set a list of group users.
Gpasswd <group1> -d <user1> : remove user1 to group1
Usermod -a -G  <group1>, <group2> <user1> : add user1 to group1 and group2.  G for secondary group, g for primary group.“—shell” change default shell
Newgrp <group1>: switch user’s primary group to group1	
Chfn <user1>: change user details 
Deluser <user1>
Adduser —disabled-password <user1> —gecos “<user1>,<888>,<123>,<567>,<nothing>” &> /dev/null: gecos is to add user details without prompt questions
Chpasswd: change the password
“<user1>:<password>” | chpasswd: usually used in shell scripts
Read -s -p “please, write password for ${USERNAME}” USER_PASS
-n: check if string is empty. Return true if empty. In shell scripting zero return true, non zero return false
Echo -e : enable backslash escape character like \n  -E: disable backslash escape character. It is the default 
-n disable trailing new line character
if [ -n “${USER_PASS}” ]  
	then 
		echo - e “\ncreating user”
		Adduser —disabled-password <user1> —gecos “<user1>,<888>,<123>,<567>,<nothing>” &> /dev/nul
		echo “${USERNAME}:${USER_PASS}” | chpasswd
else 
	echo -e “\nThe password should be great than zero”
fi

C-style for loop
for (( i=0; i<=10; i++ ))
for (( ; ; )) infinite  for loop
while :     infinite while loop
util false infinite until loop 
do 
done


for USER in user{51..79}
do 
	id ${USER} &> /dev/null
	USER_EXIST=$?
	if [ “${USER_EXIST}” -eq “0” ]
	then
		echo “The user ${USER} exist in the system.”
	else
		echo “The user ${USER} does not exist in the system”
        fi
done

((a=1)
While ((a < 10))
Do 
	((a++))
Done

((a=1))
Until (( a == 5))    condition should be false to execute the body
do
	((a++))
Done

Echo “Yes” | tr [a-z] [A-Z] :  tr command converts it to all uppercase
Echo “Yes” | tr [A-Z] [a-z]:  tr command converts it to all lowercase

tr [:lower:] [:upper:] <<END  Use ‘END’ or \${} and \$(),  if you don’t want to use ${} or $()
Use <<- END if you want to suppress the leading TAB character in the output but this will not works for space.

USER_MORE=“yes”
while [ ${USER_MORE} = “yes” -o ${USER_MORE} = “y” ]
do
	echo “define tasks here”
	read -p “ Do you want to continue again (press y or yes to continue :” USER_MORE
	USER_MORE=$( echo ${USER_MORE} | tr [A-Z] [a-z])
	echo “your input is : ${USER_MORE}”
done


-a: creates index array. -A creates associative array.  Declare is used to declare any type of variables
declare -a USER_NAMES
USER_NAMES=( $(seq 50 100) )

declare -A UN
UN=( [user1]=“user1”,  [user2]=“user2”,  [user3]=“I am good”, )
echo ${UN[user1]}

user * or @ to access all the values in the array, not keys
When you use them without “” they are the same
However, if with the “”, with asterisk *, it will print in one line and taken as a single item in the array. \\
@ will print values on each new line and taken as a different item in the array.
echo ${UN[*]}
echo ${UN[@]}

for i in  “${UN[*]}”
do
	echo, “the value with * ${i} will be printed in one line”
done

USER_HOME=$( awk -F ‘:’ -v user=“${USER}” ‘$1==user {print $6}’ /etc/passwd ) 
-F: separator  -v: declare a variable. If you use variables inside awk, you must declare them first. 

tar -Pczf  <user1.tar.gz>  -C </home> <user1>: -c create tar file, -z compress it, -f specify a file path, -P means the file path is absolute path -C directory path which you don’t want to include in the archive path, you can use dirname command to get the path, then the top directory after exaction, you can use basename command to get the current directory that contains all user files 
tar -Pxzf  <user1.tar.gz>: decompress a tar file
DIR=$( dirname “${USER_HOME}”)
BASE=$( basename “${USER_HOME}”)
tar -C ${DIR} -Pczf ${TO_PATH}/${USER}.tar.gz ${BASE}

$0: path for the executable 
$#:number of command line  arguments, $0 is not included.
$@ or $*: show all command line arguments
Shift <1>: $1 got lost, $2 becomes $1



Groupmod <group1> -n  <group1renamed>: rename the group
Groupmod <group1> -g <new group1 id>: change group ID

Su -l <user1> : switch and login in as user1, -l is the same as -

Ps -e | grep bash: show bash process from all the running process
Ps -t <tty number>: show all the process running in a particular terminal 
Ps -u <user1>: show all the process running by user1
Ps -C <program name>: show all the process related to a particular program
Ps -lx: show all available param 
Ps -C bash -u <user> -o, user,pid,ppid,tty,time,cmd,stat,%mem,%cpu  —sort %mem: -o define what to show. —sort %mem means sort memory usage in increasing order, -%mem means sort in decreasing order
Pstree -s -p <pid> : -s show parent process 
Pstree <user1> : show user1 initiated process. -p will show the id
Pstree -c: will show all process and not in combined format. - H <pid> highlight specific process

Sleep 2993 &: command + & create bg processs 
Jobs: view all bg process in its own terminal
Fg %<job id>: change bg process to fg process
ctrl+z, then bg %<job id>: first stop the process by ctrl+z, then change fg process to bg process 
Ctrl + c : interrupt a process then the process will get terminated. SIGINT
SIGHUP signal is sent to process when its controlling terminal is closed.
Nohub sleep 2992: even you kill the parent process, the sleep process will still run in the background won’t be terminated.
Disown <job id>: to prevent the current running process from being terminated

Kill - l:show all process signal
Kill -SIGCONT %<job id> or <process ID. Get from “ ps -C <command> >:continue the stopped bg process 
Kill -SIGTSTP: stop a bg process
Kill -SIGSTOP: force to stop a bg process
Kill -SIGTERM <process ID. Get from “ ps -C <command> >: Terminate a process
Kill -SIGKILL et from “ ps -C <command> >: force to terminate a process 

You can also use pkill with process name instead of process id
Pkill -15 sleep


Top:
User up/down arrow to move up and down
? Or h: for help page
f: shows all column name and use ‘d’ key to select or deselect columns for displaying. Then press right arrow key to select process, use up/down arrow key to move to the desired position, then left arrow key to confirm.
u: find process belongs to certain users. Then username
L: search process by string name such as ‘bash’
k: send  a signal to kill a process, then specify signal number but one user can’t kill process by another user unless sudo
d: change interface updates from default 3.0 seconds to whatever you define.



LESS:
When press key:
1. g: Go to beginning of the file
2. G: Go to the end of the file
3. /<search item>: search from the top to down of the file, press ’n’ to go to the next occurrence
4. ?<search item>: search from the down to top
5. :<any number key>: Go to a specific number of line


Nano:
Ctrl + shift + _: go to line and column then type <line number>,<column number 
Ctrl + w: then enter the word you want to search, then use alt + w or q to go to the next or previous occurrence.
Ctrl + \: search and replace. You can use up/down arrow to cycle through previously searched term.
Hold down shift + arrow key  or use ctrl +6 then arrow key: to make selection. Then alt + 6 to copy, ctrl+u to paste. Without selection, alt+6 will copy the whole line.
If you select by mouse: you can use ctrl + shift + c to copy and ctrl+shift +v to paste. You can also use ctrl+shift+c and ctrl+shift +v to copy or paste things in the terminal 

Setuid: 4 or u+s
Setgid:2 or g+s
Stickbit:1 or +t

Chown <user2:group2> <file1> <file2>: change user and group ownership for file1 and file2
Chown -R <user2:group2> <dir1>: recursively change ownership for dir1
Chgrp <GROUP1> file*: change to group1 owner for all the files which name starts with file 
Chgrp <GROUP1> file[2-4]: change to group1 owner for all the files which name starts with file2, file3, file4
Touch file{10..19}: create files with file names from file10 to file19
Read: works input(). To use the result of read, use ${REPLY} which is default if you don’t supply a var name. To supply a var name, use “read <SERVICE>”,  -p “please enter service name:” will explain the promp
-d ‘:’ means if you enter : the promp will end. -s will not show input
readonly <var1> <var2>  means var1 and var2 can not be changed/
overwritten/unset
readonly <var1=“value”>







Environment variables :
Export a=“1” : with export will create environment variables. Without it, will create shell variables.
Chsh:change default shell then enter the location for the desired shell executable.  You have to log out from the user account then log back again to make it permanent.

Env: show all environment env
Unset <var name>
Shell variables:
Set: show all shell variables
Set | nano - : output set command to nano then ctrl+shift+_ , then ctrl+y to see the start of the file

Set -o : shell option
Set -o <command>: switch on the command
Set +o <command>: switch off the command
!!: execute the last command
Press Ctrl + d: terminal exits

Password hashing in /etc/shaow
$6$: sha512
$1$: md5
$2$ blowfish
!<hashing $6$>:in /etc/passwd if start with ! It means the account is locked.
!: if only ! Means password isn’t set when user account is created
Empty field means password is deleted using password-d <username>
Sudo passwd -l <username>: lock an user account 
Sudo passed -u <username>: unlock an user account 
*Or ? Means process user, user account is locked, user can not log in to the system
Sudo chage -l <username>: list info of each field in the shadow file 
Sudo chage <username>: change all the fields in the shadow file
Sudo chage -E 0 <username>: expire a user account so no matter what he can’t log in.
Sudo chage -d 0 <username>: expire an user account password. He can reset and still log in.

Create .hushlogin file in user’s home directory, it will suppress of login output whenever you ssh into the remote server.

REM_USER=“root”
declare -a REM_SERVERS=( “192.168.1.4”, “192.168.1.12” )
for target in “${REM_SERVERS[@]}”
do
	echo “talking to ${target} ……”
	ssh -T ${REM_USER}@${target}<<-END          
		pwd
		touch msg
		echo “$(whoami) to \$(whoami)” >> msg  
done

 \$(whoami)means escape the command expansion, thus no execution will happen at local but will happen in the remote server, in this case will print root.
ssh  -t root@192.168.1.4 <top>: Force to give pseudo-terminal if you run with a command
ssh -T  root@192.168.1.4 : To suppress pseudo-terminal  but in the shell script, whether -t or -T will not give you a pseudo-terminal  with here document but use -T will suppress the output message.


System wide crontab file: /etc/crontab
User’s specific crontab files: /var/spool/cron/crontabs/<username>
Visit crontab.guru
crontab -e : create a new user cron job file
crontab -r : delete user’s cron jobs 
crontab -l : list user’s cron jobs 
