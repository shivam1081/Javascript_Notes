## 1. Callback Hell
**Definition:** 
- When one callback function is calling another callback, it is called callback hell or pyramid of doom.

**Example** 

Certainly! Here's the same example using the provided code to demonstrate callback hell:

```javascript
// Simulating asynchronous operations using setTimeout
function createOrder(cart, callback) {
  setTimeout(() => {
    console.log('Order created');
    callback({ orderId: 123 });
  }, 1000);
}

function proceedToPayment(orderId, callback) {
  setTimeout(() => {
    console.log('Payment processed for order:', orderId);
    callback({ paymentId: 456 });
  }, 1000);
}

function showOrderSummary(paymentId, callback) {
  setTimeout(() => {
    console.log('Order summary for payment:', paymentId);
    callback({ summary: 'Order summary details' });
  }, 1000);
}

function updateWallet(summary, callback) {
  setTimeout(() => {
    console.log('Wallet updated with:', summary);
    callback({ wallet: 'Wallet updated successfully' });
  }, 1000);
}

// Nested callbacks - callback hell
createOrder(['shoes', 't-shirts', 'pants'], function(order) {
  proceedToPayment(order.orderId, function(payment) {
    showOrderSummary(payment.paymentId, function(summary) {
      updateWallet(summary.summary, function(walletUpdate) {
        console.log(walletUpdate.wallet);
      });
    });
  });
});
```

#### Explanation:

1. **createOrder**: Creates an order and returns an order ID.
2. **proceedToPayment**: Processes payment for the order and returns a payment ID.
3. **showOrderSummary**: Shows the order summary based on the payment ID.
4. **updateWallet**: Updates the wallet with the order summary.

Each function's callback is nested within the previous function's callback, creating a pyramid-like structure and demonstrating "callback hell".

## 2. Inversion of Control
- Occurs when we hand over control of our code to another function or library, leading to loss of control over when and how functions are executed.

## 3. Promises
- **Purpose:** Used for handling asynchronous operations in JavaScript.

### Promises Example:
**Callback Approach:**
```javascript
const orders = ['shoes', 't-shirts', 'pants'];
createOrder(orders, function (orderId) {
  proceedToPayment(orderId);
});
// Returns orderId but we are blindly trusting that it will call or not call it twice.
```

**Promise Approach:**
```javascript
const promise = createOrder(cart);
// This is an async operation, and JavaScript will create an empty object like {default: undefined}
// After some time, this object will be filled with data automatically.
promise.then(function(orderId) {
  proceedToPayment(orderId);
});
// In the earlier method, we were blindly passing the callback, but now we attach the method with the object.
```

### API Example:
```javascript
const GITHUB_API = "https://api.github.com/users/shivam1081";
const user = fetch(GITHUB_API);
console.log(user); // Logs the promise object

user.then(function(data) {
  console.log(data); // Logs the data once the promise is resolved
});
```

**Note:** The promise object initially shows "pending", but the Promise State will show "fulfilled" once resolved.

### Promise States:
1. **Pending**
2. **Fulfilled**
3. **Rejected**

### Key Points:
- Promise objects are immutable and cannot be edited.
- JavaScript guarantees that a promise will be resolved once.
- Use **Promise Chaining** to avoid callback hell by returning a promise from a promise and passing data through the chain.

```javascript
createOrder(cart)
  .then(function(orderId) {
    return proceedToPayment(orderId);
  })
  .then(function(paymentInfo) {
    // Further processing
  });
```

### Interview Questions:
1. **What is a promise?**
   - A placeholder for a certain period until we receive information from an async operation.
   - A container for a future value.
   - An object representing the eventual completion or failure of an asynchronous operation.

### How to Create Our Own Promise:
```javascript
function createOrder(cart) {
  return new Promise(function(resolve, reject) {
    // async operation to create order
    if (orderCreatedSuccessfully) {
      resolve(orderId);
    } else {
      reject('Order creation failed');
    }
  });
}
```
## 4. Creating a Promise, Chaining and Error Handling 
```javascript
const items = ['pants', 'shirts', 'shoes'];
createOrder()
.then(orderID => {
    console.log(orderID);
    return orderID;
})
.then(orderID => {
    return proceedToPayment(orderID);
})
.then(function (paymentInfo) {
    console.log(paymentInfo);
})
.catch(function (err) {
    console.log(err);
});

// Create my own promise
function createOrder() {
    const pr = new Promise(function (resolve, reject) {
        // createOrder
        // validateCart
        // orderId
        const validateCard = true;
        if (!validateCard) {
            const err = new Error('Cart not valid');
            reject(err);
        }
        const orderId = '12345';
        if (orderId) {
            resolve(orderId);
        }
    });
    return pr;
}

function proceedToPayment(orderID) {
    return new Promise(function (resolve, reject) {
        resolve('Payment successful');
    });
}

```
### Imp Points :- 
1. Here Promise is constructor and it takes a function with two params reject resolve that is provided by Javascript only.
2. If we create a promise then if we want to reject the promise then use reject() else use resolve().
3. We can resolve the promise only once and reject multiple times.
4. Remember we always attach a function to the promise object.

