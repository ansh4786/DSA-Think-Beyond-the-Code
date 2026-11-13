# C code guide

## Compile
```bash
gcc -std=c11 -Wall -Wextra -pedantic file.c -o program
./program
```

Windows:
```bash
gcc -std=c11 -Wall -Wextra -pedantic file.c -o program.exe
program.exe
```

## Safety and assumptions
- Examples are educational starting points, not production libraries.
- Check the return value of `scanf`, `malloc`, and input functions.
- Free dynamically allocated memory when finished.
- Binary search requires sorted input.
- Dijkstra's algorithm in this course requires non-negative edge weights.
- The infix-to-postfix example supports single-character operands and operators `+ - * / ^` with parentheses; it does not parse unary operators or multi-digit operands.
- The postfix evaluator supports space-separated integer operands and `+ - * /`; integer division truncates toward zero.
- The graph examples use small fixed-size adjacency matrices for clarity.
- Prim's example expects an undirected, connected weighted graph.
- The topological-sort example detects a cycle when fewer than V vertices are output.

## Complexity convention
Complexities describe growth as input size increases. Constants and lower-order terms are omitted. State whether a result is best, average, worst, or amortised when that distinction matters.
