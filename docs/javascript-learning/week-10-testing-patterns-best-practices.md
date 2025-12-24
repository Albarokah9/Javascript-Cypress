# Minggu 10: Testing Patterns & Best Practices

## 📚 Pengantar

Minggu ini kita akan belajar patterns dan best practices yang membedakan test automation yang "works" dengan yang "production-ready". Ini adalah minggu yang sangat penting untuk career development Anda sebagai automation engineer.

---

## 1️⃣ Page Object Model (POM)

### Complete POM Implementation

```javascript
// cypress/support/pages/LoginPage.js
export class LoginPage {
  // Elements
  elements = {
    usernameInput: () => cy.get('#username'),
    passwordInput: () => cy.get('#password'),
    rememberMeCheckbox: () => cy.get('#remember-me'),
    submitButton: () => cy.get('button[type="submit"]'),
    errorMessage: () => cy.get('.error-message'),
    forgotPasswordLink: () => cy.get('a[href="/forgot-password"]')
  };
  
  // Actions
  visit() {
    cy.visit('/login');
    return this;
  }
  
  fillUsername(username) {
    this.elements.usernameInput().clear().type(username);
    return this;
  }
  
  fillPassword(password) {
    this.elements.passwordInput().clear().type(password);
    return this;
  }
  
  checkRememberMe() {
    this.elements.rememberMeCheckbox().check();
    return this;
  }
  
  submit() {
    this.elements.submitButton().click();
    return this;
  }
  
  login({ username, password, rememberMe = false }) {
    this.fillUsername(username);
    this.fillPassword(password);
    if (rememberMe) this.checkRememberMe();
    this.submit();
    return this;
  }
  
  // Assertions
  verifyErrorMessage(message) {
    this.elements.errorMessage()
      .should('be.visible')
      .and('contain', message);
    return this;
  }
  
  verifyLoginSuccess() {
    cy.url().should('not.include', '/login');
    return this;
  }
}

// Usage
import { LoginPage } from '../support/pages/LoginPage';

describe('Login Tests', () => {
  const loginPage = new LoginPage();
  
  beforeEach(() => {
    loginPage.visit();
  });
  
  it('should login successfully', () => {
    loginPage
      .login({
        username: 'testuser',
        password: 'password123',
        rememberMe: true
      })
      .verifyLoginSuccess();
  });
});
```

---

## 2️⃣ App Actions Pattern

App Actions bypass UI untuk faster test setup.

```javascript
// cypress/support/app-actions.js

Cypress.Commands.add('createUser', (userData) => {
  return cy.request('POST', '/api/users', userData);
});

Cypress.Commands.add('createPost', (postData) => {
  return cy.request({
    method: 'POST',
    url: '/api/posts',
    headers: {
      'Authorization': `Bearer ${window.localStorage.getItem('token')}`
    },
    body: postData
  });
});

Cypress.Commands.add('seedDatabase', (data) => {
  return cy.task('db:seed', data);
});

// Usage
describe('Post Management', () => {
  beforeEach(() => {
    // Fast setup via API instead of UI
    cy.createUser({
      username: 'testuser',
      email: 'test@example.com',
      password: 'password123'
    });
    
    cy.loginViaAPI('testuser', 'password123');
  });
  
  it('should create post via UI', () => {
    // Only test the actual UI functionality
    cy.visit('/posts/new');
    cy.get('#title').type('Test Post');
    cy.get('#content').type('This is test content');
    cy.get('button[type="submit"]').click();
    
    cy.get('.success-message').should('be.visible');
  });
});
```

---

## 3️⃣ Test Data Management

### Centralized Test Data

```javascript
// cypress/fixtures/testData.js
export const users = {
  admin: {
    username: 'admin',
    password: 'Admin123!',
    email: 'admin@example.com',
    role: 'admin'
  },
  user: {
    username: 'user',
    password: 'User123!',
    email: 'user@example.com',
    role: 'user'
  }
};

export const selectors = {
  login: {
    username: '#username',
    password: '#password',
    submit: 'button[type="submit"]'
  },
  dashboard: {
    welcome: '.welcome-message',
    userMenu: '.user-menu'
  }
};

export const apiEndpoints = {
  auth: {
    login: '/api/auth/login',
    logout: '/api/auth/logout',
    validate: '/api/auth/validate'
  },
  users: {
    list: '/api/users',
    create: '/api/users',
    update: (id) => `/api/users/${id}`,
    delete: (id) => `/api/users/${id}`
  }
};

// Usage
import { users, selectors, apiEndpoints } from '../fixtures/testData';

describe('Login', () => {
  it('should login as admin', () => {
    const { username, password } = users.admin;
    
    cy.visit('/login');
    cy.get(selectors.login.username).type(username);
    cy.get(selectors.login.password).type(password);
    cy.get(selectors.login.submit).click();
  });
});
```

---

## 4️⃣ Environment Configuration

### Multi-Environment Setup

