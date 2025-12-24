# Minggu 4: Promises & Async/Await

## 📚 Pengantar

Minggu ini adalah **turning point** dalam pembelajaran JavaScript! Asynchronous JavaScript adalah konsep yang SANGAT penting untuk Cypress karena **hampir semua Cypress commands adalah asynchronous**. Memahami konsep ini akan membuat Anda mengerti kenapa Cypress bekerja dengan cara tertentu.

---

## 1️⃣ Synchronous vs Asynchronous

### Synchronous Code (Blocking)

```javascript
console.log('First');
console.log('Second');
console.log('Third');

// Output (berurutan):
// First
// Second
// Third
```

Code dijalankan **satu per satu**, menunggu setiap line selesai sebelum lanjut ke line berikutnya.

---

### Asynchronous Code (Non-blocking)

```javascript
console.log('First');

setTimeout(() => {
  console.log('Second (delayed)');
}, 2000);

console.log('Third');

// Output:
// First
// Third
// Second (delayed) <- Muncul setelah 2 detik
```

Code tidak menunggu operation selesai, melanjutkan ke line berikutnya.

**Kenapa perlu Async?**
- Network requests (API calls)
- File operations
- Database queries
- Timers
- User interactions

---

## 2️⃣ Callbacks

Callback adalah function yang dipanggil setelah async operation selesai.

```javascript
// Callback example
function fetchData(callback) {
  setTimeout(() => {
    const data = { name: 'John', age: 30 };
    callback(data);
  }, 1000);
}

fetchData((result) => {
  console.log('Data received:', result);
});
// Output (after 1 second): Data received: { name: 'John', age: 30 }
```

**Callback Hell (Problem):**
```javascript
// Nested callbacks - hard to read!
getData((data) => {
  processData(data, (processed) => {
    saveData(processed, (saved) => {
      sendNotification(saved, (sent) => {
        console.log('All done!');
      });
    });
  });
});
```

---

## 3️⃣ Promises

Promises adalah solusi modern untuk async operations. Promise merepresentasikan nilai yang mungkin tersedia **sekarang**, **nanti**, atau **tidak sama sekali**.

### Promise States

1. **Pending** - Initial state, belum selesai
2. **Fulfilled** - Operation berhasil
3. **Rejected** - Operation gagal

### Creating Promises

```javascript
const myPromise = new Promise((resolve, reject) => {
  // Async operation
  const success = true;
  
  setTimeout(() => {
    if (success) {
      resolve('Operation successful!');
    } else {
      reject('Operation failed!');
    }
  }, 1000);
});

// Using the promise
myPromise
  .then((result) => {
    console.log(result); // 'Operation successful!'
  })
  .catch((error) => {
    console.error(error);
  });
```

---

### Promise Methods: .then(), .catch(), .finally()

```javascript
fetch('https://api.example.com/users')
  .then((response) => {
    console.log('Response received');
    return response.json(); // Returns another promise
  })
  .then((data) => {
    console.log('Data:', data);
    return data.users;
  })
  .then((users) => {
    console.log('Users:', users);
  })
  .catch((error) => {
    console.error('Error:', error);
  })
  .finally(() => {
    console.log('Operation complete (success or fail)');
  });
```

**Promise Chaining:**
```javascript
const promise = new Promise((resolve) => {
  resolve(5);
});

promise
  .then((num) => {
    console.log(num); // 5
    return num * 2;
  })
  .then((num) => {
    console.log(num); // 10
    return num + 3;
  })
  .then((num) => {
    console.log(num); // 13
  });
```

---

### Promise.all() - Run Multiple Promises in Parallel

```javascript
const promise1 = Promise.resolve(3);
const promise2 = new Promise((resolve) => setTimeout(() => resolve('foo'), 100));
const promise3 = fetch('https://api.example.com/data');

Promise.all([promise1, promise2, promise3])
  .then((values) => {
    console.log(values); // [3, 'foo', Response]
  })
  .catch((error) => {
    console.error('One of the promises failed:', error);
  });
```

---

## 4️⃣ Async/Await (Modern Syntax)

Async/await membuat async code terlihat seperti synchronous code.

### Basic Syntax

```javascript
// Function with async keyword
async function fetchUser() {
  // await pauses execution until promise resolves
  const response = await fetch('https://api.example.com/user/1');
  const data = await response.json();
  return data;
}

// Calling async function
fetchUser()
  .then((user) => {
    console.log(user);
  })
  .catch((error) => {
    console.error(error);
  });
```

**Comparison:**
```javascript
// Using .then()
function getUser() {
  return fetch('https://api.example.com/user/1')
    .then(response => response.json())
    .then(data => {
      console.log(data);
      return data;
    });
}

// Using async/await (cleaner!)
async function getUser() {
  const response = await fetch('https://api.example.com/user/1');
  const data = await response.json();
  console.log(data);
  return data;
}
```

---

### Error Handling with Try/Catch

```javascript
async function fetchUserData(userId) {
  try {
    const response = await fetch(`https://api.example.com/users/${userId}`);
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Failed to fetch user:', error);
    throw error; // Re-throw if needed
  } finally {
    console.log('Fetch attempt completed');
  }
}

// Usage
fetchUserData(1)
  .then(user => console.log(user))
  .catch(error => console.error('Error in main:', error));
```

---

### Multiple Async Operations

```javascript
async function getAllData() {
  try {
    // Sequential (one after another)
    const users = await fetch('/api/users').then(r => r.json());
    const posts = await fetch('/api/posts').then(r => r.json());
    const comments = await fetch('/api/comments').then(r => r.json());
    
    return { users, posts, comments };
  } catch (error) {
    console.error('Error fetching data:', error);
  }
}

