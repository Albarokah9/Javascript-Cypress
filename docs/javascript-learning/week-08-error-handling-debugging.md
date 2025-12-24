# Minggu 8: Error Handling & Debugging

## 📚 Pengantar

Error handling dan debugging adalah skills penting untuk menulis tests yang robust dan maintainable. Minggu ini kita akan belajar bagaimana handle errors dengan baik dan debug tests effectively.

---

## 1️⃣ Try/Catch Blocks

### Basic Try/Catch

```javascript
function parseJSON(jsonString) {
  try {
    const data = JSON.parse(jsonString);
    return { success: true, data };
  } catch (error) {
    console.error('Failed to parse JSON:', error.message);
    return { success: false, error: error.message };
  }
}

const result1 = parseJSON('{"name": "John"}');
console.log(result1); // { success: true, data: { name: 'John' } }

const result2 = parseJSON('invalid json');
console.log(result2); // { success: false, error: '...' }
```

---

### Try/Catch/Finally

```javascript
async function fetchData(url) {
  let response;
  try {
    response = await fetch(url);
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Fetch failed:', error);
    throw error;
  } finally {
    console.log('Fetch attempt completed');
    // Cleanup code here
  }
}
```

---

## 2️⃣ Custom Error Messages in Cypress

### Descriptive Assertions

```javascript
describe('Custom Error Messages', () => {
  it('should have descriptive assertions', () => {
    cy.get('.user-list', 'User list should be visible after login')
      .should('be.visible');
    
    cy.get('.user-count')
      .invoke('text')
      .then((count) => {
        expect(parseInt(count), 'User count should be greater than 0')
          .to.be.greaterThan(0);
      });
  });
  
  it('should provide context in error messages', () => {
    cy.get('.product-price').then(($price) => {
      const price = parseFloat($price.text().replace('$', ''));
      
      expect(price, `Price ${price} should be positive`).to.be.greaterThan(0);
      expect(price, `Price ${price} should be less than $1000`).to.be.lessThan(1000);
    });
  });
});
```

---

### Handling Expected Errors

```javascript
describe('Expected Errors', () => {
  it('should handle form validation errors', () => {
    cy.get('#submit-button').click();
    
    // Verify error message appears
    cy.get('.error-message')
      .should('be.visible')
      .and('contain', 'Please fill all required fields');
  });
  
  it('should handle API errors', () => {
    cy.intercept('POST', '/api/login', {
      statusCode: 401,
      body: { error: 'Invalid credentials' }
    });
    
    cy.visit('/login');
    cy.get('#username').type('invalid');
    cy.get('#password').type('wrong');
    cy.get('button[type="submit"]').click();
    
    cy.get('.error-message').should('contain', 'Invalid credentials');
  });
});
```

---

## 3️⃣ Debugging Tools

### cy.debug() - Pause and Inspect

```javascript
describe('cy.debug()', () => {
  it('should pause execution for debugging', () => {
    cy.visit('/dashboard');
    
    cy.get('.user-data')
      .debug() // Pauses here, opens DevTools
      .should('be.visible');
    
    // You can inspect the element in console
    // Type 'subject' in console to see the element
  });
});
```

---

### cy.pause() - Manual Pause

```javascript
describe('cy.pause()', () => {
  it('should pause test execution', () => {
    cy.visit('/dashboard');
    cy.pause(); // Test pauses here
    
    // Click "Resume" button in Cypress UI to continue
    cy.get('.widget').should('be.visible');
  });
  
  it('should pause conditionally', () => {
    const shouldPause = Cypress.env('DEBUG');
    
    cy.visit('/login');
    
    if (shouldPause) {
      cy.pause();
    }
    
    cy.get('#username').type('testuser');
  });
});
```

---

### cy.log() - Custom Logging

