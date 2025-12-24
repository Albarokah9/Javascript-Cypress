# Minggu 2: Functions & Arrays

## 📚 Pengantar

Selamat datang di Minggu 2! Minggu ini kita akan mempelajari dua konsep fundamental yang akan membuat code Anda lebih **reusable**, **maintainable**, dan **powerful**: **Functions** dan **Array Methods**.

Functions adalah building blocks dari JavaScript yang memungkinkan Anda membuat code yang dapat digunakan kembali. Array methods akan membuat data manipulation menjadi jauh lebih mudah dan elegant.

---

## 1️⃣ Functions

### Apa itu Functions?

Function adalah blok code yang dapat digunakan kembali untuk melakukan tugas tertentu. Bayangkan seperti "mesin" yang menerima input, melakukan proses, dan menghasilkan output.

**Keuntungan menggunakan Functions:**
- ✅ **Reusability** - Tulis sekali, gunakan berkali-kali
- ✅ **Organization** - Code lebih terstruktur dan mudah dibaca
- ✅ **Maintainability** - Mudah untuk update dan debug
- ✅ **Testing** - Lebih mudah untuk test individual functions

---

### Function Declaration

```javascript
// Syntax
function functionName(parameter1, parameter2) {
  // Code to execute
  return result;
}

// Example
function greet(name) {
  return `Hello, ${name}!`;
}

// Calling the function
const message = greet('Alice');
console.log(message); // "Hello, Alice!"
```

**Contoh dengan Multiple Parameters:**
```javascript
function calculateTotal(price, quantity, taxRate) {
  const subtotal = price * quantity;
  const tax = subtotal * taxRate;
  const total = subtotal + tax;
  return total;
}

const orderTotal = calculateTotal(29.99, 3, 0.1);
console.log(orderTotal); // 98.967
```

---

### Function Expression

```javascript
// Function assigned to a variable
const greet = function(name) {
  return `Hello, ${name}!`;
};

const message = greet('Bob');
console.log(message); // "Hello, Bob!"
```

**Perbedaan dengan Function Declaration:**
- Function declarations di-"hoist" (bisa dipanggil sebelum dideklarasikan)
- Function expressions tidak di-hoist

```javascript
// ✅ Works - Function Declaration
sayHello(); // "Hello!"
function sayHello() {
  console.log('Hello!');
}

// ❌ Error - Function Expression
sayGoodbye(); // Error: sayGoodbye is not a function
const sayGoodbye = function() {
  console.log('Goodbye!');
};
```

---

### Arrow Functions (Modern ES6+)

Arrow functions adalah cara modern dan concise untuk menulis functions.

**Syntax:**
```javascript
// Traditional function
function add(a, b) {
  return a + b;
}

// Arrow function
const add = (a, b) => {
  return a + b;
};

// Arrow function (concise - implicit return)
const add = (a, b) => a + b;

// Single parameter (parentheses optional)
const square = x => x * x;

// No parameters
const sayHello = () => 'Hello!';
```

**Contoh Praktis:**
```javascript
// Multiple lines
const calculateDiscount = (price, discountPercent) => {
  const discount = price * (discountPercent / 100);
  const finalPrice = price - discount;
  return finalPrice;
};

// Single line (implicit return)
const double = num => num * 2;
const isAdult = age => age >= 18;
const getFullName = (first, last) => `${first} ${last}`;

console.log(double(5));              // 10
console.log(isAdult(20));            // true
console.log(getFullName('John', 'Doe')); // "John Doe"
```

---

### Parameters dan Arguments

**Default Parameters:**
```javascript
// Without default
function greet(name) {
  return `Hello, ${name}!`;
}
console.log(greet()); // "Hello, undefined!"

// With default parameter
function greet(name = 'Guest') {
  return `Hello, ${name}!`;
}
console.log(greet());        // "Hello, Guest!"
console.log(greet('Alice')); // "Hello, Alice!"
```

**Multiple Default Parameters:**
```javascript
const createUser = (username, role = 'user', isActive = true) => {
  return {
    username,
    role,
    isActive,
    createdAt: new Date()
  };
};

console.log(createUser('alice'));
// { username: 'alice', role: 'user', isActive: true, createdAt: ... }

console.log(createUser('bob', 'admin'));
// { username: 'bob', role: 'admin', isActive: true, createdAt: ... }

console.log(createUser('charlie', 'moderator', false));
// { username: 'charlie', role: 'moderator', isActive: false, createdAt: ... }
```

