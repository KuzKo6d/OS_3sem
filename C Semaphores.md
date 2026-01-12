Part of [[3.2 IPC - система межроцессного взаимодействия#IPC Массив семафоров|IPC]] (Inter Process Communication)

```bash
ipcmk -S 2 # make two semaphores
ipcs # list 
ipcrm # remove by key
```

> IPC tools live untill you remove them. End of program don't remove it automatically unlike file descriptors.

# System Calls
## Ftok()
```c
key_t ftok("/usr/passwd", 'k')
```
we can make same key several times by using this call with same arguments.

## Semget()
```c
int semget(key_t key, int nsems, int flags)
```
- `nsems` - semaphore vector dimensions.
- `flags` - permissions
	- `IPC_CREAT`
	- `IPC_EXCL` - exclude
	- `0664`
	- `0777`
	- `IPC_CREAT|0664`

## Semctl()
```c
int semctl(int semid, int semnum, int cmd, union semnun arg)
```
> ctl - control. It's a swiss knife.


### Union
Union is similar to structures.
```c
union my_un {
	int i,
	char c
} a;

a.i = 4;
printf("%d", a.i); // returns 4 bytes (all ctruct content)
printf("%d", a.c); // returns first byte
```
It will allocate size of biggest var.

How to use?
```c
union semun {
	int arg, // SETVAL
	shruct semid_ds *buf, // IPC_STAT, IPC_SET
	unsigned short *array // GETALL, SETALL 
};

union semun myarg;
myarg.arg = 3;
```
- `arg` -
- `buf` - permissions and etc. (fields check in man)
- `array` - work with entire vector.

### Usage
```c
semctl(semid, 0, IPC_RMID, 0); // remove semaphore group
```
## Semop()
```c
int semop(int semid, shtuct buf *ops, int nops)
```
- `ops` - structure with multiple operations. Executes atomic. 

```c
struct buf {
	unsigned int semnum,
	short semop,
	short semflg
};
```
- `semop`:
	- `semop = 0` - block untill 0
	- `semop < 0` - block untill $|semop|\geq 0$
	- `semop > 0` - semop++
- `semflg`:
	- `IPC_NOWAIT` - not blocking.
	- `SEM_UNDO` - rollback semaphores if executed process down.

# Ping-Pong With Semaphores
Let's make two semaphores: 
semaphore 0 - 
semaphore 1 - son's semaphore

| Father                            | Son                       |
| --------------------------------- | ------------------------- |
| writes ball                       | tries to semaphore 1 down |
| semaphore 1 up                    | read ball                 |
| while (1) {tries semaphore 0 down | ...                       |
| read                              | write ball                |
| ...                               | semaphore 0 up            |
| write                             |                           |
| semaphore 1 up}                   |                           |

```c
#define MAX_BALL 10

key_t key = ftok("/etc/passwd", 'a');
int semid = semget(key, 2, 0644|IPC_CREAT);
int ball = 0;
int fd[2];
int pid;

struct sembuf son_up = {1, 1, 0};
struct sembuf son_down = {1, -1, 0};
struct sembuf father_up = {0, 1, 0};
struct sembuf father_down = {0, -1, 0};

if (pipe(fd) == -1) return 1;

if ((pid = fork()) == -1) return 1;
if (pid > 0) { // father
	write(fd[1],&ball, sizeof(int));
	semop(semid, &son_up, 1);
	while (ball < MAX_BALL) {
		semop(semid, &father_down, 1);
		read(fd[0], &ball, sizeof(int));
		ball++;
		semop(semid, &son_up, 1);
	}
	
} else { // son
	while (ball < MAX_BALL) {
		semop(semid, &son_down, 1);
		read(fd[0], &ball, sizeof(int));
		ball++;
		write(fd[1], &ball, sizeof(int));
		...
	}
}
```


# 
```c
char (*(*p())[])();
```
`p()` - function
`*p()` - returns pointer
`[]` - array
array of pointers
pointer to array of pointers
pointer to function, returns pointer to array of pointers to functions which take nothing, return char.

# Bash trick

```bash
./server &
```
> Now we can use terminal while server works