# Minggu 1: Dasar-Dasar JavaScript

## 📚 Pengantar

Selamat datang di minggu pertama pembelajaran JavaScript! Minggu ini kita akan mempelajari fondasi JavaScript yang akan menjadi dasar untuk semua pembelajaran selanjutnya. Konsep-konsep ini sangat penting untuk memahami bagaimana Cypress bekerja dan bagaimana menulis test automation yang efektif.

---

## 1️⃣ Variables dan Scope

### Apa itu Variables?

Variables adalah "wadah" untuk menyimpan data. Bayangkan seperti kotak yang diberi label, dimana Anda bisa menyimpan nilai dan mengambilnya kembali kapan saja.

### Tiga Cara Mendeklarasikan Variables

JavaScript modern (ES6+) memiliki tiga cara untuk mendeklarasikan variables:

#### **1. `let` - Variable yang bisa diubah**

```javascript
let username = 'testuser';
console.log(username); // Output: testuser

username = 'newuser'; // Bisa diubah
console.log(username); // Output: newuser
```

**Kapan menggunakan `let`:**
- Ketika nilai variable akan berubah
- Untuk counters, flags, atau temporary values

**Contoh dalam Cypress:**
```javascript
describe('Login Test', () => {
  it('should count login attempts', () => {
    let loginAttempts = 0; // Counter yang akan berubah
    
    cy.visit('/login');
    loginAttempts++; // Increment counter
    
    cy.get('#username').type('user1');
    cy.get('#password').type('wrongpassword');
    cy.get('button[type="submit"]').click();
    
    cy.log(`Login attempts: ${loginAttempts}`);
  });
});
```

---

#### **2. `const` - Variable yang tidak bisa diubah**

```javascript
const API_URL = 'https://api.example.com';
console.log(API_URL); // Output: https://api.example.com

// API_URL = 'https://new-api.com'; // ❌ ERROR! Tidak bisa diubah
```

**Kapan menggunakan `const`:**
- Untuk nilai yang tidak akan berubah
- Configuration values
- Test data yang fixed
- **Best Practice:** Gunakan `const` sebagai default, gunakan `let` hanya jika benar-benar perlu

**Contoh dalam Cypress:**
```javascript
describe('User Management', () => {
  // Test data yang tidak berubah
  const VALID_USERNAME = 'admin@example.com';
  const VALID_PASSWORD = 'SecurePass123!';
  const BASE_URL = 'https://staging.example.com';
  
  it('should login with valid credentials', () => {
    cy.visit(BASE_URL + '/login');
    cy.get('#username').type(VALID_USERNAME);
    cy.get('#password').type(VALID_PASSWORD);
    cy.get('button[type="submit"]').click();
    
    cy.url().should('include', '/dashboard');
  });
});
```

---

#### **3. `var` - Cara lama (HINDARI!)**

```javascript
var oldWay = 'This is outdated';
```

**Mengapa hindari `var`?**
- Scope yang membingungkan (function-scoped, bukan block-scoped)
- Dapat menyebabkan bugs yang sulit dilacak
- Sudah digantikan oleh `let` dan `const`

**Perbandingan Scope:**
```javascript
// Block scope dengan let
if (true) {
  let blockScoped = 'Only available inside this block';
  console.log(blockScoped); // ✅ Works
}
// console.log(blockScoped); // ❌ ERROR! Not defined

// Function scope dengan var (confusing!)
if (true) {
  var functionScoped = 'Available outside the block!';
  console.log(functionScoped); // ✅ Works
}
console.log(functionScoped); // ✅ Works (unexpected!)
```

---

### Variable Naming Conventions

**Best Practices:**
```javascript
// ✅ Good - descriptive, camelCase
const userEmail = 'test@example.com';
const maxLoginAttempts = 3;
const isUserLoggedIn = true;

// ✅ Good - UPPERCASE untuk constants
const API_TIMEOUT = 5000;
const MAX_RETRY_COUNT = 3;

// ❌ Bad - tidak deskriptif
const x = 'test@example.com';
const data = 3;
const flag = true;

// ❌ Bad - naming convention salah
const UserEmail = 'test@example.com'; // PascalCase untuk classes
const user_email = 'test@example.com'; // snake_case bukan JavaScript style
```

---

## 2️⃣ Data Types

JavaScript memiliki beberapa tipe data fundamental yang perlu Anda pahami.

### **Primitive Data Types**

#### **1. String - Teks**

