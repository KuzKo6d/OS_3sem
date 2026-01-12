#programming 
## Strings methods
- `printf/scanf` - standard i/o stream (stdin, stdout)
- `sprintf/sscanf` - same but with strings
- `sprintf(char* str, const char* format, ...)`
```c
char buf[256];
int i = 135;
sprintf(buf, "%d", i);
```
> write down `"135"` to `buf`

- `sscanf(const char* str, const char* format, ...)`
- `my_prog -n 256` 
	- -> `argv`:
		- pointer to `"my_prog\0"`
		- pointer to `"-n\0"`
		- pointer to `"256"`
		- `NULL`
- `sscanf(argv[2], "%d", &i);`
- `malloc(i * sizeof(int))`

## Reading string
> \#include \<stdio.h\>
- `char* gets(char* buf);`
	- read string to buf untill `'\n'` or `EOF`
	- *never use it*
- `scanf("%s", buf);`
- `scanf("%10s", buf);`
	- stops with `'\n'`, `EOF` or 11th symbol
- `char* fgets(char* s, int size, FILE* stream);`
	- `s` - link to first symbol
	- `size` - length of string
	- `stream` may be connected with file, console, 

> \#include \<string.h\>
> or
> \#include \<cstring\>

- `str...` - watching for string ending markers
- `mem...` - not watching
### Str...
- `char* strcpy(char* dst, const char* src);`
	- `buf = malloc(...)`
	- `strcpy(buf, "Hello world!")`
- `char* strncpy(char* dst, const char* src, size_t n);`
	- determines length of string
- `char* strcat(char* dst, const char* src);`
	- not controls memory volume
	- handles `'\n'` and other
- `char* strncat(char* dst, const char* src, size_t n;)`
- `size_t strlen(const char* s);`
	- *length* of string without `'\0'` counting
- `char* strchr(const char* s, char c);`
	- returns *first in* index
- `char* strrch(const char* s, char c);`
	- returns *last in* index
- `char* strstr(const char* needle, const char* haystack);`
	- returns pointer of first char of `needle` in `haystack`
	- *substring search*
	- searching for `needle` in `haystack`
	- `NULL` if not found
- `char* strtok(char* s, const char* delim);`
	- *tokenize string by* `delim` separator
	- `delim` -> `'\0'`
	- `delim` is `char` or couple of independent `char`s
	- example:
```c
  // print the heap array of char
  word = strtok(str, ",");
  while (word != NULL) {
    printf("word: %s\n", word);
    word = strtok(NULL, ",");
  }
```
- `char* strerror(int err_code);`
	- returns error code
	- example:
		- `int error;`
		- `int ret_val = func(&error);`
		- `if (!error) {`
		- `working with ret_val`
		- `} else {`
		- `diagnostics}`
- `long int strtol(const char* ptr, char** end, int base)`
	- `ptr` - pointer to first symbol of string
	- `end` - pointer to pointer to end (123*n*a)
	- `base` - basis of number system
### Mem...
- `void* memset(void* s, int c, size_t n);`
	- set first `n` symbols of `s` to little byte of `c`
	- *it's fast* unlike `for`
### Static variables
> Global variable declares only in first execution and accessable only from that func
```c
int func() {
	static int x = 10;
	
	return 0;
}
```
#### example
```c
int f() {
static int x = 0;
x++;
printf("called %d times\n");
return 0;
}
```
### Global errno variable
```c
#include <errno.h>

fopen();
printf("%d", errno);
```
> Not all std functions sets `errno` to `0`

```c
printf("%s", streerror(errno));

f = fopen()
if (!f) {
printf("fopen errno\n");
return 1;
}
```

### Transformations strings to ...
- `int atoi(const char* s)`
	- ASCII to int
	- `atoi` - int
	- `atol` - long
	- `atod` - double
	- `int a = atoi("123");`
	- `int a = atoi("b34");` - returns 0 or smth else. it's error

## Standard streams
- `stdout`
- `stdin`
- `stderr` - standard error stream

## Examples
### 1.
```

```