# object

<!-- vim-markdown-toc GFM -->

* [feature](#feature)
    - [pointer](#pointer)
        + [array](#array)
        + [string](#string)
    - [reference](#reference)
* [common](#common)
    - [constant](#constant)
    - [function](#function)
        + [value](#value)
            * [parameter](#parameter)
                - [call by value](#call-by-value)
                - [call by value of pointer](#call-by-value-of-pointer)
                - [call by value of reference](#call-by-value-of-reference)
            * [return](#return)
                - [return by value](#return-by-value)
                - [return by value of pointer](#return-by-value-of-pointer)
                - [return by value of reference](#return-by-value-of-reference)
* [const](#const)
    - [variable](#variable)
        + [value](#value-1)
        + [pointer](#pointer-1)
        + [reference](#reference-1)
    - [function](#function-1)
        + [parameter](#parameter-1)
            * [call by value](#call-by-value-1)
            * [call by value of pointer](#call-by-value-of-pointer-1)
            * [call by value of reference](#call-by-value-of-reference-1)
        + [return](#return-1)
            * [return by value of pointer](#return-by-value-of-pointer-1)
    - [class](#class)
        + [member function](#member-function)
* [other](#other)
    - [cast 與 alignment](#cast-與-alignment)

<!-- vim-markdown-toc -->

---

-   在執行階段有明確資料的儲存區域
-   在生命週期中有對應的記憶體位址

## feature

### pointer

-   在生命週期內指向物件開頭的位址（只能加減，不能乘除)
-   可指向未完整宣告的物件
-   void 指標無法提取指向的變數存值，必須強制型轉
-   void 指標與 char 指標有相同的 alignment

#### array

-   array 沒完整宣告時（沒明確空間），可以換成 pointer
-   array 有完整宣告時（有明確空間），不能換成 pointer

-   array 在宣告中，若有 extern，不能換成 pointer

-   兩者在表達式中，可以互換

```cpp
arr[1]; // 等同於 1[arr] 以及 *(arr + 1)

&arr == arr
&arr + 1 != arr + 1
arr + 1 // 等同於 &*(arr+1) 以及 &arr[0] + 1
```

#### string

```cpp
char *str = "" // 指向 static 位址
*(str+1) = 'x'; //（X）

char str[] = "" // 指向 stack 位址
```

### reference

-   參考變數在宣告時，就要初始化所參考的變數

```cpp
int &ref; //（X）
```

-   一旦參考變數參考特定變數後，就不能再參考別的變數

```cpp
int &ref = var1;
&ref = var2; //（X）
```

## common

### constant

-   1.0 是 double 型態
-   1.0f 是 float 型態

### function

#### value

##### parameter

###### call by value

-   複製外部變數的值當參數傳入函數

```cpp
void func(int var) {
    printf("%d\n", var); // 參數存值為外部變數存值的複製，更新時與外部變數存值無關
    printf("%p\n", (void *)&var); // 參數位址
}
```

###### call by value of pointer

-   取得外部變數的位址當參數傳入函數

```cpp
void func(int *ptr) { // 或 void func(int ptr[]) {
    printf("%d\n", *ptr); // 參數提取外部變數存值，更新時改變外部變數存值
    printf("%p\n", (void *)ptr); // 參數存值為外部變數位址
    printf("%p\n", (void *)&ptr); // 參數位址
}
```

###### call by value of reference

```cpp
void func(int &ref) {
    printf("%d\n", ref); // 參數提取外部變數存值，更新時改變外部變數存值
    printf("%p\n", (void *)&ref); // 參數位址提取外部變數位址，參數本身只是別名，沒有位址
}
```

##### return

###### return by value

-   函數回傳複製的函數回傳值

###### return by value of pointer

-   函數回傳值必須保留變數

```cpp
int * func() {
    static int var = 0; // int var = 0（X）

    return &var;
}
```

###### return by value of reference

-   函數回傳值必須保留變數

```cpp
int & func() {
    static int var = 0; // int var = 0（X）

    return var;
}
```

## const

-   const 在編譯階段被分配記憶體空間，機制同一般變數
-   define 在預處理階段已被展開，編譯執行階段不再存在

```cpp
#define VAR 0
const var = 0;
```

### variable

#### value

-   變數存值不能被改變

```cpp
const int var; // 或 int const var;
var = 1; //（X）
```

#### pointer

-   指標不能改變本身存值，即不能改變指向的變數位址

```cpp
int *const ptr = &var1;
ptr = &var2; //（X）
```

-   指標不能改變指向的變數存值

```cpp
const int *ptr = &var; // 或 int const* ptr = &var;
*ptr = 0; //（X）
```

-   指標不能改變本身存值，也指標不能改變指向的變數存值

```cpp
const int *const ptr = &var1;
ptr = &var2; //（X）
*ptr = 0; //（X）
```

#### reference

-   參考不能改變參考的變數存值

```
const int &ref = var;
ref = 0; //（X）
```

### function

#### parameter

##### call by value

-   複製外部變數的值當常數變數的參數傳入函數

```cpp
void func(const int var) { // 或 void func(int const var) {
    var = 1; //（X）
}
```

##### call by value of pointer

-   指標不能改變本身存值，即不能改變指向的變數位址

```cpp
void func(int *const ptr) {
    // ptr != &ptr 即 arr != &arr，與 common array 不同
}
```

-   指標不能改變指向的變數存值

```cpp
void func(const int *ptr) { // 或 void func(int const * ptr);
    *ptr = 0; //（X）
}
```

##### call by value of reference

-   參考不能改變參考的變數存值

```cpp
void func(const int &ref) {
    ref = 0; //（X）
}
```

#### return

##### return by value of pointer

-   指標不能改變指向的函數回傳值

```cpp
const int * func() { // 或 int const * func() {
    static int var = 0;

    return &var;
}

const int *ptr = func();
*ptr = 1; //（X）
```

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

## other

### cast 與 alignment