---

### Return Values

```javascript
// Function with return
function add(a, b) {
  return a + b;
  console.log('This will never execute'); // Code after return is ignored
}

const result = add(5, 3);
console.log(result); // 8

// Function without return (returns undefined)
function logMessage(message) {
  console.log(message);
  // No return statement
}

const result2 = logMessage('Hello');
console.log(result2); // undefined
```

**Early Return Pattern:**
```javascript
function validateAge(age) {
  // Early return for invalid input
  if (age < 0) {
    return 'Age cannot be negative';
  }
  
  if (age < 18) {
    return 'Minor';
  }
  
  if (age < 65) {
    return 'Adult';
  }
  
  return 'Senior';
}

console.log(validateAge(-5));  // "Age cannot be negative"
console.log(validateAge(15));  // "Minor"
console.log(validateAge(30));  // "Adult"
console.log(validateAge(70));  // "Senior"
```

---

### Functions dalam Cypress

**Custom Commands:**
```javascript
// cypress/support/commands.js

// Function untuk login
Cypress.Commands.add('login', (username, password) => {
  cy.visit('/login');
  cy.get('#username').type(username);
  cy.get('#password').type(password);
  cy.get('button[type="submit"]').click();
});

// Function dengan default parameters
Cypress.Commands.add('loginAsAdmin', (username = 'admin', password = 'admin123') => {
  cy.login(username, password);
  cy.url().should('include', '/admin');
});

// Function dengan return value (using cy.then)
Cypress.Commands.add('getAuthToken', () => {
  return cy.window().then((win) => {
    return win.localStorage.getItem('authToken');
  });
});
```

**Usage dalam Tests:**
```javascript
describe('Login Tests', () => {
  it('should login with custom credentials', () => {
    cy.login('testuser', 'testpass123');
    cy.url().should('include', '/dashboard');
  });
  
  it('should login as admin with defaults', () => {
    cy.loginAsAdmin();
    cy.get('.admin-panel').should('be.visible');
  });
  
  it('should get auth token', () => {
    cy.login('user', 'pass');
    cy.getAuthToken().then((token) => {
      expect(token).to.not.be.null;
      cy.log(`Auth token: ${token}`);
    });
  });
});
```

**Helper Functions:**
```javascript
// cypress/support/helpers.js

// Generate random email
export const generateEmail = (prefix = 'user') => {
  const timestamp = Date.now();
  return `${prefix}_${timestamp}@test.com`;
};

// Format currency
export const formatCurrency = (amount) => {
  return `$${amount.toFixed(2)}`;
};

// Wait for element with custom timeout
export const waitForElement = (selector, timeout = 10000) => {
  return cy.get(selector, { timeout });
};

// Usage in test
import { generateEmail, formatCurrency } from '../support/helpers';

describe('Helper Functions', () => {
  it('should use helper functions', () => {
    const email = generateEmail('testuser');
    cy.log(email); // "testuser_1234567890@test.com"
    
    const price = formatCurrency(29.99);
    cy.log(price); // "$29.99"
  });
});
```

---

## 2️⃣ Arrays - Deep Dive

### Array Basics Review

```javascript
const fruits = ['apple', 'banana', 'orange'];
const numbers = [1, 2, 3, 4, 5];
const mixed = ['text', 123, true, { name: 'John' }];

// Access elements
console.log(fruits[0]); // 'apple'
console.log(fruits[fruits.length - 1]); // 'orange' (last element)

// Modify elements
fruits[1] = 'mango';
console.log(fruits); // ['apple', 'mango', 'orange']
```

---

### Array Methods - Essential untuk Cypress

#### **1. forEach() - Iterate Over Array**

```javascript
const users = ['Alice', 'Bob', 'Charlie'];

// Traditional for loop
for (let i = 0; i < users.length; i++) {
  console.log(users[i]);
}

// forEach (cleaner)
users.forEach((user) => {
  console.log(user);
});

// forEach with index
users.forEach((user, index) => {
  console.log(`${index + 1}. ${user}`);
});
// Output:
// 1. Alice
// 2. Bob
// 3. Charlie
```

