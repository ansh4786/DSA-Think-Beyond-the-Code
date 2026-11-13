# Module 1 — Fundamentals of DSA

*Before choosing a solution, understand the problem.*

## 1. Data structures

### WHY?
Imagine keeping 500 student marks in 500 unrelated variables. Updating, searching, and processing them would be painful. A data structure gives data an organised form.

### IDEA
A **data structure** is a way of organising and storing data so it can be accessed and modified efficiently.

- **Linear:** elements are arranged sequentially — arrays, linked lists, stacks, queues.
- **Non-linear:** elements form hierarchies or networks — trees and graphs.
- **Static:** size is generally fixed after creation — a conventional fixed-size array.
- **Dynamic:** size can change during execution — linked lists allocated with `malloc`.

### THINK
Which structure would you choose for (a) browser back history, (b) people waiting in a queue, and (c) a family tree?

**Try:** stack, queue, tree. Explain why.

## 2. Algorithms and analysis

### WHY?
A solution must be more than correct: it should finish in reasonable time and use reasonable memory.

### IDEA
An **algorithm** is a finite, ordered set of unambiguous steps that solves a problem.

A good algorithm should have:
- clearly defined input and output;
- definite steps;
- finite termination;
- effective, executable operations;
- correctness for valid inputs.

**Time complexity** describes how running time grows with input size. **Space complexity** describes how memory use grows.

Common growth rates:

| Notation | Meaning | Example |
|---|---|---|
| O(1) | Constant | Access `a[i]` |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Scan an array |
| O(n log n) | Linearithmic | Merge sort |
| O(n²) | Quadratic | Two nested full loops |

- **Big-O:** asymptotic upper bound.
- **Big-Omega (Ω):** asymptotic lower bound.
- **Big-Theta (Θ):** tight asymptotic bound.

Cases:
- **Best:** most favourable input.
- **Average:** expected behaviour over inputs.
- **Worst:** most costly input of a given size.

### THINK
An algorithm checks every item in a list once. If the list doubles in size, roughly what happens to the number of checks?

**Answer:** It roughly doubles — linear growth, O(n).

## 3. Arrays

### WHY?
When you need many values of the same type and want direct access by position, an array is useful.

### IDEA
An array stores same-type elements in contiguous memory. In C, indexing starts at zero.

```c
int marks[5] = {70, 82, 65, 91, 76};
printf("%d", marks[2]);  // 65
```

For an array with base address `BA`, element size `w`, and lower bound `LB`:

`LOC(A[i]) = BA + (i - LB) * w`

For a 2D array in row-major order with `N` columns:

`LOC(A[i][j]) = BA + ((i * N) + j) * w`  (zero-based indexing)

### Insertion at a position

```c
#include <stdio.h>

int main(void) {
    int a[100] = {10, 20, 30, 40};
    int n = 4, pos = 2, value = 25;

    if (n >= 100 || pos < 0 || pos > n) {
        printf("Invalid insertion\n");
        return 1;
    }

    for (int i = n; i > pos; --i)
        a[i] = a[i - 1];

    a[pos] = value;
    ++n;

    for (int i = 0; i < n; ++i)
        printf("%d ", a[i]);
    return 0;
}
```

### Deletion at a position

```c
#include <stdio.h>

int main(void) {
    int a[100] = {10, 20, 30, 40};
    int n = 4, pos = 1;

    if (pos < 0 || pos >= n) {
        printf("Invalid deletion\n");
        return 1;
    }

    for (int i = pos; i < n - 1; ++i)
        a[i] = a[i + 1];

    --n;

    for (int i = 0; i < n; ++i)
        printf("%d ", a[i]);
    return 0;
}
```

Access by index is O(1). Searching an unsorted array is O(n). Insertion/deletion at an arbitrary position is O(n) because elements may need shifting.

### THINK
Why can an array access `a[900]` directly, but inserting a new value at `a[2]` may require moving many elements?

## 4. Sparse matrices

### WHY?
A matrix may contain thousands of entries but very few non-zero values. Storing every zero wastes memory.

