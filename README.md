# git-commands

This file is to store git commands which I have learnt

# Basic Commands:

date - shows the date for today
ls - lists files in directory
cd - go to another directory
cd .. - move back up in a directory (. represents current directory .. is parent directory)
pwd - print working directory (shows the full path of the directory)
ls -IA - lists hidden files
touch - creates new files

# Parameters:
.. - . is current directory and .. is parent directory
" - " - these are called switches

# More commands:
mkdir - make a new directory 
rmdir - remove a directory (only works for empty directory)
rm - removes files (difficult to recover after)
rm -i - -i parameter is for interactive and confirm you want to delete each file
rm -f - forces remove file
cp someFile newFile - copy command takes two parameters (the first file is to be copied and the second is the new file it will be created from)
mv newFile ../newFile - if we wanted to move the file elsewhere. First is file to be moved (and its location) and second is where it should be moved to

** If you run "mv" without providing a new destination, you can rename a file **
mv newFile newerFile - This renamed newFile to newerFile

cat SomeText - Once you made a file you can view the contents using cat (someText is a file with some text inside it)
cat means concatenate - just to combine
cat can create short text documents right inside the terminal
cat > someMoreText - This shows a blinking cursor to type text in, then you leave by CTRL+C
cat someText someMoreText > combined - this combines two files
cat combined - you can see the combined files

# Viewing Larger Files - if you had some long text in longText.txt file
less longText.txt - shows all the text in terminal and you can scroll up and down, q for quit
head -3 longtext.txt to show the first 3 lines (can change for any other amount)
tail -3 longtext.txt for the last 3 lines 
tail -f log.txt -f shows new lines as they are added to the log (useful if making changes and need to view log)

# Getting help with commands
man ls - manual command and you input the parameter you need help with (in this case ls)

Intermediate Commands

# Streams: Every program on computer has 3 streams
- Keyboard is input stream (program reads what you typed on keyboard) #0 stdin
- Screen is output stream (sending data to the output/print on screen) #1 stout
- Error stream - Usually sent to display but only with error #2 stderr

We need to know the above as we can redirect streams, connect to different streams from different programs.

# Pipes & Redirection: - Pipes allow you to redirect streams

Example 1:
cat combined | less
- The "|" symbol/pipe passes the output stream (what's printed on screen) of the command to the LEFT of the pipe to the input stream of the command on the right
- In this example it is passing the output of the file "combined" into the "less" command
- This will make "less" take the input from the input stream as opposed to opening the file given as an argument 
- The output of cat combined will NOT be shown on the screen at all, since we redirected the output stream to the input stream of LESS.

Example 2: 
- If you had lots of files in current directory and would like to view them using "less" instead of just printing thousands of lines on the screen, you can redirect the output of ls to less.

ls -IA | less
- Then you will be able to move up and down the text as if the output were a normal file

Example 3:
- In addition to redirecting the output stream of one program to the input stream of another one, we can redirect the output to a FILE

cat combined > newCombined
- The greater-than symbol writes the output stream of the command on the left to the file on the right. We did not have the "newCombined" file before, so we wrote the output of "combined" into a new file that we named "newCombined"

# Wildcards

- Display exactly what you want with *
- If we had 1000 files, how can we grab just the txt ones?
- ls.*txt is used - * is the wild card
- ls new*.txt - shows only the ones that contain new and are txt
- ls *n* - shows only files that contain the letter "n"

Example breakdown: find . -name "*.txt" -print
- Starts by saying find all the files starting with the current directory (.)
- With any name that ends with .txt
- Print it to screen
- Find command - if you have additional directories inside the directory you're searching in, it will also go into those and an continue th search

Example 2: cd ~ find . -name "*.txt" -print
- cd is to change directories into home directory
- find command to go through and print all txt files in home directory, plus any other directories inside the home directory

Example 3: Create a txt document that lists every pdf file in downloads directory?
cd ~/Downloads find. -name"*.pdf" -print myPdfs.txt

# grep
- If you wanted to look INSIDE files
- Example, if you wanted to find files which contain the word 'binary'
grep binary combined
- parameters, first is the word searched for, second is the files to search through (in this case, files containing combined")
- Also possible to use PIPE to redirect output to grep, so it works with the text from the input stream, instead of the FILE we gave it as an argument
- EG printing all txt files in home directory that have 'readme' in their name
find ~ -name "*.txt" -print | grep README
- Command finds all txt files, but instead of printing them, all filenames will be redirected to grep
- Grep will only print those filenames that include 'readme'

# regular expressions
- grep is a regular expression which describes a pattern you are looking for
- more powerful than wildcards

Example 1:
find ~/Documents -name "*" -print | grep "\d\+"
- This command looks for all files in Documents directory that have numbers in the name path

# counting words
- What if a file was lots of words or we wanted to check the wordcount
wc longText.txt (output is 8  492  3003)
- These show the lines, words and character counts for the txt

- You can use PIPES to count the output of other programes
- For example below, how many readme files do we have in the HOME directory 

find ~ -name "*.txt" -print | grep README | wc -I
- first, find will find all the txt files
- grep selects those with readme in their name
- wc -I will count how many lines were given to it by grep

# permissions

- control access to every file on computer 
- some files can be read-only, some can be unavailable to users

whoami <- find our what your username is by this command
- permission classes: "user", "group", "others"
- permission levels: read, write and execute 

"User" class - determines permission that the owner of the file has (a file can have at MOST 1 owner)
"Group" class - permissions that apply to the group of user. Any user can belong to one or more groups of users
"Others" class - all users that don't fall into the other two classes

"read" permission - read file
"write" - write to a file (create, delete, rename)
"execute" - execute the file (EG in order to run a program, it must have execute permission set)

View Permissions by listing them in a "long" format
ls -l

Permission example: "drwxr-xr-x"
- first letter shows the type of file. "d" for directory 
- first letter if it is a hyphen "-", is just a file and not a directory
- three groups of characters: "r", "w", "x" or "-"
- first group determines permissions of the "user" class 
- in this case, the user has read, write and execute permissions due to the rwx after the d
- second group are the permissions of the group class
-"r-x" is shown after which means that the group has read and execute permission, but not write
- last three characters are permission for "other" class of users 
- "r-x" only read and execute

Changing permissions
- Command responsible is "chmod" 
- For example if you wanted to give a user write permission on a file "readme.txt" you'd do
chmod u+w readme.txt
- "u" stands for user (it would be "g" for group, "o" for others and "a" for all
- "+" means we're adding permission ("-" would mean we're removing it)
- "w" stands for write, you can combine several permissions
For example, below we are removing the read and execute permission for all users:
chmod a-rx

# Shebang
- If we wanted to run a program (just like irb) by typing its name in the terminal without explicitly invoking Ruby (i.e typing ruby file.rb) then we would need to:
1) fix the permissions
2) add a shebang

