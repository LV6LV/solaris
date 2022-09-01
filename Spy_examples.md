# Solaris

First we need to identify the PID process that the user is using 

To get the pid:
```
ps -ef -o ppid,pid,tty,comm
```
To spy on what the process is doing:
```
truss -f -w all -t write -p $PID
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

More to follow
