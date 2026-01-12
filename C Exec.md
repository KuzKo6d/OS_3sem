# Exec()
## Variants
- `l`/`v` - `list`/`vector`
- `p` - use `PATH` (environment variable)
- `e` - use custom environment variables

## 
`execlp("ls", "ls", "-l", NULL)`
- Here we use `p` to not write the whole path.
- Write `argv` parameters after `"ls"`. Write from `argv[0]`.

`execvp("ls", argv)`
- `argv` ends with `NULL`.


> Exec pastes image of program into process.

When `exec()` was used:
- PID - saves
- PPID - saves
- Descriptors - saves
- Signal handlers - drops
- Signal ignore - saves (except `SIGCHLD`?)

# Conveyor example
`ls | wc`

```c
int main() {
	int fd[2];
	pipe(fd);
	int pid1, pid2;
	
	if ((pid1 = fork()) == -1) return 1;
	if (pid1 == 0) { // child 1 ls
		dup2(fd[1], 1);
		close(fd[0]);
		close(0);
		execlp("ls", "ls", NULL);
	}
	
	if ((pid2 = fork()) == -1) return 1;
	if (pid2 == 0) { // child 2 wc
		dup2(fd[0], 0);
		close(fd[1]);
		close(1)
		execlp("wc", "wc", NULL);
	} else {
		close(fd[0]);
		close(fd[1]);
		
		waitpid(pid1, NULL);
		waitpid(pid2, NULL);
	
		return 0;
	}
}
```

`ls; ls -l`
```c
int main() {
	int pid1, pid2;
	
	if ((pid1 = fork()) == -1) return 1;
	if (pid1 == 0) { // son1 ls
		execlp("ls", "ls", NULL);
	} else {
		wait(NULL);
	}
	
	if ((pid2 = fork()) == -1) return 1;
	if (pid2 == 0) { // son2 ls -l
		execlp("ls", "ls", "-l", NULL);
	} else {
		wait(NULL);
	}
	
	return 1;
}
```

`ls ./Nosuchdir -a && ls -l`
```c
int main() {
	int pid1, pid2;
	int status;
	
	if ((pid1 = fork()) == -1) return 1;
	if (pid1 == 0) { // son1 ls
		execlp("ls", "ls", NULL);
	} else {
		wait(&status);
		
		// check exit fact
		if (WEXITED(status) != 0) return 1; 
		// check exit status (0 or error codes)
		if (WEXITSTATUS(status) != 0) return 1;
	}
	
	if ((pid2 = fork()) == -1) return 1;
	if (pid2 == 0) { // son2 ls -l
		execlp("ls", "ls", "-l", NULL);
	} else {
		wait(NULL);
	}
	
	return 1;
}
```

> Write all tree from grand son, son and father.

```c
int main() {
	int fd[2];
	int pid_father = getpid();
	int pid_son;
	pipe(fd);
	
	if ((pid_son = fork()) == -1) return 1;
	if (pid_son == 0) { // son
		int pid_son_son;
		if ((pid_son_son = fork()) == -1) return 1;
		if (pid_son_son == 0) { // grand son
			close(fd[0]);
			close(fd[1]);
		
			printf("Father - %d\n", pid_father);
			printf("Son - %d\n", pid_son);
			printf("Grand son - %d\n", pid_son_son);
			
		} else { // son
			close(fd[0]);
			
			printf("Father - %d\n", pid_father);
			printf("Son - %d\n", pid_son);
			printf("Grand son - %d\n", pid_son_son);

			if (write(fd[1], pid_son_son, sizeof(int)) != sizeof(int)) {
				wait(NULL);
				return 1;
			}
			wait(NULL);
			return 0;
		}
	} else { // father
		int pid_son_son;
		close(fd[1]);
		if (read(fd[0], pid_son_son, sizeof(int)) != sizeof(int)) {
			wait(NULL);
			return 1;
		}
		close(fd[0]);
	
		printf("Father - %d\n", pid_father);
		printf("Son - %d\n", pid_son);
		printf("Grand son - %d\n", pid_son_son);	
	
		wait(NULL);
		return 0;
	}
}
```

> Same for father, son1, son2.

```c
int main() {
	int fd[2];
	pipe(fd);
	int pid_son1, pid_son2;
	
	if ((pid_son1 = fork()) == -1) return 1;
	if (pid_son1 == 0) { // son1
		close(fd[1]);
		read(fd[0], pid_son2, sizeof(int));
		
		printf("father: %d, son1: %d, son2: %d\n", getppid(), getpid(), pid_son2);
		
		close(fd[0]);
		return 0;
	}
		
	if ((pid_son2 = fork()) == -1) return 1;
	if (pid_son2 == 0) { // son2
		close(fd[0]);
		write(fd[1], getpid(), sizeof(int));
		close(fd[1]);
		
		printf("father: %d, son1: %d, son2: %d\n", getppid(), pid_son1, getpid());
		
		return 1;	
	} else { // father
		wait(NULL);
		wait(NULL);
		
		printf("father: %d, son1: %d, son2: %d\n", getppid(), pid_son1, pid_son2);
		
		return 0;
	}
}
```


> Duplex in pipe may be implemented by using file existential as flag.