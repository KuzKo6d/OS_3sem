#programming 
## Two examples
```c
char a[] = "Hello world!";
a[3] = 'z';
```
> It's okay.
```c
char *a = "Hello world!";
a[3] = 'z';
```
> It will drop. Tried to change string constant.

# System Calls Input/Output
> `fscanf`, `fread` and etc. uses integrated buffer

> `read` and `write` has no buffer

## Standard IO Streams
> Use redirection

- Utils:
	- `cat` - file content
	- `head` - file header
	- `wc` - count of lines, words, letters
- Generating IO streams:
	- `ls -l > "dir.txt"`
	- `>` - redirection parameter of stdout
	- `<` - redirection parameter of stdin
	- `2>` - redirection parameter of stderr
```bash
my_prog.out > "out.txt" 2> "err.txt" < "in.txt"
```

> How do we do it in C program?
> 
- `int dup(int fd);`
	- Returns first free descriptor. Its content equal to `fd` content
	- Example:
```c
fd = open();
close(0);
dup(fd);

read(0, ...);
close(fd);
```

- `int dup2(int old_fd, int new_fd);`
	- Returns first free descriptor. 
	- `old_fd` line in [[C System Calls Input Output#File Descriptor Table|FDT]] := `new_fd` line. `old_fd` close, if necessary
	- Example:
```c
stdout_save = dup(1);
dup2(1, fd);
printf("i'm in fd!");

dup2(1, stdout_save);
```

## File Descriptor Table
- `fd = open();`

| 0   | stdin  |
| --- | ------ |
| 1   | stdout |
| 2   | stderr |
| 3   |        |
| ... |        |

## Open
- `int open(const char *path, int flags, mode_t mode)`
	- `flags` - access modes and etc.
	- Access modes:
		- `O_RDONLY` - `r`
		- `O_WRONLY` - `w`
		- `O_APPEND` - `a`
		- `O_RDWR` - `r+`
		- `O_CREAT` - create if not exists
		- `O_TRUNC` - truncate file content
		- `O_EXCLUDE` - returns `-1` if file already exists (only with O_CREAT)
	- `mode` - access permissions for file
	- Returns file descriptor number or `-1` in error case.
	- Example:
```c
fd = open("my_file.dat", O_TRUNC|O_CREAT, 0664);
```

> `fopen -> FILE*`. `open -> int (file descriptor)`.

## Read / Write
> About regular files, but also we have:
> - sockets
> - channels
> - devices files
- `int read(int fd, const void *buf, size_t size)`
	- Returns count of read symbols or `-1` in error case.
	- Example:
```c
read(0, buf, 256);
```

- `int write(int fd, void *buf, size_t size)`
	- Returns count of written symbols or `-1` in error case.
## Seek
- `off_t lseek(int fd, off_f offset, int whence)`
	- `whence` is start position. It has amount of constants:
		- `SEEK_SET`
		- `SEEK_CUR`
		- `SEEK_END`
	- Examples:
```c
lseek(fd, 0, SEEK_END);
```
> returns length of file
```c
lseek(fd, 0, SEEK_CUR);
```
## Close

## Terminal
```c
int main() {
	if() return 0;
	else return 1;
}
```

- Execution code:
	- `gcc my_prog.c -o my_prog`
	- `./my_prog
	- `echo Hello
	- `echo $?` -> execution code
- Multiple commands:
	- `&&` - execute second if first was success
	- `||` - execute all
	- `mkdir NewFolder && touch ./NewFolder/tst.txt`
	- `vim ./NewFolder/tst.txt`

