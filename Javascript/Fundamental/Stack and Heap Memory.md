---
title: Memory allocation / Garbage collection and Memory leaks 🟠
---
It's fine to complete a simple project even though you know nothing about the above topics. Therefore, there's no shame to pass this chapter if you're too new to them.
## Memory
In all language, memory consists in address and value <br>
![image](https://user-images.githubusercontent.com/89114612/141340337-bf1aff17-ea1b-436e-93f0-71c0e9a0da5b.png)

## Memory Allocation
Once we maniuplate javascript code, the JS engine allocates memory for this and releases it once it's not needed anymore.
![image](https://user-images.githubusercontent.com/89114612/141302512-f92864fc-c62e-43b9-aac6-3f1e9e2653f7.png)
In the above image, there are three steps in the Memory life cycle:
1) Allocate the memory you need
2) Use the allocated memory (read, write)
3) Release the allocated memory when it is not needed anymore

Used of allocated memory should be done by developer in all languages. However, as a high level language, javascript finish the first and last step implicitly and provide no explicit operation to them. 
<br>

Therefore, we can conclude that the process of memory life cycle **in js** is...
1) Allocates memory automatically
2) Uses the allocated memory 
3) Releases the allocated memory when it is not needed anymore automatically

This chapter would be correspondingly divided into **allocation**, **use** and **release** parts.

## 1. Allocation
**When allocation occurs, Js engine creates a memory location for variable and assign value**

Technically speaking, allocation occurs only if the variable is defined, which means there's a value assigned to the variable.

Before further explaination, You may confused about the differences between declaration and definition. 
```
//In fact, every language has its own rule on defining declaration and definition.
/*In language that holds strict rules: 
  Declaration... 
  - is used to inform to the compiler the variable information except the conatined value (e.g. let num/int num in C)
  - won't occur allocation to memory
  Definition...
  - assigns an actual value to the declared variable and telling where it gets stored
  - occurs allocation to memory
  - one variable can only be defined once
*/
//From the above definition, the value decides whether the variable would be stored in the memory
//Therefore, the condition of occuring allocation is the assignment/definition of variable's actual value.
```

**However, Thing happens differently in Javascript...**

In Javascript, you can say that declaration guarantee definition. 
```
//In Javascript, once you declare a variable, the value undefined is assigned to it even though it has not been explicitly initialized by developer. 
//[let num;] means [let num = undefined;]
//All variables in javascript occur allocation no matter there is a explicitly initialized value or not.
```

## 2. Use
Instances of using allocated memory: **Write/Read the value of variable + Passing the argument**
```
let x;         //x = undefined and allocated to a memory
x = 10;        //writes to x
console.log(x) //reads from x and prints 10
```

Takes writing variable value as the focus instance:
```
//declaration
let num = 20;
```

To beginners, the above code is understood as a variable ```num```stores value 20. 

However, while the above code is executed, the JS actually...
1) creates a unique identifier ```num``` for the variable 
2) allocates a memory for ```num``` (e.g. 0x09)
3) stores the written value (20) to the allocated memory (0x09) 

**How the memory looks:**
| identifier | Address | Value |  
|    :---:   |   :---: |  :-:  | 
|    num     |   0x09  |  20   | 

Therefore, what the above code means is allocates  ```num``` to a memory address and store the value 20 in that address.

This is a crucial distinction to understand that:
1. The identifier equal to the address but not value
2. The address is the one conatins the value

- **Technically, identifier equals to the memory address that holds the value.**
<figure>
<img src="https://user-images.githubusercontent.com/89114612/141510771-e142d2f4-f1f3-4499-aa1d-9108f249fb9b.png" style="width:600px;height:200px;">
<figurecaption>[img from https://medium.com/@ethannam/javascripts-memory-model-7c972cd2c239]</figurecaption>
</figure>

<br>
<br>

**Let's modify/write the value now**
```
let num = 20;
num = 25; //write
console.log(num); //read
```
It's obvious that the printed result is 25.<br>
However, primitive values in javascript are immutable. <br>
- **Therefore, the changed stuff is the address that the identifier refers to but not the value.**

Process of handling ```num``` modification:
1) Detects that a new value is assigned to ```num```
2) Creates a new memory address
3) Allocates ```num``` to the new address
4) stores the new value in the new address

