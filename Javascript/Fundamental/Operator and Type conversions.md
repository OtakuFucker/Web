---
title: Operator and Type conversions
---

## Basic operators

Types of basic operators and their instances:
- Math operators: addition ```+```, multiplication ```*```, subtraction ```-```, division ```/```, Remainder ```%```, Exponentiation (power of) ```**```.
- Logical operators: ```||```, ```&&``` 
- Increment/decrement operators: ```++```, ```--```
- Comparison operators: ```>```, ```>=```, ```<```, ```<=```, ```==```, ```===```, ```!=```, ```!==```
- Conditional operator: ```?```
- Nullish coalescing operator: ```??```
- Assignment operator: ```=``` 

### Terminology: Operands, Unary, binary
1. Operands: What operators are applied to. For example, ```5``` and ```5``` are the operands in the sum ```5*5``` and ```*``` is the operator.
2. Uniary: If the operator is applied to only one operand, it's a **unary operator**.
3. Binary: If the operator is applied to two operands, it's a **binary operator**.
```
let x = 1;
x = -x;
console.log(x); //result: -1, - is a unary negation operator here
let y = 1, z = 3;
console.log(z - y); //result: 2, - is a  binary subtract opertaor here
```
**One symbol can refer to two different operators based on different application**

