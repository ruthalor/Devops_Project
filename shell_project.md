### Writing Our First Shell Script

On our terminal, we open a folder called <em>shell-scripting</em> using the `mkdir shell-scriping` command
create a file called <em> user input</em> with  the command `touch user-input.sh`
![](./images/01.%20make%20shell%20dir.png)

`vi` into the `sh` file and write the`code` below 

```
#!/bin/bash

# Prompt the user for their name
echo "Enter your name:"
read name

# Display a greeting with the entered name
echo "Hello, $name! Nice to meet you."

```
Now this will prompt an error because we have no admin rights to run this file
so we run the `chmod` command which is `sudo chmod +x user-input.sh`

Note the `#!bin/bash` at the beginning of this scfript helps to specify the type of bash interpreter to be used in excecuting the script.
![](./images/02.%20vi%20to%20the%20script.png)


Here we have our first script running!



### Directory Manipulation And Navigation

This script is to display the current directory, create a new directory called "my_directory" change to that directory, create two files inside, list the files, move back one level up, remove the "my directory" and its content and finally list the files in the current directory again.

To execute this, open a file name <em>navigating-linux-filesystem.sh</em> and write the code below:

```
#!/bin/bash

# Display current directory
echo "Current directory: $PWD"

# Create a new directory
echo "Creating a new directory..."
mkdir my_directory
echo "New directory created."

# Change to the new directory
echo "Changing to the new directory..."
cd my_directory
echo "Current directory: $PWD"

# Create some files
echo "Creating files..."
touch file1.txt
touch file2.txt
echo "Files created."

# List the files in the current directory
echo "Files in the current directory:"
ls

# Move one level up
echo "Moving one level up..."
cd ..
echo "Current directory: $PWD"

# Remove the new directory and its contents
echo "Removing the new directory..."
rm -rf my_directory
echo "Directory removed."

# List the files in the current directory again
echo "Files in the current directory:"
ls
```

Remember to run the command `sudo chmod +x navigating-linux-filesystem.sh` to execute permission on the file

Run the script using the command `./navigating-linux-filesystem.sh`
![](./images/03.%20directory%20manipulation.png)

### File Operations and Sorting

This script focuses on <em>File Operations and Sorting</em>

This script creates three files (file1.txt. file2.txt, file 3.txt), displays the files in their current orders, sorts them alphabetically, saves the files in sorted_files.txt, displays the sorted files, removes the original files, renames the sorted files to sorted_files_sorted_alphabetically.txt, and finally displays the contents of thr final sorted file.

Lets proceed with these steps:

Open a file called <em>sorting.sh</em> with the command ` vi sorting.sh`

Write the code below and save

```
#!/bin/bash

# Create three files
echo "Creating files..."
echo "This is file3." > file3.txt
echo "This is file1." > file1.txt
echo "This is file2." > file2.txt
echo "Files created."

# Display the files in their current order
echo "Files in their current order:"
ls

# Sort the files alphabetically
echo "Sorting files alphabetically..."
ls | sort > sorted_files.txt
echo "Files sorted."

# Display the sorted files
echo "Sorted files:"
cat sorted_files.txt

# Remove the original files
echo "Removing original files..."
rm file1.txt file2.txt file3.txt
echo "Original files removed."

# Rename the sorted file to a more descriptive name
echo "Renaming sorted file..."
mv sorted_files.txt sorted_files_sorted_alphabetically.txt
echo "File renamed."

# Display the final sorted file
echo "Final sorted file:"
cat sorted_files_sorted_alphabetically.txt

```

Set execute permission for the <em>sorting.sh</em> file using the command `sudo chmod +x sorting.sh`
Run the script with the command `./sorting.sh`
Here is the outcome:

![](./images/04.%20file%20operations%20and%20sorting.png)

### Working with Numbersn and Calculations

This script defines two variables num1 and num2 with numeric values, performs basic arithmetic operations (addition, subtraction, multiplication, division, and modulus), and displays the results. It also perfomes more complex calculations such as raising num1 to the power of 2 and calculating the square root of num2, and displays those results as well.

To proceed

Create and Open a file called <em> calculations.sh</em>
Write and save the code
```
#!/bin/bash

# Define two variables with numeric values
num1=10
num2=5

# Perform basic arithmetic operations
sum=$((num1 + num2))
difference=$((num1 - num2))
product=$((num1 * num2))
quotient=$((num1 / num2))
remainder=$((num1 % num2))

# Display the results
echo "Number 1: $num1"
echo "Number 2: $num2"
echo "Sum: $sum"
echo "Difference: $difference"
echo "Product: $product"
echo "Quotient: $quotient"
echo "Remainder: $remainder"

# Perform some more complex calculations
power_of_2=$((num1 ** 2))
square_root=$(echo "sqrt($num2)" | bc)

# Display the results
echo "Number 1 raised to the power of 2: $power_of_2"
echo "Square root of number 2: $square_root"


```


Set executable permission with the command `sudo chmod +x calculations.sh`
Run the script with the command `./calculations.sh`
![](./images/05.%20numbers%20and%20calculations.png)


### File backup and Timestamping

This script is focused on file backup and timestamp. 

The script defines the source directory and backup directory paths. It then creates a timestamp using the current date and time, and creates a backup directory with the timestamp appended to its name. The script then copies all files from the source direcrtory to the backup directory using the `cp` command with the `-r` option for recursive copying. Finally, it displays a message indicating the completion of the backup process and shows the path of the backup directory with the timestamp.

To execute this:
Create and open a file with the name <em> backup.sh</em>

Write the codes below and save:

```
#!/bin/bash

# Define the source directory and backup directory
source_dir="/path/to/source_directory"
backup_dir="/path/to/backup_directory"

# Create a timestamp with the current date and time
timestamp=$(date +"%Y%m%d%H%M%S")

# Create a backup directory with the timestamp
backup_dir_with_timestamp="$backup_dir/backup_$timestamp"

# Create the backup directory
mkdir -p "$backup_dir_with_timestamp"

# Copy all files from the source directory to the backup directory
cp -r "$source_dir"/* "$backup_dir_with_timestamp"

# Display a message indicating the backup process is complete
echo "Backup completed. Files copied to: $backup_dir_with_timestamp"

```
Set executable permission on backup.sh using the command `sudo chmod +x backup.sh`

Run the script with the command `./backup.sh`
Here is the outcome
![](./images/06.%20backup%20and%20timestamp.png)

