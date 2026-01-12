Linux has two standards:
- IPC SysV (from Unix System V)
- IPC POSIX

# Methods
`key = ftok("/etc/passwd", 'a')`
We can use same key for semaphores, shared memory and etc.

```c
int shmget(key, size_t size, int flg);
```
Creates shred memory.

- `size` - it rounds to page size\*n
- `flg` - `IPC_CREAT|IPC_EXCL|0644`



```c
void *shmat(int shmid, const void *shmaddr, int shmflg);

int shmdt(int shmid);
```
Attach memory/detach memory. It connects program to memory. Access to array that belongs to several processes.

- `shmid`
- `shmaddr`
	- `NULL` - system will decide, where to attach.
- `shmflg`
	- `SHM_RDONLY`
	- `SHM_RND`

```c
shmctl(int shmid, int cmd, struct shmid_ds *buf);
```
- `cmd`:
	- `IPC_STAT` - read sheared memory parameters.
	- `IPC_SET` - set shared memory parameters.
	- `IPC_RMID` - delete for process. Deletes completely if all processes deleted it.

# File Permissions

|             | Owner | Owner's group | All |
| ----------- | ----- | ------------- | --- |
| Permissions | rwx   | rwx           | rwx |
| Binary      | 110   | 110           | 100 |
| Oct         | 6     | 6             | 4   |


# Example
Program 1.
```c
key_t key = ftok("/etc/passwd", 'Z');

int shmid = shmget(key, 4096, IPC_CREAT|0664);

char *buf = (char*) shmat(shmid, NULL, 0);
sprintf(buf, "Miumiu"); // not atomic btw
shmdt(shmid);
return 0;

/* instead of sprintf
char local_buf[100] = {"Miumiu"};
int len = strlen(local_buf);
for (int i = 0; i < len; i++) {
	buf[i] = local_buf[i];
}
buf[len] = '\0';
*/
```

Program 2.
```c
key_t key = ftok("/etc/passwd", 'Z');

int shmid = shmget(key, 4096, 0);

char *buf = (char*) shmat(shmid, NULL, 0);

printf("%s\n", buf);

shmdt(shmid);
shmctl(shmid, IPC_RMID, NULL);
return 0;

/*
int len = strlen(buf);
write(1, buf, sizeof(char) * len);
*/
```


# Example With Semaphores
Process 1 writes -> 2 reads -> 3 reads -> {2 times} -> end.
Semaphores:
- 1st - write, +2, wait for 0, write, +1
- 2d - -2, read, +3, -4, read, +5, return
- 3d - -3, read, -5, read, return
> It's not clear. Let's make 3 binary.

semop.h
```c
strct sembuf
	f_up = {0, 1, 0},
	f_down = {0, -1, 0},
	s_up = {1, 1, 0},
	s_down = {1, -1, 0},
	t_up = {2, 1, 0},
	t_down = {2, -1, 0};
```

Process 1.
```c
#include <semop.h>

key_t key = ftok("/etc/passwd", 'a');
int shmid = shmget(key, 4096, IPC_CREAT|0664);
int semid = semget(key, 3, IPC_CREAT|0664);

char *buf = (char*) shmat(shmid, NULL, 0);
buf[0] = '2';
sprintf(buf + 1, "Miumiu for 2d process");

semop(semid, &s_up, 1);
semop(semid, &f_down, 1);

buf[0] = '3';
sprintf(buf + 1, "Miumiu for 3d process");

semop(semid, &s_up, 1);
semof(semid, &f_down, 1);

shmdt(shmid);
semctl(semid, 0, IPC_RMID, 0);
shmctl(shmid, IPC_RMID, NULL);

return 0;
```

Process 2.
```c
#include <semop.h>

key_t key = ftok("/etc/passwd", 'a');
int shmid = shmget(key, 4096, IPC_CREAT|0664);
int semid = semget(key, 3, IPC_CREAT|0664);
key_t key = ftok("/etc/passwd", 'a');
int shmid = shmget(key, 4096, IPC_CREAT|0664);
int semid = semget(key, 3, IPC_CREAT|0664);

char *buf = (char*) shmat(shmid, NULL, 0);

￼for (int i = 0; i < 2; i++) {
	semop(semid, &s_down);
	￼if (buf[0] == '2') {
		printf("%s\n", buf + 1);
	}
	semop(semid, &t_up);
}

return 0;
char *buf = shmat(shmid, NULL, 0);

for (int i = 0; i < 2; i++) {
	semop(semid, &s_down);
	if (buf[0] == '2') {
		printf("%s\n", buf + 1);
	}
	semop(semid, &t_up);
}

shmdt(shmid);
return 0;
```

Process 3.
```c
#include <semop.h>

key_t key = ftok("/etc/passwd", 'a');
int shmid = shmget(key, 4096, IPC_CREAT|0664);
int semid = semget(key, 3, IPC_CREAT|0664);

char *buf = shmat(shmid, NULL, 0);

for (int i = 0; i < 2; i++) {
	semop(semid, &t_down);
	if (buf[0] == '3') {
		printf("%s\n", buf + 1);
	}
	semop(semid, &f_up);
}

shmdt(shmid);
return 0;
```