# Resizable array

> C 语言的数组的大小是固定的。但有时候，我们需要一个可以变换大小的数组。那么怎样用 C 语言去实现它呢？
>
> 试想一组提供可调整大小的 `int` 数组机制的函数。

- Growable  (可成长的)
- Get the current(当前的) size
- Access to the elements(单元)

## The Inteface

<p align="center">接口定义</p>

> 可以实现一个函数库，该函数库可以提供一个自动增长的数组。

```c
typedef struct {
    int *array;
    int size;
} Array;
```

Why `struct` not `struct *`?

You have no control over whether it is a local variable or not. And it can be misunderstood.

### `Array array_create(int init_size);`

用于创建数组

```c
Array array_create(int init_size) {
    Array a;
    a.array = (int*)malloc(sizeof(int)*init_size);
    a.size = init_size;
    return a;
}
```

 Why `Array` not `Array *`?

If `a == NULL` ? Or `a` already existes.

### `void array_free(Array *a);`

用于回收数组的空间

```c
void array_free(Array *a) {
    free(a->array);
    a->size = 0;
}
```

---

### `int array_size(const Array *a);`

这个数组里有多少个单元可以用

```c
int array_size(const Array *a) {
    return a->size;
}
```

Why not take the member directly? Like `a.size`.

In the way it's done here, it's called encapsulation(封装).

### `int* array_at(Array *a, int index);`

访问、获得或数组中某个单元

```c
int* array_at(Array *a, int index) {
    if (index >= a->size) {
        array_inflate(a, index-a->size);
    }
    return &(a->array[index]);
}
```

Why `int*` not int?

- Use `array_at()`

  ```c
  Array a = array_create(10);
  *(array_at(&a, 5)) = 6;
  *(array_at(&a, 10)) = *(array_at(&a, 5));
  ```

- Will it be better

  to have two access functions:

    `Array a = array_create(10);`

  - `array_get(&a, 5, 6);`
  - `array_set(&a, 10, array_get(&a, 5));`

- Memory in block

  ```c
  int* array_at(Array *a, int index) {
      if (index >= a->size) {
          array_inflate(a, (index/BLOCK_SIZE + 1)*BLOCK_SIZE-a -> size);
      }
  }
  return &(a->array[index]);
  ```

---

### `void array_inflate(Array *A, int more_size);`

```c
void array_inflate(Array *a, int more_size) {
    int* p = (int*)malloc(sizeof(int)*(a->size+more_size));
    for (int i=0; i<a->size; i++) p[i] = a->array[i];
    free(a->array);
    a->array = p;
    a->size = s->size+more_size;
}
```



```c
void array_inflate(Array *a, int more_size) {
    int* p = (int*)malloc(sizeof(int)*(a->size+more_size));
    memocpy((void*)p, (void*)a->array, a->size*sizeof(int));
    free(a->array);
    a->array = p;
    a->size = s->size+more_size;
}
```

- Why not take the whole array

  ```c
  int* array_get(Array* a) {
      return a->array;
  }
  ```

  Lack of protection for both user and developer

- Access functions

  the use of access functions seems not so elegant

  - Use operator overload in C++
  - Design specific functions for specified application, do not treat it as an array



## issues

Allocate new memory each time it inflates is an easy and clean way. But:

- It takes time to copy
- May fail in memory restricted situation

**Not efficient enough**

So why can't there be a type that doesn't need to be copied and be linked to new applications. (可能有语法错误)



:exclamation: Didn't understand much, need to review.