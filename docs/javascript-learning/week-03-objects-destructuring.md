# Minggu 3: Objects & Destructuring

## 📚 Pengantar

Minggu ini kita akan deep dive ke dalam **Objects** - salah satu struktur data paling penting dalam JavaScript. Objects memungkinkan kita untuk organize data yang complex dan membuat code yang lebih maintainable. Kita juga akan belajar **Destructuring**, teknik modern untuk extract data dari objects dan arrays dengan cara yang elegant.

---

## 1️⃣ Objects - Deep Dive

### Apa itu Objects?

Object adalah collection dari **key-value pairs**. Bayangkan seperti kamus dimana setiap kata (key) memiliki definisi (value).

```javascript
const user = {
  name: 'John Doe',
  email: 'john@example.com',
  age: 30,
  isActive: true
};
```

---

### Creating Objects

**Object Literal (Paling Umum):**
```javascript
const person = {
  firstName: 'Alice',
  lastName: 'Smith',
  age: 25,
  email: 'alice@example.com'
};
```

**Empty Object:**
```javascript
const emptyObj = {};
```

**Object with Methods:**
```javascript
const calculator = {
  add: function(a, b) {
    return a + b;
  },
  subtract: function(a, b) {
    return a - b;
  }
};

console.log(calculator.add(5, 3)); // 8
```

**Shorthand Method Syntax (ES6):**
```javascript
const calculator = {
  add(a, b) {
    return a + b;
  },
  subtract(a, b) {
    return a - b;
  }
};
```

---

### Accessing Object Properties

**Dot Notation:**
```javascript
const user = {
  name: 'John',
  email: 'john@example.com'
};

console.log(user.name);  // 'John'
console.log(user.email); // 'john@example.com'
```

**Bracket Notation:**
```javascript
console.log(user['name']);  // 'John'
console.log(user['email']); // 'john@example.com'

// Useful for dynamic keys
const key = 'name';
console.log(user[key]); // 'John'

// Keys with spaces or special characters
const obj = {
  'first name': 'John',
  'user-email': 'john@example.com'
};

console.log(obj['first name']);  // 'John'
console.log(obj['user-email']); // 'john@example.com'
```

---

### Adding and Modifying Properties

```javascript
const user = {
  name: 'John'
};

// Add new property
user.email = 'john@example.com';
user.age = 30;

// Modify existing property
user.name = 'John Doe';

console.log(user);
// {
//   name: 'John Doe',
//   email: 'john@example.com',
//   age: 30
// }
```

---

### Deleting Properties

```javascript
const user = {
  name: 'John',
  email: 'john@example.com',
  tempData: 'temporary'
};

delete user.tempData;

console.log(user);
// {
//   name: 'John',
//   email: 'john@example.com'
// }
```

---

### Checking if Property Exists

```javascript
const user = {
  name: 'John',
  email: 'john@example.com'
};

// Using 'in' operator
console.log('name' in user);    // true
console.log('age' in user);     // false

// Using hasOwnProperty
console.log(user.hasOwnProperty('name'));  // true
console.log(user.hasOwnProperty('age'));   // false

// Check for undefined
console.log(user.name !== undefined);  // true
console.log(user.age !== undefined);   // false
```

---

### Nested Objects

```javascript
const user = {
  name: 'John Doe',
  contact: {
    email: 'john@example.com',
    phone: '123-456-7890',
    address: {
      street: '123 Main St',
      city: 'New York',
      country: 'USA'
    }
  },
  preferences: {
    theme: 'dark',
    language: 'en'
  }
};

// Accessing nested properties
console.log(user.contact.email);              // 'john@example.com'
console.log(user.contact.address.city);       // 'New York'
console.log(user.preferences.theme);          // 'dark'

// Modifying nested properties
user.contact.phone = '098-765-4321';
user.contact.address.city = 'Los Angeles';
```

---

### Object Methods

```javascript
const user = {
  firstName: 'John',
  lastName: 'Doe',
  age: 30,
  
  // Method using 'this' keyword
  getFullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  
  greet() {
    return `Hello, I'm ${this.getFullName()}`;
  },
  
  haveBirthday() {
    this.age++;
    return `Happy birthday! You are now ${this.age}`;
  }
};