```javascript
const firstName = 'John';
const lastName = "Doe";
const fullName = `${firstName} ${lastName}`; // Template literal

console.log(fullName); // Output: John Doe
```

**String Methods yang Sering Digunakan:**
```javascript
const email = 'TEST@EXAMPLE.COM';

// Mengubah case
console.log(email.toLowerCase()); // test@example.com
console.log(email.toUpperCase()); // TEST@EXAMPLE.COM

// Mendapatkan panjang
console.log(email.length); // 17

// Mencari substring
console.log(email.includes('@')); // true
console.log(email.startsWith('TEST')); // true
console.log(email.endsWith('.COM')); // true

// Memotong string
console.log(email.slice(0, 4)); // TEST
console.log(email.substring(5, 12)); // EXAMPLE

// Mengganti text
console.log(email.replace('TEST', 'user')); // user@EXAMPLE.COM

// Split menjadi array
console.log(email.split('@')); // ['TEST', 'EXAMPLE.COM']
```

**Contoh dalam Cypress:**
```javascript
describe('String Manipulation in Tests', () => {
  it('should verify email format', () => {
    cy.get('#email-input').type('user@example.com');
    
    cy.get('#email-input').invoke('val').then((email) => {
      // Validasi email menggunakan string methods
      expect(email).to.include('@');
      expect(email.toLowerCase()).to.equal('user@example.com');
      
      const [username, domain] = email.split('@');
      expect(username).to.equal('user');
      expect(domain).to.equal('example.com');
    });
  });
  
  it('should handle dynamic selectors', () => {
    const userId = '12345';
    const selector = `[data-user-id="${userId}"]`; // Template literal
    
    cy.get(selector).should('be.visible');
  });
});
```

---

#### **2. Number - Angka**

```javascript
const age = 25;
const price = 99.99;
const negative = -10;
const scientific = 2e3; // 2000
```

**Number Operations:**
```javascript
const a = 10;
const b = 3;

console.log(a + b);  // 13 - Addition
console.log(a - b);  // 7  - Subtraction
console.log(a * b);  // 30 - Multiplication
console.log(a / b);  // 3.333... - Division
console.log(a % b);  // 1  - Modulus (remainder)
console.log(a ** b); // 1000 - Exponentiation

// Increment & Decrement
let counter = 0;
counter++;  // counter = 1
counter--;  // counter = 0
counter += 5; // counter = 5
counter *= 2; // counter = 10
```

**Number Methods:**
```javascript
const price = 99.999;

console.log(price.toFixed(2)); // "100.00" - Round to 2 decimals
console.log(Math.round(price)); // 100 - Round to nearest integer
console.log(Math.floor(price)); // 99 - Round down
console.log(Math.ceil(price));  // 100 - Round up

// Random numbers
console.log(Math.random()); // Random number between 0 and 1
console.log(Math.floor(Math.random() * 100)); // Random integer 0-99

// Min and Max
console.log(Math.min(5, 10, 3, 8)); // 3
console.log(Math.max(5, 10, 3, 8)); // 10
```

**Contoh dalam Cypress:**
```javascript
describe('Number Operations in Tests', () => {
  it('should verify cart total calculation', () => {
    const itemPrice = 29.99;
    const quantity = 3;
    const expectedTotal = itemPrice * quantity;
    
    cy.get('.cart-total').invoke('text').then((text) => {
      const actualTotal = parseFloat(text.replace('$', ''));
      expect(actualTotal).to.equal(parseFloat(expectedTotal.toFixed(2)));
    });
  });
  
  it('should generate random test data', () => {
    const randomUserId = Math.floor(Math.random() * 10000);
    const randomEmail = `user${randomUserId}@test.com`;
    
    cy.get('#email').type(randomEmail);
    cy.log(`Generated email: ${randomEmail}`);
  });
});
```

---

#### **3. Boolean - True/False**

```javascript
const isLoggedIn = true;
const hasPermission = false;
const isActive = true;
```

**Boolean dalam Kondisi:**
```javascript
const age = 18;
const isAdult = age >= 18; // true
const isMinor = age < 18;  // false

console.log(isAdult); // true
console.log(isMinor); // false
```