**Cypress Example:**
```javascript
describe('forEach in Cypress', () => {
  it('should test multiple navigation links', () => {
    const navLinks = ['Home', 'About', 'Services', 'Contact'];
    
    navLinks.forEach((linkText) => {
      cy.contains('a', linkText)
        .should('be.visible')
        .and('have.attr', 'href');
    });
  });
  
  it('should verify table rows', () => {
    const expectedData = [
      { name: 'Alice', role: 'Admin' },
      { name: 'Bob', role: 'User' },
      { name: 'Charlie', role: 'Moderator' }
    ];
    
    cy.get('table tbody tr').each(($row, index) => {
      const expected = expectedData[index];
      cy.wrap($row).within(() => {
        cy.get('td').eq(0).should('contain', expected.name);
        cy.get('td').eq(1).should('contain', expected.role);
      });
    });
  });
});
```

---

#### **2. map() - Transform Array**

`map()` creates a **new array** by transforming each element.

```javascript
const numbers = [1, 2, 3, 4, 5];

// Double each number
const doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]
console.log(numbers); // [1, 2, 3, 4, 5] (original unchanged)

// Extract property from objects
const users = [
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 30 },
  { name: 'Charlie', age: 35 }
];

const names = users.map(user => user.name);
console.log(names); // ['Alice', 'Bob', 'Charlie']

const ages = users.map(user => user.age);
console.log(ages); // [25, 30, 35]

// Transform objects
const userSummaries = users.map(user => {
  return `${user.name} is ${user.age} years old`;
});
console.log(userSummaries);
// ['Alice is 25 years old', 'Bob is 30 years old', 'Charlie is 35 years old']
```

**Cypress Example:**
```javascript
describe('map() in Cypress', () => {
  it('should extract text from elements', () => {
    cy.get('.user-list li').then($items => {
      // Convert jQuery object to array and map
      const usernames = [...$items].map(item => item.textContent.trim());
      
      cy.log('Usernames:', usernames);
      expect(usernames).to.include('Alice');
      expect(usernames).to.have.length.greaterThan(0);
    });
  });
  
  it('should prepare test data', () => {
    const baseUsers = ['user1', 'user2', 'user3'];
    
    // Transform to full user objects
    const testUsers = baseUsers.map((username, index) => ({
      username,
      email: `${username}@test.com`,
      password: `Pass${index}123!`
    }));
    
    testUsers.forEach(user => {
      cy.log(`Testing: ${user.username}`);
      cy.visit('/register');
      cy.get('#username').type(user.username);
      cy.get('#email').type(user.email);
      cy.get('#password').type(user.password);
      cy.get('button[type="submit"]').click();
    });
  });
});
```

---

#### **3. filter() - Filter Array**

`filter()` creates a **new array** with elements that pass a test.

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Get even numbers
const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // [2, 4, 6, 8, 10]

// Get odd numbers
const oddNumbers = numbers.filter(num => num % 2 !== 0);
console.log(oddNumbers); // [1, 3, 5, 7, 9]

// Filter objects
const users = [
  { name: 'Alice', age: 25, role: 'admin' },
  { name: 'Bob', age: 17, role: 'user' },
  { name: 'Charlie', age: 30, role: 'user' },
  { name: 'David', age: 16, role: 'user' }
];

// Get adults only
const adults = users.filter(user => user.age >= 18);
console.log(adults);
// [
//   { name: 'Alice', age: 25, role: 'admin' },
//   { name: 'Charlie', age: 30, role: 'user' }
// ]

// Get admins only
const admins = users.filter(user => user.role === 'admin');
console.log(admins);
// [{ name: 'Alice', age: 25, role: 'admin' }]

