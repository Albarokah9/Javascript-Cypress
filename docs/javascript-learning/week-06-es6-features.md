# Minggu 6: ES6+ Features

## 📚 Pengantar

ES6 (ECMAScript 2015) dan versi-versi setelahnya membawa banyak fitur modern ke JavaScript. Minggu ini kita akan belajar fitur-fitur yang paling berguna untuk Cypress testing.

---

## 1️⃣ Template Literals

Template literals memungkinkan string interpolation dan multi-line strings.

### String Interpolation

```javascript
// Old way
const name = 'John';
const age = 30;
const message = 'Hello, my name is ' + name + ' and I\'m ' + age + ' years old';

// Template literal (modern)
const message = `Hello, my name is ${name} and I'm ${age} years old`;

// Expression evaluation
const price = 29.99;
const quantity = 3;
const total = `Total: $${(price * quantity).toFixed(2)}`;
console.log(total); // "Total: $89.97"
```

### Multi-line Strings

```javascript
// Old way
const html = '<div class="user">\n' +
             '  <h2>' + name + '</h2>\n' +
             '  <p>Age: ' + age + '</p>\n' +
             '</div>';

// Template literal
const html = `
  <div class="user">
    <h2>${name}</h2>
    <p>Age: ${age}</p>
  </div>
`;
```

### Cypress Usage

```javascript
describe('Template Literals in Cypress', () => {
  it('should use template literals for selectors', () => {
    const userId = '12345';
    cy.get(`[data-user-id="${userId}"]`).should('be.visible');
    
    const className = 'active-user';
    cy.get(`.${className}`).should('exist');
  });
  
  it('should use for logging', () => {
    const username = 'testuser';
    const testNumber = 5;
    cy.log(`Testing user: ${username} (Test #${testNumber})`);
  });
});
```

---

## 2️⃣ Default Parameters

```javascript
// Without default
function greet(name) {
  name = name || 'Guest'; // Old way
  return `Hello, ${name}!`;
}

// With default parameter (modern)
function greet(name = 'Guest') {
  return `Hello, ${name}!`;
}

console.log(greet());        // "Hello, Guest!"
console.log(greet('Alice')); // "Hello, Alice!"

// Multiple defaults
function createUser(username, role = 'user', isActive = true) {
  return { username, role, isActive };
}

console.log(createUser('john'));
// { username: 'john', role: 'user', isActive: true }

console.log(createUser('admin', 'admin', false));
// { username: 'admin', role: 'admin', isActive: false }
```

### Cypress Custom Command

```javascript
// cypress/support/commands.js
Cypress.Commands.add('login', (username = 'testuser', password = 'password123') => {
  cy.visit('/login');
  cy.get('#username').type(username);
  cy.get('#password').type(password);
  cy.get('button[type="submit"]').click();
});

// Usage
cy.login(); // Uses defaults
cy.login('admin'); // Custom username, default password
cy.login('admin', 'admin123'); // Both custom
```

---

## 3️⃣ Rest Parameters

Rest parameters allow functions to accept unlimited arguments as an array.

```javascript
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3));           // 6
console.log(sum(1, 2, 3, 4, 5));     // 15
console.log(sum(10, 20, 30, 40, 50, 60)); // 210

// Combine with regular parameters
function multiply(multiplier, ...numbers) {
  return numbers.map(num => num * multiplier);
}

console.log(multiply(2, 1, 2, 3)); // [2, 4, 6]
console.log(multiply(10, 5, 10, 15)); // [50, 100, 150]
```

### Cypress Usage

```javascript
// Custom command with rest parameters
Cypress.Commands.add('fillForm', (...fields) => {
  fields.forEach(({ selector, value }) => {
    cy.get(selector).type(value);
  });
});

// Usage
describe('Rest Parameters', () => {
  it('should fill multiple fields', () => {
    cy.visit('/register');
    
    cy.fillForm(
      { selector: '#name', value: 'John Doe' },
      { selector: '#email', value: 'john@example.com' },
      { selector: '#phone', value: '123-456-7890' },
      { selector: '#address', value: '123 Main St' }
    );
  });
});
```

---

## 4️⃣ Optional Chaining (?.)

Optional chaining safely accesses nested properties without errors.

```javascript
const user = {
  name: 'John',
  address: {
    city: 'New York'
  }
};