console.log(user.getFullName());  // 'John Doe'
console.log(user.greet());        // "Hello, I'm John Doe"
console.log(user.haveBirthday()); // 'Happy birthday! You are now 31'
console.log(user.age);            // 31
```

---

### The 'this' Keyword

`this` refers to the object that the method belongs to.

```javascript
const person = {
  name: 'Alice',
  sayHello() {
    console.log(`Hello, I'm ${this.name}`);
  }
};

person.sayHello(); // "Hello, I'm Alice"

// Arrow functions don't have their own 'this'
const person2 = {
  name: 'Bob',
  sayHello: () => {
    console.log(`Hello, I'm ${this.name}`); // 'this' is undefined
  }
};
```

**⚠️ Important:** Gunakan regular functions (bukan arrow functions) untuk object methods yang perlu akses ke `this`.

---

### Object.keys(), Object.values(), Object.entries()

```javascript
const user = {
  name: 'John',
  email: 'john@example.com',
  age: 30
};

// Get all keys
const keys = Object.keys(user);
console.log(keys); // ['name', 'email', 'age']

// Get all values
const values = Object.values(user);
console.log(values); // ['John', 'john@example.com', 30]

// Get key-value pairs
const entries = Object.entries(user);
console.log(entries);
// [
//   ['name', 'John'],
//   ['email', 'john@example.com'],
//   ['age', 30]
// ]

// Iterate over object
Object.keys(user).forEach(key => {
  console.log(`${key}: ${user[key]}`);
});
// name: John
// email: john@example.com
// age: 30
```

---

### Spread Operator with Objects

```javascript
const user = {
  name: 'John',
  email: 'john@example.com'
};

// Copy object
const userCopy = { ...user };
console.log(userCopy); // { name: 'John', email: 'john@example.com' }

// Add properties while copying
const extendedUser = {
  ...user,
  age: 30,
  role: 'admin'
};
console.log(extendedUser);
// {
//   name: 'John',
//   email: 'john@example.com',
//   age: 30,
//   role: 'admin'
// }

// Merge objects
const contact = { phone: '123-456-7890' };
const address = { city: 'New York', country: 'USA' };

const fullInfo = { ...user, ...contact, ...address };
console.log(fullInfo);
// {
//   name: 'John',
//   email: 'john@example.com',
//   phone: '123-456-7890',
//   city: 'New York',
//   country: 'USA'
// }

// Override properties
const updatedUser = {
  ...user,
  email: 'newemail@example.com' // Override
};
console.log(updatedUser);
// { name: 'John', email: 'newemail@example.com' }
```

---

## 2️⃣ Destructuring

Destructuring adalah syntax untuk extract values dari objects atau arrays ke dalam variables.

### Object Destructuring

**Basic Syntax:**
```javascript
const user = {
  name: 'John Doe',
  email: 'john@example.com',
  age: 30
};

// Without destructuring
const name = user.name;
const email = user.email;
const age = user.age;

// With destructuring (cleaner!)
const { name, email, age } = user;

console.log(name);  // 'John Doe'
console.log(email); // 'john@example.com'
console.log(age);   // 30
```

**Renaming Variables:**
```javascript
const user = {
  name: 'John Doe',
  email: 'john@example.com'
};

// Rename while destructuring
const { name: userName, email: userEmail } = user;

console.log(userName);  // 'John Doe'
console.log(userEmail); // 'john@example.com'
// console.log(name);   // Error: name is not defined
```

**Default Values:**
```javascript
const user = {
  name: 'John Doe',
  email: 'john@example.com'
};

// Provide default if property doesn't exist
const { name, email, age = 25, role = 'user' } = user;

console.log(name);  // 'John Doe'
console.log(email); // 'john@example.com'
console.log(age);   // 25 (default)
console.log(role);  // 'user' (default)
```

**Nested Destructuring:**
```javascript
const user = {
  name: 'John Doe',
  contact: {
    email: 'john@example.com',
    phone: '123-456-7890',
    address: {
      city: 'New York',
      country: 'USA'
    }
  }
};