// Multiple conditions
const adultUsers = users.filter(user => user.age >= 18 && user.role === 'user');
console.log(adultUsers);
// [{ name: 'Charlie', age: 30, role: 'user' }]
```

**Cypress Example:**
```javascript
describe('filter() in Cypress', () => {
  it('should filter test data', () => {
    const allUsers = [
      { username: 'admin1', role: 'admin', active: true },
      { username: 'user1', role: 'user', active: true },
      { username: 'user2', role: 'user', active: false },
      { username: 'admin2', role: 'admin', active: true }
    ];
    
    // Test only active admins
    const activeAdmins = allUsers.filter(u => u.role === 'admin' && u.active);
    
    activeAdmins.forEach(admin => {
      cy.log(`Testing admin: ${admin.username}`);
      // Test logic here
    });
  });
  
  it('should filter visible elements', () => {
    cy.get('.item-list .item').then($items => {
      const visibleItems = [...$items].filter(item => {
        return Cypress.$(item).is(':visible');
      });
      
      cy.log(`Found ${visibleItems.length} visible items`);
      expect(visibleItems.length).to.be.greaterThan(0);
    });
  });
});
```

---

#### **4. find() - Find Single Element**

`find()` returns the **first element** that passes a test.

```javascript
const users = [
  { id: 1, name: 'Alice', email: 'alice@example.com' },
  { id: 2, name: 'Bob', email: 'bob@example.com' },
  { id: 3, name: 'Charlie', email: 'charlie@example.com' }
];

// Find user by id
const user = users.find(u => u.id === 2);
console.log(user);
// { id: 2, name: 'Bob', email: 'bob@example.com' }

// Find user by name
const alice = users.find(u => u.name === 'Alice');
console.log(alice);
// { id: 1, name: 'Alice', email: 'alice@example.com' }

// Not found returns undefined
const notFound = users.find(u => u.id === 999);
console.log(notFound); // undefined
```

**Cypress Example:**
```javascript
describe('find() in Cypress', () => {
  it('should find specific test data', () => {
    const testUsers = [
      { username: 'user1', password: 'pass1', role: 'user' },
      { username: 'admin', password: 'admin123', role: 'admin' },
      { username: 'user2', password: 'pass2', role: 'user' }
    ];
    
    // Find admin user
    const adminUser = testUsers.find(u => u.role === 'admin');
    
    if (adminUser) {
      cy.login(adminUser.username, adminUser.password);
      cy.get('.admin-panel').should('be.visible');
    }
  });
});
```

---

#### **5. some() - Test if ANY Element Passes**

```javascript
const numbers = [1, 2, 3, 4, 5];

// Check if any number is greater than 3
const hasLargeNumber = numbers.some(num => num > 3);
console.log(hasLargeNumber); // true

// Check if any number is negative
const hasNegative = numbers.some(num => num < 0);
console.log(hasNegative); // false

const users = [
  { name: 'Alice', role: 'user' },
  { name: 'Bob', role: 'admin' },
  { name: 'Charlie', role: 'user' }
];

// Check if there's at least one admin
const hasAdmin = users.some(u => u.role === 'admin');
console.log(hasAdmin); // true
```

**Cypress Example:**
```javascript
describe('some() in Cypress', () => {
  it('should check if any element matches', () => {
    cy.get('.notification').then($notifications => {
      const hasError = [...$notifications].some(el => {
        return Cypress.$(el).hasClass('error');
      });
      
      if (hasError) {
        cy.log('Error notification found!');
      }
    });
  });
});
```

---

#### **6. every() - Test if ALL Elements Pass**

```javascript
const numbers = [2, 4, 6, 8, 10];

// Check if all numbers are even
const allEven = numbers.every(num => num % 2 === 0);
console.log(allEven); // true

// Check if all numbers are positive
const allPositive = numbers.every(num => num > 0);
console.log(allPositive); // true

const users = [
  { name: 'Alice', age: 25, verified: true },
  { name: 'Bob', age: 30, verified: true },
  { name: 'Charlie', age: 35, verified: false }
];

// Check if all users are verified
const allVerified = users.every(u => u.verified);
console.log(allVerified); // false

// Check if all users are adults
const allAdults = users.every(u => u.age >= 18);
console.log(allAdults); // true
```

**Cypress Example:**
```javascript
describe('every() in Cypress', () => {
  it('should verify all elements have required attributes', () => {
    cy.get('.product-card').then($cards => {
      const allHavePrice = [...$cards].every(card => {
        return Cypress.$(card).find('.price').length > 0;
      });
      
      expect(allHavePrice).to.be.true;
    });
  });
});
```

---

#### **7. reduce() - Reduce to Single Value**

`reduce()` is powerful but can be complex. It reduces an array to a single value.

```javascript
const numbers = [1, 2, 3, 4, 5];

// Sum all numbers
const sum = numbers.reduce((accumulator, current) => {
  return accumulator + current;
}, 0); // 0 is initial value

