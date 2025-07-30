# Javascript-interview-questions

### 1- What is event loop in JavaScript ###
The event loop in JavaScript is a fundamental concept that enabled **non-blocking, asynchronous behaviour** - even through Javascript is **single-threaded**  

**Simple terms** - Perform long task like Api Calls or timers *without freezing* the page  

**How it works** JavaScript RunTime  
```
1- Call Stack - where function calls are executed.  
2- web Api - for async task like setTimeout, fetch etc.  
3- Callback Que - Hold messages (like callbacks) waiting to be processed.
4- Event loop - constantly check
   A - is the call stack is empty
   B - if yes it moves the next callback from the queue to the stack

Example : -
console.log('start');  
setTimeout(()=>{  
   sonsole.log('inside timeout')  
}, 0);
console.log('End');

output:  
Start  
End  
inside timeout  

```
### 2- What is JavaScript Callback? ###  
- Callback is a function that passed as an arguments to another function;  
**usecase** - To handle async task(Api calls, timmers).  
            - avoid blocking the main thread.
```
**Example:**  
Function fullname (name, callBackFn){  
    console.log("Hellow ", name)  
    callBackFn()  
}  
function country(){  
 console.log("India")  
}  
fullname('ajay', country)  
Output:  
Helloe ajay  
India  
```
### 3 - What is Closure in JavaScript? ###  
A closure is a function that remember the variable from its **outer (lexical) scope**  
even after the outer function has finished executing.  
***A closure in JavaScript is the combination of a function and its lexical environment***  

**Lexical Scoping:** JavaScript uses lexical scoping, meaning that the scope of a variable is determined by its position in the source code. An inner function inherently has access to the variables and parameters of its containing (outer) function.
```
let globalVar = "I am global"; // Global scope  
function outerFunction() {
  let outerVar = "I am in outerFunction"; // outerFunction's scope   
  function innerFunction() {
    let innerVar = "I am in innerFunction"; // innerFunction's scope  
    console.log(globalVar); // Accessible  
    console.log(outerVar);  // Accessible  
    console.log(innerVar);  // Accessible  
  }  
  innerFunction();  
  // console.log(innerVar); // Not accessible here (ReferenceError)  
}  
outerFunction();  
// console.log(outerVar); // Not accessible here (ReferenceError)  
```

**why we use closures**  Create Private variables, **Data Privacy**

> NOTE: 
Data can be hidden from the global scope and only accessed or modified through specific, controlled functions returned by the outer function.

### 3 - What is Hoisting in JavaScript? ###   
Hoisting is JavaScript default behaviour of moving declerations to the top (before code eecution)  
***variables and functions are hoisted - declared before any code is executed***  
But Only the **declarations** are hoisted, not the **initialization**.    
**Example - 1:**  
```
console.log(x); undefined  
var x = 5  
-------------------------
Javascript interprets it like this manually:  
var x;  
console.log(x)  
x = 5;  
// That`s why you dont get error - just undefined //  
```
**Example- 2 (let and const)**  
```
console.log(a)  // ReferenceError//  
let a = 10;  
The variable a is hoisted but cannot be accessed untill it is declared, This is ther Temporal Dead Zone  
```
**Example - 3 (Functions)**  
```diff
+ Function are fully hoisted, so you can call them before they are define
sayHellow() // working //
function sayHellow(){
   console.log("Hello! Ajay")
}
```
**Example - 4 (Arrow Functions)**   
```diff
hellow(); // type error : hellow is not a function  //  
var hellow = ()=>{  
console.log("hellow")  
}  
- hellow is hoisted as undefined //
```
### 4 - difference between object assign and spread operator ###
**Object.assign()** - This method mutates the target object. It takes a target object as the first argument and one or more source objects as subsequent arguments.  
```diff
const obj1 = { a: 1 };  
const obj2 = { b: 2 };  
Object.assign(obj1, obj2);  
! obj1 is now { a: 1, b: 2 } //
```
**Spread Operator** - The object spread operator creates a new object. It copies the enumerable own properties from the source objects into a new object, leaving the original objects unchanged.  
```diff
const obj1 = { a: 1 };
const obj2 = { b: 2 };
const newObj = { ...obj1, ...obj2 };  
! // newObj is { a: 1, b: 2 }, obj1 and obj2 are unchanged //
```
### 5 - difference between object assign and object create ###
**Object.create(**) - Creates a new object with a specified prototype object  
**Object.assign()** - Copies enumerable own properties from one or more source objects to a target object  
> NOTE:  
> Object.assign(target, ...sources)  
> **target**: The target object to which source properties are copied.  
> **sources**: One or more source objects from which properties are copied.  







