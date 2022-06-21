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

When a shell script (or other program) is started, a new process group is created, which any children of it will join.

Running a pipeline command (`foo | bar`) interactively is also a job, because it makes sense to stop/resume all its processes at the same time.

## Shell scripts

Just as any other program, shell scripts may want to tidy up and shut down gracefully on cancellation.

The way to do this is to `trap` (catch) signals and react to them.

Some shells, including Bash, implements "cooperative exit", where the parent shell interrupts only if the child process did not "ignore" SIGINT.

To make your program (or shell script) behave nicely, you should therefore let the outside world know that you died from the SIGINT. To achieve this, you must re-raise the signal by sending it to yourself.

```shell
#!/bin/sh
trap 'cleanup' INT

cleanup() {
    echo "Received SIGINT" >&2
    # clean up temp resources
    trap - INT  # unset trap
    kill -s INT "$$"  # kill self
}

echo "I am pid $$"
sleep 30
```

### Other considerations in shell scripts

If you have started asynchronous processes (`foo &`), these will ignore SIGINT.

If you enable job control (`set -m`), all background processes run in a separate process group.

If you want to kill your children, you can't be 100% sure that the PID wasn't recycled the moment before `kill` executes.

A trap is not executed until foreground command has completed (see [spec](https://pubs.opengroup.org/onlinepubs/009695399/utilities/xcu_chap02.html#tag_02_11)). This is especially relevant for SIGTSTP.

### Footnotes

¹ A group always begins having a leader process with PID equal to process group id

### References

- https://www.linusakesson.net/programming/tty/
- https://www.cons.org/cracauer/sigint.html
- https://www.gnu.org/software/libc/manual/html_node/Termination-Signals.html
- https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_11
- https://mywiki.wooledge.org/SignalTrap
