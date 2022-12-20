# Solaris

First we need to identify the PID process that the user is using 

To get the pid:
```
ps -ef -o ppid,pid,tty,comm
```
To spy on what the process is doing:
```
truss -f -w all -t write -p $PID

truss -f -w all -t write -T -o trace.out -r -v -p $pid

```
```
mount -F lofs /tmp /proc/PID
```

This allows you to spy what the a user/process is doing.

```
#!/bin/bash
function listProcess {
	ps -ef -o ppid,pid,tty,com | grep ${process}
}
function spyTruss {
	truss -f -w all -t write -p $pid
}

echo -ne "\r"
read -p "Which process would you like to check?" process
listProcess
echo -ne "\r"
read -p "Which PID would you like to spy on?"
spyTruss
```
New and improved?
```
#!/bin/bash

# Lists all processes with the specified name
# usage: listProcessesByName process_name
function listProcessesByName {
  # Use the -w option to match the process_name only as a complete word
  ps -ef -o ppid,pid,tty,com | grep -w ${1}
}

# Traces the specified process
# usage: spyTruss pid
function spyTruss {
  # Check if the pid variable is set and is a valid integer
  if [[ -n "${1}" && "${1}" =~ ^[0-9]+$ ]]; then
    # Use the -p option to specify the PID of the process to trace
    truss -f -w all -t write -p ${1}
  else
    # Handle the case where the pid variable is not set or is invalid
    echo "Error: invalid or missing PID"
  fi
}

# Read the process name from the user
read -r -p "Which process would you like to check? " process

# List all processes with the specified name
listProcessesByName ${process}

# Read the PID from the user
read -r -p "Which PID would you like to spy on? " pid

# Trace the specified process
spyTruss ${pid}
```

More to follow
