# Minggu 11-12: Real-World Capstone Projects

## 📚 Pengantar

Dua minggu terakhir adalah saatnya untuk **mengaplikasikan semua yang telah dipelajari** dalam project nyata! Pilih salah satu project di bawah ini atau buat project sendiri yang sesuai dengan kebutuhan Anda.

---

## 🎯 Project 1: E-Commerce Testing Suite

### Overview
Buat comprehensive test suite untuk aplikasi e-commerce yang mencakup user journey dari browsing hingga checkout.

### Features to Test

#### 1. User Authentication
```javascript
// cypress/e2e/auth/registration.cy.js
import { RegistrationPage } from '../../support/pages/RegistrationPage';

describe('User Registration', () => {
  const registrationPage = new RegistrationPage();
  
  beforeEach(() => {
    registrationPage.visit();
  });
  
  it('should register new user successfully', () => {
    const userData = {
      firstName: 'John',
      lastName: 'Doe',
      email: `user_${Date.now()}@test.com`,
      password: 'SecurePass123!',
      confirmPassword: 'SecurePass123!'
    };
    
    registrationPage.fillRegistrationForm(userData);
    registrationPage.submit();
    
    cy.url().should('include', '/welcome');
    cy.get('.success-message').should('contain', 'Registration successful');
  });
  
  it('should validate email format', () => {
    registrationPage.fillEmail('invalid-email');
    registrationPage.submit();
    
    registrationPage.verifyErrorMessage('Please enter a valid email');
  });
  
  it('should validate password strength', () => {
    registrationPage.fillPassword('weak');
    registrationPage.submit();
    
    registrationPage.verifyErrorMessage('Password must be at least 8 characters');
  });
});
```

#### 2. Product Browsing & Search
```javascript
// cypress/e2e/products/product-search.cy.js
describe('Product Search', () => {
  beforeEach(() => {
    cy.loginSession('testuser', 'password123');
    cy.visit('/products');
  });
  
  it('should search products by keyword', () => {
    cy.get('[data-cy="search-input"]').type('laptop');
    cy.get('[data-cy="search-button"]').click();
    
    cy.get('.product-card').should('have.length.greaterThan', 0);
    cy.get('.product-card').each(($card) => {
      cy.wrap($card).find('.product-name').invoke('text').then((name) => {
        expect(name.toLowerCase()).to.include('laptop');
      });
    });
  });
  
  it('should filter products by category', () => {
    cy.get('[data-cy="category-filter"]').select('Electronics');
    
    cy.get('.product-card').each(($card) => {
      cy.wrap($card).find('.product-category').should('contain', 'Electronics');
    });
  });
  
  it('should sort products by price', () => {
    cy.get('[data-cy="sort-select"]').select('price-low-high');
    
    cy.get('.product-price').then(($prices) => {
      const prices = [...$prices].map(el => parseFloat(el.textContent.replace('$', '')));
      const sortedPrices = [...prices].sort((a, b) => a - b);
      expect(prices).to.deep.equal(sortedPrices);
    });
  });
});
```

#### 3. Shopping Cart
```javascript
// cypress/e2e/cart/shopping-cart.cy.js
describe('Shopping Cart', () => {
  beforeEach(() => {
    cy.loginSession('testuser', 'password123');
    cy.visit('/products');
  });
  
  it('should add product to cart', () => {
    cy.get('.product-card').first().within(() => {
      cy.get('[data-cy="add-to-cart"]').click();
    });
    
    cy.get('.cart-badge').should('contain', '1');
    cy.get('.cart-notification').should('contain', 'Product added to cart');
  });
  
  it('should update product quantity in cart', () => {
    cy.addProductToCart('Product 1');
    cy.visit('/cart');
    
    cy.get('.cart-item').first().within(() => {
      cy.get('[data-cy="quantity-input"]').clear().type('3');
      cy.get('[data-cy="update-quantity"]').click();
    });
    
    cy.get('.cart-item').first().find('.item-total').should('contain', '$89.97');
  });
  
  it('should remove product from cart', () => {
    cy.addProductToCart('Product 1');
    cy.visit('/cart');
    
    cy.get('.cart-item').first().find('[data-cy="remove-item"]').click();
    cy.get('.cart-empty-message').should('be.visible');
  });
  
  it('should calculate cart total correctly', () => {
    const products = [
      { name: 'Product 1', price: 29.99, quantity: 2 },
      { name: 'Product 2', price: 49.99, quantity: 1 }
    ];
    
    products.forEach(product => {
      cy.addProductToCart(product.name, product.quantity);
    });
    
    cy.visit('/cart');
    
    const expectedTotal = products.reduce((sum, p) => sum + (p.price * p.quantity), 0);
    cy.get('.cart-total').should('contain', `$${expectedTotal.toFixed(2)}`);
  });
});
```

