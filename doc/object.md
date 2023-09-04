# object

<!-- vim-markdown-toc GFM -->

+ [common](#common)
    * [value](#value)
        - [function](#function)
            + [declaration](#declaration)
                * [value](#value-1)
                    - [parameter](#parameter)
                    - [return](#return)
                * [pointer](#pointer)
                    - [parameter](#parameter-1)
                    - [return](#return-1)
    * [pointer](#pointer-1)
        - [function](#function-1)
            + [declaration](#declaration-1)
        - [array](#array)
            + [pointer](#pointer-2)
                * [function](#function-2)
                    - [declaration](#declaration-2)
    * [reference](#reference)
+ [array](#array-1)
    * [pointer](#pointer-3)
        - [function](#function-3)
+ [const](#const)
+ [volatile](#volatile)

<!-- vim-markdown-toc -->

---

# common

## value

### function

#### declaration

##### value

###### parameter

###### return

##### pointer

###### parameter

###### return

## pointer

### function

#### declaration

-   函數指標 fptr 指向函數 func

```cpp
int func(int var);

int (*fptr)(int ) = func;
```

-   型態定義後的函數指標 fptr 指向函數 func

```cpp
typedef int (*fptype)(int );

int func(int var);

fptype fptr = func;
```

### array

#### pointer

##### function

###### declaration

```cpp
int (*arr[SIZE])(int ); // 陣列arr中每個元素為一個指向函數的指標
```

## reference

# array

## pointer

### function

# const

# volatile