```javascript
describe('cy.log()', () => {
  it('should log custom messages', () => {
    cy.log('Starting login test');
    
    cy.visit('/login');
    cy.log('Visited login page');
    
    cy.get('#username').type('testuser');
    cy.log('Entered username');
    
    cy.get('.price').invoke('text').then((price) => {
      cy.log(`Current price: ${price}`);
      cy.log('Price check', price);
    });
  });
  
  it('should log with structured data', () => {
    const testData = {
      username: 'testuser',
      testNumber: 5,
      timestamp: new Date().toISOString()
    };
    
    cy.log('Test data:', JSON.stringify(testData, null, 2));
  });
});
```

---

### Console Methods

```javascript
describe('Console Methods', () => {
  it('should use console for debugging', () => {
    cy.get('.user-data').then(($data) => {
      // Basic logging
      console.log('User data:', $data);
      
      // Table format
      console.table({
        text: $data.text(),
        visible: $data.is(':visible'),
        class: $data.attr('class'),
        id: $data.attr('id')
      });
      
      // Grouped logging
      console.group('User Data Details');
      console.log('Text:', $data.text());
      console.log('Visible:', $data.is(':visible'));
      console.groupEnd();
    });
  });
});
```

---

## 4️⃣ Error Recovery Patterns

### Retry on Failure

```javascript
describe('Retry Patterns', () => {
  it('should retry with custom timeout', () => {
    cy.get('.dynamic-content', { timeout: 10000 })
      .should('be.visible');
  });
  
  it('should retry with multiple attempts', () => {
    cy.get('.slow-loading-element', {
      timeout: 15000,
      interval: 500
    }).should('exist');
  });
});
```

---

### Handling Optional Elements

```javascript
describe('Optional Elements', () => {
  it('should handle optional banners', () => {
    cy.get('body').then(($body) => {
      if ($body.find('.cookie-banner').length > 0) {
        cy.log('Cookie banner found, closing it');
        cy.get('.cookie-banner .close').click();
      } else {
        cy.log('No cookie banner present');
      }
    });
  });
  
  it('should handle optional modals', () => {
    cy.visit('/dashboard');
    
    // Check if modal exists before interacting
    cy.get('body').then(($body) => {
      const $modal = $body.find('.welcome-modal');
      
      if ($modal.length > 0 && $modal.is(':visible')) {
        cy.get('.welcome-modal .dismiss').click();
      }
    });
  });
});
```

---

### Conditional Testing

```javascript
describe('Conditional Testing', () => {
  it('should handle different states', () => {
    cy.get('.user-status').then(($status) => {
      const statusText = $status.text();
      
      if (statusText === 'Active') {
        cy.get('.edit-button').should('be.enabled');
      } else if (statusText === 'Pending') {
        cy.get('.approve-button').should('be.visible');
      } else {
        cy.get('.reactivate-button').should('be.visible');
      }
    });
  });
});
```

---

## 5️⃣ Debugging Best Practices

### Add Meaningful Logs

```javascript
describe('Meaningful Logging', () => {
  it('should log test progress', () => {
    cy.log('=== Starting User Registration Test ===');
    
    const testUser = {
      username: 'testuser',
      email: 'test@example.com'
    };
    
    cy.log(`Test user: ${testUser.username}`);
    
    cy.visit('/register');
    cy.log('✓ Navigated to registration page');
    
    cy.get('#username').type(testUser.username);
    cy.log(`✓ Entered username: ${testUser.username}`);
    
    cy.get('#email').type(testUser.email);
    cy.log(`✓ Entered email: ${testUser.email}`);
    
    cy.get('button[type="submit"]').click();
    cy.log('✓ Submitted form');
    
    cy.url().should('include', '/welcome');
    cy.log('✓ Registration successful');
  });
});
```

---

### Screenshot on Failure

```javascript
// cypress.config.js
module.exports = {
  e2e: {
    screenshotOnRunFailure: true,
    video: true,
    
    setupNodeEvents(on, config) {
      on('task', {
        failed: require('cypress-failed-log/src/failed')()
      });
    }
  }
};

// In test
describe('Screenshot on Failure', () => {
  afterEach(function() {
    if (this.currentTest.state === 'failed') {
      cy.screenshot(`FAILED-${this.currentTest.title}`);
    }
  });
  
  it('should capture screenshot on failure', () => {
    cy.visit('/dashboard');
    cy.get('.non-existent-element').should('exist'); // Will fail
  });
});
```