// Destructure nested objects
const {
  name,
  contact: {
    email,
    address: { city, country }
  }
} = user;

console.log(name);    // 'John Doe'
console.log(email);   // 'john@example.com'
console.log(city);    // 'New York'
console.log(country); // 'USA'
```

---

### Array Destructuring

```javascript
const colors = ['red', 'green', 'blue'];

// Without destructuring
const first = colors[0];
const second = colors[1];

// With destructuring
const [firstColor, secondColor, thirdColor] = colors;

console.log(firstColor);  // 'red'
console.log(secondColor); // 'green'
console.log(thirdColor);  // 'blue'

// Skip elements
const [, , third] = colors;
console.log(third); // 'blue'

// Rest operator
const [primary, ...otherColors] = colors;
console.log(primary);      // 'red'
console.log(otherColors);  // ['green', 'blue']
```

---

### Destructuring in Function Parameters

**Object Parameters:**
```javascript
// Without destructuring
function createUser(userObj) {
  console.log(userObj.name);
  console.log(userObj.email);
  console.log(userObj.age);
}

// With destructuring (cleaner!)
function createUser({ name, email, age = 18 }) {
  console.log(name);
  console.log(email);
  console.log(age);
}

createUser({
  name: 'John',
  email: 'john@example.com',
  age: 30
});
```

**Array Parameters:**
```javascript
function getCoordinates([x, y]) {
  console.log(`X: ${x}, Y: ${y}`);
}

getCoordinates([10, 20]); // X: 10, Y: 20
```

---

## 3️⃣ Objects dalam Cypress

### Page Object Model (POM)

```javascript
// cypress/support/pages/LoginPage.js

class LoginPage {
  // Elements as object
  elements = {
    usernameInput: () => cy.get('#username'),
    passwordInput: () => cy.get('#password'),
    submitButton: () => cy.get('button[type="submit"]'),
    errorMessage: () => cy.get('.error-message'),
    successMessage: () => cy.get('.success-message')
  };
  
  // Methods
  visit() {
    cy.visit('/login');
  }
  
  fillUsername(username) {
    this.elements.usernameInput().clear().type(username);
  }
  
  fillPassword(password) {
    this.elements.passwordInput().clear().type(password);
  }
  
  submit() {
    this.elements.submitButton().click();
  }
  
  // Method with object parameter and destructuring
  login({ username, password }) {
    this.fillUsername(username);
    this.fillPassword(password);
    this.submit();
  }
  
  verifyErrorMessage(message) {
    this.elements.errorMessage()
      .should('be.visible')
      .and('contain', message);
  }
  
  verifySuccessMessage() {
    this.elements.successMessage().should('be.visible');
  }
}

export default LoginPage;
```

**Usage in Test:**
```javascript
import LoginPage from '../support/pages/LoginPage';

describe('Login Tests with POM', () => {
  const loginPage = new LoginPage();
  
  beforeEach(() => {
    loginPage.visit();
  });
  
  it('should login with valid credentials', () => {
    // Using destructured object parameter
    loginPage.login({
      username: 'testuser',
      password: 'TestPass123!'
    });
    
    cy.url().should('include', '/dashboard');
  });
  
  it('should show error for invalid credentials', () => {
    loginPage.login({
      username: 'invalid',
      password: 'wrong'
    });
    
    loginPage.verifyErrorMessage('Invalid credentials');
  });
});
```

---

### Test Data Management with Objects

```javascript
// cypress/fixtures/testData.js

export const users = {
  admin: {
    username: 'admin',
    password: 'Admin123!',
    email: 'admin@example.com',
    role: 'admin',
    permissions: ['read', 'write', 'delete']
  },
  
  regularUser: {
    username: 'user',
    password: 'User123!',
    email: 'user@example.com',
    role: 'user',
    permissions: ['read']
  },
  
  moderator: {
    username: 'moderator',
    password: 'Mod123!',
    email: 'mod@example.com',
    role: 'moderator',
    permissions: ['read', 'write']
  }
};

export const testConfig = {
  baseUrl: 'https://staging.example.com',
  apiUrl: 'https://api.staging.example.com',
  timeout: 10000,
  retries: 3
};