#### 4. Checkout Process
```javascript
// cypress/e2e/checkout/checkout.cy.js
describe('Checkout Process', () => {
  beforeEach(() => {
    cy.loginSession('testuser', 'password123');
    cy.addProductToCart('Test Product');
    cy.visit('/checkout');
  });
  
  it('should complete checkout with valid data', () => {
    // Shipping information
    cy.get('#shipping-address').type('123 Main St');
    cy.get('#shipping-city').type('New York');
    cy.get('#shipping-zip').type('10001');
    cy.get('#shipping-country').select('United States');
    
    // Payment information
    cy.get('#card-number').type('4242424242424242');
    cy.get('#card-expiry').type('12/25');
    cy.get('#card-cvc').type('123');
    
    // Place order
    cy.get('[data-cy="place-order"]').click();
    
    // Verify success
    cy.url().should('include', '/order-confirmation');
    cy.get('.order-number').should('be.visible');
  });
  
  it('should validate required fields', () => {
    cy.get('[data-cy="place-order"]').click();
    
    cy.get('.error-message').should('contain', 'Please fill all required fields');
  });
});
```

### Deliverables
- [ ] 20+ test cases covering all user flows
- [ ] Page Object Model implementation
- [ ] Custom commands for common actions
- [ ] API testing for backend validation
- [ ] Test data management
- [ ] CI/CD configuration

---

## 🎯 Project 2: Admin Dashboard Testing

### Overview
Test suite untuk admin dashboard dengan CRUD operations, data tables, dan complex interactions.

### Features to Test

#### 1. User Management
```javascript
// cypress/e2e/admin/user-management.cy.js
describe('User Management', () => {
  beforeEach(() => {
    cy.loginAsAdmin();
    cy.visit('/admin/users');
  });
  
  it('should create new user', () => {
    cy.get('[data-cy="create-user-btn"]').click();
    
    cy.get('#username').type('newuser');
    cy.get('#email').type('newuser@example.com');
    cy.get('#role').select('User');
    cy.get('#password').type('Password123!');
    
    cy.get('[data-cy="save-user"]').click();
    
    cy.get('.success-message').should('contain', 'User created successfully');
    cy.get('.user-table').should('contain', 'newuser');
  });
  
  it('should edit existing user', () => {
    cy.get('.user-table tbody tr').first().within(() => {
      cy.get('[data-cy="edit-user"]').click();
    });
    
    cy.get('#email').clear().type('updated@example.com');
    cy.get('[data-cy="save-user"]').click();
    
    cy.get('.success-message').should('be.visible');
  });
  
  it('should delete user', () => {
    cy.get('.user-table tbody tr').first().within(() => {
      cy.get('[data-cy="delete-user"]').click();
    });
    
    cy.get('.confirm-dialog').within(() => {
      cy.get('[data-cy="confirm-delete"]').click();
    });
    
    cy.get('.success-message').should('contain', 'User deleted');
  });
});
```

#### 2. Data Table Operations
```javascript
// cypress/e2e/admin/data-table.cy.js
describe('Data Table Operations', () => {
  beforeEach(() => {
    cy.loginAsAdmin();
    cy.visit('/admin/users');
  });
  
  it('should sort table by column', () => {
    cy.get('[data-cy="sort-name"]').click();
    
    cy.get('.user-table tbody tr td:first-child').then(($cells) => {
      const names = [...$cells].map(cell => cell.textContent);
      const sortedNames = [...names].sort();
      expect(names).to.deep.equal(sortedNames);
    });
  });
  
  it('should filter table data', () => {
    cy.get('[data-cy="filter-input"]').type('admin');
    
    cy.get('.user-table tbody tr').each(($row) => {
      cy.wrap($row).should('contain', 'admin');
    });
  });
  
  it('should paginate table', () => {
    cy.get('[data-cy="pagination-next"]').click();
    cy.get('[data-cy="current-page"]').should('contain', '2');
  });
});
```

#### 3. File Upload
```javascript
// cypress/e2e/admin/file-upload.cy.js
describe('File Upload', () => {
  it('should upload CSV file', () => {
    cy.loginAsAdmin();
    cy.visit('/admin/import');
    
    cy.get('input[type="file"]').selectFile('cypress/fixtures/users.csv');
    cy.get('[data-cy="upload-btn"]').click();
    
    cy.get('.upload-progress').should('be.visible');
    cy.get('.success-message', { timeout: 10000 })
      .should('contain', 'File uploaded successfully');
  });
});
```

