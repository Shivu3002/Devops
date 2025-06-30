# Devops linux commads and its uses:

1:nproc – Number of Processing Units--
Purpose: Displays the number of available CPU cores.
Use Case: Useful for performance tuning or configuring parallel processes (e.g., in Jenkins builds or Docker)

2:df – Disk Free
Purpose: Shows available and used disk space on mounted file systems.
df -h  :-h: human-readable format (e.g., shows GB/MB instead of bytes)
Use Case: Check disk usage to avoid storage full issues

3. chmod – Change File Permissions
Purpose: Changes file or directory permissions

chmod [options] mode file
Examples:
chmod 755 script.sh → Gives read/write/execute to owner, read/execute to group & others
chmod +x file.sh → Adds execute permission
Permissions Reference:
r = read (4)
w = write (2)
x = execute (1)
Use Case: Used to grant or restrict access (e.g., making a .sh file executable).

4. top – System Resource Monitor
Purpose: Real-time display of system performance (CPU, memory, processes).

CPU usage
Memory usage
Running processes
PID, USER, %CPU, %MEM, TIME+
Use Case: Troubleshoot high CPU/memory usage or check running processes.
Press q to exit top.

5. free – Memory Usage
Purpose: Displays total, used, and free RAM and swap.

Syntax:free -h

Use Case: Monitor memory pressure or usage before deploying applications.

