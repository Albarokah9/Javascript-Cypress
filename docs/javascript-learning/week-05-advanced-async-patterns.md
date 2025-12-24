# Minggu 5: Advanced Async Patterns dalam Cypress

## 📚 Pengantar

Minggu ini kita akan deep dive ke dalam **Cypress-specific async patterns**. Anda akan belajar bagaimana menggunakan `cy.intercept()` untuk network testing, `cy.task()` untuk Node.js operations, dan aliasing untuk membuat tests lebih maintainable.

---

## 1️⃣ cy.intercept() - Network Mocking

`cy.intercept()` adalah salah satu fitur paling powerful di Cypress untuk testing network requests.

### Basic Intercept

```javascript
describe('Network Mocking with cy.intercept()', () => {
  it('should intercept and wait for API call', () => {
    // Setup intercept BEFORE action that triggers it
    cy.intercept('POST', '/api/login').as('loginRequest');
    
    cy.visit('/login');
    cy.get('#username').type('testuser');
    cy.get('#password').type('password123');
    cy.get('button[type="submit"]').click();
    
    // Wait for the request
    cy.wait('@loginRequest').then((interception) => {
      expect(interception.response.statusCode).to.equal(200);
      expect(interception.response.body).to.have.property('token');
      
      const { token, user } = interception.response.body;
      cy.log(`Token: ${token}`);
      cy.log(`User: ${user.username}`);
    });
  });
});
```

---

### Mocking API Response

```javascript
describe('Mocking API Responses', () => {
  it('should mock API response', () => {
    // Mock the response
    cy.intercept('GET', '/api/users', {
      statusCode: 200,
      body: [
        { id: 1, name: 'Alice', role: 'admin' },
        { id: 2, name: 'Bob', role: 'user' }
      ]
    }).as('getUsers');
    
    cy.visit('/users');
    cy.wait('@getUsers');
    
    cy.get('.user-list').should('contain', 'Alice');
    cy.get('.user-list').should('contain', 'Bob');
  });
  
  it('should mock with fixture file', () => {
    cy.intercept('GET', '/api/products', {
      fixture: 'products.json'
    }).as('getProducts');
    
    cy.visit('/products');
    cy.wait('@getProducts');
  });
});
```

---

### Modifying Requests

```javascript
describe('Modifying Requests', () => {
  it('should modify request before sending', () => {
    cy.intercept('POST', '/api/users', (req) => {
      // Modify request body
      req.body.timestamp = Date.now();
      req.body.source = 'cypress-test';
      
      // Modify headers
      req.headers['X-Custom-Header'] = 'test-value';
      
      // Continue with modified request
      req.continue();
    }).as('createUser');
    
    cy.get('#create-user-btn').click();
    cy.wait('@createUser');
  });
  
  it('should modify response', () => {
    cy.intercept('GET', '/api/user/profile', (req) => {
      req.continue((res) => {
        // Modify response
        res.body.isTestUser = true;
        res.body.environment = 'test';
      });
    }).as('getProfile');
    
    cy.visit('/profile');
    cy.wait('@getProfile');
  });
});
```

---

### Simulating Network Errors

```javascript
describe('Network Error Simulation', () => {
  it('should simulate 500 error', () => {
    cy.intercept('GET', '/api/data', {
      statusCode: 500,
      body: { error: 'Internal Server Error' }
    });
    
    cy.visit('/dashboard');
    cy.get('.error-message').should('be.visible');
    cy.get('.error-message').should('contain', 'Internal Server Error');
  });
  
  it('should simulate network timeout', () => {
    cy.intercept('GET', '/api/slow-endpoint', (req) => {
      req.destroy(); // Simulate network failure
    });
    
    cy.visit('/data');
    cy.get('.timeout-error').should('be.visible');
  });
  
  it('should simulate slow network', () => {
    cy.intercept('GET', '/api/data', (req) => {
      req.continue((res) => {
        res.delay = 5000; // 5 second delay
      });
    }).as('slowRequest');
    
    cy.visit('/dashboard');
    cy.get('.loading-spinner').should('be.visible');
    cy.wait('@slowRequest');
    cy.get('.loading-spinner').should('not.exist');
  });
});
```

---

### Multiple Intercepts

```javascript
describe('Multiple Intercepts', () => {
  it('should handle multiple API calls', () => {
    cy.intercept('GET', '/api/users').as('getUsers');
    cy.intercept('GET', '/api/posts').as('getPosts');
    cy.intercept('GET', '/api/comments').as('getComments');
    
    cy.visit('/dashboard');
    
    // Wait for all requests
    cy.wait(['@getUsers', '@getPosts', '@getComments']).then((interceptions) => {
      const [users, posts, comments] = interceptions;
      
      expect(users.response.statusCode).to.equal(200);
      expect(posts.response.statusCode).to.equal(200);
      expect(comments.response.statusCode).to.equal(200);
    });
  });
});
```

