# Digital Systems

---

<details markdown="1">
  <summary>Table of Contents</summary>

<!-- TOC -->
* [Digital Systems](#digital-systems)
  * [1 Boolean Algebra Laws](#1-boolean-algebra-laws)
    * [1.1 Operations](#11-operations)
    * [1.2 Order of Operations](#12-order-of-operations)
    * [1.3 Equality Laws and Theorems](#13-equality-laws-and-theorems)
<!-- TOC -->

</details>

---

## 1 Boolean Algebra Laws

### 1.1 Operations

- X AND Y = Product operation = $X Y$.
- X OR Y = Addition operation = $X + Y$.
- NOT X = Bar operation = $\bar{X}$.

### 1.2 Order of Operations

1. Brackets
2. NOT
3. AND
4. OR

### 1.3 Equality Laws and Theorems

| Law/Theorem  |                                                                                                                                       |                                                                                                                                        |
|--------------|:-------------------------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------------------------------------------------------:|
| Identity     |                                                           $$X \cdot 1 = X$$                                                           |                                                             $$X + 0 = X$$                                                              |
| Annulment    |                                                           $$X \cdot 0 = 0$$                                                           |                                                             $$X + 1 = 1$$                                                              |
| Complement   |                                                           $$X \bar{X} = 0$$                                                           |                                                          $$X + \bar{X} = 1$$                                                           |
| Commutative  |                                                             $$X Y = Y X$$                                                             |                                                           $$X + Y = Y + X$$                                                            |
| Idempotent   |                                                      $$\bar{\bar{X}} = X X = X$$                                                      |                                                             $$X + X = X$$                                                              |
| Associative  |                                            $$\left( X Y \right) Z = X \left( Y Z \right)$$                                            |                                        $$\left( X + Y \right) + Z = X + \left( Y + Z \right)$$                                         |
| Distributive |                                                 $$X \left( Y + Z \right) = XY + XZ$$                                                  |                                         $$X + YZ = \left( X + Y \right) \left( X + Z \right)$$                                         |
| Unity        |                                                        $$X Y + \bar{X} Y = Y$$                                                        |                                       $$\left( X + Y \right) + \left( \bar{X} + Y \right) = Y$$                                        |
| Absorption   |                                                            $$X + X Y = X$$                                                            |                                                     $$X \left( X + Y \right) = X$$                                                     |
| Absorption   |                                                       $$X + \bar{X} Y = X + Y$$                                                       |                                                 $$X \left( \bar{X} + Y \right) = XY$$                                                  |
| DeMorgan's   |                                        $$\overline{ \left( X Y \right) } = \bar{X} + \bar{Y}$$                                        |                                           $$\bar{ \left( X + Y \right) } = \bar{X} \bar{Y}$$                                           |
| Consensus    |                                               $$XY + YZ + \bar{X} Z = XY + \bar{X} Z$$                                                |                                                                                                                                        |
| XOR          | $$X \oplus Y = \bar{X} \oplus \bar{Y} = \overline{ \left( \bar{X} \oplus Y \right) } = \overline{ \left( X \oplus \bar{Y} \right) }$$ | $$\overline{ \left( X \oplus Y \right) } = \overline{ \left( \bar{X} \oplus \bar{Y} \right) } = \bar{X} \oplus Y = X \oplus \bar{Y} $$ |
