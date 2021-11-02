# C Lang Supplement Knowledge (Review)

[C Lang](https://www.bilibili.com/video/BV1Q44y1x7Q4)



### printf and scanf

- `“%d”, x`: integer; `%f`: floating point number; `%lf`: double precision number; `%.2f`: fp with 2 decimal digits; `%g`: scientific; `%%`: `%` in output; `%s`: string.
- `\t`: tab; `\n`: enter
- In `scanf`: the input should add `&` before the var. name

### Macro

- **Header fies**: `#include<>`

  - `stdio.h`: Standard input / output
  - `math.h`: Spec. math requires (`sqrt`)
  - `stdlib.h`: Dynamic allocation requires (`malloc()`)
  - `string.h`: String operations (`strcopy`, …)

- Use `#define MANY 5` and apply `i < MANY` and `printf("%d", MANY)`, etc. to pre-define and easy to modify 

  Better to use all UPPER-CASE

### Data Types

#### **int**

`sizeof(char) = 1` (byte = 8 bit (0 / 1))

`char <= short <= int (~4) <= long`

- `int`: -2^-31^, … , -1, 0, 1, … , 2^31^ - 1

- `unsigned(int)`: 0, 1, …, 2^32^ - 1 (int but no negative values)

  If need to use limit values, `<limits.h>`: `INT_MIN`, `INT_MAX`, `UINT_MAX`

#### **ASCII** 

(and Extended ASCII): 48(0) 49(1) … 65(A) … 97(a) … 127(DEL)

#### **float**

`float (~4; >=6 decimal number) <= double (~8; >=10) <= long double (>=10)`

Sign + Exp + matissa

#### Conversion / Promotion 

`char -> short -> int -> long -> float -> double`

### Expressions

- **Summing**: `sum += num` equiv to `sum = sum + num`

  `i = i+1` equiv to `i++` / `i += 1` (`++i`: Plus then use; `i++`: Use then plus)

- Unary: `++i`, `-x`
- Binary: `a*b`, `a == b`
- Ternary: `a<0 ? -a:b` (if a<0 => -a; no => b)

### Operations

- `%`: mod
- `0 < b < 8`: means `(return(0<b)) < 8` (for b = -1, `0<b` False => 0; `0<8` True => 1)

#### Priority

1. `()`
2. `++` / `--` / `+` / `-` (signs) / (types)
3. `*` / `/` / `%`
4. `+` / `-`
5. `<` / `<=` / `>` / `>=`
6. `==` / `!=`
7. `&&`
8. `||`
9. `?=`
10. `=` / `+=` / …

### if & Switch

- `if` - `else if` - `else`

  `if(0)`: won’t execute

- **Switch**

  ``` c
  switch (exp)	// must return an integer
  {
      case value_1: ....
          break;
      case value_2: ....
          break;
  
      ....
              
      default: ....	// optional            
  }
  ```

#### **Example**: Calculator

``` c
#include <stdio.h>

int main() 
{
    char op;
    int num1, num2, res;
    scanf("%d %c %d", &num1, &op, &num2);
    switch(op) {
        case '+': res = num1 + num2; break;
        case '-': res = num1 - num2; break;
        case '*': res = num1 * num2; break;
        case '/': res = num1 / num2; break;
        default: printf("Error: unknown operation.\n"); return 1;
    }
    printf(....);
    return 0;
}
```

### Bool

Use `#include<stdbool.h>` to apply bool type var. (logic)

Usually don’t mix with other types of data.

``` c
#include <stdbool.h>

int main(){
    int n;
	scanf("%d", &n);
    bool notZero = n;	// n not zero => true; n is zero => false (int->bool)
    if(notZero) {
        printf("n is non-zero!\n");
    }
}
```

`!5`: 0;	`!!5`: 1;	`5 && 0`: 0;	`5 && 'A'`: 1;	`5 || -5`: 1;	`5 || 0`: 1

### Loops

- **while**:

  ``` c
  while(exp)
      statement;
  ```

- **do while**: (at least 1 loop will be done)

  ``` c
  do
      statement;
  while(exp)
  ```

- **for**:

  ``` c
  for (exp1; exp2; exp3)	// usually only with `i`
      statement;
  ```

  Similar to while loop:

  ``` c
  exp1;	// `exp1` initializes the loop
  while (exp2) {	// `exp2` is the condition to check
      statement;
      exp3;	// `exp3` is the extra operation
  }
  ```

- **break**: jump out of the current {}

- **continue**: jump out of the current iteration (and jump to another new iteration) - Only in loops

#### **Example**: Factorial loop

``` c
#include<stdio.h>

int main() 
{
    int n;
    if (scanf("%d", &n) < 1)
        printf("Input error.\n")
        return 1;
    if (n > MAX4FACT)
        printf("Too big a number.\n")
        return 2;
    int i = 2;
    int factorial = 1; // at most `12!`
    
    /* while loop
    while (i <= n) {
        factorial *= 1;	 // or just `factorial *= i++`
        i++;	// or `++i`
    }  */
    
    // for loop
    for (; i <= n; i++) {
        factorial *= 1;
    }
    
    printf("%d! = %d", n, factorial);
    return 0;
}
```

#### **Example**: Greatest Common Divisor

``` c
#include <stdio.h>

int main() {
    int m, n;
    scan("%d%d", &m, &n);
    if(m == 0 && n == 0)	printf("Input error.\n");
    int m_orig = m, n_orig = n;
    if(m < 0)	m = -m;
    if(n < 0)	n = -n;
    while (n != 0) {
        int temp = m % n;
        m = n;
        n = temp;
    }
    printf("The gcd of %d and %d is %d", m_orig, n_orig, m);
    return 0;        
}
```

#### **Example**: Check prime

``` c
#include<stdio.h>
#include<math.h>

int main() 
{
    int n;
    scanf("%d", &n);
    int n_orig = n;
    if (n < 0)	n = -n;
    int is_prime = 1;
    if (n < 2 || n != 2 && n % 2 == 0)
        is_prime = 0;
    else {
        for (int i = 3; i < sqrt(n) + 0.5; i += 2) {
            if (n % i == 0) {
                is_prime = 0;
                break;                    
            }
        }
    }
    printf("%d is %sa prime\n", n_orig, is_prime ? "": "not ");
    // if is a prime => %s = ""; if not %s = "not " 
    return 0;
}
```

### Array

#### 1D Array

`int grades[800]`: Array with 800 elements: `grades[0]` ~ `grades[799]`

The length should be an integer, it can’t be a var.

##### **Initialization**

- ``` c
  for(i = 0; i < 800; i++) 
      scanf("%lf", &grades[i]);
  ```

- ``` c
  int grades[5] = {100, 97, 79, 0, 0};	// few elements
  ```

- ``` c
  int grades[5] = {100, 97, 79};	// defines the first 3 elem (last 2 as 0 defaultly)
  ```

- ``` c
  int grades[] = {100, 97, 79, 0, 0};		//equiv to the above 2
  ```

##### Example: Sieve

``` c
#include <stdio.h>
#include <math.h>

#define N 37	// output all prime numbers under 37

int main() {
    int sqrt_N = sqrt(N) + 0.5;
    bool prime[N+1] = {false, false};	// 0 and 1 are not prime
    for (int i = 2; i < N+1; ++i) {
         prime[i] = true;
    }
    for (int p = 2; p <= sqrt_N; ++p) {
        if (prime[p]) {
            for (int i = p; i*p <= N; ++i) {
                prime[i*p] = false;
            }
        }
    }
    printf("The prime numbers below %d are \n", N);
    for (int i = 2; i <= N; i++) {
        if (prime[i]) {
            printf("%d ", i);
        }
    }
}
```

#### 2D Array

##### Initialization

- `double matrix[2][3] = {{1, 2, 3}, {4, 5, 6}};`	(2 lines, 3 col)
- `double mat[2][3] = {{0}};`

##### Example: Blur a Picture

Pixel -> RGB Values

**Blur**: Average color with neighbors

``` c
#include <stdio.h>

#define N 10
#define M 10
#define FALSE 0
#define TRUE 1

int inBound(int i, int j);
int meanAroundPixel(int picture[N][M], int i, int j);
void copy2dArray(int to[N][M], int from[N][M]);			// in the declaration of high dim arrays
void bluePicture(int picture[N][M]);					// from the 2nd dim on, the size should be spec.

int main()
{
    ...
    int picture [N][M] = {...};
    blurPicture(picture);
    ...
}

int inBound(int i, int j) 
{	// in the allowed range
    return (	j >= 0 && j < N 
             && j >= 0 && j < M);
}

int meanAroundPixel(int picture[N][M], int ii, int jj)
{
    int di, dj, sum = 0, neighbours = 0;
    for (di = -1; di <= 1; ++di)
        for (dj = -1; dj <= 1; ++dj)
            if (inBound(ii+di, jj+dj)) {
                sum += picture[ii+di][jj+dj];
                neighbours++;
            }
    return (int)((double)sum / neighbours + 0.5);
}

void copy2dArray(int to[][M], int from[][M])
{
    int i, j;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            to[i][j] = from[i][j];
    return;
}

void blurPicture(int picture[][M])
{
    int ii, jj;
    int blurred[N][M];
    for (ii = 0; ii < N; ++ii)
        for (jj = 0; jj < M; ++jj)
            blurred[ii][jj] = meanAroundPixel(picture, ii, jj);
    copy2dArray(picture, blurred);
    return;
}
```

### Function

- `int` & `void`: return or not return a value

- if want to use a function, declare first: `void xxx();` before the main function

  `long power(int, int)` / `long power(int base, int exp)` 

``` c
return-type fcn-name(par-list)
{
    var. def.;
    statements;
    return sta;
}
```

Call by value (the var. in main and in function is not the same)

### Variable

- **Duration**: temp / fixed

- **Scope**: local / global 

  generally, in loops / functions will be local; define at first in the main func will be global

  In a smaller scope of `{}`, if a var (w/ the same name) has been re-defined, in this scope and in smaller scopes will be the re-defined value; if not re-defined, use the value in the larger scope.

#### Types

- **Global var.**: Global, defined until the program finishes

  Defines at the real beginning of a program (right after `#include<>` and other macros, before any function declaration)

  If re-defines in a function w/ a same name, it will be covered.

- **Static var.**: Global, defined until the program finishes

  `static data_type data` in some function, after this function, the static var. will still be valid.

  Useful when counting the function run account

#### Stack

Last in first out (LIFO) - Usually stores local var.

Attention that due to the existance of the stacks, the functions are called by value, the value cannot pass from the smaller scope to the larger scope.

### Pointer

#### Example: Swap

Use `*a` to point to the var. `a`'s position 

``` c
void swap(int* a, int* b) {	// actually swap the 2 addresses
    int temp = *a;			// temp now points to the position where stores a
    *a = *b;				// &a (address) -> a (var/value); &b -> b
    *b = temp;
    return;
}

int main()
{
    int a = 3, b = 5;
    swap(&a, &b);	// extract the address of a and b
    printf("a = %d, b = %d", a, b);
    return 0;
}
```

#### Definition

`type *`: pointer to the type

(`*p`: **p is a pointer** pointing to sth; `&a`: **a is a variable** and this is its address; `p = &a`:OK; `y = *p`: y is now the value of a; `*p = 7`: OK, a = 7)

```c
char c;
char *cp1, *cp2 = &c;
```

#### Return Multiple

Void cannot return value, but if use pointers, it can be used to “return” multiple values

#### Generic Pointer: void*

- Point to nothing: `int *p = NULL;`
- Temporally need a pointer but cannot define yet: `void *p2;`

``` c
int a = 5, b, *p1;
char c = 'x';
void *p2, *p3 = &c;		// generic, can be used as a pointer of any type of data 
p1 = &a; 
p2 = p1;
b = *p1 + *(int*)p2;	// to use an undefined pointer, still need to add the type

// sizeof(*p1) = sizeof(int)
// sizeof(*p2) not defined
```

#### Operations

- Constant movement: `+/- n`: Move `n*sizeof()` bytes
- `++`, `--`, `+=`, `-=`: OK
- `-`: Ok if same “entity” (e.g. array): continuous
- `+`, `*`, `/`: Cannot!

``` c
int *p, *q;
p = 300; q = 400;
// p + 1 == p++ == 304; p + 2 == 308 (sizeof(int) = 4); q - p == 25;
// (char*)p + 2 == 302; (char*)q - (char*)p == 100;
// q - (char*)p: invalid
```

#### Array with Pointer

Actually the name of the array is the pointer itself

For an array `double s[3];`

- `&s[0]` => `s`
- `&s[1]` => `s+1`
- `&s[i]` => `s+i`
- `s[i]` => `*(s+i)`

Arrays occupy memory at definition, pointers don’t (unless they are actually def. to point to sth).

So a pointer cannot be given a value (or given a value as an array elem)

### Dynamic Allocation

- Requires `stdlib.h` header file
- `malloc(): void *`

``` c
double *p;	// still not know what is the size of p
int n;
...
p = (double*)malloc(n * sizeof(double));	// p points to double, so `(double*)` (same type) & `sizeof(double)` here
// the memory here is [n x sizeof(double)]
if (p == NULL)
    return 1;
...
for (i = 0; i < n; i++)
    scanf("%lf", &p[i]);
...
free (p);	// Release memory
```

### String

chars + `\0` (ASCII: 0)

#### Definition

- `char s[] = {'H', 'e', 'l', 'l', 'o', '!', '\0'};`
- `char s[] = "Hello!";` (No need `\0`)

The string length: No need to count `\0`; if as an array: Need to count `\0`

Use in **checking**:

- `while (s[i])` & `while (*p)`: can check if a char is `\0` or not (if is, will be false => break the loop)

#### Operations

Requires `string.h`

- `strlen(s)`: Find the length of `s`
- `strcpy(d, s)`: Copy string `s` to `d`
- `strcat(d, s)`: Concatenate (add) `s` after `d`
- `strcmp(s1, s2)`: Compare if `s1` and `s2` equal => yes: 0; no: which is bigger (alphabetical)
  
  ``` c
  strcmp("lion", "zebra");		// < 0: "l" smaller than "z" (smaller)
  strcmp("tiger", "tiger");		// 0
  strcmp("koala", "Koala");		// > 0: "k" after "K" in ASCII
  strcmp("rat", "rat snake");		// < 0
  strcmp("jaguar?", "jaguar!");	// > 0: "?" 
  ```

#### Complex Application: Sorting

##### Max-Sort

``` c
// Find the index of the maximal elem
int index_of_max(int a[], int n)
{
    int i, i_max = 0;
    for (i = 1; i < n; ++i)
        if (a[i] > a[i_max])
            i_max = i;
    return i_max;
}

void max_sort(int a[], int n)
{
    int length;
    for (length = n; length > 1; --length){
        int i_max = index_of_max(a, length);
        swap(&a[length - 1], &a[i_max]);
    }
    return;
}
```

##### Bubble Sort

``` c
int bubble(int a[], int n)
{
    int i, swapped = 0;
    for (i = 1; i < n; ++i)
        if (a[i-1] > a[i]){
            swap(&a[i], &a[i-1]);
            swapped = 1;
        }
    return swapped;
}

void bubble_sort(int a[], int n)
{
    int not_sorted = 1;
    while ((n > 1) && not_sorted)
        not_sorted = bubble(a, n--);
}
```

##### Merge

$\mathcal{O}(na + nb)$ 

``` c
void merge(int a[], int na, int b[], int nb, int c[])
{
    int ia, ib, ic;
    for (ia = ib = ic = 0; (ia < na) && (ib < nb); ic++) {
        if (a[ia] < b[ib]) {
            c[ic] = a[ia];
            ia++;
        }
        else {
            c[ic] = b[ib];
            ib++;
        }
    }
    for (; ia < na; ia++, ic++) c[ic] = a[ia];
    for (; ib < nb; ib++, ic++) c[ic] = b[ib];
}
```

### Complexitiy

Worst case

- $\sum_i^{n} i \sim \mathcal{O}(n^2)$ 
- $\sum_i^{n} \frac{1}{i} \sim \mathcal{O}(\log n)$
- $\sum_i^{n} i^2 \sim \mathcal{O}(n^3)$
- $\sum_i^{n} i^k \sim \mathcal{O}(n^{k+1})$ 

### Recursion

“n” <- “n-1”

Call the function in itself. Easier to use when do some reversely printing work

- Time complexity: recur = iter
- Space complexity: recur > iter

#### Example: Factorial

``` c
int factorial (int n)
{
    if (n == 0) return 1;	// halting (stop) condition (0! == 1) Here return and have jumped out of the `if`
    return n * factorial(n-1); 	// recursive step
}
```

``` c
int factorial (int n)	// wrapper fcn
{
    if (n < 0 || n > MAX)	return 0;
    return rec_fac(n);
}
int rec_fac (int n)
{
    if (n <= 1)	return 1;
    return n * rec_fac(n-1);
}
```

#### Example: Binary Search

``` c
int binSearch(int a[], int n, int val)
{
    if (n == 1)	return a[0] == val? 0: NOT_FOUND;	// hauting cond
    int mid = n / 2;
    if (a[mid] < val) {
        int result = binSearch (a+mid+1, n-mid-1, val);
        return result == NOT_FOUND? NOT_FOUND: result + mid + 1;
    }
    return binSearch(a, mid+1, val);
}
```

 #### Example: Fibonacci

0 1 1 2 3 5 8 13 21 34 55

``` c
int Fibo(int n)
{
    static int f[N] = {0, 1};	// use static to avoid wrong additions
    if (n < 0 || n >= N)	return -1;
    return recFibo(n, f);
}

int recFibo (int n, int f[])
{
    if (n > 2)	return n;
    if (f[n] != 0)	return f[n];
    return f[n] = recFibo(n-1, f) + recFibo(n-2, f);
}
```

### Enumerated Types (enum)

Available in C99

Defines a new type and a set of named values

The names are more important than its given value since the values should only be integers.

``` c
// def
enum Gender {MALE, FEMALE};	// MALE -> 0; FEMALE -> 1
enum Month {JAN, FEB, MAR, /* ... */, DEC, MONTH_NUM};	// DEC -> 11; MONTH_NUM -> 12 (Useful trick to count)
enum Season {SUMMER = 1, FALL, WINTER = 8, SPRING};	// FALL -> 2; SPRING -> 9

// usage
enum Gender gender = MALE;
enum Season seasons[MONTH_NUM];	// number of elem in the array = 12
seasons[JAN] = WINTER;

enum Month next_month(enum Month current) {
    if (current == DEC)
        return JAN;
    return (enum Month) (current + 1);
}
```

#### Bool

Actually bool can be regarded as an enum type: `enum bool {FALSE, TRUE};`

