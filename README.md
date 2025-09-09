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

//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
### What was the output ###
```diff
var name = "Lokesh Prajapati";
(function() {
  console.log(name);
  var name = "Lokesh Prajapati";
})(); // Output: undefined;
+the var name declaration inside the function is hoisted to the top of the function scope.
```
```
var x = 1;
if (function test() {}) {
  x += typeof test;
}
console.log(x); // Output: 1undefined;
```
```diff
function sayHelloWorld() {
  return sayGoodbyWorld();
  var sayGoodbyWorld = function() {
    return "Hello, World!";
  };
  function sayGoodbyWorld() {
    return "Goodbye, World!";
  }
}
+console.log(sayHelloWorld()); //Goodbye, World!
```
```
function Parent() {}
function Child() {}
Child.prototype = new Parent();
var obj = new Child();
console.log(obj instanceof Parent); // Output: true;
```
```
var x = 10;
function testValue() {
  console.log(x);
  var x = 20;
}
testValue(); // Output: undefined;
```
```
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  return "Hello, my name is " + this.name;
};

var person1 = new Person("Lokesh Prajapati");
var person2 = new Person("Lucky");

console.log(person1.greet === person2.greet); // Output: true;
```
```
function sayHi() {
  return hi;
  var hi = "Hello, World!";
}
console.log(sayHi()); // Output: undefined.
```
```
var x = 5;
function outer() {
  var x = 10;
  function inner() {
    console.log(x);
  }
  return inner;
}
var finalResult = outer();
finalResult(); //10
```
```
var x = 10;
function testNum() {
  console.log(x);
  if (true) {
    var x = 20;
  }
  console.log(x);
}
testNum(); // 20
```