export const selectors = {
  login: {
    usernameInput: '#username',
    passwordInput: '#password',
    submitButton: 'button[type="submit"]',
    errorMessage: '.error-message'
  },
  dashboard: {
    welcomeMessage: '.welcome-message',
    userMenu: '.user-menu',
    logoutButton: '.logout-button'
  }
};
```

**Usage with Destructuring:**
```javascript
import { users, selectors } from '../fixtures/testData';

describe('User Management Tests', () => {
  it('should login as admin', () => {
    // Destructure admin user
    const { username, password } = users.admin;
    
    cy.visit('/login');
    cy.get(selectors.login.usernameInput).type(username);
    cy.get(selectors.login.passwordInput).type(password);
    cy.get(selectors.login.submitButton).click();
    
    cy.url().should('include', '/admin');
  });
  
  it('should verify user permissions', () => {
    const { regularUser, admin } = users;
    
    // Test regular user
    cy.login(regularUser.username, regularUser.password);
    expect(regularUser.permissions).to.include('read');
    expect(regularUser.permissions).to.not.include('delete');
    
    // Test admin
    cy.login(admin.username, admin.password);
    expect(admin.permissions).to.include('delete');
  });
});
```

---

### Custom Commands with Object Parameters

```javascript
// cypress/support/commands.js

// Command with object parameter
Cypress.Commands.add('loginWithCredentials', ({ username, password, rememberMe = false }) => {
  cy.visit('/login');
  cy.get('#username').type(username);
  cy.get('#password').type(password);
  
  if (rememberMe) {
    cy.get('#remember-me').check();
  }
  
  cy.get('button[type="submit"]').click();
});

// Command that returns object
Cypress.Commands.add('getUserInfo', () => {
  return cy.window().then((win) => {
    return {
      username: win.localStorage.getItem('username'),
      token: win.localStorage.getItem('authToken'),
      role: win.localStorage.getItem('userRole')
    };
  });
});

// Usage
describe('Custom Commands with Objects', () => {
  it('should login and verify user info', () => {
    cy.loginWithCredentials({
      username: 'testuser',
      password: 'TestPass123!',
      rememberMe: true
    });
    
    cy.getUserInfo().then((userInfo) => {
      const { username, token, role } = userInfo;
      
      expect(username).to.equal('testuser');
      expect(token).to.not.be.null;
      expect(role).to.equal('user');
    });
  });
});
```

---

### Working with API Responses

```javascript
describe('API Testing with Objects', () => {
  it('should verify user API response structure', () => {
    cy.request('GET', '/api/users/1').then((response) => {
      // Destructure response
      const { status, body } = response;
      
      expect(status).to.equal(200);
      
      // Destructure body
      const { id, name, email, address } = body;
      
      expect(id).to.be.a('number');
      expect(name).to.be.a('string');
      expect(email).to.include('@');
      
      // Nested destructuring
      const { city, country } = address;
      expect(city).to.not.be.empty;
      expect(country).to.not.be.empty;
    });
  });
  
  it('should create user with object data', () => {
    const newUser = {
      name: 'Test User',
      email: 'test@example.com',
      age: 25,
      address: {
        street: '123 Main St',
        city: 'New York',
        country: 'USA'
      }
    };
    
    cy.request('POST', '/api/users', newUser).then((response) => {
      expect(response.status).to.equal(201);
      
      const { body } = response;
      expect(body).to.include(newUser);
      expect(body).to.have.property('id');
    });
  });
});
```

---

## 🎯 Praktik: Real-World Examples

### Example 1: Complete Page Object Model

```javascript
// cypress/support/pages/DashboardPage.js

class DashboardPage {
  elements = {
    header: () => cy.get('.dashboard-header'),
    userProfile: () => cy.get('.user-profile'),
    statsCards: () => cy.get('.stats-card'),
    recentActivity: () => cy.get('.recent-activity'),
    navigationMenu: () => cy.get('.nav-menu')
  };
  
  visit() {
    cy.visit('/dashboard');
  }
  
  verifyPageLoaded() {
    this.elements.header().should('be.visible');
    this.elements.userProfile().should('be.visible');
  }
  