**Now the memory looks:**
| identifier | Address | Value |  
|    :---:   |   :---: |  :-:  | 
|    num     |   0x10  |  25   | 

<figure>
<img src="https://user-images.githubusercontent.com/89114612/141513663-ffc87a52-ba00-43c8-b925-554c5b508a7b.png" style="width:600px;height:200px;">
<figurecaption>[img from https://medium.com/@ethannam/javascripts-memory-model-7c972cd2c239]</figurecaption>
</figure>

### Primitive value/Reference and Stack/Heap
The previous chapter mentioned that variable contains primitive value and reference performs differently while modify their value. <br>
It's due to the differnet memory structure they allocated.
### What if we assign an identifier to another variable during declaration?
```
let num = 20;
let num2 = num;
```
What the above code do is copying the address of identifier **num**(0x09) to num2. 

#### Memories stored inside Stack
| identifier | Address | Value |  
|  :---:     |  :---:  |  :-:  | 
|   num      |   0x09  |  20   | 
|   num2     |   0x09  |  20   |

- All types of **variables** are stored in the stack, the bifurcation starts from which type does the variable contain:  
- Identifier can be treated as the same existence as the address (e.g. (num == 0x09) = 20)

#### Memories stored inside Stack
| variable          | Address | Value |  
|  :---:            |  :---:  |  :-:  | 
| let num = 10      |   0x09  |  10   | 
| let num2 = num    |   0x09  |  10   |
| let obj = {}      |   0x10  |  0x11 |
| let obj2 = obj    |   0x10  |  0x11 |

### Same:
1) What is passing from the assigned variable is its address
2) Both the assigned and assign variable point to the same address if there isn't any modification on it (assigned one)**
### Differences:
1) If the contained variable is **Primitive value**, the value is stored in Value section directly. Otherwise, the value section store another address.
2) When the assigned variable is modified...
  ```
  num2 = 24;
  ```
  - If the contained value is primitive value, a new address for storing the value 24 will be allocated

**FYI, it raises a deeper interpertion to the difference between let and const in the previous chapter. As you can see when the value is changed, what it really changes is the address that the variable points to, and then add the corresponding value to the new adrress.**

**Therefore, their actual difference is let allows the change to the address while const rejects so**

**Some methods just modify the value, while some assign a completely new address. !!Change of value is different from change of memory, it's a important point that has always to be remembered in the future!!**
  ```
  let obj = {name: `Eric`};
  let obj2 = obj;
  obj2.name = `Eric Leung`;
  console.log(obj.name); //result: Eric Leung
  ```
  - If the contained value is reference, all data in the variables that point to the same address would be modified

Let's conclude different stacks of the memory allocation first

#### Address section: 
- Identifer/ Address/ Variable stored inside stacks

#### Value section
- If value is primitive value, it's stored inside **value section of stack** directly
- If value is reference, it's actual value stored inside **heap memory** and pointed by the address which is inside **value section of stack**

## JavaScript’s memory model: the call stack and the heap
### Stack
The stack is where primitives value are stored. The key feature is that the data stored inside the stack is in a stacking order and following the rule of FILO (first in last out)
### Heap
As an instance of memory structure, heap also consists in address and value
However...
1) The heap is used to store non-primitives values 
2) Heap store non-primitives values, data that can grow dynamically—perfect for arrays and objects, unorderly and randomly.
3) The "allocated adddress" in the heap is the **value of object-type variable** in the stack

<figure>
<img src="https://user-images.githubusercontent.com/89114612/141468386-9b11b0ea-b24f-49b8-a80d-5f8bb1ea451a.png">
<figcaption>figure from: https://medium.com/@ethannam/javascripts-memory-model-7c972cd2c239</figcaption>
</figure>










































## Stack and Heap
All data types (static data) are allocated to **stack memory**, while Reference Type would be allocated to **heap memory** afterward

The process of memory allocation:<br>
1. Static memory allocation (for all data) > 2. Dynamic memory allocation (for references)

### Stack: Static memory allocation
