#programming
> Obfuscated C
### Basic moments
```c
#include <stdio.h>
int x; // global variables here

int main() 
{
	int a; // local func variable
	a = 4; // assign value
	int b = 5; // or just init
	printf("Hello world!\n"); // print f - format
	printf("I like %d %d\n", x, y) // %d for int
	return 0;
}
```
#### comments
`/* */` - multistring
`//` - single string
#### 
`"aboba"` - string, ended by '\0'
`'C'` - char
#### scape - последовательности
- **\n** - enter
- **\t** - tab
### Basic types
> long <=> long int
- char (may be signed) - 1 byte
- int (*signed*/unsigned) - 4 bytes
- short - 2 bytes
- long - 4/8 bytes
- long long (after ) - 8 bytes
---
- float - 4 bytes
- double (sec precision) 8 bytes
- long double - 10 bytes
---
- bool (added in C23)
---
- ~~string~~
### Constants
`const int x = 4;` - declaring unvariable variable
`#define x 5` - declaring using preprocessor (pastes like macros and doesn't use memory)
### Operators
#### Sizeof
- `sizeof(<type>)` or `sizeof(<object>)` - возвращает количество выведенных символов
#### Scanf
- `Scanf("%i %ld", &m, &n)` - возвращает количество считанных переменных (-1 при ctrl+d как конец ввода/файла)
- *завершается*  при расхождении ожидаемого и фактического ввода
#### Printf
#### Getchar
`int getchar()` - returns only `int` because it have `-1` for End of FIle
`int ch; ch = getchar()`
#### Putchar
`int putchar(<char>)` - writes char

### Strings
string - array of char ended by '\0'

| 0   | 1   | 2   | 3   | 4   | 5   |
| --- | --- | --- | --- | --- | --- |
| H   | e   | l   | l   | o   | \0  |
### Translational specifiers
- `%d` - int
- `%f`
- `%lf` - float
	- `%.2lf` - 2 signs after dot
	- `%6.2lf` - 6 signs for all, 2 after dot
- `%dd` - double


| specifiers   | act             |
| ------------ | --------------- |
| `%d`         | int             |
|              |                 |
|              |                 |
| `%g` or `%e` | sintific format |

### Arrays
`int arr[LEN];` 
`int arr[5] = {1, 2, 3, 4, 5};` - how does it work with 6 or 2 values?
`arr[0] = 5`
`arr[4]` - take last int in case

### Loops
#### while
```
while (<condition>)
{

}
```
#### do while
#### for
```
for (<expression1>; <expression2>; <expression3>)
{

}
```
- expression1 - execute ones before operation
- expression2 - condition checks before iteration
- expression3 - executes after iteration
#### increments
- `x++` - postfix
- `++x` - prefix
```c
int a, x = 4;
a = x++ // a := x and x := x + 1
a = ++x // a := x + 1 and x := x + 1
```

### Example
```c
#define LEN 5
int i;
int arr[LEN];

// fill the array
for (i = 0; i<LEN; i++)
{
	arr[i] = i
	printf("")
}
```
### Functions
```c
void smartPrint(int i); // header/declaration

int main ()
{
	smartPrint(3);
	return 0;
}

void smartPrint(int i) // defenition
{
	printf("\*\*\*%d\*\*\*\n", i);
}

```
### Include
```c
// myProg.h
int fun1()
{

}
int fun2()
{

}
```
```c
// myProg.c
#include <stdio.h> // system folders
#include "myProg.h" // relative path

int main()
{
	fun1();
	return 0;
}
```