---

## 2️⃣ cy.task() - Node.js Operations

`cy.task()` memungkinkan Anda menjalankan code Node.js dari tests Cypress.

### Setup cy.task()

```javascript
// cypress.config.js
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  e2e: {
    setupNodeEvents(on, config) {
      on('task', {
        // Simple task
        log(message) {
          console.log(message);
          return null;
        },
        
        // Async task
        async fetchExternalData() {
          const response = await fetch('https://api.example.com/data');
          const data = await response.json();
          return data;
        },
        
        // Database operation
        async queryDatabase(query) {
          // Your database logic here
          const db = require('./db');
          const results = await db.query(query);
          return results;
        },
        
        // File operation
        async readFile(filename) {
          const fs = require('fs').promises;
          const content = await fs.readFile(filename, 'utf8');
          return content;
        },
        
        // Generate test data
        generateTestUser() {
          return {
            username: `user_${Date.now()}`,
            email: `user_${Date.now()}@test.com`,
            password: 'TestPass123!'
          };
        }
      });
    }
  }
});
```

---

### Using cy.task() in Tests

```javascript
describe('cy.task() Examples', () => {
  it('should execute Node.js task', () => {
    cy.task('fetchExternalData').then((data) => {
      expect(data).to.not.be.null;
      cy.log('External data:', data);
    });
  });
  
  it('should query database', () => {
    cy.task('queryDatabase', 'SELECT * FROM users WHERE role = "admin"')
      .then((results) => {
        expect(results).to.be.an('array');
        expect(results.length).to.be.greaterThan(0);
      });
  });
  
  it('should read file', () => {
    cy.task('readFile', 'cypress/fixtures/config.json').then((content) => {
      const config = JSON.parse(content);
      expect(config).to.have.property('apiUrl');
    });
  });
  
  it('should generate test data', () => {
    cy.task('generateTestUser').then((user) => {
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

## 3️⃣ Aliasing dengan cy.as()

Aliasing membuat code lebih readable dan memungkinkan reuse values.

### Aliasing Elements

```javascript
describe('Aliasing Elements', () => {
  it('should use aliases for elements', () => {
    cy.get('.user-list').as('userList');
    cy.get('.search-input').as('searchInput');
    cy.get('.filter-dropdown').as('filterDropdown');
    
    // Reuse aliases
    cy.get('@userList').should('be.visible');
    cy.get('@searchInput').type('John');
    cy.get('@userList').find('li').should('have.length.greaterThan', 0);
    
    cy.get('@filterDropdown').select('Active');
    cy.get('@userList').find('.active-user').should('exist');
  });
});
```

---

### Aliasing Requests

```javascript
describe('Aliasing Requests', () => {
  it('should use aliases for requests', () => {
    cy.intercept('GET', '/api/users').as('getUsers');
    cy.intercept('POST', '/api/users').as('createUser');
    cy.intercept('PUT', '/api/users/*').as('updateUser');
    cy.intercept('DELETE', '/api/users/*').as('deleteUser');
    
    // Get users
    cy.visit('/users');
    cy.wait('@getUsers').its('response.statusCode').should('equal', 200);
    
    // Create user
    cy.get('#create-user-btn').click();
    cy.wait('@createUser').then((interception) => {
      expect(interception.response.statusCode).to.equal(201);
      const userId = interception.response.body.id;
      cy.wrap(userId).as('newUserId');
    });
    
    // Update user
    cy.get('@newUserId').then((userId) => {
      cy.get(`[data-user-id="${userId}"] .edit-btn`).click();
      cy.get('#username').clear().type('Updated Name');
      cy.get('#save-btn').click();
      cy.wait('@updateUser');
    });
  });
});
```

---

### Aliasing Values

```javascript
describe('Aliasing Values', () => {
  it('should alias values for reuse', () => {
    cy.get('.total-price')
      .invoke('text')
      .as('totalPrice');
    
    cy.get('.tax-amount')
      .invoke('text')
      .as('taxAmount');
    
    // Use aliased values
    cy.get('@totalPrice').then((total) => {
      cy.get('@taxAmount').then((tax) => {
        const totalNum = parseFloat(total.replace('$', ''));
        const taxNum = parseFloat(tax.replace('$', ''));
        
        cy.log(`Total: ${totalNum}, Tax: ${taxNum}`);
        expect(totalNum).to.be.greaterThan(taxNum);
      });
    });
  });
  
  it('should alias fixture data', () => {
    cy.fixture('users.json').as('testUsers');
    
    cy.get('@testUsers').then((users) => {
      users.forEach((user) => {
        cy.log(`Testing user: ${user.username}`);
        cy.visit('/login');
        cy.get('#username').type(user.username);
        cy.get('#password').type(user.password);
        cy.get('button[type="submit"]').click();
        cy.url().should('include', '/dashboard');
        cy.get('.logout').click();
      });
    });
  });
});
```

---

## 4️⃣ Complex Async Scenarios

### Sequential API Calls

```javascript
describe('Sequential API Calls', () => {
  it('should make dependent API calls', () => {
    // First request
    cy.request('POST', '/api/auth/login', {
      username: 'testuser',
      password: 'password123'
    }).then((loginResponse) => {
      const token = loginResponse.body.token;
      
      // Second request using token from first
      return cy.request({
        method: 'GET',
        url: '/api/user/profile',
        headers: {
          'Authorization': `Bearer ${token}`
        }
      });
    }).then((profileResponse) => {
      const userId = profileResponse.body.id;
      
      // Third request using userId from second
      return cy.request(`/api/users/${userId}/posts`);
    }).then((postsResponse) => {
      expect(postsResponse.status).to.equal(200);
      expect(postsResponse.body).to.be.an('array');
    });
  });
});
```

---

### Parallel API Calls

```javascript
describe('Parallel API Calls', () => {
  it('should make parallel requests', () => {
    cy.wrap(null).then(() => {
      return Promise.all([
        cy.request('/api/users'),
        cy.request('/api/posts'),
        cy.request('/api/comments')
      ]);
    }).then(([usersResponse, postsResponse, commentsResponse]) => {
      expect(usersResponse.status).to.equal(200);
      expect(postsResponse.status).to.equal(200);
      expect(commentsResponse.status).to.equal(200);
      
      cy.log(`Users: ${usersResponse.body.length}`);
      cy.log(`Posts: ${postsResponse.body.length}`);
      cy.log(`Comments: ${commentsResponse.body.length}`);
    });
  });
});
```

---

## 🎯 Real-World Example

### Complete Login Flow with API Validation

```javascript
describe('Complete Login Flow', () => {
  beforeEach(() => {
    // Setup intercepts
    cy.intercept('POST', '/api/auth/login').as('loginAPI');
    cy.intercept('GET', '/api/user/profile').as('profileAPI');
    cy.intercept('GET', '/api/user/preferences').as('preferencesAPI');
  });
  
  it('should login and verify via multiple APIs', () => {
    // Perform login
    cy.visit('/login');
    cy.get('#username').type('testuser');
    cy.get('#password').type('TestPass123!');
    cy.get('button[type="submit"]').click();
    
    // Wait and verify login API
    cy.wait('@loginAPI').then((interception) => {
      const { response } = interception;
      
      expect(response.statusCode).to.equal(200);
      expect(response.body).to.have.property('token');
      expect(response.body).to.have.property('user');
      
      const { token, user } = response.body;
      
      // Store token for later use
      cy.wrap(token).as('authToken');
      cy.wrap(user.id).as('userId');
    });
    
    // Wait for profile API
    cy.wait('@profileAPI').then((interception) => {
      expect(interception.response.statusCode).to.equal(200);
      expect(interception.response.body.username).to.equal('testuser');
    });
    
    // Wait for preferences API
    cy.wait('@preferencesAPI').then((interception) => {
      expect(interception.response.statusCode).to.equal(200);
    });
    
    // Verify UI
    cy.url().should('include', '/dashboard');
    cy.get('.welcome-message').should('contain', 'testuser');
    
    // Use stored token for additional request
    cy.get('@authToken').then((token) => {
      cy.get('@userId').then((userId) => {
        cy.request({
          method: 'GET',
          url: `/api/users/${userId}/activity`,
          headers: {
            'Authorization': `Bearer ${token}`
          }
        }).then((response) => {
          expect(response.status).to.equal(200);
          expect(response.body).to.be.an('array');
        });
      });
    });
  });
});
```

---

## 🎓 Ringkasan Minggu 5

### Yang Sudah Dipelajari:

✅ **cy.intercept():**
- Intercept dan wait untuk requests
- Mock API responses
- Modify requests dan responses
- Simulate network errors
- Handle multiple intercepts

✅ **cy.task():**
- Execute Node.js code
- Database operations
- File operations
- Generate test data

✅ **Aliasing:**
- Alias elements untuk reuse
- Alias requests untuk tracking
- Alias values untuk sharing data

✅ **Complex Scenarios:**
- Sequential API calls
- Parallel API calls
- Dependent requests

---

### Key Takeaways:

1. **cy.intercept()** adalah tool paling powerful untuk network testing
2. **cy.task()** bridge antara Cypress dan Node.js
3. **Aliases** membuat code lebih maintainable
4. **Combine patterns** untuk complex scenarios

---

### Checklist Penguasaan:

- [ ] Mock API responses dengan cy.intercept()
- [ ] Modify requests dan responses
- [ ] Simulate network errors
- [ ] Execute Node.js tasks
- [ ] Use aliases effectively
- [ ] Handle sequential dan parallel requests

**Next Week:** Minggu 6 - ES6+ Features! 🚀