console.log(sum); // 15

// How it works:
// Iteration 1: accumulator = 0, current = 1, return 0 + 1 = 1
// Iteration 2: accumulator = 1, current = 2, return 1 + 2 = 3
// Iteration 3: accumulator = 3, current = 3, return 3 + 3 = 6
// Iteration 4: accumulator = 6, current = 4, return 6 + 4 = 10
// Iteration 5: accumulator = 10, current = 5, return 10 + 5 = 15

// Find maximum
const max = numbers.reduce((max, current) => {
  return current > max ? current : max;
}, numbers[0]);
console.log(max); // 5

// Count occurrences
const fruits = ['apple', 'banana', 'apple', 'orange', 'banana', 'apple'];
const count = fruits.reduce((acc, fruit) => {
  acc[fruit] = (acc[fruit] || 0) + 1;
  return acc;
}, {});
console.log(count);
// { apple: 3, banana: 2, orange: 1 }
```

**Cypress Example:**
```javascript
describe('reduce() in Cypress', () => {
  it('should calculate total from cart items', () => {
    const cartItems = [
      { name: 'Item 1', price: 10, quantity: 2 },
      { name: 'Item 2', price: 15, quantity: 1 },
      { name: 'Item 3', price: 20, quantity: 3 }
    ];
    
    const total = cartItems.reduce((sum, item) => {
      return sum + (item.price * item.quantity);
    }, 0);
    
    cy.log(`Cart total: $${total}`); // $95
    
    cy.get('.cart-total').should('contain', total);
  });
});
```

---

### Array Method Chaining

You can chain multiple array methods together!

```javascript
const users = [
  { name: 'Alice', age: 25, role: 'admin', active: true },
  { name: 'Bob', age: 17, role: 'user', active: true },
  { name: 'Charlie', age: 30, role: 'user', active: false },
  { name: 'David', age: 22, role: 'user', active: true },
  { name: 'Eve', age: 28, role: 'admin', active: true }
];

// Get names of active adult users
const activeAdultUserNames = users
  .filter(u => u.active)           // Get active users
  .filter(u => u.age >= 18)        // Get adults
  .filter(u => u.role === 'user')  // Get regular users
  .map(u => u.name);               // Extract names

console.log(activeAdultUserNames); // ['David']

// Calculate average age of active admins
const activeAdmins = users
  .filter(u => u.active && u.role === 'admin');

const averageAge = activeAdmins
  .map(u => u.age)
  .reduce((sum, age) => sum + age, 0) / activeAdmins.length;

console.log(averageAge); // 26.5
```

**Cypress Example:**
```javascript
describe('Array Method Chaining', () => {
  it('should process complex data', () => {
    const products = [
      { name: 'Laptop', price: 1000, category: 'Electronics', inStock: true },
      { name: 'Phone', price: 500, category: 'Electronics', inStock: true },
      { name: 'Desk', price: 300, category: 'Furniture', inStock: false },
      { name: 'Chair', price: 150, category: 'Furniture', inStock: true }
    ];
    
    // Get names of in-stock electronics under $600
    const affordableElectronics = products
      .filter(p => p.inStock)
      .filter(p => p.category === 'Electronics')
      .filter(p => p.price < 600)
      .map(p => p.name);
    
    cy.log('Affordable electronics:', affordableElectronics);
    // ['Phone']
    
    expect(affordableElectronics).to.include('Phone');
  });
});
```

---

## 3️⃣ Spread Operator (...)

The spread operator is incredibly useful for working with arrays and objects.

### Spread with Arrays

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

// Combine arrays
const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]

// Add elements
const withExtra = [...arr1, 4, 5];
console.log(withExtra); // [1, 2, 3, 4, 5]

// Copy array
const copy = [...arr1];
console.log(copy); // [1, 2, 3]
copy.push(4);
console.log(arr1); // [1, 2, 3] (original unchanged)
console.log(copy); // [1, 2, 3, 4]
```

**Cypress Example:**
```javascript
describe('Spread Operator in Cypress', () => {
  it('should combine test data sets', () => {
    const basicUsers = [
      { username: 'user1', role: 'user' },
      { username: 'user2', role: 'user' }
    ];
    
    const adminUsers = [
      { username: 'admin1', role: 'admin' },
      { username: 'admin2', role: 'admin' }
    ];
    
    const allUsers = [...basicUsers, ...adminUsers];
    
    allUsers.forEach(user => {
      cy.log(`Testing: ${user.username} (${user.role})`);
    });
  });
});
```