---

## 🎯 Project 3: Social Media App Testing

### Overview
Test social media features seperti posts, comments, likes, dan user interactions.

### Features to Test

#### 1. Post Management
```javascript
// cypress/e2e/posts/create-post.cy.js
describe('Post Management', () => {
  beforeEach(() => {
    cy.loginSession('testuser', 'password123');
    cy.visit('/feed');
  });
  
  it('should create new post', () => {
    cy.get('[data-cy="create-post-btn"]').click();
    
    cy.get('[data-cy="post-content"]').type('This is a test post!');
    cy.get('[data-cy="post-submit"]').click();
    
    cy.get('.post-list').first().should('contain', 'This is a test post!');
  });
  
  it('should edit post', () => {
    cy.get('.post-list').first().within(() => {
      cy.get('[data-cy="post-menu"]').click();
      cy.get('[data-cy="edit-post"]').click();
    });
    
    cy.get('[data-cy="post-content"]').clear().type('Updated post content');
    cy.get('[data-cy="save-post"]').click();
    
    cy.get('.post-list').first().should('contain', 'Updated post content');
  });
  
  it('should delete post', () => {
    cy.get('.post-list').first().within(() => {
      cy.get('[data-cy="post-menu"]').click();
      cy.get('[data-cy="delete-post"]').click();
    });
    
    cy.get('.confirm-dialog [data-cy="confirm"]').click();
    cy.get('.success-message').should('contain', 'Post deleted');
  });
});
```

#### 2. Comments & Interactions
```javascript
// cypress/e2e/posts/interactions.cy.js
describe('Post Interactions', () => {
  beforeEach(() => {
    cy.loginSession('testuser', 'password123');
    cy.visit('/feed');
  });
  
  it('should like post', () => {
    cy.get('.post-list').first().within(() => {
      cy.get('[data-cy="like-count"]').invoke('text').then((initialCount) => {
        cy.get('[data-cy="like-btn"]').click();
        cy.get('[data-cy="like-count"]').should('contain', parseInt(initialCount) + 1);
      });
    });
  });
  
  it('should add comment', () => {
    cy.get('.post-list').first().within(() => {
      cy.get('[data-cy="comment-input"]').type('Great post!');
      cy.get('[data-cy="comment-submit"]').click();
      
      cy.get('.comment-list').should('contain', 'Great post!');
    });
  });
});
```

---

## 📋 Project Checklist

### Setup & Configuration
- [ ] Initialize Cypress project
- [ ] Configure cypress.config.js
- [ ] Setup folder structure
- [ ] Install necessary plugins

### Implementation
- [ ] Create Page Object Models
- [ ] Implement custom commands
- [ ] Setup test data management
- [ ] Create helper functions
- [ ] Implement API testing

### Test Coverage
- [ ] Happy path scenarios
- [ ] Error handling scenarios
- [ ] Edge cases
- [ ] Cross-browser testing
- [ ] Responsive testing

### Documentation
- [ ] README with setup instructions
- [ ] Test plan document
- [ ] Known issues/limitations
- [ ] Future improvements

### CI/CD
- [ ] GitHub Actions / GitLab CI configuration
- [ ] Test reports generation
- [ ] Screenshot/video on failure
- [ ] Slack/Email notifications

---

## 🎓 Final Tips

### Code Quality
```javascript
// ✅ Good - Descriptive, maintainable
describe('User Registration Flow', () => {
  const registrationPage = new RegistrationPage();
  
  it('should successfully register user with valid data', () => {
    const userData = generateTestUser();
    registrationPage.visit().fillForm(userData).submit();
    cy.url().should('include', '/welcome');
  });
});

// ❌ Bad - Hard to maintain
describe('Test', () => {
  it('works', () => {
    cy.visit('/register');
    cy.get('#name').type('John');
    cy.get('#email').type('john@test.com');
    cy.get('button').click();
  });
});
```

### Performance
- Use `cy.session()` untuk login
- Implement App Actions untuk setup
- Minimize UI interactions
- Use API untuk data seeding

### Reliability
- Add proper waits dan assertions
- Handle flaky tests
- Use retry logic
- Implement proper error handling

---

## 🎉 Selamat!

Anda telah menyelesaikan pembelajaran JavaScript untuk Cypress Automation Testing! 

**Next Steps:**
1. Pilih dan selesaikan salah satu capstone project
2. Share project Anda di GitHub
3. Contribute ke open-source Cypress projects
4. Explore advanced topics (Visual Testing, Component Testing)
5. Consider Cypress certification

**Keep Learning! 🚀**
