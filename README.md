# Interview

<!-- vim-markdown-toc GFM -->

* [variable](#variable)
    - [value](#value)
    - [pointer](#pointer)
    - [reference](#reference)
* [function](#function)
    - [parameter](#parameter)
        + [call（pass）by value](#callpassby-value)
        + [call（pass）by pointer（address）](#callpassby-pointeraddress)
        + [call（pass）by reference](#callpassby-reference)
    - [return](#return)
        + [return by value](#return-by-value)
        + [return by pointer](#return-by-pointer)
        + [return by reference](#return-by-reference)
* [class](#class)
* [const](#const)
    - [variable](#variable-1)
        + [value](#value-1)
        + [pointer](#pointer-1)
        + [reference](#reference-1)
    - [function](#function-1)
        + [parameter](#parameter-1)
            * [call by value](#call-by-value)
            * [call by pointer（address）](#call-by-pointeraddress)
            * [call by reference](#call-by-reference)
        + [return](#return-1)
            * [return by value](#return-by-value-1)
            * [return by pointer](#return-by-pointer-1)
            * [return by reference](#return-by-reference-1)
    - [class](#class-1)
        + [member function](#member-function)

<!-- vim-markdown-toc -->

---

## variable

### value

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

#### call（pass）by pointer（address）

-   取得外部變數的位址當參數傳入函數

```cpp
void func(int* ptr) {
    printf("%d\n", *ptr); // 參數提領外部變數存值，更新時改變外部變數存值
    printf("%p\n", (void *)ptr); // 參數存值為外部變數位址
    printf("%p\n", (void *)&ptr); // 參數位址
}
```

#### call（pass）by reference

```cpp
void func(int& ref) {
    printf("%d\n", ref); // 參數提領外部變數存值，更新時改變外部變數存值
    printf("%p\n", (void *)&ref); // 參數位址提領外部變數位址，參數本身只是別名，沒有位址
}
```

### return

#### return by value

-   函數回傳複製的函數回傳值

#### return by pointer

-   函數回傳值必須保留變數

```cpp
int* func() {
    static int var = 0; // int var = 0（X）

    return &var;
}
```

#### return by reference

-   函數回傳值必須保留變數

```cpp
int& func() {
    static int var = 0; // int var = 0（X）

    return var;
}
```

## class

-   (todo)

## const

### variable

#### value

-   define 在預處理階段已被展開，編譯執行階段不再存在
-   const 在編譯階段被分配記憶體空間，機制同一般變數

```cpp
#define VAR 0
const var = 0;
```

-   變數存值不能被改變

```cpp
const int var; // 或 int const var;
var = 1; //（X）
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

##### call by value

-   (null)

##### call by pointer（address）

-   指標不能改變指向的變數存值

```cpp
void func(const int * ptr) { // 或 void func(int const * ptr);
    *ptr = 0; //（X）
}
```

##### call by reference

-   參考不能改變參考的變數存值

```cpp
void func(const int& ref) {
    ref = 0; //（X）
}
```

#### return

##### return by value

-   (null)

##### return by pointer

-   指標不能改變指向的函數回傳值

```cpp
const int* func() { // 或 int const* func() {
    static int var = 0;

    return &var;
}

const int* ptr = func();
*ptr = 1; //（X）
```

##### return by reference

-   (null)

### class

#### member function

-   成員函數不能改變變數物件，也不能呼叫非 const 成員函數

```cpp
class Cls {
    public:
        int var = 0;

        void func1() const{
            var = 1; //（X）
            func2(); //（X）
        }

        void func2() {
        }
};
```