---

### Debug Commands

```javascript
// Custom debug command
Cypress.Commands.add('debugElement', { prevSubject: true }, (subject) => {
  console.log('=== Element Debug Info ===');
  console.log('Text:', subject.text());
  console.log('HTML:', subject.html());
  console.log('Visible:', subject.is(':visible'));
  console.log('Classes:', subject.attr('class'));
  console.log('ID:', subject.attr('id'));
  console.log('Data attributes:', subject.data());
  console.table({
    tag: subject.prop('tagName'),
    text: subject.text(),
    visible: subject.is(':visible')
  });
  
  return subject;
});

// Usage
describe('Debug Command', () => {
  it('should debug element', () => {
    cy.get('.user-profile')
      .debugElement()
      .should('be.visible');
  });
});
```

---

## 🎯 Real-World Example

### Comprehensive Error Handling

```javascript
describe('Robust Login Test', () => {
  beforeEach(() => {
    cy.log('=== Test Setup ===');
    cy.visit('/login', { failOnStatusCode: false });
  });
  
  it('should handle login with comprehensive error handling', () => {
    cy.log('Starting login test');
    
    // Check if page loaded correctly
    cy.get('body').then(($body) => {
      if ($body.find('.error-page').length > 0) {
        throw new Error('Page failed to load - error page displayed');
      }
    });
    
    // Handle optional cookie banner
    cy.get('body').then(($body) => {
      if ($body.find('.cookie-banner').length > 0) {
        cy.log('Dismissing cookie banner');
        cy.get('.cookie-banner .accept').click();
      }
    });
    
    // Perform login with validation
    cy.get('#username', 'Username field should exist')
      .should('be.visible')
      .type('testuser');
    cy.log('✓ Username entered');
    
    cy.get('#password', 'Password field should exist')
      .should('be.visible')
      .type('password123');
    cy.log('✓ Password entered');
    
    // Intercept login request
    cy.intercept('POST', '/api/login').as('loginRequest');
    
    cy.get('button[type="submit"]').click();
    cy.log('✓ Login submitted');
    
    // Wait for response with timeout
    cy.wait('@loginRequest', { timeout: 10000 }).then((interception) => {
      const { statusCode, body } = interception.response;
      
      cy.log(`Login response: ${statusCode}`);
      
      if (statusCode === 200) {
        cy.log('✓ Login successful');
        expect(body).to.have.property('token');
      } else {
        cy.log(`✗ Login failed: ${body.error}`);
        throw new Error(`Login failed with status ${statusCode}`);
      }
    });
    
    // Verify redirect
    cy.url({ timeout: 5000 }).should('include', '/dashboard');
    cy.log('✓ Redirected to dashboard');
  });
});
```

---

## 🎓 Ringkasan Minggu 8

### Yang Sudah Dipelajari:

✅ **Try/Catch** - Error handling dalam JavaScript
✅ **Custom Error Messages** - Descriptive assertions
✅ **Debugging Tools** - cy.debug(), cy.pause(), cy.log()
✅ **Console Methods** - console.log(), console.table()
✅ **Error Recovery** - Retry patterns, optional elements

---

### Key Takeaways:

1. **Always add context** to error messages
2. **Use cy.log()** untuk track test progress
3. **Handle optional elements** gracefully
4. **Debug systematically** dengan tools yang tepat

---

### Checklist Penguasaan:

- [ ] Implement try/catch blocks
- [ ] Write descriptive error messages
- [ ] Use debugging tools effectively
- [ ] Handle optional elements
- [ ] Add meaningful logs to tests

**Next Week:** Minggu 9 - Custom Commands & Plugins! 🚀