**Contoh dalam Cypress:**
```javascript
describe('Boolean Logic in Tests', () => {
  it('should check element visibility', () => {
    cy.get('.modal').then(($modal) => {
      const isVisible = $modal.is(':visible');
      
      if (isVisible) {
        cy.log('Modal is visible');
        cy.get('.modal-close').click();
      } else {
        cy.log('Modal is not visible');
      }
    });
  });
  
  it('should verify user status', () => {
    const isAdmin = true;
    const hasEditPermission = true;
    
    if (isAdmin && hasEditPermission) {
      cy.get('.edit-button').should('be.visible');
    } else {
      cy.get('.edit-button').should('not.exist');
    }
  });
});
```

---

#### **4. Null dan Undefined**

```javascript
let notAssigned; // undefined - variable declared but not assigned
let emptyValue = null; // null - intentionally empty

console.log(notAssigned); // undefined
console.log(emptyValue);  // null

// Checking for null/undefined
if (notAssigned === undefined) {
  console.log('Variable is undefined');
}

if (emptyValue === null) {
  console.log('Variable is null');
}
```

**Contoh dalam Cypress:**
```javascript
describe('Handling Null and Undefined', () => {
  it('should handle optional elements', () => {
    cy.get('body').then(($body) => {
      // Check if element exists before interacting
      if ($body.find('.optional-banner').length > 0) {
        cy.get('.optional-banner').click();
      } else {
        cy.log('Optional banner not found');
      }
    });
  });
});
```

---

### **Reference Data Types**

#### **5. Array - Kumpulan Data**

```javascript
const fruits = ['apple', 'banana', 'orange'];
const numbers = [1, 2, 3, 4, 5];
const mixed = ['text', 123, true, null]; // Bisa mixed types

// Accessing array elements (0-indexed)
console.log(fruits[0]); // 'apple'
console.log(fruits[1]); // 'banana'
console.log(fruits[2]); // 'orange'

// Array length
console.log(fruits.length); // 3
```

**Basic Array Methods:**
```javascript
const users = ['Alice', 'Bob', 'Charlie'];

// Add elements
users.push('David');      // Add to end: ['Alice', 'Bob', 'Charlie', 'David']
users.unshift('Zoe');     // Add to start: ['Zoe', 'Alice', 'Bob', 'Charlie', 'David']

// Remove elements
users.pop();              // Remove from end: ['Zoe', 'Alice', 'Bob', 'Charlie']
users.shift();            // Remove from start: ['Alice', 'Bob', 'Charlie']

// Find elements
console.log(users.includes('Bob')); // true
console.log(users.indexOf('Bob'));  // 1

// Join array to string
console.log(users.join(', ')); // 'Alice, Bob, Charlie'
```

**Contoh dalam Cypress:**
```javascript
describe('Array Usage in Tests', () => {
  it('should test multiple users', () => {
    const testUsers = [
      { username: 'user1', password: 'pass1' },
      { username: 'user2', password: 'pass2' },
      { username: 'user3', password: 'pass3' }
    ];
    
    testUsers.forEach((user, index) => {
      cy.log(`Testing user ${index + 1}: ${user.username}`);
      cy.visit('/login');
      cy.get('#username').type(user.username);
      cy.get('#password').type(user.password);
      cy.get('button[type="submit"]').click();
      cy.url().should('include', '/dashboard');
      cy.get('.logout').click();
    });
  });
  
  it('should verify list items', () => {
    const expectedItems = ['Item 1', 'Item 2', 'Item 3'];
    
    cy.get('.item-list li').should('have.length', expectedItems.length);
    
    cy.get('.item-list li').each(($el, index) => {
      expect($el.text()).to.equal(expectedItems[index]);
    });
  });
});
```

---

#### **6. Object - Pasangan Key-Value**

```javascript
const user = {
  name: 'John Doe',
  email: 'john@example.com',
  age: 30,
  isActive: true
};

// Accessing object properties
console.log(user.name);        // 'John Doe' - Dot notation
console.log(user['email']);    // 'john@example.com' - Bracket notation

// Adding/Modifying properties
user.phone = '123-456-7890';   // Add new property
user.age = 31;                 // Modify existing property

console.log(user);
// {
//   name: 'John Doe',
//   email: 'john@example.com',
//   age: 31,
//   isActive: true,
//   phone: '123-456-7890'
// }
```

**Contoh dalam Cypress:**
```javascript
describe('Object Usage in Tests', () => {
  it('should use test data objects', () => {
    const testData = {
      username: 'testuser',
      password: 'TestPass123!',
      email: 'test@example.com',
      firstName: 'Test',
      lastName: 'User'
    };
    
    cy.visit('/register');
    cy.get('#username').type(testData.username);
    cy.get('#email').type(testData.email);
    cy.get('#password').type(testData.password);
    cy.get('#firstName').type(testData.firstName);
    cy.get('#lastName').type(testData.lastName);
    cy.get('button[type="submit"]').click();
  });
});
```

