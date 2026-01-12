> Никольский Илья Михайлович
> Жуков Константин Андреевич

> man 3 scanf
```c
int num1, num2;
long long mult;
mult = (long long)num1 * num2; // convert to long long
```

### Conditional operators
```
if (<logical expresiion>) {}
else if () {}
else {}
```
- `==` - equal
- `>=` - equal larger
- `&&` - and
- `||` - or
- `!` - not
### While
```
while (<condition>) {}

do{
}while(<logical expression>);
```
- `break` - досрочный выход (может занять время перезагрузкой конвеера процессора)
- `continue` - досрочный переход на новую итерацию
### For
```
for (<expression1>; <expression2>; <expression3>) {}
```
- `expression1` - execute ones before operation
- `expression2` - condition checks before iteration
- `expression3` - executes after iteration
- В выражениях можно через `,` указывать несколько условий
### Operations
`int x = 3` - инициализация
`x = 3` - присваивание
`++x` - возвращает увеличенное
`x++` - возвращает исходное
- возвращает значение, в отличии от оператора
### Boolean

| value   | boolean |
| ------- | ------- |
| `0`     | `false` |
| `not 0` | `true`  |
