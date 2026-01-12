#programming 
## Dynamic memory
`int a[3];`
`int b[] = {4, 6, 7};`
> the trouble is not determined needed volume of memory

the resolution is 
- `int a[x]` - dynamic arrays (not recommended)
- `malloc`/`free`
### malloc
`malloc(buffer size)`
- takes buffer size in bytes
- type is `void*` - non-typed pointer
- returns pointer or `NULL` if operation was unsuccess
#### example
```c
int x = 5;
int* buf = (int*) malloc(x*sizeof(int));
buf[0] = 4;
buf[2] = 5;
...
free(buf);
free(buf); // error. double free
```

- Отсутствие free не фатально, но может вызвать утечку памяти *memory leak.*
- Также можно *потерять указатель* на выделенную память, что вызовет проблему с free.
```c
int func()
{
	int b[4]; // belongs to stack
	int a = 4; // belongs to stack
	int* buf = (int*) malloc(...); // belongs to heap
}
```
- `heap` - куча
```c
buf = malloc(LEN*sizeof(int));
for (i=0; i<LEN; i++)
	buf[i] = 0;
```
- Нет гарантий, что память будет выделена
#### Calloc
> решает проблему отсутствия гарантии невыделенности памяти.


```c
void* calloc(size_t nmemb, size_t size);
```
- `nmemb` - count of elements
- `size` - size of element
- It may be used for struct or just variables
##### usage
`ptr = (int*) calloc(10, sizeof(int))`

#### realloc
> перевыделение памяти

- `n bytes -> n+1 bytes` - очень ресурсоёмкое занятие
- `NULL` return is err
##### usage
`void* realloc(void* ptr, size_t size);`
- `sizeof()` returns `size_t`

### Valgrind
> tool for tracking dynamic memory

- executes program and collect information about dynamic memory
- `gcc -g my-prog.c`
- `valgrind a.out`

#### Address operations
```
int* a;
a = (int*) malloc(5*sizeof(int));
a++;
free(a); // it's loss. but with a-- it's ok
```

### Examples
#### copy argv
```c
int find_len(char*);

int main(int argc, char** argv)
{
	char** argv_2 = (char**) malloc((argc+1) * sizeof(char*));
	int len_i;
	for (int i=0; i<argc; i++)
	{
		len_i = find_len(argv[i])
		argv_2[i] = (char*) malloc(len_i*sizeof(char));
		for (j=0; j<len_i; j++)
		{
			argv_2[i][j] = argv[i][j];
		} // for
	} // for
}
```
- copy argv to another char**
- `argc` is count of parameters
	- `./a.out -m 10` -> `argc = 3`
- `argv` - pointer to array of pointers

| argv stores | pointers | to     |        |
| ----------- | -------- | ------ | ------ |
| `"./a.out"` | `"-m"`   | `"10"` | `NULL` |
```c
{
	for (i=0; i<argc; i++)
	{
		free(argv_2[i]);
	} // for
	free (argv_2);
} // main
```
- now we clear argv_2 memory
```c
int find_len(char* string)
{
	int i = 0;
	while (string[i] != '\0')
	{
		i++;
	}
	return i + 1;
}
```

#### Matrix
> Fill third-diag matrix in single array
$$\begin{matrix}
2 & 1 & 0 & 0 & 0 \\
1 & 2 & 1 & 0 & 0 \\
0 & 1 & 2 & 1 & 0 \\
0 & 0 & 1 & 2 & 1 \\
0 & 0 & 0 & 1 & 2
\end{matrix}$$

```c
int* get_matr(int diag_el, int n)
{
	int* matr = (int*) calloc(n*n, sizeof(int));
	int i;
	for (i=0; i<n; i++)
	{
		matr[i*(n+1)] = dial_el;
	} // for
}
```

- продумать, как правильно заполнить побочные диагонали.