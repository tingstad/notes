# Unix signals and shell scripts

Signals are standardized notifications used for asynchronous interprocess communication.

Other than which signal was sent, the signals do not carry any additional information, not even who sent them.

A program is usually interested in handling signals so that is can shut down gracefully, after cleaning up temp files etc.

The default "kill" signal is TERM (terminate). Another important signal is INT (interrupt).

## Unix processes

Every process, except the root process, has a parent (environment variables etc. are inherited). This gives ut a process tree.
When a program runs a new command, that new process will be a child of the original process.
If a parent is killed, the child will get a new parent (an ancestor process).

All processes also belong to a _process group_. A process may join an existing group or a new one¹.

Signals may be sent to individual processes or process groups!

```shell
kill -s TERM 123      # signals process 123
kill -s TERM -- -123  # signals process group 123
```

To see processes' group id and more, run:

```shell
# user ID,  PID,  parent, group ID, state, command, args
ps -o uid -o pid -o ppid -o pgid -o stat -o comm -o args
```

Get process group id of given PID:
```shell
ps -o pgid= -p "$$"  # ($$ = current shell)
```

## Shell

The shell uses process groups for job control. Job == process group. These can be in the background or foreground.

When Ctrl-C is entered in the terminal, SIGINT is sent to the foreground process group (all processes in the group).

When a shell script (or other program) is started, a new process group is created, which any of its children will join.

Running a pipeline command (`foo | bar`) is also a job, because it makes sense to stop/resume all its processes at the same time.

## Shell scripts

Just as any other program, shell scripts may want to clean up and shut down gracefully on cancellation.

The way to do this is to `trap` (catch) signals and react to them.

Some shells, including Bash, implements "cooperative exit", where the parent shell interrupts only if the child process did not "ignore" SIGINT.

To make your program (or shell script) behave nicely, you must therefore not "ignore" SIGINT TODO


### Footnotes

¹ A group always begins having a leader process with PID equal to process group id