### 14 Create a div structre by vanila javascript ###
<a href="https://flems.io/#0=N4IgtglgJlA2CmIBcAWA7AOgBwEYA0IAzgMYBOA9rLMgNoAMedAugQGYQKG2gB2AhmERIQGABYAXMNQLFyPcfHnIQAHigQAbgAJoAXgA6IAO6k+ABzPxShgHz6eWrfZUB6dRpsgCheAmLiIOS5hOiQATgBaUJwANhAAXzxeASERACsuGTkFJWFWAFcef0CHMng+BQARcjAAZXFSLQAKAEpge0ctBHEtKAq+LV0tGg7O4FFy9R4AcyRDABV4Qh6ACUmIGZxDPF6luZAFQg3pra8tACN88XE5RYAPcX2AYQniAGstAHlWVitDRNGjnG6xm+0Wyy0az4U2mACZtrtCPtDsccE4zpdrrd4A9nq8Pk8+LBiPlYBUSv88ICtMDoccwUtViDpgBmBFQPaGFGbWFsjFXG48e6PQy1MB8Ug9b6-awJKkOTo0iZ00ELRmQ5kodmcg5LVG8hGYwXC-ZiiVSn5-OXUoHKmH7KEwgCs2qRXL1PL5OyN2NxovFkq+ltl8WpTGp3V6mgAklBBr1yCTBPIMNN4OIAKIIZPiABCAE9Y01DCZzJZZS0ANwR9NaCRSYjxwwqMw2LQAGXIpHgYC00bMhHyveqCEaRx6E27OxUywoMxsT1gEHeGu7rlncmmdhA1L64j4GFYXYzfGIoiaTV8YBauhs7QVisjPmKcnjUETQ8U4gwZQq8CzPZfsWIDuIYVaKo4NpdLWoiwm+H45j+3Z-gBObAbBYHVg+nSRmY8FJl+SHlAoqFAYYZiYVBkY+g4QzvgRKa-iR2ZkSANGUdhjjPgEcg-mShCEO2EDLBg0JQMBhziGBUHcSUokWIoUAvBw4mwS0MnwC+PDyZYPBKaIKlNGY6mcVosm8WWinKbA4k0SZEF1rCGAbDwVjCvGV5iMyWguC4WhiVoCgPD5LhQWYGBBeITzZF+HmwGAGAcoQIX+TAgU4j0vlQVBNHOTwrmkO5QyeTRwqVpBpm5cs+YID+lBdk2ICkNM5xNAwWjtXQ6kgFhDlVeINXwBg5ynm80wUIUcZDIYzWtbCTpOjsnXdTlAq8dVtXnF2HKNNNIB0GYdyGL1EHuLGOlWQZNlNOZPBVlBECsJe8VeSq0yDHt4JMm98IgG0UE4bWZhonRCGEUx-4sfIwEUX9J0OWZmk8dpll6dZ4nA-ZCNaMDeUFSs8wALLtvG9bElBobYfELRONhkZ3Phn4pgAjvkVj5rUvhI12ACCVDAexcOjNSdyHsep7nk0fA3neUEHmJGYaF+QnLIoVjAcQS7vAirS3veCMHhtQ2yLADVDIbA21SbZt7bNbVLUt3VaAA-OiADEPx0F7dCGFoSDu973vHQD-kYEbw2jeN5CTfGFuDRH7xRzHui2y19sdY7vuu4YHusIHvv+zngde4YFNY441PUrIPCEJQQ2m9MrTCzwlP2AURTI1o0cKKQrT69BPSyIUPRDHQJ3t1pOj5er-0PkP8gANQL4C3biPkpClNH8ijJTjir+vDguX8Lf2JGAxDN3M9YdXte1Q3UutCZN91xg998I-9jP3f5CN+-LQmZ-ZCVQaj1F7gAngXgiBcy0sEfaSA6ARBiKEOgcpkiCGUD+ASkDq45HEMoN2pYFKNH7kYaA4hRBIBwF7AApCdCYEBpgSCQHwK45ATpbVIDteBh0zKUGgFoXOgcTrqEIGYMk+YkCsAQHcce0iIiEP9oQk6ZgxL0gOjI0Y0xzCUPUSdSKEQiQMJ4EgYgX4rBYUpu4CKjJ+70MYY8FhNx2HbSsEgJ0PDb78LdjAKAyjVGgioYdWROJ-Z0A6loYgRJiBNBZCyDAsTYnUK0BELQgS7j3QVFtO4EQjgAC96QcJ2hELJ5VaaUxov3EkpBa6kCQGYcgGwe4WPsAAAUEOoAYTRIA8HkWQihHVDo0z4HpZo4psmkKgOQ-2TovaDJpKMKxkl5kPikSE8JYTInEiaFQugSSUlpIyRXewlM2nwA6aMjYvTJn9JmTgOZwy4xdL4OMvp-s0CwnUTTfuiz1T9y0Ksu4oTwmbOiTMvZqTPknUpvESBPg-DI1gbCFAlCEhMHiEAA">click</a>
```diff
function createDomStr (){
let data = [
{heading:"Test Heading1", des:"tesing1", buttonText:"Check Offer"},
{heading:"Test Heading2", des:"tesing1 ", buttonText:"Check Calculation"},
{heading:"Test Heading3", des:"tesing123", buttonText:"Smart Offer"},
{heading:"Test Heading4", des:"tesing123", buttonText:"Smart Offer"},
{heading:"Heading5", des:"tesing123", buttonText:"Smart Offer"}
]
let divId = document.getElementById("wrapper");
let htmlc = "<p> Lorem Ipsum Doler sit here, <strong>Click Here</strong>"
data.forEach((elm)=>{
let section = document.createElement("div");      
+let h2 = document.createElement("h2");
let p = document.createElement("p");
let button = document.createElement("button");
section.classList.add("test")
section.appendChild(h2)
section.appendChild(p)
section.appendChild(button)
+h2.innerText = elm.heading // add text //
+p.textContent = elm.des // add text //

button.innerText = elm.buttonText;  
+button.style.color = "rgb(0, 0, 0)";
button.style.background = "rgb(255, 0, 0)"
button.style.border = "0px";
divId.appendChild(section);
if(elm.heading == "Test Heading2"){
let p1 = document.createElement("p");
section.appendChild(p1)
+p1.innerHTML = htmlc
}
})
/////////////
let x = document.querySelectorAll("button");
x.forEach((a)=>{
+a.addEventListener("click", ()=>{
a.style.color = a.style.color == "rgb(0, 0, 0)" ? "#ff0000" : "#000000";
a.style.background = a.style.background == "rgb(0, 0, 0)" ? "#ff0000" : "#000000"
})
///////////
})
}


function outer(){
let count = 0;
function inner(){
count++
return count
}
return inner
}
let a = outer();
+console.log(a())
console.log(a())
console.log(a())

createDomStr()
```
### promise with timer return array[1,2,3] ###
```diff
const pr = function(){
    return new Promise((resolve)=>{
      let arr = [];
      let countVar = 1
      let timer = setInterval(() => {
        arr.push(countVar)
        countVar++     
        //console.log(arr)   
        if(arr.length === 3){
         clearInterval(timer);
         resolve(arr)
      }  
      }, 1000);      
    })
     
  }
  const a = pr().then((res)=>{
    console.log(res) /// [1,2,3]
  }
```
### 15  lighthouse chrome extension ###
Lighthouse is a free, open-source, automated tool by Google for improving the quality of web pages. It's available as a Chrome extension and within Chrome DevTools, and can also be used as a Node module. Lighthouse audits web pages for performance, accessibility, SEO, and other best practices, providing detailed reports with suggestions for improvement. 

