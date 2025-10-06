# 1 Basic of C (and Programming)

---

<details markdown="1">
  <summary>Table of Contents</summary>

<!-- TOC -->
* [1 Basic of C (and Programming)](#1-basic-of-c-and-programming)
  * [1 Pointers](#1-pointers)
    * [1.1](#11)
  * [2 Data Structures](#2-data-structures)
    * [2.1 Arrays](#21-arrays)
    * [2.2 Linked Lists](#22-linked-lists)
    * [2.3 Stacks](#23-stacks)
    * [2.4 Queues](#24-queues)
    * [2.5 Trees and Graphs](#25-trees-and-graphs)
  * [3 Bitwise Operators](#3-bitwise-operators)
    * [3.1 Bitwise AND](#31-bitwise-and)
    * [3.2 Bitwise OR](#32-bitwise-or)
    * [3.3 Bitwise XOR](#33-bitwise-xor)
    * [3.4 Bitwise NOT](#34-bitwise-not)
    * [3.5 Left Shift](#35-left-shift)
    * [3.6 Right Shift](#36-right-shift)
    * [3.7 Check if a Specific Bit is Set](#37-check-if-a-specific-bit-is-set)
    * [3.8 Set a Specific Bit](#38-set-a-specific-bit)
    * [3.9 Clear a Specific Bit](#39-clear-a-specific-bit)
    * [3.10 Toggle a Specific Bit](#310-toggle-a-specific-bit)
    * [3.11 Extract N Bits](#311-extract-n-bits)
    * [3.12 Combine Bytes](#312-combine-bytes)
    * [3.13 Split a Larger Data Type into Bytes](#313-split-a-larger-data-type-into-bytes)
    * [3.14 Circular Shift](#314-circular-shift)
  * [4 Execution Time Analysis](#4-execution-time-analysis)
  * [5 Operating Systems](#5-operating-systems)
    * [5.1 Threads and Processes](#51-threads-and-processes)
  * [6 Scheduling](#6-scheduling)
    * [6.1 Tasks](#61-tasks)
      * [6.1.1 Task States](#611-task-states)
      * [6.1.2 Preemption](#612-preemption)
<!-- TOC -->

</details>

---

## 1 Pointers

### 1.1

---

## 2 Data Structures

### 2.1 Arrays

A set of elements of the same data type stored contiguously in memory.

- Fixed size (non-dynamic).
- Advantages:
    - Fast accessing of elements with an index at O(1) complexity.
- Disadvantages:
    - Insertion and deletion is costly at O(n) complexity.

### 2.2 Linked Lists

Pairs forming nodes where each node has a data element and a pointer to the next node.

- Advantages:
    - Insertions and deletions at the head is efficient at O(1) complexity.
    - No need for contiguous memory (dynamic).
- Disadvantages:
    - Slow access searching or accessing elements at O(n) complexity.
    - Extra memory used to store pointers.

### 2.3 Stacks

A linear data structure following LIFO (last in, first out), either an array or linked list.

- Push: add an element to the top of the stack.
- Pop: remove an element from the top of the stack.

### 2.4 Queues

A linear data structure following FIFO (first in, first out), either an array or linked list.

- Enqueue: add an element to the end of the queue.
- Dequeue: remove an element from the front of the queue.

### 2.5 Trees and Graphs

Non-linear data structures made of nodes connected by edges.

- Behave similar to linked lists in terms of edge paths.

---

## 3 Bitwise Operators

### 3.1 Bitwise AND

- Purpose: Masking bits (extract specific bits).
- Example:

```c
uint8_t value = 0b11010110;
uint8_t mask = 0b00001111;
uint8_t result = value & mask; // result = 0b00000110.
```

### 3.2 Bitwise OR

- Purpose: Setting specific bits.
- Example:

```c
uint8_t value = 0b11010100;
uint8_t mask = 0b00000011;
uint8_t result = value | mask; // result = 0b11010111.
```

### 3.3 Bitwise XOR

- Purpose: Toggling bits.
- Example:

```c
uint8_t value = 0b11010110;
uint8_t mask = 0b00000110;
uint8_t result = value ^ mask; // result = 0b11010000.
```

### 3.4 Bitwise NOT

- Purpose: Inverting all bits.
- Example:

```c
uint8_t value = 0b11010110;
uint8_t result = ~value; // result = 0b00101001.
```

### 3.5 Left Shift

- Purpose: Shifting bits to the left, often for multiplying by powers of 2.
- Example:

```c
uint8_t value = 0b00000101;
uint8_t result = value << 2; // result = 0b00010100.
```

### 3.6 Right Shift

- Purpose: Shifting bits to the right, often for dividing by powers of 2.
- Example:

```c
uint8_t value = 0b00010100;
uint8_t result = value >> 2; // result = 0b00000101.
```

### 3.7 Check if a Specific Bit is Set

- Purpose: Test whether a bit at a specific position is 1.
- Example:

```c
uint8_t value = 0b10101010;
uint8_t bit_position = 3;
uint8_t is_set = (value & (1 << bit_position)) != 0; // is_set = true.
```

### 3.8 Set a Specific Bit

- Purpose: Turn a specific bit ON.
- Example:

```c
uint8_t value = 0b10101010;
uint8_t bit_position = 2;
value |= (1 << bit_position); // value = 0b10101110.
```

### 3.9 Clear a Specific Bit

- Purpose: Turn a specific bit OFF.
- Example:

```c
uint8_t value = 0b10101010;
uint8_t bit_position = 1;
value &= ~(1 << bit_position); // value = 0b10101000.
```

### 3.10 Toggle a Specific Bit

- Purpose: Flip the state of a specific bit.
- Example:

```c
uint8_t value = 0b10101010;
uint8_t bit_position = 0;
value ^= (1 << bit_position); // value = 0b10101011.
```

### 3.11 Extract N Bits

- Purpose: Extract a specific number of bits from a value.
- Example:

```c
uint8_t value = 0b11011011;
uint8_t start_bit = 2;
uint8_t num_bits = 3;
uint8_t mask = (1 << num_bits) - 1;
uint8_t result = (value >> start_bit) & mask; // result = 0b00000110.
```

### 3.12 Combine Bytes

- Purpose: Combine multiple bytes into a larger data type (e.g., 16-bit or
  32-bit values).
- Example:

```c
uint8_t low_byte = 0x34;
uint8_t high_byte = 0x12;
uint16_t combined = (high_byte << 8) | low_byte; // combined = 0x1234.
```

### 3.13 Split a Larger Data Type into Bytes

- Purpose: Extract bytes from a multibyte variable.
- Example:

```c
uint16_t value = 0x1234;
uint8_t low_byte = value & 0xFF;         // low_byte = 0x34.
uint8_t high_byte = (value >> 8) & 0xFF; // high_byte = 0x12.
```

### 3.14 Circular Shift

- Purpose: Rotate bits left or right.
- Example (Left Rotate):

```c
uint8_t value = 0b11010110;
uint8_t result = (value << 3) | (value >> (8 - 3)); // result = 0b10110110.
```

---

## 4 Execution Time Analysis

1. **Offboard**: Inspecting compiled assembly code.
    - Tooling: `objdump`, CMake post-build disassembly, ELF/map analysis.
    - Purpose: Verify what the compiler actually emitted (instruction count, inlining, branches).
    - Limitation: Can't capture runtime effects (interrupts, caches, bus contention).
    - Example ARM GCC toolchain `CMakeLists.txt` segment:
        ```cmake
        add_custom_command(TARGET ${CMAKE_PROJECT_NAME} POST_BUILD
                COMMAND arm-none-eabi-objdump -d -S $<TARGET_FILE:${CMAKE_PROJECT_NAME}>
                > ${CMAKE_PROJECT_NAME}.disasm)
        ```
        - Every CMake build automatically produces a human-readable assembly listing of the
          firmware, with C source lines interleaved (when built with debug info enabled, e.g. `-g`),
          for inspection and debugging.
2. **Onboard (external)**: Using an oscilloscope/logic analyzer.
    - Tooling: GPIO pulse instrumentation + scope/logic analyzer.
    - Purpose: Hard real-time observation of latencies, jitter, ISR execution windows.
    - Limitation: Only shows what you toggle, coarse system visibility unless heavily instrumented.
3. **Onboard (internal)**: Using timer/counter peripherals.
    - Tooling: Data Watchpoint and Trace unit (DWT) Cycle Counter Register (CYCCNT), SysTick,
      general timers.
    - Purpose: Cycle-accurate, low-overhead measurement of function/runtime sections.
    - Limitation: Measures only where you target, unknown system-wide activity.

---

## 5 Operating Systems

### 5.1 Threads and Processes

A **process** is an independent program running on your computer, with its own memory and resources
managed by the operating system. Each process operates in isolation, meaning one process can't
directly access another's memory, which makes them stable, but slower to communicate. Starting a new
process is relatively expensive since the system must allocate separate memory and resources for it.

A **thread**, on the other hand, is a smaller unit of execution within a process. Multiple threads
can run at the same time within the same program, sharing the same memory space and data. This makes
threads faster and more efficient for multitasking, but also more prone to errors like race
conditions if they try to access shared data at the same time without proper coordination.

---

## 6 Scheduling

### 6.1 Tasks

Generally, tasks are either "periodic" or "aperiodic":

1. **Periodic**:
    - Cyclic execution: Repeats at specific frequency.
    - Hard deadlines: Each instance must complete execution prior to the next instance starting to
      prevent backlogs.
2. **Aperiodic**:
    - Event-driven: Triggered be an event.
    - Deadlines: Either no deadlines or soft deadlines.

Given task $T_i$:

- $r_i$: The **release time**, or time a task takes to become available for execution.
    - The response time is the time between a task being released and finishing its execution.
- $e_i$: The **execution time** of a task.
- $D_i$: The **relative deadline**, or the maximum allowable response time.
- $p_i$: The **period**, the time between the release times of two consecutive instances.
- $\phi_i$: The **phase** or release time of the task's first instance.
- $u_i$: The **utilization** ratio of a task's execution time over its period:
  $$u_i = \frac{e_i}{p_i}$$

#### 6.1.1 Task States

There are 3 general states for a given task:

1. **Ready**: Ready to execute.
2. **Running**: Actively executing.
3. **Blocked**: Awaiting an external event or prerequisite conditions before executing.

#### 6.1.2 Preemption

Preemption is the act of temporarily interrupting a running task to run a higher priority task.

- **Preemptable**: preemption is possible for a given task.
- **Nonpreemptable**: preemption is not possible (no interruption) for a given task.
- **Partially preemptable**: certain parts of a task are not allowed to be interrupted.
