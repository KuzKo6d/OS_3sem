#programming 
# Fork()
`pid_t fork()`
- Returns `-1` / `0` / `num > 0`
- Usage:
```c
if (pid = fork() < 0) return 1;
if (pid > 0) {
	/*parent*/
	/*wait()*/
} else {
	/*"child"*/
}
```
- Parent needs to `wait()` for end of child. Else system need some time to process situation.
# Wait()
```c
int status; /* stores exit status inside */
wait(&status);

wait(NULL); /* or this if status not needed */
```

Macros for analiysing status we use macros:
```c
if (WIFEXITED(status)) { // wait if exited
	printf("%d", WEXITSTATUS(status);
}
```

# Pipe()
Unnamed channel - only for family processes
```c
int fd[2];
pipe(fd);
```
- `fd[0]` - read descriptor
- `fd[1]` - write descriptor
- returns `-1` and sets `errno` or `0`
> Under hood it's a file, that belongs to kernel.

Diff between simple file and pipe is name and some read/write issues.

Read n bytes. In pipe m bytes:
- n <= m - read n bytes
- n > m - read m bytes
- m = 0 - block until data appears
	- if all writers are closed - reader takes EOF. (read will return 0)

Write n bytes
- There is needed space - write n bytes
- No space - block
- All read descriptors are closed - write returns error `broken pipe` and signal `SIGPIPE`

Let's use two pipes for duplex connection:
> But sometimes we need to use only one pipe. List of file descriptors is not infinity in size.
- Process 1 -> Channel 2 -> Process 2
- Process 2 -> Channel 1 -> Process 1

Pipe in terminal:
`ls|wc` where `|` - is pipe. It's a conveyor.

`cat big-file.txt | head` - returns first 10 strings and closes channel. Cat returns text to pipe instead of stdout -> Head uses pipe instead of stdin -> Head returns result to stdout.
# Fcntl()
> Swiss-knife function

Provides to change working mode without closing descriptor.


# Conveyor example
```c
#include <>

int main() {
	int fd[2];
	pipe(fd);

	if (pid = fork() == -1) return 1;
	if (pid > 0) { // father - writer
		close(fd[0]);
		dup2(1, fd[1]);
		
		char *str1 = "abobamiumiu";
		write(fd[1], str1, strlen(str1) + 1;
		close(fd[1]);
		
		wait(NULL);
		return 0;
		
	} else { // son
		close(fd[1]);
		dup2(0, fd[0]);
		char *buf = (char*) malloc(101 * sizeof(char));
		buf[100] = '\0'
		
		while (read(fd[0], buf, 100)) {
			printf("%s", buf);
		}
		close(fd[0]);
		
		printf("\n");
		free(buf);
		return 0;
	}
}
```

> EOF - is not a symbol. It's just an indicator, returned by function.
> Function has several ways to detect end of file.

> Мы не можем взять яблоки в обход завхоза.
> 
> \- Никольский 

> Если писатель в течение часа ничего не написал, то, наверное, он уже умер.
> 
> - Никольский


- [[C DZ5.canvas]]