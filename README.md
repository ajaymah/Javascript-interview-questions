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
Closures are functions that have access to variables from an outer function even after the outer function has finished executing. They “remember” the environment in which they were created  
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
> 

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

### 6- Multiple Api parallel calls in javascript what is good approach //
If you need to process the results of successful calls even if some fail, or require specific error handling for each individual call, you can use Promise.allSettled() instead of **Promise.all()**. **Promise.allSettled()** returns a Promise that resolves after all the given Promises have either fulfilled or rejected, providing an array of objects describing the outcome of each Promise.  
**Promise.allSettled() (Wait for All Outcomes):**   
In summary:  
**Promise.all():** All or nothing; rejects on first failure.  
**Promise.allSettled():** Always resolves with the status of every promise, regardless of success or failure.  
```
const fetchData = async ()=>{
    const URL1 = 'https://jsonplaceholder.typicode.com/users';
    const URL2 = 'https://jsonplaceholder.typicode.com/todos';
    const URL3 = 'https://jsonplaceholder.typicode.com/albums';
    const apiUrls = [URL1,URL2, URL3];
    try {
      const res = await Promise.all(
        apiUrls.map(url => fetch(url).then((res) => {
           console.log(res.status)
           return res.json()
          })
        )
      );
      console.log(res)
    } catch (error){     
    }     
  }
```
```
const fetchDataWithAllsettled = async ()=>{
    const URL1 = 'https://jsonplaceholder.typicode.com/users';
    const URL2 = 'https://jsonplaceholder.typicode.com/todosdd';
    const URL3 = 'https://jsonplaceholder.typicode.com/albumsd';
    const apiUrls = [URL1,URL2, URL3];
    try {
      const result = await Promise.allSettled(apiUrls.map(url=>
         fetch(url).then((res)=> {
          if(res.ok){
             return res.json()
          } else {
            throw new Error(res.status)
          }
         
        })
      ))
      result.map((result,ind)=>{
        if(result.status ==='fulfilled'){
          console.log(ind, result.value)
        } else {
          console.log(ind, result.reason.message)
        }
      })
     
    } catch (error){     
      console.log(error)
    }     
  }
```

### 7- What Is Implicit ###  
Implicit Type Coercion (or Implicit Type Conversion): This is the automatic conversion of one data type to another by the JavaScript engine during an operation. This often happens when operators are used with operands of different data types.
```
**Example**:  
let result = "5" + 2; // The number 2 is implicitly converted to the string "2"  
console.log(result); // Output: "52" (string concatenation)  

let value = "10" - 5; // The string "10" is implicitly converted to the number 10  
console.log(value); // Output: 5 (numeric subtraction)  
```
### 8- What Is the NaN Property in JavaScript? ###  
The NaN property in JavaScript stands for “Not a Number,” and is used to represent a value that is not a legal number or is undefined. The isNaN() function is used to check whether a value is NaN, and first converts the given value into a Number type if necessary.  

### 9- Pass by Value and Pass by Reference in JavaScript ###  
In JavaScript, primitive data types are passed by value and non-primitive data types are passed by reference.  

### 10- IIFE ###  
Immediately Invoked Function Expression (IIFE) is a function that runs as soon as it is defined. An IIFE involves having a function expression enclosed in parenthesis and then immediately calling the function expression.  
```
Example:
(function() {
  var message = "Hello from inside the IIFE!";
  console.log(message); // Output: Hello from inside the IIFE!
})();
```
### 11-  What Is “strict” mode in JavaScript and How Is It Enabled? ###  
Strict mode in JavaScript is a feature in ECMAScript 5 that allows you to write code or a function in a “strict” operational environment. In strict mode, certain code actions are prevented (such as not being allowed to create global variables or use duplicate arguments) and more errors/exceptions will be thrown, making debugging code easier to handle. Strict mode in JavaScript can be enabled by adding the "use strict" statement at the top of code or a function.   

### 12- What Is the Purpose of the bind() Method in JavaScript? ###  
The bind() method is used to create a new function with a specified this value and an initial set of arguments. It allows you to set the context of a function permanently.  
```
const person = {
   name:"Ajay",
   greet: function(){
   console.log("Hi " + this.name)
   }
}
const greeting = person.greet
const boundFn = greeting.bind(person)
```
### 13-  What Is Recursion in JavaScript? ###  
Recursion is a programming technique that allows a function to call itself. Recursion can be used to solve a variety of problems,   

### 14- Explain the Concept of Prototypes in JavaScript. ###
Prototypes are a mechanism used by JavaScript objects for inheritance. Every JavaScript object has a prototype, which provides properties and methods that can be accessed by that object.  
```
function person(name){
   this.name = name
}
person.prototype.greet = function(){
   console.log(this.name)
}
const personName = new person('Ajay')
person.greet()
```

### 15- Explain WeakMap and WeakSet in JavaScript ###
WeakMap in JavaScript allows you to store **key-value pairs** where the keys must be **objects**, and the references to these keys are "weak." This means that if an object used as a key in a WeakMap is no longer referenced elsewhere in your code, it can be garbage-collected, and the corresponding entry in the WeakMap will also be automatically removed.  
```
// Create some objects to use as keys
let user1 = { id: 1, name: "Alice" };
let user2 = { id: 2, name: "Bob" };

// Set values associated with the object keys
weakMap.set(user1, "Admin");
weakMap.set(user2, "Editor");

// Retrieve values
console.log(weakMap.get(user1)); // Output: Admin
console.log(weakMap.get(user2)); // Output: Editor
```
**Weekset**
A JavaScript WeakSet stores a collection of unique objects, similar to a Set, but with a key difference: it holds weak references to its elements. This means that if an object stored in a WeakSet is no longer referenced anywhere else in the code, it can be garbage-collected, and automatically removed from the WeakSet.  
```
// Create a new WeakSet
const myWeakSet = new WeakSet();

// Create some objects
let obj1 = { id: 1, name: "Object One" };
let obj2 = { id: 2, name: "Object Two" };
const obj3 = { id: 3, name: "Object Three" }; // obj3 will always be referenced

// Add objects to the WeakSet
myWeakSet.add(obj1);
myWeakSet.add(obj2);
myWeakSet.add(obj3);
```
### 16- What is Async and defer ###
**Async**  
When we include a script with the **async attribute**, it tells the browser to download the script asynchronously while parsing the HTML document. The script is downloaded in the background without blocking the HTML parsing process.  
Once the script is downloaded, it's executed asynchronously, meaning it can run at any time, even before the HTML document has finished parsing.  
**Defer**  
When we include a script with the defer attribute, it also tells the browser to download the script asynchronously while parsing the HTML document.  
The script's execution is deferred until the HTML document has been fully parsed and the DOM is ready  
**When to Use Which:**  
- Use async for scripts that are self-contained and don't depend on the DOM or other scripts for their functionality.  
 - Use defer for scripts that need access to the fully parsed DOM or have dependencies on other scripts and require a specific execution order.

### 17 - difference between asynchronous and synchronous ###
**Synchronous** : Each operation waits for the previous one to complete before starting. This can lead to delays and reduced responsiveness.  
Tasks are executed one after the other  

**Asynchronous** : Operations can start and run independently. The system can continue with other tasks while waiting for the asynchronous operation to complete.  
Allows you to perform operations like fetching data from a server without blocking the rest of your code.


