---
Title: Variable and Basic data types
---
 
## Variable
Variable is a "name storage" of data

### Keywords to declare variable in ES6
1) **let** (changeable)
2) **const** (unchangeable) 

**Rules:** <br>
1. Whether the value can be changed depends on the keyword <br>
2. Both let and const cannot be redeclared 

### Structure of a variable decaration:<br>
Example: let value = 20:
- **let value** > keyword and storage name;
- **=** > assign operator;
- **20** > assigned data;

Process: get the data > data assigned by assign operator > data stored by named variable <br>
**The storage is a from right to left process.**

### Let
let is an advancement of **var**, keyword of changeable variable before ES6. <br>
The propose of let has solved demerits of var: <br>
1. Unexpected consequences casued by hoisting
2. Acceptance of variable redeclaration 

### Const
const is almostly the same as let. <br>
The differences are its value **was fixed after assigned** and **should be assigned during initialization**.

const is good for you when...
1. You're sure that variable will never change, then const will secure variable against unexpected change
2. You want to store some different-to-remember values (like color in hexadecmial format)
```
/*FYI, All letters of the name would be uppercase if it's 
  1. constant that are known prior to execution and 
  2. calculated in run-time*/
const COLOR_RED = "#F00";
const COLOR_GREEN = "#0F0";
const COLOR_BLUE = "#00F";
const COLOR_ORANGE = "#FF7F00";
//COLOR_RED is obviously more readable than "#F00".
// ...when we need to pick a color
let color = COLOR_ORANGE;
alert(color); // #FF7F00
```
After all, you have to make sure to the exact value before using const.

```
Good Practise:
1. Due to the consideration of readability, developers should always set a clear and meaningful variable name
2. There is a unspoken rule to create variable name in a camelCase style
3. Usually create a new variable if needed, reuse is evil and that's why const is proposed 
```
## Data Types
Javascript is a weak language, which means all the variables are declared as let or const. Its data types need to be detected by the assigned value. <br>
There're 8 Data Types in Javascript:
1. number
2. bigint
3. string (there is only string but no char type in Javascript)
4. boolen (logical type)
5. null
6. undefined
7. symbo (after ES6)
8. object

- All of the above 8 are called Basic data type. 
- Normally, basic data types can be understood as return values of the **typeof operator**. (exception: typeof object and function)
- Only 6 out of 8 basic data types are widely used, among them **bigint** and **symbol** are rarely appear  
- Data Types except object are defined as **Primitive values**, while object is defined as **Reference**

### Importance of understanding different Data Types
**!!Data types influence the assign method to the variable!!**

Generally speaking, the method depends on whether the assigned value is included in Primitive values or Refernce
- Primitive value: assigns the **value** to variable
```
let name = `max`;
let copyName = name;
console.log(copyName); //wt really did here is creating a new and independent string value max
```
- Reference: assigns the **address** to variable
```
let obj = {name: wilson}
let obj2 = obj; //wt reallly did here is making both object point to the same address
```
They seem the same while we are "copying" and assigning them, but the result will be completely different if we modify the assigned variable.
#### Difference between assigns value and address
- Assign value: Modify the assigned variable influences itself only
- Assign address: Modify an assigned variable influences all variables that point to the same address
 
The above differences are due to the data structure they allocated to