// Without optional chaining
const country = user.address && user.address.country; // undefined

// With optional chaining (cleaner!)
const country = user.address?.country; // undefined (no error!)
const city = user.address?.city;       // 'New York'

// Deep nesting
const zipCode = user.address?.details?.zipCode; // undefined (safe!)

// With function calls
const result = user.getData?.(); // Only calls if getData exists

// With arrays
const firstItem = user.items?.[0]; // Safe array access
```

### Cypress Usage

```javascript
describe('Optional Chaining in Cypress', () => {
  it('should safely access nested data', () => {
    cy.get('.user-profile').then(($profile) => {
      const email = $profile.data('user')?.email ?? 'No email';
      const phone = $profile.data('user')?.contact?.phone ?? 'No phone';
      const city = $profile.data('user')?.address?.city ?? 'No city';
      
      cy.log(`Email: ${email}`);
      cy.log(`Phone: ${phone}`);
      cy.log(`City: ${city}`);
    });
  });
  
  it('should handle API responses safely', () => {
    cy.request('/api/user/1').then((response) => {
      const userName = response.body?.user?.name ?? 'Unknown';
      const userRole = response.body?.user?.role ?? 'guest';
      
      expect(userName).to.not.equal('Unknown');
    });
  });
});
```

---

## 5️⃣ Nullish Coalescing (??)

Returns right side only if left side is `null` or `undefined`.

```javascript
const value1 = null ?? 'default';      // 'default'
const value2 = undefined ?? 'default'; // 'default'
const value3 = 0 ?? 'default';         // 0 (not 'default'!)
const value4 = '' ?? 'default';        // '' (not 'default'!)
const value5 = false ?? 'default';     // false (not 'default'!)

// vs OR operator (||)
const value6 = 0 || 'default';         // 'default' (0 is falsy)
const value7 = '' || 'default';        // 'default' ('' is falsy)
const value8 = false || 'default';     // 'default' (false is falsy)
```

**When to use:**
- Use `??` when you want to keep falsy values like `0`, `''`, `false`
- Use `||` when you want to replace all falsy values

### Cypress Usage

```javascript
describe('Nullish Coalescing', () => {
  it('should use environment variables with defaults', () => {
    const timeout = Cypress.env('TIMEOUT') ?? 5000;
    const retries = Cypress.env('RETRIES') ?? 2;
    const baseUrl = Cypress.env('BASE_URL') ?? 'http://localhost:3000';
    
    cy.log(`Timeout: ${timeout}ms`);
    cy.log(`Retries: ${retries}`);
    cy.log(`Base URL: ${baseUrl}`);
  });
  
  it('should handle optional config', () => {
    const config = {
      maxRetries: 0, // 0 is valid!
      timeout: null  // null should use default
    };
    
    const retries = config.maxRetries ?? 3; // 0 (keeps 0)
    const timeout = config.timeout ?? 5000;  // 5000 (null replaced)
    
    expect(retries).to.equal(0);
    expect(timeout).to.equal(5000);
  });
});
```

---

## 6️⃣ Modules (import/export)

Modules memungkinkan code organization yang lebih baik.

### Exporting

```javascript
// utils/helpers.js

// Named exports
export const generateEmail = (prefix = 'user') => {
  return `${prefix}_${Date.now()}@test.com`;
};

export const formatCurrency = (amount) => {
  return `$${amount.toFixed(2)}`;
};

export const randomString = (length = 10) => {
  return Math.random().toString(36).substring(2, length + 2);
};

// Default export
export default class TestHelper {
  static generateUserId() {
    return `user_${Date.now()}`;
  }
  
