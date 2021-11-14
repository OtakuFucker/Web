---
title: Memory allocation, Stack and Heap 🟠
---
It's fine to complete a simple project even though you know nothing about this. Therefore, there's no shame to pass this chapter if you're too new to this.
## Memory
In all language, memory consists of address and value.

![image](https://user-images.githubusercontent.com/89114612/141340337-bf1aff17-ea1b-436e-93f0-71c0e9a0da5b.png)

There are various types of memory structures.
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

This chapter correspondingly focuses on **allocation**, **use** parts.

## 1. Allocation
**When allocation occurs, Js engine creates a memory location for variable and assign a value to it.**

Technically speaking, allocation occurs only if the variable is defined, which means there's a initializing value.

Before further explaination, You may confused about the differences between declaration and definition. 
```
//In fact, every language has its own rule on defining declaration and definition.
/*In language that holds strict rules: 
  Declaration... 
  - is used to inform to the compiler the variable information except the conatined value (e.g. [int num;] in C)
  - won't trigger allocation to memory
  Definition...
  - assigns an actual value to the declared variable and telling where it gets stored
  - triggers allocation to memory
  - one variable can only be defined once
*/
//From the above definition, is there a value inside variable decides whether the variable would be stored in the memory or not.
//Therefore, the condition of occuring allocation is the assignment/definition to variable.
```

**However, Thing happens differently in Javascript...**

In Javascript, you can say that declaration guarantees definition. 
```
/*In Javascript, once you declare a variable, the value undefined is assigned to it. 
 [let num;] means [let num = undefined;]*/
//Declared variable has a default value even though it has not been explicitly initialized.
//Therefore, Declared-only variables in javascript are also able to trigger allocation.
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
<figurecaption>[img from: https://medium.com/@ethannam/javascripts-memory-model-7c972cd2c239]</figurecaption>
</figure>

<br>
<br>

**Let's modify/write the value now**
```
let num = 20;
num = 25; //write == assign
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

### Primitive value/Reference and Stack/Heap
The previous part explains that the value won't be changed due to the feature of primitive values. <br>
This part involves in a deeper discription on what **type of value** means. <br>

---
### 2.1. Primitive value and Reference

Main distinction between Primitive value and Reference is **the former is passed by value** while **the latter is by reference** during assignment.

***Passed by value: Copying the value to finish the assignment***

The previous part shows a instance of assignment of primitive value to variable. The result is that the address of the address of the variable is changed.

- Thing happens the same during declaration 
```
let num = 10;
let num2 = num;
```
- Wt the above codes do is copy and assign the value that sits in the address of ```num``` to ```num2```

Process of handling assignment to ```num2```:
1) Copy the value sits in the address of ```num```
2) Creates a new memory address
3) Allocates ```num2``` to the new address
4) Stores the assigned value in the new address

Now we can deduce some features of Primitive value and Passed by value:
1. Assinging a primitive value means either **writing a new primitive value to variable** or **assigning/initializing variable by another one that contains a primitive value**.
3. Primitive value is immutable in js, what it really means is primitive value sits in the address cannot be changed.
4. Everytime a value/variable containes value is passed, the js engine creates an new address for the assigned variable to store that value.
5. It's impossible that two variables point to the same address. (you may understand it as every variable that contains primitive value is independent)
6. Therefore, reassign primitive value to variable would only influence the taregt.

In the nutshell, the execution behind passed by value is copy the value and create an address for the variable to store that value.

***Passed by reference: Makeing two variables point to the same address during assignment.***
```
//This is a case of assigning an object to variable;
let obj = {name: "Eric"};
let obj2 = obj;
console.log(obj2.name); //result: Eric
//Now try to modify the value
obj2.name = "cuhk_Eric";
consoel.log(obj2.name); //result: cuhk_Eric
onj.name = "Eric_Leung";
console.log(obj2.name); //result: Eric_Leung
```
- The modification to the assigned variable modifies the value of the original variable simulatenously, vice versa.
- This reason is because both variables are pointing to the same address, so their value will be influenced by each other during assignment.

**What does reference means?**
It's easy to understand what is passed by value as primitive is the passed value itself. However, what does the term reference really refers to? We may need to further understand the always mentioned concept, address, means to allocation and it involves in understanding memory **stack** and **heap**.

---
### 2.2. Stack and Heap

Stack an Heap are two types of memories. Both of them consist of address and value section.

Normally, what we always hear is that primitive value is stored in stack while refernce is in heap. <br>
The above discription is in some sense correct but not accerately enough to explain how the allocation works. <br>

In fact, during allocation...
1) No matter which kind of data type is assigned, the js enigine allocates the identifier to an address in stack first.
2) Then js engine inputs the value in the value section. 
-  If the assigned value is **primitive data**, the value sits in the address of stack will be **that value**.<br>
   <figure>
   <img src="https://user-images.githubusercontent.com/89114612/141673808-0c8046a7-ae70-446f-af24-f575f39e32b5.png">
   <figurecaption>[img from: https://felixgerschau.com/javascript-memory-management/]</figurecaption>
   </figure>
-  If the assigned value is **reference**, the value sits in the address of stack will be **another address that points to heap memory**.
   <figure>
   <img src="https://user-images.githubusercontent.com/89114612/141673896-7ccb7134-da80-4785-8cd3-98370770a357.png">
   <figurecaption>[img from: https://felixgerschau.com/javascript-memory-management/]</figurecaption>
   </figure>

**All variables are actually first point to their corresponding address in stack, the difference is between which memory is its value stored in.**

#### Stack
1. Stack is a linear data structure.
2. Variables are stored in stack one by one and based on the rule of ```FILO```.(first in last out)
```
let num = 1;
let num2 = 2;
let num3 = 3;
let num4 = 4;
let obj = {};
```
|   Stack    |    :---:|:-:    |
|    :---:   |   :---: |  :-:  | 
| identifier | Address | Value |
|    obj     |   0x05  |  0x10 | 
|    num4    |   0x04  |  4    | 
|    num3    |   0x03  |  3    | 
|    num2    |   0x02  |  2    | 
|    num     |   0x01  |  1    | 

#### Heap 
1. Stack is a non-linear data structure.
2. Variable are stored in the heap randomly.

<figure>
<img src="https://user-images.githubusercontent.com/89114612/141675699-de74f8f8-9e4b-46b3-8904-3d2c1355802a.png" style="height:450px;">
</figure>

<br>

Let's go back to the mentioned question about what did reference means. The actual meaning of refernce is **the address that point to the object that stored in heap**.

## Summary
***In-depth understanding to stack and heap is so complex even to developers. It's fairly enough to know these features abstractly in this period.***
