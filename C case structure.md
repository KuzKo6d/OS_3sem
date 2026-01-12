### Syntax
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
### With enum
> [[C enumerated type|Enumerated]] type is useful in case
```c
enum month {jan = 1, feb, ..., dec}
enum month ml = feb

switch (ml) {
case feb: <operators>; break;
case apr:
case dec: <operators>; break;
default: <operators>; break;
}
```