### 15 debouncing and throttling in javascript ###
timer is reset after every 1 second  
<a href ="https://flems.io/#0=N4IgZglgNgpgziAXAbVAOwIYFsZJAOgAsAXLKEAGhAGMB7NYmBvEAXwvW10QICsEqdBk2J4wAVzTViEegAIAJjABGtSdRgAKMGgqKYUDAE8AlMAA6aOXNjE5MnBAUBuS9YBOMYuPdWJUmXlNfBCMdwBzODM3azlqWDCAFQgcNWJNBxgnE1crWMynOQBeOTgvZNTxdM0TIoA+CzzYuR1g0IiomOt2JUNTZpjWSyG0SyE4OzKw6nCARXEYdyNiuU0ANwwoBdq6uUbrcdpYfChacPXN7eHLMfoJ0phpwgB3CGJCABFVdRgVpW+pFopu4ZvNFkY9ABGAAMsJytzQcCOMBOZ00wOoLzenwBGk05hAhCcChJCgJJhMCKRx1O5wxWPeXzUgPxhOJpPJlKkd2RqLpjxBDJxzLxBKJpJJnKpvNp6IFmNejNxWjF7MlIAplBAZVg0lkiLw0MQ0IAtJCAByIAAskLYHBAmBweHw1DgAho9EYzB4bAAuqwgA">click</a>
```function debounce(fn, delay){
  let timeid;
  return function (...args){
    clearTimeout(timeid);
    timeid = setTimeout(()=>{
      fn(...args)
    },delay)    
  }
}
const searcgQuery = (value)=> {
  console.log(value)
}
const searchwithDbounce = debounce(searcgQuery, 1000);
console.log(searchwithDbounce("Hi"))
console.log(searchwithDbounce("Hello"))
```


<a href="https://flems.io/#0=N4Igxg9gdgzhA2BTEAucD4EMAONEBMQAaEAMwEskZUBtUKTAW2TQDoALAF0fmPSk6IBqECAC+RekxYhWAK2olIAoZxGkArlDCdy0AASd2AJwidOSABSkoRffkRYAngEp9wADpR9P-Uk5+mDCcACrkzPoAvPoADADcXr76xoicGsbemtq6Bpas+ZjGAOYwbp7eSb7++lAQAO5R+gAimIKstXWWLgkVlfrkpPqWHfoAtIHBYREAfNEOzmWJfZU2eQXFpT3LlViT4YiNHVvLYks+Yj2nUF7Kwfp4hWBFAIoaiMZOjZYAbpjwby5ItN3EtbghEKx4BAij8-gCvFcbtA7g9jGB2HVyEYmgAjCBaMAHaJGUzmKyop6vd5OIgARhiDO6SNg4Mh0MsFIxWPYuPx2kQlgA5Jg5JhPvggoNSIKXC4vF5MVB8PVWJh8PgAKLfVQAGXIwSE7yFKRg5AAXohBXYSWYLAKulFgeUkmCkGyYYKAEqIU0W-SIbUCQzGchFIrvAgAQhllyIAFYGTFZT0vHw8EgdHpYCIAEwxFAAZnEkhADGYIlYYBgin4gmEaHEAF0xEA">Click</a>

### OOJS core cocept ###
core concept - 
**Encapsulation:** Bundles data and methods that operate on that data within a single unit (an object) and hides the internal details from the outside world. 
**Inheritance:** Enables a new class (subclass or derived class) to inherit properties and  methods from an existing class (superclass or base class).  
**Polymorphism:** Means "many forms."  it allows objects of different classes to be treated as objects of a common type. This  
**Abstraction:** Simplifies complex reality by modeling classes based on essential properties relevant to the program. It hides unnecessary details and presents a simplified view of an object, making the code easier to understand and use.  

### 17- fatcorial of 5 ###
```
function fatctorialVal(n) {
  // Base case: The factorial of 0 is 1.
  if (n === 0) {
    return 1;
  } lse {
    return n * fatctorialVal(n - 1);
  }
}
console.log(fatctorialVal(5))
console.log(5*4*3*2*1, "d")
```

### 18- Recurson create a chart ###
```diff
const data = [{
  name: "CEO",
  children: [
    {
      name: "CTO",
      children: [
        {
          name: "Dev Manager",
          children: [{ name: "Developer 1" }, { name: "Developer 2" }],
        },
      ],
    },
    { name: "CFO" },
    {
      name: "COO",
      children: [{ name: "Operations Manager" }],
    },
  ],
},{
  name: "CEO1",
  children: []
}];

function Recurson(data, parentID){
  let parent = document.getElementById(parentID)
  data.forEach((item)=>{
     let div = document.createElement("div");    
     div.textContent = item.name
     div.setAttribute("id", item.name)
     parent.appendChild(div)
     if(item.children && item.children.length > 0){
        Recurson(item.children, item.name);
     } 
  })
}
Recurson(data, "menu") 

```
### 19- Call Apply and Bind ###  

<a href="https://onecompiler.com/javascript/43w7wgxxr" target="_blank"> Click Here </a>