```javascript
// cypress.config.js
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  e2e: {
    baseUrl: process.env.CYPRESS_BASE_URL || 'http://localhost:3000',
    env: {
      apiUrl: process.env.API_URL || 'http://localhost:4000',
      environment: process.env.ENVIRONMENT || 'development'
    },
    
    setupNodeEvents(on, config) {
      // Environment-specific config
      const environment = config.env.environment;
      
      if (environment === 'production') {
        config.defaultCommandTimeout = 10000;
        config.retries = { runMode: 2, openMode: 0 };
      } else if (environment === 'staging') {
        config.defaultCommandTimeout = 8000;
        config.retries = { runMode: 1, openMode: 0 };
      }
      
      return config;
    }
  }
});

// Usage in tests
describe('Environment Config', () => {
  it('should use environment variables', () => {
    const apiUrl = Cypress.env('apiUrl');
    const env = Cypress.env('environment');
    
    cy.log(`Testing in ${env} environment`);
    cy.request(`${apiUrl}/health`).its('status').should('equal', 200);
  });
});
```

---

## 5️⃣ Session Management

### Using cy.session()

```javascript
// cypress/support/commands.js
Cypress.Commands.add('loginSession', (username, password) => {
  cy.session(
    [username, password], // Cache key
    () => {
      // Login logic
      cy.visit('/login');
      cy.get('#username').type(username);
      cy.get('#password').type(password);
      cy.get('button[type="submit"]').click();
      cy.url().should('include', '/dashboard');
    },
    {
      validate() {
        // Validate session is still valid
        cy.request('/api/auth/validate').its('status').should('equal', 200);
      },
      cacheAcrossSpecs: true
    }
  );
});

// Usage - session is cached!
describe('Multiple Tests', () => {
  beforeEach(() => {
    cy.loginSession('testuser', 'password123'); // Fast after first run
    cy.visit('/dashboard');
  });
  
  it('test 1', () => { /* ... */ });
  it('test 2', () => { /* ... */ });
  it('test 3', () => { /* ... */ });
});
```

---

## 6️⃣ Test Organization

### Folder Structure

```
cypress/
├── e2e/
│   ├── auth/
│   │   ├── login.cy.js
│   │   ├── logout.cy.js
│   │   └── registration.cy.js
│   ├── users/
│   │   ├── user-list.cy.js
│   │   └── user-management.cy.js
│   └── posts/
│       ├── create-post.cy.js
│       └── edit-post.cy.js
├── fixtures/
│   ├── testData.js
│   ├── users.json
│   └── posts.json
├── support/
│   ├── commands/
│   │   ├── auth.js
│   │   ├── api.js
│   │   └── ui.js
│   ├── pages/
│   │   ├── LoginPage.js
│   │   ├── DashboardPage.js
│   │   └── UserPage.js
│   ├── utils/
│   │   └── helpers.js
│   ├── commands.js
│   └── e2e.js
└── cypress.config.js
```

---

## 🎯 Complete Real-World Example

### Production-Ready Test Suite

```javascript
// cypress/support/pages/DashboardPage.js
export class DashboardPage {
  elements = {
    header: () => cy.get('.dashboard-header'),
    userProfile: () => cy.get('.user-profile'),
    statsCards: () => cy.get('.stats-card'),
    navigationMenu: () => cy.get('.nav-menu')
  };
  
  visit() {
    cy.visit('/dashboard');
    this.verifyPageLoaded();
    return this;
  }
  
  verifyPageLoaded() {
    this.elements.header().should('be.visible');
    this.elements.userProfile().should('be.visible');
    return this;
  }
  
  getUserProfile() {
    return this.elements.userProfile().then($profile => ({
      name: $profile.find('.user-name').text(),
      email: $profile.find('.user-email').text(),
      role: $profile.find('.user-role').text()
    }));
  }
  
  navigateTo(section) {
    this.elements.navigationMenu().contains(section).click();
    return this;
  }
}

// Test file
import { DashboardPage } from '../support/pages/DashboardPage';
import { users } from '../fixtures/testData';

describe('Dashboard Tests', () => {
  const dashboard = new DashboardPage();
  
  beforeEach(() => {
    cy.loginSession(users.admin.username, users.admin.password);
    dashboard.visit();
  });
  
  it('should display user profile correctly', () => {
    cy.wrap(dashboard.getUserProfile()).then((profile) => {
      expect(profile.name).to.not.be.empty;
      expect(profile.email).to.include('@');
      expect(profile.role).to.equal('Admin');
    });
  });
  
  it('should navigate to different sections', () => {
    dashboard.navigateTo('Users');
    cy.url().should('include', '/users');
    
    dashboard.navigateTo('Settings');
    cy.url().should('include', '/settings');
  });
});
```

---

## 🎓 Ringkasan Minggu 10

### Yang Sudah Dipelajari:

✅ **Page Object Model** - Complete implementation
✅ **App Actions** - Fast test setup
✅ **Test Data Management** - Centralized data
✅ **Environment Config** - Multi-environment support
✅ **Session Management** - cy.session() untuk performance
✅ **Test Organization** - Professional folder structure

---

### Best Practices Checklist:

- [ ] Implement POM untuk all pages
- [ ] Use App Actions untuk setup
- [ ] Centralize test data
- [ ] Configure environments
- [ ] Use cy.session() untuk login
- [ ] Organize tests by feature
- [ ] Add TypeScript untuk IntelliSense
- [ ] Document patterns dan conventions

---

### Key Takeaways:

1. **POM** meningkatkan maintainability drastis
2. **App Actions** membuat tests jauh lebih cepat
3. **Centralized data** memudahkan updates
4. **cy.session()** adalah game changer untuk performance

**Next:** Minggu 11-12 - Capstone Projects! 🚀