---

## 3️⃣ Operators

### **Arithmetic Operators**

```javascript
const a = 10;
const b = 3;

console.log(a + b);  // 13 - Addition
console.log(a - b);  // 7  - Subtraction
console.log(a * b);  // 30 - Multiplication
console.log(a / b);  // 3.333... - Division
console.log(a % b);  // 1  - Modulus (remainder)
console.log(a ** b); // 1000 - Exponentiation
```

---

### **Comparison Operators**

```javascript
const x = 5;
const y = '5';

// Equality (loose - converts types)
console.log(x == y);   // true - '5' converted to 5

// Strict Equality (recommended)
console.log(x === y);  // false - different types
console.log(x !== y);  // true - not equal (strict)

// Comparison
console.log(x > 3);    // true
console.log(x < 10);   // true
console.log(x >= 5);   // true
console.log(x <= 5);   // true
```

**⚠️ Best Practice:** Selalu gunakan `===` dan `!==` untuk menghindari type coercion yang tidak terduga!

**Contoh dalam Cypress:**
```javascript
describe('Comparison in Tests', () => {
  it('should verify element count', () => {
    cy.get('.user-list li').its('length').then((count) => {
      expect(count).to.be.greaterThan(0);
      expect(count).to.be.lessThan(100);
      
      if (count === 10) {
        cy.log('Exactly 10 users found');
      } else if (count > 10) {
        cy.log(`More than 10 users: ${count}`);
      } else {
        cy.log(`Less than 10 users: ${count}`);
      }
    });
  });
});
```

---

### **Logical Operators**

```javascript
const isLoggedIn = true;
const isAdmin = false;
const hasPermission = true;

// AND - Both must be true
console.log(isLoggedIn && isAdmin); // false

// OR - At least one must be true
console.log(isLoggedIn || isAdmin); // true

// NOT - Inverts boolean
console.log(!isLoggedIn); // false
console.log(!isAdmin);    // true

// Complex conditions
const canEdit = isLoggedIn && (isAdmin || hasPermission);
console.log(canEdit); // true
```

**Contoh dalam Cypress:**
```javascript
describe('Logical Operators in Tests', () => {
  it('should show edit button based on permissions', () => {
    const isLoggedIn = true;
    const isAdmin = false;
    const isOwner = true;
    
    if (isLoggedIn && (isAdmin || isOwner)) {
      cy.get('.edit-button').should('be.visible');
    } else {
      cy.get('.edit-button').should('not.exist');
    }
  });
  
  it('should handle multiple conditions', () => {
    cy.get('.user-status').invoke('text').then((status) => {
      const isActive = status === 'Active';
      const isPending = status === 'Pending';
      
      if (isActive || isPending) {
        cy.get('.action-button').should('be.enabled');
      } else {
        cy.get('.action-button').should('be.disabled');
      }
    });
  });
});
```

---

## 4️⃣ Conditional Statements

### **If-Else Statement**

```javascript
const age = 18;

if (age >= 18) {
  console.log('You are an adult');
} else {
  console.log('You are a minor');
}
```

**Multiple Conditions:**
```javascript
const score = 85;

if (score >= 90) {
  console.log('Grade: A');
} else if (score >= 80) {
  console.log('Grade: B');
} else if (score >= 70) {
  console.log('Grade: C');
} else if (score >= 60) {
  console.log('Grade: D');
} else {
  console.log('Grade: F');
}
```

**Contoh dalam Cypress:**
```javascript
describe('Conditional Logic in Tests', () => {
  it('should handle different user types', () => {
    const userType = 'admin';
    
    cy.visit('/dashboard');
    
    if (userType === 'admin') {
      cy.get('.admin-panel').should('be.visible');
      cy.get('.user-management').should('be.visible');
    } else if (userType === 'moderator') {
      cy.get('.moderation-tools').should('be.visible');
    } else {
      cy.get('.user-dashboard').should('be.visible');
    }
  });
  
  it('should handle optional elements', () => {
    cy.get('body').then(($body) => {
      if ($body.find('.cookie-banner').length > 0) {
        cy.log('Cookie banner found, closing it');
        cy.get('.cookie-banner .close').click();
      } else {
        cy.log('No cookie banner present');
      }
    });
  });
});
```