./hello.rb (./ is a hint for terminal that we want to run hello.rb from current directory)
bash: ./hello.rb: Permission denied
- the reason we get permission denied is because the file doesn't have the executable permission by default
- we can give the USER (u) permission to execute (x) the file
chmod u+x hello.rb
- if we re-run the file below, the permission denied error is gone but we get another problem
./hello.rb ./hello.rb: line 1: puts: command not found
- the shell complains that it does not know what 'puts' means on line 1
- this is because the computer doesn't know what programming language it was 
- it does not assume even though it looks like the file is ruby
- even if it's ruby, we have several versions of ruby, so again it can't be expected to know which version. the computer needs specific instruction

SHEBANG is the instruction for your computer that tells the computer what program to use to execute your script
- Shebang is a combination of hash ('sh) and an exclamation mark (bang) followed by the path to the interpreter, placed on the very first line of the file.

- we want our file to be executed by ruby, so we use the WHICH to find where ruby is
which ruby
/Users/sidra/.rvm/rubies/ruby-3.0.0/bin/ruby
- once we know where Ruby is located, we can add a Shebang to the hello.rb using VS code

BELOW IS DONE IN VS CODE
#!/Users/tommywilliams/.rvm/rubies/ruby-3.0.0/bin/ruby
puts "Hello world"

- The above tells the command line to use this ruby interpreter to execute the file
- Now if we type './hello.rb' it will print "Hello, world"
./hello.rb --> Hello, world!
- Behind the scenes, is that the shell (command line) passes the contents of the file to the interpreter that we specified in the shebang
- However, we have just hardcoded the path to Ruby right in the file
- It will work on this computer, but not any other because the path to Ruby will be DIFFERENT
- There is a BETTER solution, changing the shebang

VScode - file name hello.rb
#!/usr/bin/env ruby 
puts "Hello, world!"

the '/usr/bin/env ruby' command loads the environment and executes whatever ruby you have on your machine

irb is an EXAMPLE of a program that we can run WITHOUT typing ruby
- `which irb` command returns the path to irb, it is enclosed in backticks which are different to apostrophes
- This makes the command line 'expand' the command, 

# Superuser mode

- special admin privileges
- SU has all the rights and permissions to all programes
- they can do things like
rm -rf ./* which deletes all files in the current directory 
rm -rf /* which deletes all files on HARD DRIVE - do not TRY THIS!
- bad practice to work on this permanently
- you can execute a command as a superuser by prefixing it with 'sudo'
- eg if you wanted to delete a file that you didn't have permission for and you know the superuser password you can
sudo rm inaccessiblefile
- it will ask for password and then the command line will be executed with superuser privileges

# Environment
- command line operates in an environment (similar to our physical environment like country, city etc)
- command line environment, it knows where home folder is, where to find ruby, what username is etc
- all the information is STORED in ENVIRONMENT VARIABLES
- environment variables or env vars for short, describe the current terminal session
type 'env'
- you will see all of the details for the current session 
- it is shown in key value pairs (EG HOME=/Users/Sidra) - key is home and value is /Users/Sidra
- HOME env variable defines where the home directory is for the current user
- You can easily find variables responsible for setting username, language etc
echo $ENV_VAR <- view any single environment variable EG:
echo $HOME /Users/sidra

# Echo
- Echo prints whatever text you give it on the screen
$ echo "Hello" --> prints Hello
- Echo can also be used to view environment variables above
- Echo can be used to save short strings to a file. EG wanted to create a file with text "Hello, world"
echo "Hello, world" > hello.txt


# Path 
- Colon-separated list of directories where the shell will be looking for the programs you ask it to run. Example below with makers computer
echo $PATH
/Users/makers/.rvm/gems/ruby-3.0.0/bin:/Users/makers/.rvm/gems/ruby-3.0.0@global/bin:/Users/makers/.rvm/rubies/ruby-3.0.0/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/makers/.rvm/bin
- It lists all the paths, separated by colons
- When you type a command without specifying its path, the shell looks through all directories until it finds and executes it
- When you type 'ls' the shell will examine the above paths in order until it finds a file that exists 
- In the above case, 'ls' will be found in '/bin/ls' which is where a file is found

NB: Remember when we had to type './hello.rb' to execute the file, instead of just 'hello.rb'
- This is because if we hadn't explicitly specified that the program was in the current directory, the shell would look in all PATH directories and wouldn't find it there
- Every program you run from your command line without specifying where the are (ls, date, pwd) are somewhereon the PATH