// Parallel (all at once - faster!)
async function getAllDataParallel() {
  try {
    const [users, posts, comments] = await Promise.all([
      fetch('/api/users').then(r => r.json()),
      fetch('/api/posts').then(r => r.json()),
      fetch('/api/comments').then(r => r.json())
    ]);
    
    return { users, posts, comments };
  } catch (error) {
    console.error('Error fetching data:', error);
  }
}
```

---

## 5️⃣ Cypress dan Asynchronous Code

### Kenapa Cypress Commands Async?

```javascript
// ❌ WRONG - This won't work!
const button = cy.get('button'); // Returns Cypress chainable, NOT element
button.click(); // Error!

// ✅ CORRECT - Chain commands
cy.get('button').click();
```

**Cypress commands return "Chainables", bukan values langsung.**

---

### cy.then() - Working with Yielded Values

```javascript
describe('cy.then() Examples', () => {
  it('should work with yielded values', () => {
    cy.get('.username').then(($element) => {
      // $element is the actual DOM element (jQuery object)
      const text = $element.text();
      cy.log(`Username: ${text}`);
      
      expect(text).to.not.be.empty;
    });
  });
  
  it('should chain multiple then()', () => {
    cy.get('.price')
      .invoke('text')
      .then((priceText) => {
        const price = parseFloat(priceText.replace('$', ''));
        return price * 2; // Return value for next then()
      })
      .then((doubledPrice) => {
        cy.log(`Doubled price: $${doubledPrice}`);
        expect(doubledPrice).to.be.greaterThan(0);
      });
  });
});
```

---

### cy.wrap() - Wrapping Values

```javascript
describe('cy.wrap() Examples', () => {
  it('should wrap values for Cypress chain', () => {
    const user = {
      name: 'John Doe',
      email: 'john@example.com'
    };
    
    // Wrap object to use Cypress assertions
    cy.wrap(user)
      .should('have.property', 'name')
      .and('equal', 'John Doe');
    
    cy.wrap(user.email)
      .should('include', '@');
  });
  
  it('should wrap promises', () => {
    const myPromise = new Promise((resolve) => {
      setTimeout(() => resolve('Hello!'), 1000);
    });
    
    cy.wrap(myPromise).should('equal', 'Hello!');
  });
});
```

---

### cy.request() - API Testing

```javascript
describe('API Testing with cy.request()', () => {
  it('should make GET request', () => {
    cy.request('GET', 'https://jsonplaceholder.typicode.com/users/1')
      .then((response) => {
        expect(response.status).to.equal(200);
        expect(response.body).to.have.property('name');
        expect(response.body).to.have.property('email');
        
        const { name, email, address } = response.body;
        cy.log(`User: ${name}, Email: ${email}`);
      });
  });
  
  it('should make POST request', () => {
    const newUser = {
      name: 'Test User',
      email: 'test@example.com',
      age: 25
    };
    
    cy.request('POST', 'https://jsonplaceholder.typicode.com/users', newUser)
      .then((response) => {
        expect(response.status).to.equal(201);
        expect(response.body).to.include(newUser);
        expect(response.body).to.have.property('id');
      });
  });
  
  it('should handle request headers', () => {
    cy.request({
      method: 'GET',
      url: '/api/protected',
      headers: {
        'Authorization': 'Bearer token123',
        'Content-Type': 'application/json'
      }
    }).then((response) => {
      expect(response.status).to.equal(200);
    });
  });
});
```

---

## 🎯 Latihan Praktis

### Latihan 1: Promise Basics

```javascript
describe('Practice: Promises', () => {
  it('should create and use a promise', () => {
    const fetchData = () => {
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve({ user: 'John', age: 30 });
        }, 1000);
      });
    };
    
    cy.wrap(fetchData()).then((data) => {
      expect(data.user).to.equal('John');
      expect(data.age).to.equal(30);
    });
  });
});
```

### Latihan 2: Async/Await

```javascript
describe('Practice: Async/Await', () => {
  it('should use async/await pattern', () => {
    const getUserData = async () => {
      const response = await fetch('https://jsonplaceholder.typicode.com/users/1');
      const data = await response.json();
      return data;
    };
    
    cy.wrap(getUserData()).then((user) => {
      expect(user).to.have.property('name');
      expect(user).to.have.property('email');
    });
  });
});
```

---

## 🎓 Ringkasan Minggu 4

### Yang Sudah Dipelajari:

✅ **Async Fundamentals:**
- Synchronous vs Asynchronous
- Callbacks dan callback hell
- Promises (pending, fulfilled, rejected)
- .then(), .catch(), .finally()
- async/await syntax
- Error handling dengan try/catch

✅ **Cypress Async:**
- cy.then() untuk yielded values
- cy.wrap() untuk wrapping values/promises
- cy.request() untuk API testing

---

### Key Takeaways:

1. **Cypress commands are asynchronous** - Always chain them
2. **Use cy.then()** untuk access yielded values
3. **Promises** membuat async code lebih manageable
4. **async/await** membuat code lebih readable

---

### Checklist Penguasaan:

- [ ] Memahami perbedaan sync vs async
- [ ] Bekerja dengan Promises
- [ ] Menggunakan async/await
- [ ] Menguasai cy.then() dan cy.wrap()
- [ ] Implementasi API testing dengan cy.request()

**Next Week:** Minggu 5 - Advanced Async Patterns! 🚀
