# object

<!-- vim-markdown-toc GFM -->

    * [feature](#feature)
        - [value](#value)
        - [pointer](#pointer)
        - [reference](#reference)
    * [common](#common)
        - [function](#function)
            + [value](#value-1)
                * [parameter](#parameter)
                    - [call by value](#call-by-value)
                    - [call by value of pointer](#call-by-value-of-pointer)
                    - [call by value of reference](#call-by-value-of-reference)
                * [return](#return)
                    - [return by value](#return-by-value)
                    - [return by value of pointer](#return-by-value-of-pointer)
                    - [return by value of reference](#return-by-value-of-reference)
            + [pointer](#pointer-1)
        - [class](#class)
            + [member function](#member-function)
    * [const](#const)
        - [variable](#variable)
            + [value](#value-2)
            + [pointer](#pointer-2)
            + [reference](#reference-1)
        - [function](#function-1)
            + [parameter](#parameter-1)
                * [call by value](#call-by-value-1)
                * [call by value of pointer](#call-by-value-of-pointer-1)
                * [call by value of reference](#call-by-value-of-reference-1)
            + [return](#return-1)
                * [return by value](#return-by-value-1)
                * [return by value of pointer](#return-by-value-of-pointer-1)
                * [return by value of reference](#return-by-value-of-reference-1)
        - [class](#class-1)
            + [member function](#member-function-1)
+ [other](#other)
    * [pointer vs. array](#pointer-vs-array)
    * [define vs. const](#define-vs-const)

<!-- vim-markdown-toc -->

---

-   在執行階段有明確資料的儲存區域
-   在生命週期中有對應的記憶體位址

## feature

### value

-   (todo)

### pointer

-   在生命週期內指向物件開頭的位址（只能加減，不能乘除)
-   可指向未完整宣告的物件（如 void 指標）
-   void 指標無法提取指向的變數存值，必須強制型轉

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

## common

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
void func(int* ptr) {
    printf("%d\n", *ptr); // 參數提取外部變數存值，更新時改變外部變數存值
    printf("%p\n", (void *)ptr); // 參數存值為外部變數位址
    printf("%p\n", (void *)&ptr); // 參數位址
}
```

###### call by value of reference

```cpp
void func(int& ref) {
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
int* func() {
    static int var = 0; // int var = 0（X）

    return &var;
}
```

###### return by value of reference

-   函數回傳值必須保留變數

```cpp
int& func() {
    static int var = 0; // int var = 0（X）

    return var;
}
```

#### pointer

### class

#### member function

-   (todo)

## const

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

##### call by value of pointer

-   指標不能改變指向的變數存值

```cpp
void func(const int * ptr) { // 或 void func(int const * ptr);
    *ptr = 0; //（X）
}
```

##### call by value of reference

-   參考不能改變參考的變數存值

```cpp
void func(const int& ref) {
    ref = 0; //（X）
}
```

#### return

##### return by value

-   (null)

##### return by value of pointer

-   指標不能改變指向的函數回傳值

```cpp
const int* func() { // 或 int const* func() {
    static int var = 0;

    return &var;
}

const int* ptr = func();
*ptr = 1; //（X）
```

##### return by value of reference

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

# other

## pointer vs. array

-   不能互換

-   array 在宣告中有 extern，不能換成 pointer
-   array 在宣告中有明確空間，不能換成 pointer
-   array 在宣告中沒有明確空間，可以互換

-   在表達式中，可以互換

```cpp
arr[1]; // *(arr + 1)
```

## define vs. const

-   define 在預處理階段已被展開，編譯執行階段不再存在
-   const 在編譯階段被分配記憶體空間，機制同一般變數

```cpp
#define VAR 0
const var = 0;
```