  static wait(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
}
```

### Importing

```javascript
// test file

// Import default export
import TestHelper from '../utils/helpers';

// Import named exports
import { generateEmail, formatCurrency } from '../utils/helpers';

// Import all as namespace
import * as helpers from '../utils/helpers';

// Import with alias
import { generateEmail as createEmail } from '../utils/helpers';

describe('Module Examples', () => {
  it('should use imported functions', () => {
    const email = generateEmail('testuser');
    const price = formatCurrency(29.99);
    const userId = TestHelper.generateUserId();
    
    cy.log(`Email: ${email}`);
    cy.log(`Price: ${price}`);
    cy.log(`User ID: ${userId}`);
  });
  
  it('should use namespace import', () => {
    const email = helpers.generateEmail();
    const random = helpers.randomString(15);
    
    cy.log(`Email: ${email}`);
    cy.log(`Random: ${random}`);
  });
});
```

---

## 🎯 Praktik: Real-World Example

### Complete Helper Module

```javascript
// cypress/support/utils/testHelpers.js

// Generate test data
export const generateTestUser = (role = 'user') => {
  const timestamp = Date.now();
  return {
    username: `${role}_${timestamp}`,
    email: `${role}_${timestamp}@test.com`,
    password: 'TestPass123!',
    role,
    createdAt: new Date().toISOString()
  };
};

// Validate data
export const validateEmail = (email) => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};

export const validatePassword = (password) => {
  return password.length >= 8 && 
         /[A-Z]/.test(password) && 
         /[a-z]/.test(password) && 
         /[0-9]/.test(password);
};

// Format data
export const formatUserData = (users) => {
  return users.map(user => ({
    ...user,
    fullName: `${user.firstName ?? ''} ${user.lastName ?? ''}`.trim(),
    isActive: user.isActive ?? true,
    displayRole: user.role?.toUpperCase() ?? 'USER'
  }));
};

// Wait utilities
export const waitFor = (condition, timeout = 5000) => {
  const startTime = Date.now();
  
  return new Promise((resolve, reject) => {
    const check = () => {
      if (condition()) {
        resolve(true);
      } else if (Date.now() - startTime > timeout) {
        reject(new Error('Timeout waiting for condition'));
      } else {
        setTimeout(check, 100);
      }
    };
    check();
  });
};

// Default export
export default {
  generateTestUser,
  validateEmail,
  validatePassword,
  formatUserData,
  waitFor
};
```

### Usage in Tests

```javascript
import {
  generateTestUser,
  validateEmail,
  formatUserData
} from '../support/utils/testHelpers';

describe('User Registration with Helpers', () => {
  it('should register user with generated data', () => {
    const testUser = generateTestUser('admin');
    
    // Validate before using
    expect(validateEmail(testUser.email)).to.be.true;
    
    cy.visit('/register');
    cy.get('#username').type(testUser.username);
    cy.get('#email').type(testUser.email);
    cy.get('#password').type(testUser.password);
    cy.get('#role').select(testUser.role);
    cy.get('button[type="submit"]').click();
    
    cy.url().should('include', '/welcome');
    cy.get('.success-message').should('contain', testUser.username);
  });
  
  it('should format user data correctly', () => {
    const rawUsers = [
      { firstName: 'John', lastName: 'Doe', role: 'admin' },
      { firstName: 'Jane', lastName: 'Smith' } // no role
    ];
    
    const formattedUsers = formatUserData(rawUsers);
    
    expect(formattedUsers[0].fullName).to.equal('John Doe');
    expect(formattedUsers[0].displayRole).to.equal('ADMIN');
    expect(formattedUsers[1].isActive).to.be.true;
    expect(formattedUsers[1].displayRole).to.equal('USER');
  });
});
```

---

## 🎓 Ringkasan Minggu 6

### Yang Sudah Dipelajari:

✅ **Template Literals** - String interpolation dan multi-line strings
✅ **Default Parameters** - Function parameters dengan default values
✅ **Rest Parameters** - Accept unlimited arguments
✅ **Optional Chaining** - Safe property access
✅ **Nullish Coalescing** - Better default values
✅ **Modules** - Code organization dengan import/export

---

### Key Takeaways:

1. **Template literals** membuat string manipulation lebih mudah
2. **Default parameters** mengurangi boilerplate code
3. **Optional chaining** mencegah errors dari null/undefined
4. **Modules** meningkatkan code organization

---

### Checklist Penguasaan:

- [ ] Menggunakan template literals untuk strings
- [ ] Implement default parameters
- [ ] Understand optional chaining
- [ ] Use nullish coalescing correctly
- [ ] Organize code dengan modules

**Next Week:** Minggu 7 - DOM Manipulation & jQuery! 🚀