  getUserProfile() {
    return this.elements.userProfile().then($profile => {
      return {
        name: $profile.find('.user-name').text(),
        email: $profile.find('.user-email').text(),
        role: $profile.find('.user-role').text()
      };
    });
  }
  
  getStats() {
    return this.elements.statsCards().then($cards => {
      const stats = {};
      $cards.each((index, card) => {
        const $card = Cypress.$(card);
        const label = $card.find('.stat-label').text();
        const value = $card.find('.stat-value').text();
        stats[label] = value;
      });
      return stats;
    });
  }
  
  navigateTo(section) {
    this.elements.navigationMenu()
      .contains(section)
      .click();
  }
}

export default DashboardPage;

// Usage in test
import DashboardPage from '../support/pages/DashboardPage';

describe('Dashboard Tests', () => {
  const dashboard = new DashboardPage();
  
  beforeEach(() => {
    cy.login('testuser', 'password');
    dashboard.visit();
  });
  
  it('should display user profile correctly', () => {
    dashboard.getUserProfile().then((profile) => {
      const { name, email, role } = profile;
      
      expect(name).to.equal('Test User');
      expect(email).to.equal('test@example.com');
      expect(role).to.equal('User');
    });
  });
  
  it('should display correct statistics', () => {
    dashboard.getStats().then((stats) => {
      expect(stats['Total Users']).to.not.be.empty;
      expect(stats['Active Sessions']).to.not.be.empty;
    });
  });
});
```

---

### Example 2: Configuration Management

```javascript
// cypress/config/environments.js

export const environments = {
  development: {
    baseUrl: 'http://localhost:3000',
    apiUrl: 'http://localhost:4000',
    database: 'dev_db',
    features: {
      newDashboard: true,
      betaFeatures: true
    }
  },
  
  staging: {
    baseUrl: 'https://staging.example.com',
    apiUrl: 'https://api.staging.example.com',
    database: 'staging_db',
    features: {
      newDashboard: true,
      betaFeatures: false
    }
  },
  
  production: {
    baseUrl: 'https://example.com',
    apiUrl: 'https://api.example.com',
    database: 'prod_db',
    features: {
      newDashboard: false,
      betaFeatures: false
    }
  }
};

export const getConfig = (env = 'development') => {
  return environments[env];
};

// Usage
import { getConfig } from '../config/environments';

describe('Environment-specific Tests', () => {
  it('should use correct environment', () => {
    const env = Cypress.env('ENVIRONMENT') || 'development';
    const { baseUrl, apiUrl, features } = getConfig(env);
    
    cy.visit(baseUrl);
    
    if (features.newDashboard) {
      cy.get('.new-dashboard').should('be.visible');
    } else {
      cy.get('.old-dashboard').should('be.visible');
    }
  });
});
```

---

## 🎓 Ringkasan Minggu 3

### Yang Sudah Dipelajari:

✅ **Objects:**
- Object literals dan properties
- Accessing dan modifying properties
- Nested objects
- Object methods dan `this` keyword
- Object.keys(), Object.values(), Object.entries()
- Spread operator dengan objects

✅ **Destructuring:**
- Object destructuring
- Array destructuring
- Nested destructuring
- Default values
- Renaming variables
- Destructuring in function parameters

✅ **Cypress Applications:**
- Page Object Model implementation
- Test data management
- Custom commands dengan object parameters
- API testing dengan objects

---

### Key Takeaways untuk Cypress:

1. **Objects perfect untuk organize data** - Test data, configurations, selectors
2. **Destructuring makes code cleaner** - Especially in function parameters
3. **POM pattern** meningkatkan maintainability
4. **Spread operator** memudahkan object manipulation
5. **`this` keyword** penting untuk object methods

---

### Checklist Penguasaan:

- [ ] Membuat dan manipulasi objects
- [ ] Bekerja dengan nested objects
- [ ] Menggunakan object methods
- [ ] Memahami `this` keyword
- [ ] Menguasai object destructuring
- [ ] Implementasi Page Object Model
- [ ] Manage test data dengan objects
- [ ] Menggunakan spread operator

**Next Week:** Minggu 4 - Promises & Async/Await! 🚀
