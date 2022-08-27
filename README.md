# Interview

<!-- vim-markdown-toc GFM -->

* [common](#common)
    - [variable](#variable)
    - [pointer](#pointer)
    - [reference](#reference)
* [function](#function)
    - [parameter](#parameter)
        + [call（pass）by value](#callpassby-value)
        + [call（pass）by value of pointer（call（pass）by address）](#callpassby-value-of-pointercallpassby-address)
        + [call（pass）by value of reference（call（pass）by reference）](#callpassby-value-of-referencecallpassby-reference)
    - [return](#return)
    - [callee](#callee)
* [const](#const)
    - [common](#common-1)
        + [variable](#variable-1)
        + [pointer](#pointer-1)
        + [reference](#reference-1)
    - [function](#function-1)
        + [parameter](#parameter-1)
            * [variable](#variable-2)
            * [pointer](#pointer-2)
            * [reference](#reference-2)
        + [return](#return-1)
            * [variable](#variable-3)
            * [pointer](#pointer-3)
            * [reference](#reference-3)
        + [callee](#callee-1)

<!-- vim-markdown-toc -->

---

## common

### variable

-   (todo)

### pointer

-   (todo)

### reference

-   參考變數在宣告時，就要初始化所參考的變數

```cpp
int& ref; //（X）
```

-   一旦參考變數參考特定變數後，就不能再參考別的變數

```cpp
int &ref = var1;
&ref = var2; //（X）
```

## function

### parameter

#### call（pass）by value

-   複製外部變數的值當參數傳入函數

```cpp
void func(int var) {
    printf("%d\n", var); // 參數存值為外部變數存值的複製，更新時與外部變數存值無關
    printf("%p\n", (void *)&var); // 參數位址
}
```

#### call（pass）by value of pointer（call（pass）by address）

-   取得外部變數的位址當參數傳入函數

```cpp
void func(int* ptr) {
    printf("%d\n", *ptr); // 參數提領外部變數存值，更新時改變外部變數存值
    printf("%p\n", (void *)ptr); // 參數存值為外部變數位址
    printf("%p\n", (void *)&ptr); // 參數位址
}
```

#### call（pass）by value of reference（call（pass）by reference）

```cpp
void func(int& ref) {
    printf("%d\n", ref); // 參數提領外部變數存值，更新時改變外部變數存值
    printf("%p\n", (void *)&ref); // 參數位址提領外部變數位址，參數本身只是別名，沒有位址
}
```

### return

-   (todo)

### callee

-   (todo)

## const

### common

#### variable

-   變數存值不能被改變

```cpp
const int var; // 或 int const var;
```

#### pointer

-   指標不能改變本身存值，即不能改變指向的變數位址

```cpp
int* const ptr = &var1;
ptr = &var2; //（X）
```

-   指標不能改變指向的變數存值

```cpp
const int* ptr = &var; // 或 int const* ptr = &var;
*ptr = 0; //（X）
```

-   指標不能改變本身存值，也指標不能改變指向的變數存值

```cpp
const int* const ptr = &var1;
ptr = &var2; //（X）
*ptr = 0; //（X）
```

#### reference

-   參考不能改變參考的變數存值

```
const int& ref = var;
ref = 0; //（X）
```

### function

#### parameter

##### variable

-   (null)

##### pointer

-   指標不能改變指向的變數存值

```cpp
void func(const int * ptr) { // 或 void func(int const * ptr);
    *ptr = 0; //（X）
}
```

##### reference

-   參考不能改變參考的變數存值

```cpp
void func(const int& ref) {
    ref = 0; //（X）
}
```

#### return

##### variable

-   (null)

##### pointer

-   指標不能改變指向的函數回傳值

```cpp
const int* func() { // 或 int const* func() {
    static int var = 0;

    return &var;
}

const int* ptr = func();
*ptr = 1;（X）
```

##### reference

-   (todo)

#### callee
