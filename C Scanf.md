
| Flag | Action |
| ---- | ------ |
| ``   |        |
# Flags
## Types
- `%d` - int
- `%i` - int (auto num base detection)
- `%u` - unsigned int
- `%f` - float
- `%lf` - double
- `%c` - char
- `%s` - string (char*)
- `%p` - pointer(void*)
- `%x` - hex
- `%o` - oct
## Size Specificators
- int:
	- `%hhd` - signed char
	- `%hd` - short int
	- `%d` - int
	- `%ld` - long int
	- `%lld` - long long int
---
- unsigned int:
	- `%hhu` - unsigned char
	- `%hu` - unsigned short
	- `%u` - unsigned int
	- `%lu` - unsigned long
	- `%llu` - unsigned long long
---
- float:
	- `%f` - float
	- `%lf` - double
	- `%Lf` - long double
---
- hex:
	- `%hhx` - unsigned char (hex)
	- `%hx` - unsigned short (hex)
	- `%x` - unsigned int (hex)
	- `%lx` - 
	- `%llx` - unsigned long long (hex)
## Other
- `%*c` - skip char in input
- `%10s` - 10 or less symbols read