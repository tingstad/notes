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

A shell script will get a new group which all children enter.


## Shell

The shell uses process groups for job control. Job == process group. These can be in the background or foreground.

When Ctrl-C is entered in the terminal, SIGINT is sent to the foreground process group (all processes in the group).


## Shell scripts

Just as any other program, shell scripts may want to clean up and shut down gracefully on cancellation.


### Footnotes

¹ A group _may_ have a leader process with PID equal to process group id
