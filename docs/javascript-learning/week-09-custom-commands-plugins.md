# Minggu 9: Custom Commands & Plugins

## 📚 Pengantar

Custom commands adalah cara powerful untuk membuat reusable code dalam Cypress. Minggu ini kita akan belajar bagaimana membuat custom commands yang professional dan maintainable.

---

## 1️⃣ Creating Custom Commands

### Parent Command

Parent commands start a new chain dan tidak memerlukan previous subject.

```javascript
// cypress/support/commands.js

Cypress.Commands.add('loginViaAPI', (username, password) => {
  cy.request('POST', '/api/auth/login', {
    username,
    password
  }).then((response) => {
    window.localStorage.setItem('token', response.body.token);
    window.localStorage.setItem('user', JSON.stringify(response.body.user));
  });
});

// Usage
describe('Parent Command', () => {
  it('should login via API', () => {
    cy.loginViaAPI('testuser', 'password123');
    cy.visit('/dashboard');
    cy.get('.welcome-message').should('be.visible');
  });
});
```

---

### Child Command

Child commands require a previous subject.

```javascript
// cypress/support/commands.js

Cypress.Commands.add('getByDataCy', { prevSubject: 'element' }, (subject, value) => {
  return cy.wrap(subject).find(`[data-cy="${value}"]`);
});

// Usage
describe('Child Command', () => {
  it('should find element by data-cy', () => {
    cy.get('.container')
      .getByDataCy('submit-button')
      .click();
  });
});
```

---

### Dual Command

Dual commands work with or without a subject.

```javascript
// cypress/support/commands.js

Cypress.Commands.add('dismiss', { prevSubject: 'optional' }, (subject) => {
  if (subject) {
    // Called on a specific element
    cy.wrap(subject).find('.close-button').click();
  } else {
    // Called without subject
    cy.get('.modal .close-button').click();
  }
});

// Usage
describe('Dual Command', () => {
  it('should work without subject', () => {
    cy.dismiss(); // Close any modal
  });
  
  it('should work with subject', () => {
    cy.get('.specific-modal').dismiss(); // Close specific modal
  });
});
```

---

## 2️⃣ Advanced Custom Commands

### Command with Options

```javascript
Cypress.Commands.add('loginWithOptions', (credentials, options = {}) => {
  const {
    rememberMe = false,
    redirect = '/dashboard',
    method = 'UI' // 'UI' or 'API'
  } = options;
  
  if (method === 'API') {
    cy.request('POST', '/api/login', credentials).then((response) => {
      window.localStorage.setItem('token', response.body.token);
    });
    cy.visit(redirect);
  } else {
    cy.visit('/login');
    cy.get('#username').type(credentials.username);
    cy.get('#password').type(credentials.password);
    
    if (rememberMe) {
      cy.get('#remember-me').check();
    }
    
    cy.get('button[type="submit"]').click();
  }
});

// Usage
cy.loginWithOptions(
  { username: 'test', password: 'pass' },
  { rememberMe: true, method: 'API' }
);
```

---

### Command that Returns Value

```javascript
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
describe('Command with Return Value', () => {
  it('should get user info', () => {
    cy.loginViaAPI('testuser', 'password');
    
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

## 3️⃣ Overwriting Commands

### Overwrite type Command

```javascript
// Add logging to type command
Cypress.Commands.overwrite('type', (originalFn, element, text, options) => {
  // Add custom logging
  if (options && options.log !== false) {
    Cypress.log({
      name: 'type',
      message: `Typing: ${options.sensitive ? '***' : text}`
    });
  }
  
  return originalFn(element, text, options);
});

// Usage
cy.get('#password').type('secret', { sensitive: true });
// Logs: "Typing: ***"
```

---

### Overwrite visit Command

```javascript
// Add auth token to all visits
Cypress.Commands.overwrite('visit', (originalVisit, url, options) => {
  const token = window.localStorage.getItem('authToken');
  
  if (token) {
    options = options || {};
    options.headers = options.headers || {};
    options.headers['Authorization'] = `Bearer ${token}`;
  }
  
  return originalVisit(url, options);
});
```

---

## 4️⃣ TypeScript Support

### Type Definitions

```typescript
// cypress/support/commands.ts

