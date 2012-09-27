Dtrace scripts.
=============
A personal collection of some Dtrace scripts (tested on OS X).

You can also run `man -k dtrace` to see a useful collection of scripts, or also
in /usr/share/examples/DTTk. Most of this have been readed from
[here](http://dtrace.org/blogs/brendan/2011/10/10/top-10-dtrace-scripts-for-mac-os-x/)

**IOSNOOP**
This "traces" disk I/O execution live. Each time a disk I/O completes, a line of
output is printed to summarize it, including process name and filename details.
This lets you instantly find out which applications are using the disk, and what
files they are reading or writing to. Disk I/O is typically slow (for non-SSD
disks), so an application calling frequent disk I/O (a dozen per second or more)
may run slowly as it waits for the disk I/O to complete.
The output columns are:

- UID = User ID,
- PID = Process ID,
- D = direction (R = read, W = write),
- BLOCK = location on disk,
- SIZE = I/O size in bytes,
- COMM = Process name,
- PATHNAME = trailing portion of file pathname.

For other options of iosnoop run iosnoop -h (no need "sudo" for this). (for
performance issues use "-stoD" to show start and end timestamps for each I/O in
microseconds.

**HFSSlower**
Script with columns:

- TIME = time of I/O completion,
- PROCESS = application name,
- D = direction (R = read, W = write),
- KB = I/O size in Kbytes,
- ms = I/O latency in milliseconds,
- FILE = filename.

If you use the argument "0", it will trace everything. If you use "10" show I/O
slower than 10 milliseconds.

**EXECSNOOP**
This traces the execution of new processes. Great at identifying *short-lived
processes* (usually too quick to be picked up by standard monitoring tools like
top(1))that may be caused by misbehaving applications and can slow down the
system.
Columns:
- STRTIME = (string) timestamp,
- UID = user ID,
- PID = process ID,
- PPID = parent process ID,
- ARGS = process name (should be process + arguments but that doesn't work yet
  on OS X).

**OPENSNOOP**
This traces file opens and prints various details, including the time and error
code when using `-ve`. Useful for failed opens (also discover the path of
configuration file for apps).

**DTRUSS**
Traces all types of system calls, which is very useful for general debugging,
especially since OS X doesn't come with a standard syscall tracer (see Linux
strace). Dtruss can trace multiple processes at the same time, matching on the
name "-n". The `-e` option shows the elapsed time for the system call in
microseconds.

**SOConnect Mac**
*currently not working on OS X 10.8*
Traces outbound TCP connections along with details. Quick way to find out what
applications are connecting to whom on the Internet.

**ERRINFO**
This tool provides a summery of which system calls were failing, showing the
process name, error code and short description of the error. Quick way to track
down failing or misconfigured applications.

**BITESIZE.D**
Script that characterizes the disk I/O workload, showing a distribution of the
size of the I/O in bytes along with the appication name.
`sudo bitesize.d`

**IOTOP**
This represent the same data as iosnoop, but in a summarized way similar to
top(1). It's handy when disk I/O is so frequent that iosnoop is too verbose and
you want a high level summary of which process is rattling the disk. (-CP -> -C
rolling output and -P disk busy percentages).

**MACLIFE.D**
Traces creation and deletion of files.