---

### **Ternary Operator (Shorthand)**

```javascript
const age = 20;
const status = age >= 18 ? 'Adult' : 'Minor';
console.log(status); // 'Adult'

// Equivalent to:
let status2;
if (age >= 18) {
  status2 = 'Adult';
} else {
  status2 = 'Minor';
}
```

**Contoh dalam Cypress:**
```javascript
describe('Ternary Operator in Tests', () => {
  it('should use ternary for concise logic', () => {
    const isProduction = false;
    const baseUrl = isProduction ? 'https://prod.com' : 'https://staging.com';
    
    cy.visit(baseUrl);
  });
  
  it('should set timeout based on environment', () => {
    const isCI = true;
    const timeout = isCI ? 10000 : 5000;
    
    cy.get('.slow-element', { timeout }).should('be.visible');
  });
});
```

---

### **Switch Statement**

```javascript
const day = 'Monday';

switch (day) {
  case 'Monday':
    console.log('Start of the work week');
    break;
  case 'Friday':
    console.log('End of the work week');
    break;
  case 'Saturday':
  case 'Sunday':
    console.log('Weekend!');
    break;
  default:
    console.log('Midweek day');
}
```

**Contoh dalam Cypress:**
```javascript
describe('Switch Statement in Tests', () => {
  it('should handle different environments', () => {
    const environment = 'staging';
    let baseUrl;
    
    switch (environment) {
      case 'production':
        baseUrl = 'https://prod.example.com';
        break;
      case 'staging':
        baseUrl = 'https://staging.example.com';
        break;
      case 'development':
        baseUrl = 'http://localhost:3000';
        break;
      default:
        baseUrl = 'http://localhost:3000';
    }
    
    cy.visit(baseUrl);
  });
});
```

---

## 5️⃣ Loops

### **For Loop**

```javascript
// Basic for loop
for (let i = 0; i < 5; i++) {
  console.log(`Iteration ${i}`);
}
// Output:
// Iteration 0
// Iteration 1
// Iteration 2
// Iteration 3
// Iteration 4

// Loop through array
const fruits = ['apple', 'banana', 'orange'];
for (let i = 0; i < fruits.length; i++) {
  console.log(`Fruit ${i + 1}: ${fruits[i]}`);
}
```

**Contoh dalam Cypress:**
```javascript
describe('For Loop in Tests', () => {
  it('should test multiple form submissions', () => {
    const testData = [
      { name: 'User 1', email: 'user1@test.com' },
      { name: 'User 2', email: 'user2@test.com' },
      { name: 'User 3', email: 'user3@test.com' }
    ];
    
    for (let i = 0; i < testData.length; i++) {
      cy.visit('/register');
      cy.get('#name').type(testData[i].name);
      cy.get('#email').type(testData[i].email);
      cy.get('button[type="submit"]').click();
      cy.contains('Registration successful').should('be.visible');
    }
  });
});
```

---

### **While Loop**

```javascript
let count = 0;

while (count < 5) {
  console.log(`Count: ${count}`);
  count++;
}
```

**⚠️ Warning:** Hati-hati dengan infinite loops! Pastikan kondisi akan menjadi false.

---

### **forEach Loop (Array Method)**

```javascript
const colors = ['red', 'green', 'blue'];

colors.forEach((color, index) => {
  console.log(`Color ${index}: ${color}`);
});
// Output:
// Color 0: red
// Color 1: green
// Color 2: blue
```

**Contoh dalam Cypress:**
```javascript
describe('forEach in Tests', () => {
  it('should verify all navigation links', () => {
    const navLinks = ['Home', 'About', 'Services', 'Contact'];
    
    navLinks.forEach((linkText) => {
      cy.contains('a', linkText).should('be.visible');
    });
  });
  
  it('should test multiple login attempts', () => {
    const credentials = [
      { user: 'admin', pass: 'admin123' },
      { user: 'user', pass: 'user123' }
    ];
    
    credentials.forEach((cred, index) => {
      cy.log(`Testing credential set ${index + 1}`);
      cy.visit('/login');
      cy.get('#username').type(cred.user);
      cy.get('#password').type(cred.pass);
      cy.get('button[type="submit"]').click();
      cy.url().should('include', '/dashboard');
      cy.get('.logout').click();
    });
  });
});
```

---

### **For...of Loop (Modern)**

