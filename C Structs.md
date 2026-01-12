#programming 
## Enumerated type (enum)
- `enum colors {RED, BLUE, GREEN};`

| RED  | GREEN | BLUE |
| ---- | ----- | ---- |
| `=0` | `=1`  | `=2` |
- `enum miumiu {A=3, B, C, D, E=16};`

| A    | B    | C    | D    | E     |
| ---- | ---- | ---- | ---- | ----- |
| `=3` | `=4` | `=5` | `=6` | `=16` |
- `enum colors c;`
- `enum colors c = 100;` - compiler will work, but it's strange
- `enum miumiu {A=3, B, C, D, E=16} a, b, c;` - to declare a, b, c of type `enum miumiu`

- `enum miumiu {A, B = A + 2, C = B + 3};` - we can use values on flight
- `enum bin {FALSE, TRUE};` - it's okay. but how you fell or codestyle
## Structs
```c
struct person {
	unsigned int age;
	char [10]name;
} p1, p2;
```
> `person` is only tag

`typedef struct person person;` - to ...

`sizeof(p1)` - ? It may be not just $\sum$ of var sizes. There is padding. (выравнивание)

> We may use `struct` args in functions and return it from them

```c
sruct struct_char {
	char field;
};
```


| `field` | \*  | \*  | \*  | ... |
| ------- | --- | --- | --- | --- |
- `sizeof(struct_char)` is 4;

## Order of bytes
- *big-endian* - from bigger to smaller
- *little-endian* - from smaller to bigger

- `0x1234` is `12|34|*|*` in *big-endian*

## Self use inside
```c
struct pint {double x, y} p;
p.x = 3;
p.y = 2.5;
struct point* p_ptr;
p_ptr = &p;
p_ptr -> x = 4.2; // &p.x
p_ptr -> y = 7.2; // &p.y
```

- `(*p_ptr).x = 4.2;` $\equiv$ `p_ptr -> x = 4.2;`
- `struct point pts[30];`

---
> Use struct itself inside
```c
struct point {
	double x, y;
	struct point *other_point;
}
```
> Use before defining
```c
struct str2;
struct str1 {struct str2* ptr_s2; ...};
struct str2 {struct str1* ptr_s1; ...};
```

##
```c
struct point {double x, y;};
struct point p1, p2;
p1 = p2; // by bit

p1 == p2; // it's error.
```

## List
```c
struct Node {
	int val;
	struct Node* next;
};
```

| element | val     | next |
| ------- | ------- | ---- |
| 0       | nothing | 1    |
| 1       | 3       | 2    |
| 2       | 123     | NULL |
> List with head is using zero element only for pointer to next

## Functions of Lists
- `add` - adding element to end
	- `int add(list, new_val)`
	- returns code of error
	- `list` is link to head
> Implementation
```c
struct Node head;
head.next = NULL;


int add (struct Node* head, int new_val) {
	if (!head) {
		return 1;
	}
	
	struct Node newnode_ptr = (struct Node*) malloc(sizeof(struct Node));
	
	if (!newnode_ptr) {
		return 2;
	} 
	
	newnode_ptr -> val = new_val;
	newnode_ptr -> next = NULL; // through . not correct because our struct is pointer
	
	while (head.next) {
		head = head.next;
	}
	
	head.next -> next = new node_ptr;
	return 0;
}
```

- `find` - take indexes of elements with fixed val
	- returns pointer to buffer
> Usage
```c
int i = 0;
int err;
int* fint_buf = find(&head, 5, &err);
if (err) {
	printf("Find function error.\n");
}

if (fint_buf) {
	printf("No such value.\n");
	return 0;
}


while (fint_buf[i] >= 0) {
	printf("Found in: %d index\n", fint_buf[i]);
	i++
}
```

> Implementation

```c
#define CHUNK 5

int* find(struct Node* head, int fint_val, int* err) {
	int code_num = 1;
	int fint_buf = Null;
	
	if (!head) {
		*err = 1;
		return Null;
	}

	while (head -> next) {
		head = head -> next;
		if (!head) {
			break;
		}
		if (fint -> val >= 0)
		node_num ++;
	}
}
```