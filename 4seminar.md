> *r* means "needs to rework"
### Enumerated type
```
enum color {white = 10, green = 14, red, blue, black = 7, yellow}
```

| index | name   | value |
| ----- | ------ | ----- |
| 0     | white  | 10    |
| 1     | green  | 14    |
| 2     | red    | 15    |
| 3     | blue   | 16    |
| 4     | black  | 7     |
| 5     | yellow | 8     |
`enum color my_color;` - declare variable of type `enum color`
`my_color = red` - assign `red`
> Compiler inserts values instead names 

### Switch *r*
```
switch (<expression>)
{
case 1: <operators>; break;
case 2: <operators>; break;
...
default: <operators>; break;
}
```
> we can merge cases

```
switch (<>)
{
case 4:
case 5:
case 6: <operators>; break;
default: <operators>; break;
}
```
> break exits switch(). may not write it.

> using *enum*
```
enum month {jan = 1, feb, ..., dec}
enum month ml = feb

switch (ml) {
case feb: <operators>; break;
case apr:
case dec: <operators>; break;
default: <operators>; break;
}
```
### Constants 
`enum {N = 5};` - enumerated constant (have field of view)
`#define N 1024` - preprocessor constant
> Use define only with CAPITAL letters

`const int count = 7;` - read only variable (technically it's variable). It's compile time constant
### Functions
```
int func (int m, int n, double x) {return 0}
```
> procedure is also function

```
void funcV (int* m, int* n) {}
```
`&m` is address of `m` or it's pointer
`*m` - take value from address
`int*` is int pointer type