declare global {
  namespace Cypress {
    interface Chainable {
      /**
       * Custom command to login via API
       * @param username - User's username
       * @param password - User's password
       * @example cy.loginViaAPI('testuser', 'password123')
       */
      loginViaAPI(username: string, password: string): Chainable<void>;
      
      /**
       * Get element by data-cy attribute
       * @param value - data-cy value
       * @example cy.get('.container').getByDataCy('submit-button')
       */
      getByDataCy(value: string): Chainable<JQuery<HTMLElement>>;
      
      /**
       * Get user information from localStorage
       * @example cy.getUserInfo().then((info) => { ... })
       */
      getUserInfo(): Chainable<{
        username: string;
        token: string;
        role: string;
      }>;
    }
  }
}

Cypress.Commands.add('loginViaAPI', (username: string, password: string) => {
  // Implementation
});

export {};
```

---

## 5️⃣ Command Library Organization

### Organize by Feature

```javascript
// cypress/support/commands/auth.js
Cypress.Commands.add('login', (username, password) => { /* ... */ });
Cypress.Commands.add('logout', () => { /* ... */ });
Cypress.Commands.add('register', (userData) => { /* ... */ });

// cypress/support/commands/api.js
Cypress.Commands.add('apiRequest', (method, endpoint, body) => { /* ... */ });
Cypress.Commands.add('createUser', (userData) => { /* ... */ });
Cypress.Commands.add('deleteUser', (userId) => { /* ... */ });

// cypress/support/commands/ui.js
Cypress.Commands.add('fillForm', (formData) => { /* ... */ });
Cypress.Commands.add('selectDropdown', (selector, value) => { /* ... */ });

// cypress/support/commands.js
import './commands/auth';
import './commands/api';
import './commands/ui';
```

---

## 🎯 Real-World Command Library

### Complete Auth Commands

```javascript
// cypress/support/commands/auth.js

Cypress.Commands.add('loginViaUI', (username, password, options = {}) => {
  const { rememberMe = false } = options;
  
  cy.visit('/login');
  cy.get('#username').type(username);
  cy.get('#password').type(password);
  
  if (rememberMe) {
    cy.get('#remember-me').check();
  }
  
  cy.get('button[type="submit"]').click();
  cy.url().should('not.include', '/login');
});

Cypress.Commands.add('loginViaAPI', (username, password) => {
  cy.request('POST', '/api/auth/login', {
    username,
    password
  }).then((response) => {
    window.localStorage.setItem('token', response.body.token);
    window.localStorage.setItem('user', JSON.stringify(response.body.user));
  });
});

Cypress.Commands.add('loginSession', (username, password) => {
  cy.session([username, password], () => {
    cy.loginViaAPI(username, password);
  }, {
    validate() {
      cy.request('/api/auth/validate').its('status').should('equal', 200);
    }
  });
});

Cypress.Commands.add('logout', () => {
  cy.get('.user-menu').click();
  cy.get('.logout-button').click();
  cy.url().should('include', '/login');
});
```

---

### Complete Form Commands

```javascript
// cypress/support/commands/forms.js

Cypress.Commands.add('fillForm', (formData) => {
  Object.entries(formData).forEach(([field, value]) => {
    cy.get(`[name="${field}"]`).type(value);
  });
});

Cypress.Commands.add('selectDropdown', (selector, value) => {
  cy.get(selector).click();
  cy.get(`${selector} option`).contains(value).click();
});

Cypress.Commands.add('uploadFile', (selector, fileName) => {
  cy.get(selector).selectFile(`cypress/fixtures/${fileName}`);
});

Cypress.Commands.add('submitForm', (selector = 'form') => {
  cy.get(`${selector} button[type="submit"]`).click();
});
```

---

## 🎓 Ringkasan Minggu 9

### Yang Sudah Dipelajari:

✅ **Custom Commands** - Parent, child, dual commands
✅ **Overwriting Commands** - Extend existing commands
✅ **TypeScript Support** - Type definitions untuk IntelliSense
✅ **Command Organization** - Organize by feature
✅ **Best Practices** - Reusable, maintainable commands

---

### Key Takeaways:

1. **Custom commands** membuat tests lebih readable
2. **TypeScript** meningkatkan developer experience
3. **Organize commands** by feature untuk maintainability
4. **Overwrite carefully** - bisa affect semua tests

---

### Checklist Penguasaan:

- [ ] Create parent, child, dual commands
- [ ] Overwrite existing commands
- [ ] Add TypeScript definitions
- [ ] Organize command library
- [ ] Document custom commands

**Next Week:** Minggu 10 - Testing Patterns & Best Practices! 🚀