6:#!/bin/bash
is called a shebang (also written as #!), and it's very important when writing shell scripts in Linux.

✅ Why We Use #!/bin/bash
🔹 1. Specifies the Script Interpreter
It tells the operating system:
“Use the Bash shell (located at /bin/bash) to run this script.”

Without it, the system may default to another shell (like /bin/sh or /bin/dash), which can cause errors if your script uses Bash-specific features.

 2. Ensures Portability and Consistency
If someone else runs your script, it ensures it runs with the intended interpreter.

Avoids unpredictable behavior caused by default shell differences.


7:set -x
The command set -x is used in a Bash script or terminal session to enable debugging mode.
Every command is prefixed with + and echoed to the screen before it runs.

set -x
echo "Starting script"
name="Shiva"
echo "Hello, $name"

🖥️ Output when run:
+ echo 'Starting script'
Starting script
+ name=Shiva
+ echo 'Hello, Shiva'
Hello, Shiva

Every command is prefixed with + and echoed to the screen before it runs.
✅ When to Use set -x
Debugging scripts that are not working correctly
Learning how a script behaves internally
Logging command execution during automation (e.g., CI/CD)

🛑 How to Turn It Off
Use set +x to disable debug mode:

#!/bin/bash
set -x
echo "Debug ON"
set +x
echo "Debug OFF"

8:ps -ef
The command ps -ef is used in Linux to list all running processes on the system in full-format output.

Stands for: Process Status
Used to view information about active processes.
🔹 -f
Stands for full-format listing, which shows more details like user, parent process, and start time.

UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 08:00 ?        00:00:03 /sbin/init
root      1234     1  0 08:01 ?        00:00:00 /usr/sbin/cron
user      4321  1234  0 08:02 pts/0    00:00:00 bash
user      4356  4321  0 08:03 pts/0    00:00:00 ps -ef

Column	Description
UID	User ID of the process owner
PID	Process ID
PPID	Parent Process ID
C	CPU usage (in % of total)
STIME	Start time of the process
TTY	Terminal associated with the process
TIME	Total CPU time used
CMD	Command that started the process

Grep command uses:
ps -ef | grep "amazon"
is used to search for running processes related to the keyword amazon

Part	Description
ps -ef	Lists all running processes with full details
`	` (pipe)
grep "amazon"	Filters the processes and shows only those that match "amazon"

🧰 When to Use It
To check if Amazon-related services (e.g., SSM agent, CloudWatch agent) are running
To troubleshoot AWS services installed on an EC2 instance
To find out whether any process using "amazon" in its command line is active


9:date | echo "today is"
won’t work as expected — and here’s why:
In Bash, the pipe (|) sends the output of the command on the left to the input (stdin) of the command on the right.
Take the output of date and send it as input to echo "today is"."
But echo "today is" ignores input from stdin — it only prints its own string.
So, the output will simply be:
today is
It completely discards the date output.

10:awk is one of the most powerful and useful command-line tools in Linux for text processing, especially for working with structured data like logs, CSVs, and tables.
✅ What is awk?
awk is a pattern scanning and processing language. It's used to:
Print specific columns
Filter rows based on conditions
Perform calculations
Format reports from text files or command output

awk 'pattern { action }' filename
Or to use it with pipelines:
command | awk 'pattern { action }'
🧪 Common Examples
1. Print specific column(s):

awk '{print $1}' file.txt
Prints the first column of each line.

Example:ps -ef | awk '{print $1, $2}'
Prints user ID and process ID from the ps output.

2. Use a delimiter (e.g., comma):

awk -F, '{print $2}' data.csv
Prints the second column from a CSV file (-F, tells awk to split by comma).

3. Filter by condition:

awk '$3 > 500' file.txt
Prints lines where the third column is greater than 500.

4. Print lines that match a pattern
awk '/error/' logfile.txt
Prints lines that contain the word "error".

5. Add column values (like sum):
awk '{sum += $2} END {print "Total:", sum}' file.txt
Adds all values from column 2 and prints the total.

🔧 Bonus: Use awk with system commands
Example: Get total memory from free -m

free -m | awk '/Mem:/ {print "Total Memory:", $2, "MB"}'
Example: List only process names from ps

ps -ef | awk '{print $8}' | sort | uniq
🧠 Summary
Task	Example Command
Print a column	awk '{print $2}'
Filter by condition	awk '$3 > 100'
Use custom delimiter	awk -F, '{print $1}'
Search for keyword	awk '/fail/'
Sum a column	awk '{sum+=$2} END {print sum}

11:
ps -ef | grep amazon | awk '{print $2}'
🔍 Explanation:
ps -ef → Lists all running processes
grep amazon → Filters only those lines that contain the word "amazon"
awk '{print $2}' → Prints the 2nd column, which is the PID (Process ID)

12:when ever we use pipe we shoul use below command for best practice and for failures
set -e	:Exit on any command failure [“Exit the script immediately if any command fails (non-zero exit code).”
set -o pipefail : pipefail	Exit if any command in a pipeline fails [“If any command in a pipeline fails, the entire pipeline fails.”]
Fail fast on any error (set -e)
Accurately catch pipeline failures (set -o pipefail)

Curl command:
✅ What is curl?
curl stands for Client URL. It is used to transfer data to or from a server using protocols like:
HTTP / HTTPS
FTP / SFTP
SCP, SMTP, etc.

📌 Basic Syntax:
curl [options] [URL]
🧪 Common Examples

1. GET Request (default)
curl https://example.com
Fetches the HTML content of a webpage.

2. GET with headers
curl -I https://example.com
Shows only response headers (like Content-Type, HTTP status, etc.)

3. POST Request with Data
curl -X POST -d "name=shiva&role=devops" https://example.com/api
Sends form data to the API using HTTP POST

4. Send JSON Data
curl -X POST -H "Content-Type: application/json" \
     -d '{"username": "shiva", "password": "pass123"}' \
     https://api.example.com/login

5. Download a File
curl -O https://example.com/file.zip
Downloads the file to current directory with same name

6. Follow Redirects
curl -L https://short.url
Follows HTTP 301/302 redirects

7. Add Custom Headers
curl -H "Authorization: Bearer TOKEN123" https://api.example.com/data
Sends an Authorization header (commonly used in APIs)

🔍 Useful Options
Option	Description
-X	Specifies HTTP method (GET, POST...)
-d	Sends data in body of request
-H	Adds headers (e.g., Content-Type)
-L	Follows redirects
-O	Saves output to file
-I	Fetches headers only
-u	Sends HTTP Basic Auth credentials

✅ Example: Test API Health
curl -s -o /dev/null -w "%{http_code}" https://example.com/health
Returns just the status code (e.g., 200) — great for automation or monitoring scripts.

🧠 Summary
Use Case	Command Example
API Testing	curl -X POST -d 'key=value' https://...
Auth Header	curl -H "Authorization: Bearer ..."
Download a file	curl -O https://...
Follow redirects	curl -L https://...
Debug HTTP response	curl -v https://...

✅ What is wget?
wget stands for Web Get. It’s a non-interactive command-line utility to download files from the web using:
HTTP / HTTPS
FTP
Unlike curl, which is more general-purpose for APIs and data transfer, wget is specialized for downloading.
📌 Basic Syntax:

wget [options] [URL]
🧪 Common wget Examples
1. Download a File
wget https://example.com/file.zip
Downloads file.zip to your current directory.

2. Download and Rename
wget -O newname.zip https://example.com/file.zip
Saves the file with the name newname.zip.

3. Download in Background
wget -b https://example.com/largefile.iso
Runs the download in the background and saves log to wget-log.

4. Resume a Partially Downloaded File
wget -c https://example.com/bigfile.zip
Continues download if it was interrupted.

5. Download Entire Website (Recursive)
wget -r https://example.com
Recursively downloads all pages and subpages.

Limit recursion depth:

wget -r -l 1 https://example.com
6. Download with User Agent or Headers
wget --user-agent="Mozilla/5.0" https://example.com
7. FTP Download (with credentials)

wget ftp://username:password@ftp.example.com/file.zip
✅ Key wget Options
Option	Description
-O	Output file with a custom name
-c	Continue/resume download
-r	Recursive download
-b	Background download
--limit-rate	Limit download speed (e.g. --limit-rate=500k)
--no-check-certificate	Ignore SSL errors
--user-agent	Set browser type header

🆚 wget vs curl
Feature	wget	curl
File downloads	✅ Very strong	✅ Good
REST APIs / JSON	❌ Not suitable	✅ Best for APIs
Recursive site mirror	✅ Yes (-r)	❌ No
FTP support	✅ Built-in	✅ Yes
Uploads	❌ Limited	✅ Full upload support

Find command:
The find command in Linux is a powerful utility used to search for files and directories in a directory hierarchy based on various criteria like name, size, type, time, and permissions.

✅ Basic Syntax

find [path] [options] [expression]
[path] — where to search (e.g., /, .)

[options/expressions] — how to filter the search

🧪 Common find Examples
1. Find by File Name
find . -name "file.txt"
Searches for file.txt starting in the current directory (.)

2. Find by File Extension
find . -name "*.log"
Finds all .log files under the current directory

3. Find Files by Type
Only files:
find . -type f
Only directories:

find . -type d
4. Find and Delete Files

find . -name "*.tmp" -delete
Deletes all .tmp files
⚠️ Always test with -print before using -delete.

5. Find by Size
find /var -size +100M
Finds files larger than 100 MB under /var
+100M = more than 100 MB
-10k = less than 10 KB

Exact: 100M = exactly 100 MB
6. Find by Modification Time

find . -mtime -7
Files modified in the last 7 days
-mtime -7 → modified within 7 days
-mtime +30 → modified more than 30 days ago

7. Find and Execute a Command
find . -name "*.sh" -exec chmod +x {} \;
Finds all .sh files and makes them executable.

{} is replaced by the found file

\; ends the -exec command

🧠 Summary Table
Option	Meaning / Use
-name	Match file/directory name
-type	f for file, d for directory
-size	File size filters like +100M, -10k
-mtime	Last modified time (-7 = within 7 days)
-exec	Run a command on each result
-delete	Delete matched files (use with care!)

📌 Pro Tip:
If you just want to list all files in a folder and subfolders:
find .

✅ Basic if Syntax in Bash:
if [ condition ]; then
    # commands if condition is true
else
    # commands if condition is false
fi
if → starts the condition
then → begins the true-block
else → optional, runs if the condition is false
fi → closes the if-statement (literally “if” spelled backward)

🧪 Example 1: Simple Number Check

#!/bin/bash
num=10
if [ $num -gt 5 ]; then
    echo "Number is greater than 5"
else
    echo "Number is 5 or less"
fi
🔍 Output:
csharp

Number is greater than 5
🧪 Example 2: File Existence Check

#!/bin/bash

if [ -f "/etc/passwd" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi
🔧 Common Conditions in Bash
Condition	Meaning
-f file	True if file exists
-d directory	True if directory exists
string1 = string2	True if strings match
num1 -eq num2	Equal numbers
num1 -gt num2	Greater than
num1 -lt num2	Less than
-z $var	True if variable is empty

Common Conditions in Bash
Condition	Meaning
-f file	True if file exists
-d directory	True if directory exists
string1 = string2	True if strings match
num1 -eq num2	Equal numbers
num1 -gt num2	Greater than
num1 -lt num2	Less than
-z $var	True if variable is empty

Loops:For,while,do
🔁 for Loop
✅ Syntax:

for var in list; do
    # commands

🧪 Example:

for name in Shiva Ravi Akash; do
    echo "Hello, $name"

🔍 Output:

Hello, Shiva  
Hello, Ravi  
Hello, Akash
📌 Use Case:
Looping through a list of values, files, or output from commands.

🔁 while Loop
✅ Syntax:
while [ condition ]; do
    # commands
🧪 Example:

count=1
while [ $count -le 3 ]; do
    echo "Count is $count"
    count=$((count + 1))

🔍 Output:
csharp

Count is 1  
Count is 2  
Count is 3
📌 Use Case:
Loop until a condition is no longer true — useful for waiting on services, retrying steps, etc.
🔁 Infinite while loop (use with care!)
while true; do
    echo "Running..."
    sleep 1

✅ do Keyword
In Bash, do is used to begin the block of code inside:

for loops

while loops

until loops

It’s similar to { in other languages like C or JavaScript.

🔃 Bonus: Loop Through Files

for file in *.txt; do
    echo "Found file: $file"
| **Loop Type** | **Description**              | **Example Use**                        |
| ------------- | ---------------------------- | -------------------------------------- |
| `for`         | Loop through items in a list | Files, names, command output           |
| `while`       | Loop based on a condition    | Counters, file existence, retries      |
| `do`          | Begins the command block     | Paired with `for`, `while`, or `until` |

✅ What Are Linux Signals?
A signal is sent to a process using the kill command or by the OS when specific events happen (e.g., pressing Ctrl+C).
Signal	Number	Key Combo / Trigger	Description	Use Case
SIGHUP	1	—	Hangup detected (terminal closed)	Reload config / restart daemon
SIGINT	2	Ctrl + C	Interrupt from keyboard	Gracefully stop running process
SIGQUIT	3	Ctrl + \	Quit and dump core	Debugging / forced termination
SIGKILL	9	—	Kill signal (cannot be ignored)	Force kill unresponsive process
SIGUSR1	10	—	User-defined signal 1	Custom behavior in applications
SIGUSR2	12	—	User-defined signal 2	Custom behavior in applications
SIGPIPE	13	—	Broken pipe	Write to closed pipe (e.g. socket)
SIGALRM	14	—	Alarm clock	Timeout events
SIGTERM	15	—	Termination signal	Graceful shutdown (default kill)
SIGCHLD	17	—	Child stopped or terminated	Notifies parent process
SIGCONT	18	—	Continue if stopped	Resume paused process
SIGTSTP	20	Ctrl + Z	Terminal stop	Pause foreground process

What is trap in Bash?
trap allows you to run custom commands when:
A specific signal is received (like SIGINT, SIGTERM, etc.)
A script exits, encounters an error, or gets interrupted

🧪 Common Examples
1. Gracefully handle Ctrl+C (SIGINT)

#!/bin/bash

trap 'echo "Caught Ctrl+C! Exiting..."; exit 1' SIGINT

while true; do
  echo "Working..."
  sleep 1
done
When you press Ctrl+C, the trap is triggered instead of exiting immediately.

2. Trap script exit (EXIT)

#!/bin/bash

trap 'echo "Script finished or exited!"' EXIT

echo "Doing something..."
sleep 2
EXIT runs the command when the script finishes — success or failure.

3. Clean up temporary files

#!/bin/bash

trap 'rm -f /tmp/tempfile.txt' EXIT

touch /tmp/tempfile.txt
echo "Temp file created"
Automatically deletes the file when the script ends.

4. Trap multiple signals

trap 'echo "Exiting..."; cleanup; exit' SIGINT SIGTERM
🧠 Common Signals Used with trap
Signal	Description
EXIT	When the script ends
SIGINT	Ctrl+C (interrupt)
SIGTERM	Termination (e.g. from kill)
ERR	When any command fails (with set -e)
SIGHUP	Terminal hangup

✅ Summary
Use Case	Trap Command Example
Ctrl+C handling	trap 'echo Interrupted' SIGINT
Script cleanup on exit	trap 'rm /tmp/file' EXIT
Catch command failure	trap 'echo Error occurred' ERR
Multiple signals	trap 'echo Exiting...' SIGINT SIGTERM



