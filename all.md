[# Pedagogy](https://www.youtube.com/watch?v=zhOl33qIItw&list=PLI-knp71HL3VmW2xLfkql0W82pfuRVcs4&index=10)

ctl+shift+ ‘+/-’: zoom in/out terminal 
Ls -F: show file type, such as file, directory, executable, etc.  *means executable. -sh : human readable file size.

Cat -n <file>: add line number to the output of the file
Tac <file>: show file in reverse order 

Nano -l <file1> <file2> < fill3>: show line number in each file. Alt + ‘.’or’,’ To cycle through each file.

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


Groupmod <group1> -n  <group1renamed>: rename the group
Groupmod <group1> -g <new group1 id>: change group ID

Su -l <user1> : switch and login in as user1, -l is the same as -

Ps -e | grep bash: show bash process from all the running process
Ps -t <tty number>: show all the process running in a particular terminal 
Ps -u <user1>: show all the process running by user1
Ps -C <program name>: show all the process related to a particular program
Ps -lx: show all available param 
Ps -C bash -o user, pid,ppid,tty,time,cmd,stat : -o define what to show.
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

${A}:variable expansion 
`ls -F` or $(ls -la):command expansion
 $?: status code for the last run command. 0: successfully. 1-255 errors. 127 means command not found
Exit <status code>: if status code is not defined, by default exit will return the status code of its previous command.
Test <-f file1>: to test if file1 is file type. -d is directory type.  Test can also use to compare values test < 1 -eq 1>
-a : and 
-o: or
Use [] to replace test command no difference but [] may not support all operators like &&, ||, >, <,  only support -a, -o and wild card(* ? []), so status code may return success whereas the expression should be evaluated false. -n “string” means to check if the string is empty 
[[“string” =~ str]]: ~supports regex, means check if str is the substring of string
[[“string” =~ [^a-z]tring ]]: ^ means the first letter should not from a to z





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