---

## 🎯 Praktik: Real-World Cypress Examples

### Example 1: Data-Driven Testing

```javascript
describe('Data-Driven Login Tests', () => {
  const testCases = [
    {
      description: 'valid admin credentials',
      username: 'admin',
      password: 'admin123',
      expectedUrl: '/admin/dashboard',
      shouldSucceed: true
    },
    {
      description: 'valid user credentials',
      username: 'user',
      password: 'user123',
      expectedUrl: '/dashboard',
      shouldSucceed: true
    },
    {
      description: 'invalid credentials',
      username: 'invalid',
      password: 'wrong',
      expectedUrl: '/login',
      shouldSucceed: false
    }
  ];
  
  testCases.forEach((testCase) => {
    it(`should handle ${testCase.description}`, () => {
      cy.visit('/login');
      cy.get('#username').type(testCase.username);
      cy.get('#password').type(testCase.password);
      cy.get('button[type="submit"]').click();
      
      if (testCase.shouldSucceed) {
        cy.url().should('include', testCase.expectedUrl);
      } else {
        cy.get('.error-message').should('be.visible');
      }
    });
  });
});
```

---

### Example 2: Custom Helper Functions

```javascript
// cypress/support/helpers.js

export const generateTestUser = (role = 'user') => {
  const timestamp = Date.now();
  return {
    username: `${role}_${timestamp}`,
    email: `${role}_${timestamp}@test.com`,
    password: 'TestPass123!',
    role
  };
};

export const validateEmail = (email) => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};

export const formatTestData = (users) => {
  return users.map(user => ({
    ...user,
    fullName: `${user.firstName} ${user.lastName}`,
    isActive: user.isActive ?? true
  }));
};

// Usage in test
import { generateTestUser, validateEmail, formatTestData } from '../support/helpers';

describe('Helper Functions Example', () => {
  it('should generate and validate test user', () => {
    const testUser = generateTestUser('admin');
    
    expect(validateEmail(testUser.email)).to.be.true;
    expect(testUser.role).to.equal('admin');
    
    cy.log(`Generated user: ${testUser.username}`);
  });
  
  it('should format test data', () => {
    const rawUsers = [
      { firstName: 'John', lastName: 'Doe' },
      { firstName: 'Jane', lastName: 'Smith', isActive: false }
    ];
    
    const formattedUsers = formatTestData(rawUsers);
    
    expect(formattedUsers[0].fullName).to.equal('John Doe');
    expect(formattedUsers[0].isActive).to.be.true;
    expect(formattedUsers[1].isActive).to.be.false;
  });
});
```

---

## 🎓 Ringkasan Minggu 2

### Yang Sudah Dipelajari:

✅ **Functions:**
- Function declarations, expressions, dan arrow functions
- Parameters dan default values
- Return values
- Custom Cypress commands

✅ **Array Methods:**
- `forEach()` - Iterate
- `map()` - Transform
- `filter()` - Filter
- `find()` - Find single element
- `some()` - Test if any passes
- `every()` - Test if all pass
- `reduce()` - Reduce to single value

✅ **Advanced Concepts:**
- Array method chaining
- Spread operator
- Helper functions

---

### Key Takeaways untuk Cypress:

1. **Functions membuat code reusable** - Gunakan untuk custom commands
2. **Array methods sangat powerful** - Perfect untuk data-driven testing
3. **Method chaining** membuat code lebih elegant
4. **Helper functions** meningkatkan maintainability
5. **Spread operator** memudahkan data manipulation

---

### Checklist Penguasaan:

- [ ] Bisa menulis function dengan berbagai syntax
- [ ] Memahami arrow functions
- [ ] Menggunakan default parameters
- [ ] Menguasai `map()`, `filter()`, `find()`
- [ ] Memahami `reduce()` untuk aggregation
- [ ] Bisa chain multiple array methods
- [ ] Membuat custom Cypress commands
- [ ] Implementasi helper functions

**Next Week:** Minggu 3 - Objects & Destructuring! 🚀
