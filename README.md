<!---
{
  "id": "b3531ed1-472f-4448-b60b-c58b9671a0a2",
  "teaches": "Memory Layout, RAM Consumption, and Addressing of C Arrays",
  "depends_on": ["c32afdd3-48c3-4ff2-b5f7-a2179a2f8093"],
  "author": "Stephan Bökelmann",
  "first_used": "2025-07-03",
  "keywords": ["C", "arrays", "memory layout", "RAM consumption", "addressing"]
}
--->

# Memory Layout, RAM Consumption, and Addressing of C Arrays

> In this exercise you will learn how to understand the memory layout of arrays of various data types in C, calculate their RAM consumption, and determine the address of specific elements within these arrays. Furthermore we will explore how data type sizes and pointer arithmetic affect memory addressing.

## Introduction

Arrays in C are contiguous blocks of memory that store elements of the same data type. Understanding how arrays are laid out in RAM is essential for efficient memory use and correct pointer arithmetic. The size of each array in bytes is given by the formula:

```
Size_of_Array = Number_of_Elements × sizeof(type)
```

For example, if `sizeof(int) = 4` bytes and you declare `int arr[5];`, the total memory occupied by `arr` is `5 × 4 = 20` bytes. The address of each element in the array can be computed using:

```
&arr[i] = (char*)arr + i × sizeof(type)
```

This formula treats the base address `arr` as a `char*` (byte pointer) and advances it by `i` times the size of the element type, yielding the byte-offset of the `i`th element.

In C, pointer arithmetic is automatically scaled by the size of the data type. When you write `arr + i`, the compiler actually calculates:

```
arr + i = (char*)arr + i × sizeof(type)
```

and then casts back to the appropriate pointer type.

The typical sizes of fundamental types on a 32‑bit or 64‑bit platform using the GCC compiler are:

* `sizeof(char) = 1` byte
* `sizeof(int) = 4` bytes
* `sizeof(float) = 4` bytes

Below, we will see how these sizes influence both the total RAM consumed by arrays and the computation of addresses for individual elements.

### Further Readings and Other Sources

1. Brian W. Kernighan and Dennis M. Ritchie. *The C Programming Language*, 2nd Edition, Prentice Hall, 1988. ISBN: 978-0131101630
2. ISO/IEC 9899:2011 Standard (C11): [http://www.open-std.org/jtc1/sc22/wg14/](http://www.open-std.org/jtc1/sc22/wg14/)
3. cppreference.com, "Arrays in C": [https://en.cppreference.com/w/c/language/array](https://en.cppreference.com/w/c/language/array)
4. YouTube, "C Programming Tutorial - Arrays": [https://www.youtube.com/watch?v=ZV8qWvIWmGc](https://www.youtube.com/watch?v=ZV8qWvIWmGc)

## Tasks

### Task 1: `int` Array Memory Layout and Addressing

**Problem:** Given the declaration:

```c
int arr[5];
```

Assume the base address of `arr` is `0x1000` and `sizeof(int) = 4` bytes.

**Solution Steps:**

1. **Calculate total size**:

   $$
   \text{Size} = 5 \times sizeof(int) = 5 \times 4 = 20\ \text{bytes}
   $$
2. **Draw memory layout diagram**:

   ```mermaid
   graph TB
     subgraph arr [arr (base 0x1000)]
       direction LR
       A0["arr[0]\n0x1000–0x1003"]
       A1["arr[1]\n0x1004–0x1007"]
       A2["arr[2]\n0x1008–0x100B"]
       A3["arr[3]\n0x100C–0x100F"]
       A4["arr[4]\n0x1010–0x1013"]
     end
   ```
3. **Compute address of `arr[3]`**:

   * Byte offset: `3 × 4 = 12`
   * Address: `0x1000 + 12 = 0x100C`
   * Using pointer arithmetic: `arr + 3` yields `(int*)0x100C`.

---

### Task 2: `char` Array Memory Layout and Addressing

**Problem:** Given the declaration:

```c
char buf[10];
```

Assume the base address of `buf` is `0x2000` and `sizeof(char) = 1` byte.

**Solution Steps:**

1. **Calculate total size**:

   $$
   \text{Size} = 10 \times sizeof(char) = 10 \times 1 = 10\ \text{bytes}
   $$
2. **Draw memory layout diagram**:

```mermaid
graph TB
  subgraph buf [buf (base 0x2000)]
    direction LR
    C0["buf[0]\n0x2000"] --> C1["buf[1]\n0x2001"] --> C2["buf[2]\n0x2002"] --> C3["buf[3]\n0x2003"] --> C4["buf[4]\n0x2004"] --> C5["buf[5]\n0x2005"] --> C6["buf[6]\n0x2006"] --> C7["buf[7]\n0x2007"] --> C8["buf[8]\n0x2008"] --> C9["buf[9]\n0x2009"]
  end
```
3. **Compute address of `buf[7]`**:

   * Byte offset: `7 × 1 = 7`
   * Address: `0x2000 + 7 = 0x2007`
   * Pointer form: `buf + 7` → `(char*)0x2007`.

---

### Task 3: `float` Array Memory Layout and Addressing

**Problem:** Given the declaration:

```c
float data[4];
```

Assume the base address of `data` is `0x3000` and `sizeof(float) = 4` bytes.

**Solution Steps:**

1. **Calculate total size**:

   $$
   \text{Size} = 4 \times sizeof(float) = 4 \times 4 = 16\ \text{bytes}
   $$
2. **Draw memory layout diagram**:

```mermaid
graph TB
  subgraph data [data (base 0x3000)]
    direction LR
    F0["data[0]\n0x3000–0x3003"]
    F1["data[1]\n0x3004–0x3007"]
    F2["data[2]\n0x3008–0x300B"]
    F3["data[3]\n0x300C–0x300F"]
  end
```
3. **Compute address of `data[2]`**:

   * Byte offset: `2 × 4 = 8`
   * Address: `0x3000 + 8 = 0x3008`
   * Pointer arithmetic: `data + 2` → `(float*)0x3008`.

## Questions

1. **Memory Calculation:** What is the total memory (in bytes) occupied by an array `double arr[3]` on a platform where `sizeof(double) = 8`? Show your calculation.
2. **Address Computation:** Given a base address of `0x4000`, what is the address of `arr[4]` for an `int arr[10]`? Assume `sizeof(int) = 4`.
3. **Pointer vs. Byte Arithmetic:** Explain how pointer arithmetic differs from byte-wise arithmetic when adding an integer to a pointer in C.
4. **Char Array Access:** If `char c[5]` starts at `0x5000`, what is the memory address accessed by `*(c + 2)`?
5. **Multidimensional Layout:** Describe how the memory layout of a multidimensional array `int mat[2][3]` appears in RAM, and compute the address of `mat[1][2]` given the base address of `0x6000` and `sizeof(int) = 4`.

## Advice

Understanding the memory footprint and addressing of arrays in C is foundational for efficient programming and debugging. As you practice these tasks, visualize each array as a block of contiguous bytes and rely on the `sizeof` operator to guide your calculations. When you encounter pointer arithmetic, remember that adding to a pointer advances by entire elements, not bytes, which simplifies addressing. Keep experimenting with different data types and array lengths, and revisit this sheet to reinforce your comprehension. If you’re looking for more challenges on pointer arithmetic and dynamic memory, you might explore the [Pointer Arithmetic Exercises](./pointer_arithmetic.md) and the [Dynamic Allocation Guide](./dynamic_memory.md). Continuous practice will solidify your intuition for how C handles memory under the hood.
