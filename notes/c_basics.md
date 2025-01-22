## 1 Basic Bit Operations

---

<details markdown="1">
  <summary>Table of Contents</summary>

<!-- TOC -->
  * [1 Basic Bit Operations](#1-basic-bit-operations)
    * [1.1 Bitwise AND](#11-bitwise-and)
    * [1.2 Bitwise OR](#12-bitwise-or)
    * [1.3 Bitwise XOR](#13-bitwise-xor)
    * [1.4 Bitwise NOT](#14-bitwise-not)
    * [1.5 Left Shift](#15-left-shift)
    * [1.6 Right Shift](#16-right-shift)
    * [1.7 Check if a Specific Bit is Set](#17-check-if-a-specific-bit-is-set)
    * [1.8 Set a Specific Bit](#18-set-a-specific-bit)
    * [1.9 Clear a Specific Bit](#19-clear-a-specific-bit)
    * [1.10 Toggle a Specific Bit](#110-toggle-a-specific-bit)
    * [1.11 Extract N Bits](#111-extract-n-bits)
    * [1.12 Combine Bytes](#112-combine-bytes)
    * [1.13 Split a Larger Data Type into Bytes](#113-split-a-larger-data-type-into-bytes)
    * [1.14 Circular Shift](#114-circular-shift)
<!-- TOC -->

</details>

---

### 1.1 Bitwise AND

- Purpose: Masking bits (extract specific bits).
- Example:

```c
uint8_t value = 0b11010110;
uint8_t mask = 0b00001111;
uint8_t result = value & mask; // result = 0b00000110.
```

### 1.2 Bitwise OR

- Purpose: Setting specific bits.
- Example:

```c
uint8_t value = 0b11010100;
uint8_t mask = 0b00000011;
uint8_t result = value | mask; // result = 0b11010111.
```

### 1.3 Bitwise XOR

- Purpose: Toggling bits.
- Example:

```c
uint8_t value = 0b11010110;
uint8_t mask = 0b00000110;
uint8_t result = value ^ mask; // result = 0b11010000.
```

### 1.4 Bitwise NOT

- Purpose: Inverting all bits.
- Example:

```c
uint8_t value = 0b11010110;
uint8_t result = ~value; // result = 0b00101001.
```

### 1.5 Left Shift

- Purpose: Shifting bits to the left, often for multiplying by powers of 2.
- Example:

```c
uint8_t value = 0b00000101;
uint8_t result = value << 2; // result = 0b00010100.
```

### 1.6 Right Shift

- Purpose: Shifting bits to the right, often for dividing by powers of 2.
- Example:

```c
uint8_t value = 0b00010100;
uint8_t result = value >> 2; // result = 0b00000101.
```

### 1.7 Check if a Specific Bit is Set

- Purpose: Test whether a bit at a specific position is 1.
- Example:

```c
uint8_t value = 0b10101010;
uint8_t bit_position = 3;
uint8_t is_set = (value & (1 << bit_position)) != 0; // is_set = true.
```

### 1.8 Set a Specific Bit

- Purpose: Turn a specific bit ON.
- Example:

```c
uint8_t value = 0b10101010;
uint8_t bit_position = 2;
value |= (1 << bit_position); // value = 0b10101110.
```

### 1.9 Clear a Specific Bit

- Purpose: Turn a specific bit OFF.
- Example:

```c
uint8_t value = 0b10101010;
uint8_t bit_position = 1;
value &= ~(1 << bit_position); // value = 0b10101000.
```

### 1.10 Toggle a Specific Bit

- Purpose: Flip the state of a specific bit.
- Example:

```c
uint8_t value = 0b10101010;
uint8_t bit_position = 0;
value ^= (1 << bit_position); // value = 0b10101011.
```

### 1.11 Extract N Bits

- Purpose: Extract a specific number of bits from a value.
- Example:

```c
uint8_t value = 0b11011011;
uint8_t start_bit = 2;
uint8_t num_bits = 3;
uint8_t mask = (1 << num_bits) - 1;
uint8_t result = (value >> start_bit) & mask; // result = 0b00000110.
```

### 1.12 Combine Bytes

- Purpose: Combine multiple bytes into a larger data type (e.g., 16-bit or
  32-bit values).
- Example:

```c
uint8_t low_byte = 0x34;
uint8_t high_byte = 0x12;
uint16_t combined = (high_byte << 8) | low_byte; // combined = 0x1234.
```

### 1.13 Split a Larger Data Type into Bytes

- Purpose: Extract bytes from a multibyte variable.
- Example:

```c
uint16_t value = 0x1234;
uint8_t low_byte = value & 0xFF;         // low_byte = 0x34.
uint8_t high_byte = (value >> 8) & 0xFF; // high_byte = 0x12.
```

### 1.14 Circular Shift

- Purpose: Rotate bits left or right.
- Example (Left Rotate):

```c
uint8_t value = 0b11010110;
uint8_t result = (value << 3) | (value >> (8 - 3)); // result = 0b10110110.
```