### Promise Chaining 

1. Remember we have return things to pass down the chain in case of promise chaining.
2. We can resolve the promise there only but it handled in the next level of the chain to avoid promise hell or pyramid of Dome.
3. If there is any error in the cain then the catch will handle that error at wherever level we are getting the error.
4. We can put the catch the statement for specific try. If there are then after catch then they will definitely execute.

Here's the properly formatted version of your notes:

---

## 5. Async Await

### Basic Usage

```javascript
// Returning a common value
async function getData() {
  return 'Namaste';
}

const dataPromise = getData();
dataPromise.then(res => console.log(res));

// Returning a Promise (Example 2)
const p = new Promise((resolve, reject) => {
  resolve('Promise Resolved Value !!');
});

async function getData() {
  return p;
}

const dataPromise = getData();
dataPromise.then(res => console.log(res));
```

1. An `async` function **always returns a promise**.
2. If you return a promise, the `async` function will not wrap it into another promise but return the same promise (as shown in Example 2). If a different value is returned, it will be wrapped inside a promise.

### Using Await with Async

1. The `await` keyword is used to handle promises and can only be used inside an `async` function.
2. Before `async/await`, promises were handled like this:

```javascript
const p = new Promise((resolve, reject) => {
  resolve('Promise Resolved Value !!');
});

function getData() {
  p.then(res => console.log(res));
}

getData();
```

3. With `async/await`, the handling of promises becomes simpler:

```javascript
const p = new Promise((resolve, reject) => {
  resolve('Promise Resolved Value !!');
});

async function handlePromise() {
  const val = await p;
  console.log(val);
}

handlePromise();
```

4. By placing `await` in front of a promise, it resolves the promise and waits for it to complete before proceeding.

### Difference Between Normal Promise Handling and Async/Await

```javascript
// Example 1
const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Promise Resolved Value !!");
  }, 5000);
});

// Example 2
async function handlePromise() {
  console.log("Before Promise");
  const val = await p;
  console.log("Namaste Javascript");
  console.log(val);
}

handlePromise();
```

```javascript
// Example 3
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Promise Resolved Value !!");
  }, 10000);
});

async function handlePromise() {
  console.log("Before Promise");
  const val = await p;
  console.log("Namaste Javascript");
  console.log(val);
  const val2 = await p1;
  console.log("Namaste Javascript");
  console.log(val2);
}

handlePromise();
```

```javascript
// Example 4
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Promise Resolved Value1 !!");
  }, 10000);
});

const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Promise Resolved Value2 !!");
  }, 5000);
});

async function handlePromise() {
  console.log("Before Promise");
  const val = await p1;
  console.log("Namaste Javascript1");
  console.log(val);
  const val2 = await p2;
  console.log("Namaste Javascript2");
  console.log(val2);
}

handlePromise();
```

1. In the above examples, JavaScript appears to wait for promises to be resolved:
   - **Example 2**: All promises are resolved after 10 seconds.
   - **Example 3**: If there are two promises with different timeouts, the first one will resolve first.
   - **Example 4**: The promise with the shorter delay (5 seconds) will resolve and print after the longer delay (10 seconds) promise.
2. `async/await` waits for promises written earlier to resolve before executing subsequent lines of code.

### What Happens Behind the Scenes

1. When `handlePromise` is called, JavaScript sees that there are promises to be resolved.
2. `handlePromise` will go into the call stack. When it encounters `await`, it suspends `handlePromise` without blocking the main thread.
3. After the promise resolves (e.g., after 5 seconds), `handlePromise` resumes execution from where it was suspended.

### Real World Example of Async/Await

```javascript
async function handlePromise() {
  const data = await fetch("https://api.github.com/users/shivam1081");
  const jsonValue = await data.json();
  console.log(jsonValue);
  // Fetch returns a promise. The response is a readable stream, and Response.json() is a promise that gives the JSON value.
}

handlePromise();
```

### Error Handling

Error handling in async functions is done using `try` and `catch`. Wrap the code inside a `try` block and handle errors in the `catch` block:

```javascript
async function handlePromise() {
  try {
    const data = await fetch("https://api.github.com/users/shivam1081");
    const jsonValue = await data.json();
    console.log(jsonValue);
  } catch (error) {
    console.log(error);
  }
}

handlePromise();
```

Alternatively, error handling can be done using `.catch()`:

```javascript
handlePromise().catch((err) => console.log(err));
```

### Interview Questions

**Q1. What is `async`/`await`?**

- Explain the `async` and `await` keywords, and provide examples to illustrate their usage.

### When to Use What

- `async`/`await` is syntactic sugar over `.then`/`.catch`.
- It provides a new way of writing code and can avoid promise chaining.



