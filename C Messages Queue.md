Messages in [[3.2 IPC - система межроцессного взаимодействия#IPC Очередь сообщений|lectures]].

# Methods
```c
int msgrcv(int msqid, const void msgp[.msgsz], size_t msgsz, long msgtyp, int msgflg)
```

- `msqid` - 
- `msgflg` - 



# Example: Ping-Pong
Header msg_format.h
```c
#define MAX_BALL

struct msgbuf {
	long mtype;
	int ball;
}
```


Process 1
```c
#include "msg_format.h"

int main() {
	key_t key = ftok("/etc/passwd", '%');
	int msgid = msgget(key, IPC_CREAT|0664);
	int ball = 0;
	struct msgbuf msg = {45, ball};

	while (ball < MAX_BALL)	{
		int err = msgsnd(msgid, &msg, sizeof(strct msgbuf) - sizeof(long), 0);
		if (err < 0) return -1;
		
		err = msgrcv(msgid, &msg, sizeof(struct msgbuf) - sizeof(long), 53, 0);
		if (err < 0) return -1;
		printf("Recived ball: %d\n", ball++);
		msg.ball = ball;
	}
	err = msgsnd(msgid, &msg, sizeof(struct msgbuf) - sizeof(long), 0);
	
	return 0;
}
```

Process 2
```c
#include "msg_format.h"

int main() {
	key_t key = ftok("/etc/passwd", '%');
	int msgid = msgget(key, IPC_CREAT|0664);
	int ball = 0;
	struct msgbuf msg = {53, ball};
	
	while (ball < MAX_BALL)	{
		err = msgrcv(msgid, &msg, sizeof(struct msgbuf) - sizeof(long), 53, 0);
		if (err < 0) return -1;
		printf("Recived ball: %d\n", ball++);
		msg.ball = ball;
	
		int err = msgsnd(msgid, &msg, sizeof(strct msgbuf) - sizeof(long), 0);
		if (err < 0) return -1;
	}
	
	return 0;
}
```


# Example: Broadcast
For that we need to use server.
For counting subscribers we can use semaphore. Or shared memory protected by semaphore.

We publish in one queue and post from server by using second queue.

We will broadcast messages n times, where n is count of subscribers. Each subscriber will take message and sleep some time. Sleep is needed for avoiding several messages taken situation.

```c

```