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
Callback is a function that passed as an arguments to another function;
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





