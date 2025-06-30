# Devops

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

#!/bin/bash
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


