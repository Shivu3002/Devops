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
set -e	Exit on any command failure [“Exit the script immediately if any command fails (non-zero exit code).”
set -o pipefail	Exit if any command in a pipeline fails [“If any command in a pipeline fails, the entire pipeline fails.”]
Fail fast on any error (set -e)
Accurately catch pipeline failures (set -o pipefail)


