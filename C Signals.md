# System Calls
- `signal()` - set the signal handler
- `kill()` - send signal to process
- `alarm(n)` - send `SIGALRM` after `n` seconds
- `pause()` - process blocks untill signal handled
- `sleep(n)` - sleep `n` seconds or untill signal handled (it's not using much processor resources unlike while(1))
---
- `wait()` returns `-1` if signal handled.

# Disposition
By default most of signals terminates process. Also it may stop/continue process and some other.
You cannot set signal handler for `SIGKILL` and `SIGSTOP`.
With `fork()` signal disposition inherits. With `exec()` - resets to default.

# Signal()
`sighandler_t signal(int signum, sighandler_t handler)`
or 
`void (*signal(int signum, void (*handler)(int n))) (int)`
- `void (*handler)(int n)` - pointer to function that returns void and takes int.
- In general it is function that returns pointer to function that takes int and returns void.
- `typedef void(*sighandler_t (int n))` is a sighandler_t explanation.
- `signum` - way to find out handled signal num.

*Example:*
```c
void hnd(int signum) {
	printf("Signal trapped\n");
	signal(SIGINT, hnd);
}

int main() {
	signal(SIGINT, hnd);
}
```


| Hotkey in Terminal | Signal             |
| ------------------ | ------------------ |
| Ctrl + c           | `SIGINT`           |
| Ctrl + z           | `SIGSTP`           |
| Ctrl + d           | `EOF` (not signal) |

In terminal:
- `ps -A` - list of processes
- `kill -9 <pid>` - send SIGKILL (-9) to process with specified PID

# Ping-Pong
Unnamed pipe, duplex, not close `fd[0]` and `fd[1]`.
=>
- No process takes EOF.
- Cannot use blocking ability of read().
- Need to use external sync like semaphores of signals.

> Ping-Pong models client-server interaction.

```c
#define CNT_MAX 10;

int ball = 0;
pid_t target_pid;
int fd[2]; // pipe transfers the ball

void handler(int s) {
	if (ball < CNT_MAX) {
		read(fd[0], &ball, sizeof(int));
		printf("process: %d; ball: %d", getpid(), ball);
		ball ++;
		write(fd[1], &ball, sizeof(int));
		kill(target_pid, SIGUSR1);
	} else {
		write(fd[1], &ball, sizeof(int));
		kill(target_pid, SIGUSR1);
		if (target_pid == getppid()) { // son
			exit(0);
		} else {
			wait(NULL);
			exit(0);
		}
	}
}

int main() {
	pipe(fd);
	signal(SIGUSR1, handler);
	if ((target_pid = fork()) == -1) return 1;
	if (target_pid != 0) { //father
		write(fd[1], &ball, sizeof(int));
		kill(target_pid, SIGUSR1);
		while (1) pause();
	} else {
		targetpid = getppid();
	}	
	while (1) pause(); // both father and son takes here
}
```

It's recommended to use write() instead of printf() because common buffer may make some confusion.

# Ping-Pong 2
```c
#define CNT_MAX 10;
int sig_flag = 0;

void handler(int signum) {
	sigflag = 1;
}

int main() {
	int pid;
	int ball = 0;
	int fd[2];
	pipe(fd);
	signal(SIGUSR1, handler);
	
	if ((pid = fork()) == -1) return 1;
	if (pid != 0) { // father
		write(fd[1], &ball, sizeof(int));
		kill(pid, SUGUSR1);
	}
	
	while (1) { // father and son
		while (sig_flag != 1) {
			read(fd[0], &ball, sizeof(int));
			ball++;
			write(fd[1], &ball, sizeof(int));
			kill(pid, SIGUSR1);
			sigflag = 0;
			if (ball > CNT_MAX) {
				break;
			}
			close(fd[0]);
			close(fd[1]);
			if (pid != getppid()) {
				wait(NULL);
			}
			exit(0);
		}
	}
}

```

> Not compiled. May be some bugs.