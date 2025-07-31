
This notes helps to learn the basic linux commands who willing to learn the linux 
| Command          | Description                          |
| ---------------- | ------------------------------------ |
| `echo`           | Print messages to the screen         |
| `read`           | Accept user input                    |
| `exit`           | Exit the script                      |
| `chmod`          | Change file permissions              |
| `chown`          | Change file owner/group              |
| `ls -l`          | Long listing of directory            |
| `mv`             | Move or rename files                 |
| `cp`             | Copy files                           |
| `cd`             | Change directory                     |
| `pwd`            | Print working directory              |
| `rm`             | Delete files or directories          |
| `mkdir`          | Make a new directory                 |
| `rmdir`          | Remove empty directory               |
| `touch`          | Create empty files                   |
| `find`           | Search for files                     |
| `cat`            | Show file content                    |
| `less`, `more`   | View content page-by-page            |
| `tail`, `head`   | Show end/beginning of a file         |
| `top`, `htop`    | Real-time process monitoring         |
| `ps`             | Show running processes               |
| `free`           | Show memory usage                    |
| `df`, `du`       | Show disk space                      |
| `uptime`         | Show how long the system is running  |
| `hostname`       | Display system hostname              |
| `tar`, `gzip`    | Archive/compress files               |
| `curl`, `wget`   | Download from internet (APIs, files) |
| `ping`           | Network reachability test            |
| `ifconfig`, `ip` | Show network interfaces              |
| `netstat`, `ss`  | View open ports/sockets              |

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

If your Completed the above Commands you can look at this shell script commands 

--- what is shell script ?
A **shell script** is a file containing a series of shell (bash) commands that are executed together.

grep command 

grep 'pattern' file.txt
grep -i 'error' logfile.txt    # Case-insensitive
grep -r 'main()' src/          # Recursive search in directory

sed – Stream Editor

Used to search, replace, delete, or insert text in files line by line.
sed 's/old/new/' file.txt     # Replace first occurrence in each line
sed 's/old/new/g' file.txt    # Replace all occurrences per line
sed -i 's/old/new/g' file.txt # Edit file in-place
sed example : 
sed 's/sayed/SAYED/' file.txt           # Replace first "sayed" with "SAYED"
sed -i '2,5s/sayed/SAYED/g' file.txt    # Replace "sayed" with "SAYED" only from lines 2–5
sed '3d' file.txt                       # Delete line 3

awk – Text Pattern Scanner 
awk processes files line by line and works with columns (fields).
awk '{print $0}' file.txt         # Print entire line
awk '{print $1}' file.txt         # Print first column
awk '{print $1, $3}' file.txt     # Print 1st and 3rd column
awk -F',' '{print $1}' data.csv   # Use comma as delimiter
example 
awk '$3 > 80 {print $1, $3}' data.txt      # Print if 3rd column > 80
awk '/error/ {print $0}' logfile.txt       # Print lines containing "error"
awk '{sum += $2} END {print sum}' file.txt # Sum of 2nd column
awk '{count++} END {print count}' file.txt # Line count
awk '{printf "Name: %s | Score: %d\n", $1, $2}' scores.txt

 cut – Column Extractor 
Used to extract columns from files or outputs.
example : suppose you have a file and below are the file content 
john:x:1001:1001:John Doe:/home/john:/bin/bash
alice:x:1002:1002:Alice Smith:/home/alice:/bin/bash
if you use cut command you will get below o/p 
cut -d':' -f1 users.txt      # Output: john, alice
cut -c1-10 file.txt          # Print characters 1 to 10 from each line

wc – Word/Line/Byte Count
wc -l file.txt    # Line count
wc -w file.txt    # Word count
wc -c file.txt    # Byte count
wc -m file.txt    # Character count
wc -L file.txt    # Length of the longest line
wc file.txt       # All stats: lines, words, bytes

!!!!!!!!!!!!!!!!!!!!!i will update more useful and most used commands soon !!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!! Thank You Guys !!!!!!!!!!!!!!!!!!!!!!!!