### IDEA
A **sparse matrix** has mostly zero elements. One common representation stores only non-zero entries as triples `(row, column, value)`.

Example:

```text
0 0 5
0 0 0
7 0 0
```

Triplet form (row, column, value; using zero-based indices):

```text
(0, 2, 5)
(2, 0, 7)
```

Other representations include linked structures. The best choice depends on the operations required.

### THINK
If a 1000 × 1000 matrix contains only 10 non-zero values, should we store one million numbers?

## 5. Matrix operations

### WHY?
Matrices represent tables of values used in graphics, engineering, and scientific computing.

- Addition requires equal dimensions; add corresponding entries.
- Multiplication of A (`m × n`) and B (`n × p`) produces C (`m × p`).
- `C[i][j]` is the sum of `A[i][k] * B[k][j]` over all valid `k`.

```c
for (int i = 0; i < m; ++i)
    for (int j = 0; j < p; ++j) {
        C[i][j] = 0;
        for (int k = 0; k < n; ++k)
            C[i][j] += A[i][k] * B[k][j];
    }
```

For ordinary matrix multiplication, the basic algorithm takes O(mnp) time.

## 6. Strings

### WHY?
Names, messages, passwords, and search terms are all text. C represents a string as a character array ending with the null character `'\0'`.

```c
char word[] = "code";
```

Common operations:
- length: `strlen`
- copy: `strcpy`
- compare: `strcmp`
- concatenate: `strcat`

These functions are declared in `<string.h>`. Ensure destination arrays have enough capacity.

### THINK
Why does C need `'\0'` to mark the end of a string?

## 7. Brute-force pattern matching

### WHY?
Text search is a basic operation behind finding a word in a document or locating a pattern in a string.

### IDEA
Try matching the pattern at every possible starting position in the text. If a character mismatches, shift the pattern one position and try again.

For text length `n` and pattern length `m`, worst-case time is O(nm).

```c
#include <stdio.h>
#include <string.h>

int main(void) {
    char text[] = "datastructures";
    char pattern[] = "struct";
    int n = (int)strlen(text);
    int m = (int)strlen(pattern);
    int found = 0;

    for (int i = 0; i <= n - m; ++i) {
        int j = 0;
        while (j < m && text[i + j] == pattern[j])
            ++j;

        if (j == m) {
            printf("Pattern found at index %d\n", i);
            found = 1;
        }
    }

    if (!found)
        printf("Pattern not found\n");
    return 0;
}
```

## 8. Searching

### Linear search
Check elements one by one. Works on unsorted data. Worst-case time: O(n).

### Binary search
Repeatedly halve the search interval. **The array must be sorted.** Worst-case time: O(log n).

```c
int binarySearch(const int a[], int n, int key) {
    int low = 0, high = n - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (a[mid] == key) return mid;
        if (a[mid] < key) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
```

### THINK
Can binary search work correctly on an unsorted array? Why or why not?

## Practice

1. Define a data structure and classify common types.
2. Explain algorithm characteristics and time vs space complexity.
3. Compare best, average, and worst cases.
4. Derive 1D and 2D array address calculations.
5. Write insertion and deletion algorithms for arrays.
6. Explain sparse matrix representation.
7. Write matrix addition and multiplication programs.
8. Explain C string operations.
9. Trace brute-force pattern matching.
10. Compare linear and binary search.

## ☕ Beyond the Code
If two people reach the same destination, but one takes a direct road and the other takes every possible detour, did they solve the journey equally well?


## Standalone C examples

- [`arrays.c`](arrays.c) — traversal, insertion, deletion, linear and binary search.
- [`matrix_operations.c`](matrix_operations.c) — matrix addition and multiplication.
- [`sparse_matrix_triplet.c`](sparse_matrix_triplet.c) — triplet representation.
- [`string_operations.c`](string_operations.c) — length, compare, copy, concatenate.
- [`brute_force_pattern.c`](brute_force_pattern.c) — naive pattern matching.
