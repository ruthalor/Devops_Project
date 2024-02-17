### FILE MANIPULATION

 ####  1.  Sudo Command: 
sudo stands for “superuser do” and allows users with authorization to run commands for other users.

The sudo command allows Linux users to have temporary access to sensitive areas of the system. This access is protected by a password and is only applicable for a short time. 

Here is the general syntax in using this command:
`sudo (command e.g apt upgrade)`

To run this command, it becomes

`sudo apt upgrade`

![](./images/01.%20sudo.png)


 #### 2.  Pwd Command

The "pwd" command prints the full name (the full path) of current/present working directory.

The pwd command uses the syntax 
`pwd`

To run the command:
`pwd`
![](./images/02.%20%20pwd.png)


 #### 3.  Cd Command


The cd command allows you to move between directories. The cd command takes an argument, usually the name of the folder you want to move to. Running this command without an option will take you to the home folder. Also only users with sudo privileges can execute this. 

Here is the general syntax in using this command:

`cd your directory`

If you want to switch to a completely new directory, eg  name of the directory is "CommandsLinux"

You enter the command 
`cd CommandsLinux`
The command `cd ..` moves one directory up
`cd -` moves to our previous directory.
![](./images/03.%20cd.png)

4. #### ls Command


list
Ls is short for “list”. This command lists information about directories and any type of files in the working directory.

To view files in a directory, type `ls` is followed by the desired path. 

Some options you can use the ls command include:
`ls -R` this lists all the files in the subdirectories
`ls -a` shows hidden files in addition to the visible ones
`ls -lh` shows the sizes in easily readable formats, such as KB, MB, GB and TB

![](./images/04.%20ls.png)

### 5. Cat Command: 

Cat in Linux stands for concatenation (to merge things together) and is one of the most useful and versatile Linux commands.  This command displays the contents of one or more files without having to open the file for editing.
To run this command, type cat followed by the file name and its extension. For instance:
![](./images/05.%20cat.png)
The  `cat` command is also used to merge files.

Example:
![](./images/05i.%20cat%20merge.png)

#### 6. cp Command:

You use the cp command for copying files from one location to another. This command can also copy directories, Copy files from one directory to another.
To use this command, enter cp followed by the file name and the destination directory. For example:
![](./images/06.%20%20cp%20.png)

#### 7. mv Command:
The mv command moves files and directories from one directory to another or renames a file or directory.
Additionally, this command do not produce an outcome upon execution.
To use this command, type `mv` followed by the file name and the destination directory. 

![](./images/07.%20mv%20.png)

This command can also be used to rename a file 
![](./images/07i.%20mv%20rename.png)

#### 8. mkdir Command:
The command mkdir stands for make directory.
The Linux mkdir command is used in Linux to create one or multiple  directories. The user executing this command must have the privilege to make a folder or they may recieve a  permission denied error.
The syntax for the `mkdir` command is:
`midir [option] directory_name`
`mkdir`command can also be used to make a  new directory inside a directory.

![](./images/08.%20mkdir.png)

#### 9. rmdir Command:

The rmdir command is useful when you want to remove the empty directories from the filesystem in Linux. 
For example to delete an empty subdirectory and its main directory:

![](./images/09.%20rmdir.png)

#### 10 rm Command:
The rm cmmand is used to delete files within a directory.
![](./images/10.%20rm.png)

#### 11. touch Command:
The touch command allows you to create an empty file or generate and modify a timestamp in the linux command line.
![](./images/11.%20touch%20.png)

#### 12. locate Command: 
Locate command in Linux is used to find the files by name. Adding the -i argument will turn off case sensitivity so we can search for a file even if we dont remember its exact name. 
![](./images/12.%20locate.png)

#### 13. find Command:
The find command is used to search for and locate a list of files and directories 

Example we are looking for the file called ***example3.txt*** within the home directory and its subfolders:
![](./images/13.%20find.png)