```javascript
const numbers = [10, 20, 30, 40];

for (const num of numbers) {
  console.log(num);
}
// Output: 10, 20, 30, 40
```

**Contoh dalam Cypress:**
```javascript
describe('For...of Loop in Tests', () => {
  it('should check multiple elements', () => {
    const selectors = ['.header', '.main-content', '.footer'];
    
    for (const selector of selectors) {
      cy.get(selector).should('be.visible');
    }
  });
});
```

---

## 🎯 Latihan Praktis

### **Latihan 1: Variables dan Data Types**

```javascript
describe('Practice: Variables and Data Types', () => {
  it('should manage user data', () => {
    // TODO: Buat variables untuk user data
    const username = 'testuser';
    const email = 'test@example.com';
    const age = 25;
    const isActive = true;
    
    // TODO: Log semua data
    cy.log(`Username: ${username}`);
    cy.log(`Email: ${email}`);
    cy.log(`Age: ${age}`);
    cy.log(`Active: ${isActive}`);
    
    // TODO: Verify data types
    expect(typeof username).to.equal('string');
    expect(typeof age).to.equal('number');
    expect(typeof isActive).to.equal('boolean');
  });
});
```

---

### **Latihan 2: Conditionals**

```javascript
describe('Practice: Conditionals', () => {
  it('should show different messages based on score', () => {
    const score = 85;
    let message;
    
    // TODO: Implementasi logic untuk menentukan message
    if (score >= 90) {
      message = 'Excellent!';
    } else if (score >= 80) {
      message = 'Good job!';
    } else if (score >= 70) {
      message = 'Not bad!';
    } else {
      message = 'Need improvement';
    }
    
    cy.log(message);
    expect(message).to.equal('Good job!');
  });
});
```

---

### **Latihan 3: Loops dan Arrays**

```javascript
describe('Practice: Loops and Arrays', () => {
  it('should process array of users', () => {
    const users = [
      { name: 'Alice', role: 'admin' },
      { name: 'Bob', role: 'user' },
      { name: 'Charlie', role: 'moderator' }
    ];
    
    // TODO: Loop through users dan log informasi mereka
    users.forEach((user, index) => {
      cy.log(`User ${index + 1}: ${user.name} (${user.role})`);
    });
    
    // TODO: Count admin users
    let adminCount = 0;
    for (const user of users) {
      if (user.role === 'admin') {
        adminCount++;
      }
    }
    
    cy.log(`Total admins: ${adminCount}`);
    expect(adminCount).to.equal(1);
  });
});
```

---

## 🎓 Ringkasan Minggu 1

### **Yang Sudah Dipelajari:**

✅ **Variables:**
- `let` untuk values yang berubah
- `const` untuk values yang tetap
- Hindari `var`

✅ **Data Types:**
- String, Number, Boolean
- Null dan Undefined
- Array dan Object

✅ **Operators:**
- Arithmetic: `+`, `-`, `*`, `/`, `%`, `**`
- Comparison: `===`, `!==`, `>`, `<`, `>=`, `<=`
- Logical: `&&`, `||`, `!`

✅ **Conditionals:**
- `if...else` statements
- Ternary operator
- `switch` statements

✅ **Loops:**
- `for` loop
- `while` loop
- `forEach` method
- `for...of` loop

---

### **Key Takeaways untuk Cypress:**

1. **Gunakan `const` untuk test data** yang tidak berubah
2. **Template literals** sangat berguna untuk dynamic selectors
3. **Conditional logic** membantu handle different scenarios
4. **Loops** perfect untuk data-driven testing
5. **Arrays dan Objects** untuk organize test data

---

### **Next Steps:**

Minggu depan kita akan belajar tentang **Functions & Arrays** yang akan membuat code Anda lebih reusable dan powerful!

**Preview Minggu 2:**
- Function declarations dan expressions
- Arrow functions
- Array methods (`map`, `filter`, `find`, `reduce`)
- Spread operator
- Custom Cypress commands

---

## 📝 Checklist Penguasaan

Sebelum lanjut ke Minggu 2, pastikan Anda bisa:

- [ ] Menjelaskan perbedaan `let`, `const`, dan `var`
- [ ] Memahami semua primitive data types
- [ ] Menggunakan template literals
- [ ] Menulis conditional statements
- [ ] Membuat dan iterate arrays
- [ ] Bekerja dengan objects
- [ ] Menggunakan comparison dan logical operators
- [ ] Implementasi loops dalam Cypress tests

**Selamat belajar! 🚀**
