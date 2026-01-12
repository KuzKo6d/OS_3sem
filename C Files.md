#programming 
# File input/output
> Sometimes devices/directories and other are files

> **Text stream** - sequence of symbols, separated by `\n` (like string literals)
> **Binary stream** - sequence of bytes (like numbers)

> `#include <stdio.h>`

## Open File
- FILE - Structure, stores current state of file work
	- Position in file
	- Work mode (r/w and etc.)
- `fopen()`
	- `FILE *fopen(const char *fname, const char *mode)`
	- returns pointer to `FILE` structure
	- `mode`'s:
		- `r` - read only
		- `w` - write only (it creates if not exists, or overwrites)
		- `a` - append
		- `rb` `wb` `ab` - same with binary
		- `r+` - read and write
		- `w+` - empty file and write/read
		- `a+` -set pointer to end and append/read
	- may set the `errno`:
		- if `fp = fopen("my-file.txt", "w")`
		- file not found
		- rejected by rights (OS)
```c
#include <errno.h>

errno = 0;
fp = fopen("file.txt", "w");
perror("fopen");
// or strerror from <str.h>
printf("%s", strerror(errno));
```
## Read\Write
- `fprintf()`
	- `int fprintf(FILE *fp, const char *format_string, ...)`
	- returns count of success wrote symbols
- `fscanf()`
	- `int fscanf(FILE *fp, const char *format_string, ...)`
### Single Char In
- `int fgetc(FILE *fp)`
	- computes its arg single time
	- may be slower (function call uses resources)
	- it uses *local variables*
- `int getc(FILE *fp)`
	- may be implemented like *macros*
	- may be cases with incorrect work (arg computes >1 time)
		- dangerous with side effect like `x=y++`
		- example:
```c
FILE *streams[100];
	int i = 3;
	getc(streams[i++]);
```
> Assume, gets() implementation is:
```c
getc(streams[i++])
streams[i++] -> buf = ...
streams[i++] -> sz = ...
// we have multiple increasing `i` in one iteration.
```
> It can use arg multiple time
- `int getchar(void)`
	- $\equiv$ `fgetc(stdin)`
- `int ungetc(int c, FILE *fp)`
> Write in `stdin` - `UB` (undefined behavior)
- `fgets(char buf[], int sz, FILE *fp)`
	- reads until `\n` or until `sz` symbols
### Single Char Out
- `int fputc(int c, FILE *fp)`
- `int putc(int c, FILE *fp)`
- `int putchar(int c)`
- `int fputs(const char *s, FILE *fp)`
- `int puts(const char *s)`
### Write buffer
> `stdlib` I/O is buffed (with disable possibility)
> -> write (fputs) -> buffer in memory -> file on disk

> We need buffer because it's way more fast, than writing on disk

- `int fflush(FILE *fp)`
	- resetting the buffer to disk (slow operation)
	- returns error code
> *Use `flush` or `fseek` behind read and write.*
### Check EOF
- Stream stores:
	- position
	- end indicator
	- error indicator
- `int feof(FILE *fp)`
	- `EOF` -> `1`
	- else -> `0`
- `int ferror(FILE *fp)`
- `fclear(?)` - reset indicators
## Change Pointer Position
- `int fseek(FILE *fp, long offset, int whence)`
	- `offset` - 
	- `whence` - start offset point
- `SEEK_CUR` - current position constant (it changes with position)
- `SEEK_SET` - file start constant
- `SEEK_END` - file end constant
- `long ftell(FILE *fp)`
	- returns current position
- `long fgetpos(FILE *fp)`
- `long fsetpos(FILE *fp)`
## Close File
> Simultaneously we can open limited number of files.
> By default we have opened `stdin`, `stdout`, `stderr`.
- `fclose(FILE *fp);`


## Example
> Replace substring $S_{1}$ to $S_{2}$.
 
- Simple way:
	- With additional file `src.txt` -> `dst.txt`
- Harder way (in-place):
	- It's possible if $S_{1}$ and $S_{2}$ lengths are same


### "a" -> "b"
> do it at home
```c
FILE *f = fopen("txt.txt", "r+");

while (!feof(f)) {
	c = getchar
}
```