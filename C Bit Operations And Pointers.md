#programming 
## Указатели
`const int a = 4;`
`const int *ptr &a;` - указатель на константу `\*ptr`

`int a = 4;`
`int * const ptr = &a` - константный указатель 

`array name` ~ `&a[0]`
`&a` - указатель на массив длины 10
 
`int (*b)[10] = &a;` - 
`int *ptr;`
`ptr++;`

`typedef int x;`
`x a=10;`

### перемещение по массиву с шагом
`int (*b)[10]` - b - указатель на массив из 10 `int`
`typedef int (*b)[10];` - описание типа
`b ptr;`
`int aa[20];`
`ptr = (b)aa;`
`ptr` -> start
`ptr + 1` -> middle

| 0     | 1   | ... | 10      | ... | 19  | 20  |
| ----- | --- | --- | ------- | --- | --- | --- |
| `ptr` |     |     | `ptr+1` |     |     |     |

### 
`char *S[10]` - массив указателей `char` длины 10
`char** S` ~ `char *S[]`

`int main(int argc, char** argv) {}`
`gcc prog.c -o prog` - аргументы в `gcc` попадают так

### input
`prog.out -a/-b/-c`

> argc - count of args and argv is args
```
int main(int argc, char** argv)
{
	char mode = argv[1][0];
	switch(mode)
	{
		case 'a': ...;
		break;
		case 'b': ...;
		break;
		...
		default: printf("Unknown mode\n");
		break;
	}
}
```

### Чётные числа
```
int a = {10, 13, 24, 5, 7};
int len = sizeof(a) / sizeof(int);
int i;
for(i = 0; i < len; i++)
{
	if (!(a[i]%2))
		continue;
	...
}
```

### Указатель на функцию
> например вычисление значения функции при её интегрировании

`int *f(int a);` -  func retuns pointer
`int (*f)(int a);` - pointer to func takes int and retuns int
```
integrator (double a; double b; double (*f)(double x));
double parabola(double x)
	{return x*x + 2*x - 3}
integrator (0.0, 1.0, parabola);
(0, 1, ...)
```

## Битовые операции
> Нужны для доступа к отдельным битам

### Атрибуты файла
#### Права доступа
`r` - право на чтение
`w` - право на запись
`x` - право на запуск
`-` - null

| name | x2  | x8      | x10 | x16    |
| ---- | --- | ------- | --- | ------ |
| rwx  | 111 | `\0764` |     | `0xAB` |
| r    | 100 |         | 4   |        |
|      | 010 |         | 2   |        |
|      | 001 |         | 1   |        |

#### Группы пользователей
- Владелец файла
- Группа владельца
- Остальные 
### Операции

| name | bool | bit |
| ---- | ---- | --- |
| and  | &&   | &   |
| or   | \|\| | \|  |
| xor  | !=   | ^   |
| not  | !    | ~   |
`a = a<<2`, `a <<= 2` - rol
`a = a>>2`, `a >>= 2` - ror

### Применение
- `&` - сброс флажка (clear)
- `|` - установка флажка (set)
- `^` - инвертация флажка
```
int clearFlag(undigned char*pack; int n)
{
	if (n < 0 || n > 7)
		return -1;
	if (!pack)
		return 2;
	unsigned char mask = 1 << n;
	mask ~= mask; // not and assign
	*pack &= mask;
	return 0;
}
```

> меняем n-й бит `unsigned char`

```
// exchange values
x = x^y;
y = y^x; // -> y = x^y^y=x^0=x;
x = x^y;
```

> ещё пример (поменять местами половины байта)

```
int swap_hlv(unsigned char* byte)
{
	if (byte == NULL)
		return 1;
	
	unsigned char mask = 1<<4;
	mask = mask - 1;
	
	*byte = (*byte & mask) << 4 + (*byte & ~mask) >> 4;
	return 0;
}

// 0101 1111 -> 1111 0101
```
> посчитать количество 1 в байте

```
int count_ones(unsigned char*byte; int* error)
{
	if (!byte)
	{
		*error = 1;
		return 0;
	}
	unsigned char temp = *byte;
	
	int count = 0;
	for (int i=0; i<sizeof(char); i++)
	{
		count += *temp&1;
		*temp>>1;
	}
	return count;
}
```
> метод Кернигана

```
int count_Kernigan()
{
	unsigned int v; // 1 inside
	undigned int c; // count of 1
	
	for (c=0; v; c++)
	{
		v &= v-1;
	}
}
```