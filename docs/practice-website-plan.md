# Implementation Plan: Cypress Practice Website

## 📋 Overview

Membuat practice website lengkap untuk belajar dan praktik Cypress automation testing. Website akan memiliki berbagai fitur yang mencakup semua aspek testing yang dipelajari dalam dokumentasi JavaScript untuk Cypress.

---

## 🎯 Goals

1. **Practice Environment** - Website untuk praktik semua materi Cypress
2. **Full Control** - User bisa modify dan customize sepenuhnya
3. **Comprehensive Features** - Cover semua testing scenarios
4. **Easy to Run** - Bisa run di localhost tanpa setup kompleks
5. **Production-like** - Mirip dengan real-world applications

---

## 🏗️ Architecture

### **Technology Stack**
- **Frontend:** HTML5, CSS3, Vanilla JavaScript
- **Database:** Supabase (PostgreSQL)
- **Authentication:** Supabase Auth
- **Storage:** Supabase Storage (for file uploads)
- **Deployment:** Vercel
- **CI/CD:** GitHub Actions
- **Testing:** Cypress

### **Why This Stack?**
- ✅ **Vercel:** Free hosting, automatic deployments, edge network
- ✅ **Supabase:** Free tier, real-time database, built-in auth, easy API
- ✅ **No Framework:** Pure JavaScript untuk full control
- ✅ **Production-Ready:** Real database, deployed online
- ✅ **Portfolio-Worthy:** Live URL untuk showcase ke employers

### **Architecture Diagram**
```
┌─────────────────────────────────────────────────────┐
│                    GitHub Repo                       │
│  (Source Code + Cypress Tests)                      │
└──────────────┬──────────────────────────────────────┘
               │
               │ Push/PR
               ▼
┌─────────────────────────────────────────────────────┐
│              GitHub Actions CI/CD                    │
│  - Run Cypress Tests                                │
│  - Generate Reports                                 │
│  - Deploy to Vercel (if tests pass)                │
└──────────────┬──────────────────────────────────────┘
               │
               │ Deploy
               ▼
┌─────────────────────────────────────────────────────┐
│                   Vercel                            │
│  - Host Static Files                               │
│  - Serverless Functions (API Routes)               │
│  - Edge Network (Fast globally)                    │
└──────────────┬──────────────────────────────────────┘
               │
               │ API Calls
               ▼
┌─────────────────────────────────────────────────────┐
│                  Supabase                           │
│  - PostgreSQL Database                             │
│  - Authentication                                  │
│  - Real-time Subscriptions                        │
│  - Storage (File Uploads)                         │
│  - Row Level Security                             │
└─────────────────────────────────────────────────────┘
```

---

## 📱 Features & Pages

### **1. Authentication Module**

#### **Login Page** (`/login.html`)
**Elements:**
- Username input field
- Password input field
- Remember me checkbox
- Login button
- Forgot password link
- Register link
- Error message display

**Functionality:**
- Form validation
- Authentication logic
- Session management
- Error handling
- Redirect after login

**Cypress Test Scenarios:**
- ✅ Login with valid credentials
- ✅ Login with invalid credentials
- ✅ Form validation errors
- ✅ Remember me functionality
- ✅ Redirect after successful login

---

#### **Registration Page** (`/register.html`)
**Elements:**
- First name, Last name inputs
- Email input
- Username input
- Password input
- Confirm password input
- Terms & conditions checkbox
- Register button

**Functionality:**
- Real-time validation
- Password strength indicator
- Duplicate username check
- Email format validation
- Password match validation

**Cypress Test Scenarios:**
- ✅ Register new user
- ✅ Validation errors
- ✅ Password mismatch
- ✅ Duplicate username
- ✅ Terms acceptance required

---

### **2. Dashboard** (`/dashboard.html`)

**Elements:**
- Welcome message with username
- Statistics cards (Total Users, Active Sessions, etc.)
- Recent activity list
- Navigation menu
- User profile dropdown
- Logout button

**Functionality:**
- Display user info
- Show statistics
- Navigation to other pages
- Logout functionality

**Cypress Test Scenarios:**
- ✅ Dashboard loads correctly
- ✅ User info displayed
- ✅ Navigation works
- ✅ Logout functionality

---

### **3. User Management** (`/users.html`)

**Elements:**
- User list table (sortable, filterable)
- Search input
- Add user button
- Edit/Delete buttons per row
- Pagination
- User modal (Create/Edit)

**Functionality:**
- CRUD operations
- Search/filter users
- Sort by columns
- Pagination
- Modal for add/edit

**Cypress Test Scenarios:**
- ✅ Create new user
- ✅ Edit existing user
- ✅ Delete user
- ✅ Search users
- ✅ Sort table
- ✅ Pagination

---

### **4. Product Catalog** (`/products.html`)

**Elements:**
- Product cards grid
- Search bar
- Category filter dropdown
- Price range filter
- Sort options (price, name, rating)
- Add to cart button
- Product detail modal

**Functionality:**
- Display products
- Filter by category
- Filter by price range
- Search products
- Sort products
- Add to cart

**Cypress Test Scenarios:**
- ✅ Browse products
- ✅ Search products
- ✅ Filter by category
- ✅ Filter by price
- ✅ Sort products
- ✅ Add to cart

---

### **5. Shopping Cart** (`/cart.html`)

**Elements:**
- Cart items list
- Quantity controls (+/-)
- Remove item button
- Subtotal per item
- Total price
- Discount code input
- Checkout button

**Functionality:**
- Update quantity
- Remove items
- Calculate totals
- Apply discount
- Proceed to checkout

**Cypress Test Scenarios:**
- ✅ View cart items
- ✅ Update quantity
- ✅ Remove items
- ✅ Apply discount code
- ✅ Calculate total correctly
- ✅ Proceed to checkout

---

### **6. Checkout** (`/checkout.html`)

**Elements:**
- Shipping information form
- Payment method selection
- Order summary
- Place order button
- Form validation

**Functionality:**
- Collect shipping info
- Select payment method
- Validate all fields
- Submit order
- Show confirmation

**Cypress Test Scenarios:**
- ✅ Fill shipping info
- ✅ Select payment method
- ✅ Form validation
- ✅ Place order
- ✅ Order confirmation

---

### **7. Profile Settings** (`/profile.html`)

**Elements:**
- Profile information form
- Avatar upload
- Change password section
- Notification preferences
- Save changes button

**Functionality:**
- Update profile
- Change password
- Upload avatar
- Save preferences

**Cypress Test Scenarios:**
- ✅ Update profile info
- ✅ Change password
- ✅ Upload avatar
- ✅ Save preferences

---

## 🎨 UI/UX Design

### **Design System**
```css
/* Color Palette */
--primary: #4F46E5;      /* Indigo */
--secondary: #10B981;    /* Green */
--danger: #EF4444;       /* Red */
--warning: #F59E0B;      /* Amber */
--dark: #1F2937;         /* Gray-800 */
--light: #F9FAFB;        /* Gray-50 */

/* Typography */
--font-primary: 'Inter', sans-serif;
--font-size-base: 16px;

/* Spacing */
--spacing-xs: 0.5rem;
--spacing-sm: 1rem;
--spacing-md: 1.5rem;
--spacing-lg: 2rem;
--spacing-xl: 3rem;
```

### **Components**
- Buttons (primary, secondary, danger)
- Forms (inputs, selects, checkboxes)
- Cards
- Modals
- Tables
- Navigation
- Alerts/Notifications

---

## 📂 Project Structure

```
cypress-practice-website/
├── index.html                 # Landing/Home page
├── login.html                 # Login page
├── register.html              # Registration page
├── dashboard.html             # Dashboard
├── users.html                 # User management
├── products.html              # Product catalog
├── cart.html                  # Shopping cart
├── checkout.html              # Checkout page
├── profile.html               # User profile
├── css/
│   ├── main.css              # Main styles
│   ├── components.css        # Reusable components
│   └── pages.css             # Page-specific styles
├── js/
│   ├── app.js                # Main application logic
│   ├── auth.js               # Authentication logic
│   ├── storage.js            # LocalStorage utilities
│   ├── validation.js         # Form validation
│   └── utils.js              # Helper functions
├── data/
│   ├── users.json            # Sample users data
│   └── products.json         # Sample products data
├── cypress/
│   ├── e2e/
│   │   ├── auth/
│   │   │   ├── login.cy.js
│   │   │   └── register.cy.js
│   │   ├── users/
│   │   │   └── user-management.cy.js
│   │   ├── products/
│   │   │   ├── product-catalog.cy.js
│   │   │   └── shopping-cart.cy.js
│   │   └── checkout/
│   │       └── checkout.cy.js
│   ├── fixtures/
│   │   └── testData.js
│   ├── support/
│   │   ├── commands.js
│   │   └── pages/
│   │       ├── LoginPage.js
│   │       ├── DashboardPage.js
│   │       └── UsersPage.js
│   └── cypress.config.js
└── README.md                  # Setup instructions
```

---

## 🔧 Implementation Details

### **LocalStorage Schema**

```javascript
// Users
{
  "users": [
    {
      "id": "1",
      "username": "admin",
      "password": "admin123", // In real app, this would be hashed
      "email": "admin@example.com",
      "firstName": "Admin",
      "lastName": "User",
      "role": "admin",
      "avatar": "default.png",
      "createdAt": "2024-01-01T00:00:00Z"
    }
  ]
}

// Products
{
  "products": [
    {
      "id": "1",
      "name": "Laptop",
      "category": "Electronics",
      "price": 999.99,
      "description": "High-performance laptop",
      "image": "laptop.jpg",
      "stock": 10,
      "rating": 4.5
    }
  ]
}

// Cart
{
  "cart": [
    {
      "productId": "1",
      "quantity": 2,
      "addedAt": "2024-01-01T00:00:00Z"
    }
  ]
}

// Session
{
  "session": {
    "userId": "1",
    "username": "admin",
    "token": "abc123",
    "expiresAt": "2024-01-01T00:00:00Z"
  }
}
```

---

## 🧪 Sample Cypress Tests

### **Example: Login Test**
```javascript
// cypress/e2e/auth/login.cy.js
import { LoginPage } from '../../support/pages/LoginPage';

describe('Login Functionality', () => {
  const loginPage = new LoginPage();
  
  beforeEach(() => {
    loginPage.visit();
  });
  
  it('should login with valid credentials', () => {
    loginPage.login({
      username: 'admin',
      password: 'admin123'
    });
    
    cy.url().should('include', '/dashboard');
    cy.get('.welcome-message').should('contain', 'admin');
  });
  
  it('should show error for invalid credentials', () => {
    loginPage.login({
      username: 'invalid',
      password: 'wrong'
    });
    
    loginPage.verifyErrorMessage('Invalid username or password');
  });
});
```

---

## 📝 Default Test Data

### **Default Users**
```javascript
const defaultUsers = [
  {
    username: 'admin',
    password: 'admin123',
    email: 'admin@example.com',
    role: 'admin'
  },
  {
    username: 'user',
    password: 'user123',
    email: 'user@example.com',
    role: 'user'
  },
  {
    username: 'moderator',
    password: 'mod123',
    email: 'mod@example.com',
    role: 'moderator'
  }
];
```

### **Default Products**
```javascript
const defaultProducts = [
  {
    id: '1',
    name: 'Laptop',
    category: 'Electronics',
    price: 999.99,
    stock: 10
  },
  {
    id: '2',
    name: 'Mouse',
    category: 'Accessories',
    price: 29.99,
    stock: 50
  },
  // ... more products
];
```

---

## 🚀 Setup Instructions

### **Running the Website**

**Option 1: Live Server (VS Code)**
```bash
# Install Live Server extension
# Right-click index.html → Open with Live Server
```

**Option 2: Python HTTP Server**
```bash
# Navigate to project folder
cd cypress-practice-website

# Start server
python -m http.server 8000

# Open browser
# http://localhost:8000
```

**Option 3: Node.js HTTP Server**
```bash
# Install http-server globally
npm install -g http-server

# Start server
http-server -p 8000

# Open browser
# http://localhost:8000
```

---

## 🎯 Data-Driven Testing Implementation

Website akan support **3 metode data-driven testing** untuk different skill levels:

---

### **Method 1: JSON Fixtures (Beginner)**

**Setup:**
```javascript
// cypress/fixtures/loginData.json
{
  "validUsers": [
    {
      "username": "admin",
      "password": "admin123",
      "email": "admin@test.com",
      "role": "admin",
      "expectedUrl": "/dashboard",
      "expectedMessage": "Welcome, Admin"
    },
    {
      "username": "user",
      "password": "user123",
      "email": "user@test.com",
      "role": "user",
      "expectedUrl": "/profile",
      "expectedMessage": "Welcome, User"
    },
    {
      "username": "moderator",
      "password": "mod123",
      "email": "mod@test.com",
      "role": "moderator",
      "expectedUrl": "/dashboard",
      "expectedMessage": "Welcome, Moderator"
    }
  ],
  "invalidUsers": [
    {
      "username": "invalid",
      "password": "wrong",
      "expectedError": "Invalid credentials"
    },
    {
      "username": "",
      "password": "",
      "expectedError": "Username is required"
    },
    {
      "username": "admin",
      "password": "wrongpass",
      "expectedError": "Invalid password"
    }
  ]
}
```

**Test Implementation:**
```javascript
// cypress/e2e/auth/login-data-driven.cy.js
describe('Data-Driven Login Tests (JSON)', () => {
  beforeEach(() => {
    cy.visit('/login');
  });
  
  it('should login with valid credentials from JSON', () => {
    cy.fixture('loginData').then((data) => {
      data.validUsers.forEach((user) => {
        cy.log(`Testing login for: ${user.username}`);
        
        cy.get('#username').clear().type(user.username);
        cy.get('#password').clear().type(user.password);
        cy.get('button[type="submit"]').click();
        
        cy.url().should('include', user.expectedUrl);
        cy.get('.welcome-message').should('contain', user.expectedMessage);
        cy.get('.user-role').should('contain', user.role);
        
        // Logout for next iteration
        cy.get('.logout-btn').click();
        cy.url().should('include', '/login');
      });
    });
  });
  
  it('should show errors for invalid credentials from JSON', () => {
    cy.fixture('loginData').then((data) => {
      data.invalidUsers.forEach((user) => {
        cy.log(`Testing invalid login: ${user.username || 'empty'}`);
        
        if (user.username) cy.get('#username').clear().type(user.username);
        if (user.password) cy.get('#password').clear().type(user.password);
        cy.get('button[type="submit"]').click();
        
        cy.get('.error-message').should('be.visible');
        cy.get('.error-message').should('contain', user.expectedError);
        cy.url().should('include', '/login');
      });
    });
  });
});
```

**Additional Fixtures:**
```javascript
// cypress/fixtures/productSearch.json
{
  "searchTests": [
    {
      "keyword": "laptop",
      "expectedCount": 5,
      "expectedCategories": ["Electronics"],
      "minPrice": 500,
      "maxPrice": 2000
    },
    {
      "keyword": "mouse",
      "expectedCount": 3,
      "expectedCategories": ["Accessories"],
      "minPrice": 10,
      "maxPrice": 100
    },
    {
      "keyword": "nonexistent",
      "expectedCount": 0,
      "expectedMessage": "No products found"
    }
  ]
}

// cypress/fixtures/formValidation.json
{
  "registrationTests": [
    {
      "scenario": "Empty email",
      "data": { "email": "", "password": "Pass123!", "confirmPassword": "Pass123!" },
      "expectedError": "Email is required",
      "fieldWithError": "#email"
    },
    {
      "scenario": "Invalid email format",
      "data": { "email": "invalid-email", "password": "Pass123!", "confirmPassword": "Pass123!" },
      "expectedError": "Invalid email format",
      "fieldWithError": "#email"
    },
    {
      "scenario": "Weak password",
      "data": { "email": "test@test.com", "password": "weak", "confirmPassword": "weak" },
      "expectedError": "Password must be at least 8 characters",
      "fieldWithError": "#password"
    }
  ]
}
```

---

### **Method 2: CSV Import (Intermediate)**

**Setup:**
```bash
npm install --save-dev csv-parse
```

**CSV Files:**
```csv
// cypress/fixtures/users.csv
username,email,firstName,lastName,role,password
user1,user1@test.com,John,Doe,user,Pass123!
user2,user2@test.com,Jane,Smith,admin,Pass123!
user3,user3@test.com,Bob,Johnson,moderator,Pass123!
user4,user4@test.com,Alice,Williams,user,Pass123!
user5,user5@test.com,Charlie,Brown,user,Pass123!
```

```csv
// cypress/fixtures/products.csv
name,category,price,stock,description
Laptop Pro,Electronics,1299.99,10,High-performance laptop
Wireless Mouse,Accessories,29.99,50,Ergonomic wireless mouse
Keyboard RGB,Accessories,89.99,30,Mechanical RGB keyboard
Monitor 4K,Electronics,599.99,15,27-inch 4K monitor
Webcam HD,Electronics,79.99,25,1080p HD webcam
```

**CSV Parser Utility:**
```javascript
// cypress/support/csvParser.js
import { parse } from 'csv-parse/sync';

export function parseCSV(csvContent) {
  return parse(csvContent, {
    columns: true,
    skip_empty_lines: true,
    trim: true
  });
}

export function parseCSVFile(filePath) {
  return cy.fixture(filePath).then((csvContent) => {
    return parseCSV(csvContent);
  });
}
```

**Custom Commands:**
```javascript
// cypress/support/commands.js
import { parseCSVFile } from './csvParser';

Cypress.Commands.add('loadCSV', (filePath) => {
  return parseCSVFile(filePath);
});

Cypress.Commands.add('importUsersFromCSV', (csvFile) => {
  cy.visit('/import-users');
  cy.get('input[type="file"]').selectFile(`cypress/fixtures/${csvFile}`);
  cy.get('#preview-btn').click();
  cy.get('.preview-table tbody tr').should('have.length.greaterThan', 0);
  cy.get('#import-btn').click();
  cy.get('.success-message').should('be.visible');
});
```

**Test Implementation:**
```javascript
// cypress/e2e/data-driven/csv-import.cy.js
describe('CSV Data-Driven Tests', () => {
  it('should test login with users from CSV', () => {
    cy.loadCSV('users.csv').then((users) => {
      users.forEach((user) => {
        cy.log(`Testing user: ${user.username}`);
        
        cy.visit('/login');
        cy.get('#username').type(user.username);
        cy.get('#password').type(user.password);
        cy.get('button[type="submit"]').click();
        
        cy.url().should('include', '/dashboard');
        cy.get('.user-role').should('contain', user.role);
        
        cy.get('.logout-btn').click();
      });
    });
  });
  
  it('should import and verify users from CSV', () => {
    cy.importUsersFromCSV('users.csv');
    
    // Verify users in system
    cy.visit('/users');
    
    cy.loadCSV('users.csv').then((users) => {
      users.forEach((user) => {
        cy.get('.user-table').should('contain', user.username);
        cy.get('.user-table').should('contain', user.email);
      });
    });
  });
  
  it('should create products from CSV', () => {
    cy.loadCSV('products.csv').then((products) => {
      cy.visit('/products/manage');
      
      products.forEach((product) => {
        cy.get('#add-product-btn').click();
        
        cy.get('#product-name').type(product.name);
        cy.get('#product-category').select(product.category);
        cy.get('#product-price').type(product.price);
        cy.get('#product-stock').type(product.stock);
        cy.get('#product-description').type(product.description);
        
        cy.get('#save-product-btn').click();
        cy.get('.success-message').should('contain', 'Product created');
      });
      
      // Verify all products created
      cy.visit('/products');
      products.forEach((product) => {
        cy.get('.product-list').should('contain', product.name);
      });
    });
  });
});
```

**Website CSV Import Feature:**
```javascript
// js/csvImport.js
export async function importUsersFromCSV(file) {
  const text = await file.text();
  const lines = text.split('\n');
  const headers = lines[0].split(',').map(h => h.trim());
  
  const users = [];
  for (let i = 1; i < lines.length; i++) {
    if (!lines[i].trim()) continue;
    
    const values = lines[i].split(',').map(v => v.trim());
    const user = {};
    
    headers.forEach((header, index) => {
      user[header] = values[index];
    });
    
    users.push(user);
  }
  
  return users;
}

export function downloadCSVTemplate() {
  const template = `username,email,firstName,lastName,role,password
example1,user1@test.com,John,Doe,user,Pass123!
example2,user2@test.com,Jane,Smith,admin,Pass123!`;
  
  const blob = new Blob([template], { type: 'text/csv' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'users-template.csv';
  a.click();
}
```

---

### **Method 3: Supabase Database (Advanced)**

**Database Test Data Table:**
```sql
-- Create test data table
CREATE TABLE test_data (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  test_suite TEXT NOT NULL,
  test_case TEXT NOT NULL,
  input_data JSONB NOT NULL,
  expected_output JSONB NOT NULL,
  is_active BOOLEAN DEFAULT true,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Insert test data
INSERT INTO test_data (test_suite, test_case, input_data, expected_output) VALUES
('login', 'valid_admin', 
 '{"username": "admin", "password": "admin123"}',
 '{"url": "/dashboard", "role": "admin", "message": "Welcome, Admin"}'),
 
('login', 'valid_user',
 '{"username": "user", "password": "user123"}',
 '{"url": "/profile", "role": "user", "message": "Welcome, User"}'),
 
('login', 'invalid_credentials',
 '{"username": "invalid", "password": "wrong"}',
 '{"error": "Invalid credentials", "url": "/login"}');
```

**Cypress Tasks:**
```javascript
// cypress.config.js
const { createClient } = require('@supabase/supabase-js');

module.exports = defineConfig({
  e2e: {
    setupNodeEvents(on, config) {
      const supabase = createClient(
        process.env.SUPABASE_URL,
        process.env.SUPABASE_SERVICE_KEY
      );
      
      on('task', {
        // Get test data from database
        async 'db:getTestData'(testSuite) {
          const { data, error } = await supabase
            .from('test_data')
            .select('*')
            .eq('test_suite', testSuite)
            .eq('is_active', true);
          
          if (error) throw error;
          return data;
        },
        
        // Create test user in database
        async 'db:createTestUser'(userData) {
          const { data: authData, error: authError } = await supabase.auth.admin.createUser({
            email: userData.email,
            password: userData.password,
            email_confirm: true
          });
          
          if (authError) throw authError;
          
          const { data, error } = await supabase
            .from('users')
            .insert([{
              id: authData.user.id,
              email: userData.email,
              username: userData.username,
              first_name: userData.firstName,
              last_name: userData.lastName,
              role: userData.role
            }])
            .select();
          
          if (error) throw error;
          return data[0];
        },
        
        // Cleanup test data
        async 'db:cleanup'() {
          await supabase.from('users').delete().ilike('email', '%test%');
          await supabase.from('products').delete().ilike('name', '%test%');
          await supabase.from('orders').delete().eq('status', 'test');
          return null;
        },
        
        // Get all users for testing
        async 'db:getAllUsers'() {
          const { data, error } = await supabase
            .from('users')
            .select('*')
            .order('created_at', { ascending: false });
          
          if (error) throw error;
          return data;
        }
      });
    }
  }
});
```

**Custom Commands:**
```javascript
// cypress/support/commands.js
Cypress.Commands.add('getTestData', (testSuite) => {
  return cy.task('db:getTestData', testSuite);
});

Cypress.Commands.add('createTestUser', (userData) => {
  return cy.task('db:createTestUser', userData);
});

Cypress.Commands.add('cleanupTestData', () => {
  return cy.task('db:cleanup');
});

Cypress.Commands.add('getAllUsers', () => {
  return cy.task('db:getAllUsers');
});
```

**Test Implementation:**
```javascript
// cypress/e2e/data-driven/database-driven.cy.js
describe('Database-Driven Tests', () => {
  afterEach(() => {
    cy.cleanupTestData();
  });
  
  it('should test login with data from database', () => {
    cy.getTestData('login').then((testCases) => {
      testCases.forEach((testCase) => {
        cy.log(`Testing: ${testCase.test_case}`);
        
        const input = testCase.input_data;
        const expected = testCase.expected_output;
        
        cy.visit('/login');
        cy.get('#username').type(input.username);
        cy.get('#password').type(input.password);
        cy.get('button[type="submit"]').click();
        
        if (expected.error) {
          cy.get('.error-message').should('contain', expected.error);
        } else {
          cy.url().should('include', expected.url);
          cy.get('.user-role').should('contain', expected.role);
          cy.get('.welcome-message').should('contain', expected.message);
        }
      });
    });
  });
  
  it('should create and verify users from database', () => {
    const usersToCreate = [
      { username: 'testuser1', email: 'test1@test.com', password: 'Pass123!', firstName: 'Test', lastName: 'User1', role: 'user' },
      { username: 'testuser2', email: 'test2@test.com', password: 'Pass123!', firstName: 'Test', lastName: 'User2', role: 'admin' }
    ];
    
    usersToCreate.forEach((userData) => {
      cy.createTestUser(userData);
    });
    
    // Verify in UI
    cy.visit('/users');
    usersToCreate.forEach((user) => {
      cy.get('.user-table').should('contain', user.username);
      cy.get('.user-table').should('contain', user.email);
    });
  });
  
  it('should test with real production data', () => {
    cy.getAllUsers().then((users) => {
      // Test with first 5 users
      const testUsers = users.slice(0, 5);
      
      testUsers.forEach((user) => {
        cy.log(`Testing with real user: ${user.username}`);
        
        cy.visit(`/users/${user.id}`);
        cy.get('.user-profile').should('be.visible');
        cy.get('.username').should('contain', user.username);
        cy.get('.email').should('contain', user.email);
      });
    });
  });
});
```

---

## 📊 Comparison Table

| Method | Difficulty | Setup Time | Flexibility | Best For |
|--------|-----------|------------|-------------|----------|
| **JSON Fixtures** | Easy | 5 min | Medium | Static test data, quick tests |
| **CSV Import** | Medium | 15 min | High | Bulk data, business users |
| **Supabase DB** | Advanced | 30 min | Very High | Real data, integration tests |

---

## 🎓 Learning Path

### **Week 1-4: JSON Fixtures**
- Learn basic data-driven testing
- Practice with simple test cases
- Understand fixture structure

### **Week 5-8: CSV Import**
- Implement CSV parser
- Build import feature in website
- Test bulk operations

### **Week 9-12: Supabase Database**
- Connect to real database
- Test with production-like data
- Implement cleanup strategies

---

## 🎓 Learning Path

### **Beginner (Week 1-3)**
- Practice on Login/Register pages
- Learn form interactions
- Basic assertions
- **Data-Driven:** Simple login with 2-3 users

### **Intermediate (Week 4-7)**
- User management CRUD
- Table operations
- API simulation with intercept
- **Data-Driven:** Multiple test scenarios with fixtures

### **Advanced (Week 8-12)**
- Complete shopping flow
- Custom commands
- Page Object Model
- CI/CD integration
- **Data-Driven:** CSV import, bulk operations, test data generation

---

---

## 🔄 CI/CD Implementation

### **Why CI/CD for Cypress?**
- ✅ Automatic test execution on every commit
- ✅ Early bug detection
- ✅ Consistent test environment
- ✅ Test reports dan screenshots
- ✅ Integration dengan GitHub/GitLab
- ✅ **Portfolio showcase** - Employers love seeing CI/CD experience!

---

### **Option 1: GitHub Actions (Recommended)**

**File:** `.github/workflows/cypress.yml`

```yaml
name: Cypress Tests

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]
  schedule:
    # Run tests every day at 9 AM
    - cron: '0 9 * * *'

jobs:
  cypress-run:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        browser: [chrome, firefox, edge]
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Start web server
        run: |
          npm run start &
          npx wait-on http://localhost:8000
      
      - name: Run Cypress tests
        uses: cypress-io/github-action@v5
        with:
          browser: ${{ matrix.browser }}
          record: true
          parallel: true
          group: 'UI Tests - ${{ matrix.browser }}'
        env:
          CYPRESS_RECORD_KEY: ${{ secrets.CYPRESS_RECORD_KEY }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Upload screenshots on failure
        uses: actions/upload-artifact@v3
        if: failure()
        with:
          name: cypress-screenshots-${{ matrix.browser }}
          path: cypress/screenshots
      
      - name: Upload videos
        uses: actions/upload-artifact@v3
        if: always()
        with:
          name: cypress-videos-${{ matrix.browser }}
          path: cypress/videos
      
      - name: Generate test report
        if: always()
        run: |
          npm run report:merge
          npm run report:generate
      
      - name: Upload test report
        uses: actions/upload-artifact@v3
        if: always()
        with:
          name: test-report-${{ matrix.browser }}
          path: cypress/reports
      
      - name: Send Slack notification
        if: always()
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          text: 'Cypress tests completed on ${{ matrix.browser }}'
          webhook_url: ${{ secrets.SLACK_WEBHOOK }}

  # Deploy to GitHub Pages (for test reports)
  deploy-report:
    needs: cypress-run
    runs-on: ubuntu-latest
    if: always()
    
    steps:
      - name: Download test reports
        uses: actions/download-artifact@v3
        with:
          path: reports
      
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./reports
```

---

### **Option 2: GitLab CI/CD**

**File:** `.gitlab-ci.yml`

```yaml
image: cypress/browsers:node18.12.0-chrome106-ff106

stages:
  - test
  - report
  - deploy

variables:
  npm_config_cache: "$CI_PROJECT_DIR/.npm"
  CYPRESS_CACHE_FOLDER: "$CI_PROJECT_DIR/cache/Cypress"

cache:
  paths:
    - .npm
    - cache/Cypress
    - node_modules

# Install dependencies
install:
  stage: .pre
  script:
    - npm ci
  artifacts:
    paths:
      - node_modules
    expire_in: 1 day

# Run tests in Chrome
test-chrome:
  stage: test
  script:
    - npm run start &
    - npx wait-on http://localhost:8000
    - npx cypress run --browser chrome --record --key $CYPRESS_RECORD_KEY
  artifacts:
    when: always
    paths:
      - cypress/screenshots
      - cypress/videos
      - cypress/reports
    expire_in: 1 week

# Run tests in Firefox
test-firefox:
  stage: test
  script:
    - npm run start &
    - npx wait-on http://localhost:8000
    - npx cypress run --browser firefox
  artifacts:
    when: always
    paths:
      - cypress/screenshots
      - cypress/videos
    expire_in: 1 week

# Generate test report
generate-report:
  stage: report
  script:
    - npm run report:merge
    - npm run report:generate
  artifacts:
    paths:
      - cypress/reports/html
    expire_in: 30 days

# Deploy report to GitLab Pages
pages:
  stage: deploy
  dependencies:
    - generate-report
  script:
    - mkdir public
    - cp -r cypress/reports/html/* public/
  artifacts:
    paths:
      - public
  only:
    - main
```

---

### **Option 3: Jenkins Pipeline**

**File:** `Jenkinsfile`

```groovy
pipeline {
    agent any
    
    environment {
        CYPRESS_CACHE_FOLDER = "${WORKSPACE}/.cache/Cypress"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }
        
        stage('Start Server') {
            steps {
                sh 'npm run start &'
                sh 'npx wait-on http://localhost:8000'
            }
        }
        
        stage('Run Cypress Tests') {
            parallel {
                stage('Chrome') {
                    steps {
                        sh 'npx cypress run --browser chrome --record --key ${CYPRESS_RECORD_KEY}'
                    }
                }
                stage('Firefox') {
                    steps {
                        sh 'npx cypress run --browser firefox'
                    }
                }
            }
        }
        
        stage('Generate Report') {
            steps {
                sh 'npm run report:merge'
                sh 'npm run report:generate'
            }
        }
    }
    
    post {
        always {
            // Archive test results
            archiveArtifacts artifacts: 'cypress/screenshots/**/*', allowEmptyArchive: true
            archiveArtifacts artifacts: 'cypress/videos/**/*', allowEmptyArchive: true
            archiveArtifacts artifacts: 'cypress/reports/**/*', allowEmptyArchive: true
            
            // Publish HTML report
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'cypress/reports/html',
                reportFiles: 'index.html',
                reportName: 'Cypress Test Report'
            ])
            
            // Send email notification
            emailext (
                subject: "Cypress Tests - ${currentBuild.result}",
                body: "Build ${env.BUILD_NUMBER} - ${currentBuild.result}",
                to: 'team@example.com'
            )
        }
        
        failure {
            // Send Slack notification on failure
            slackSend (
                color: 'danger',
                message: "Cypress tests failed: ${env.JOB_NAME} - ${env.BUILD_NUMBER}"
            )
        }
    }
}
```

---

### **Test Reporting Setup**

**Install Dependencies:**
```bash
npm install --save-dev mochawesome mochawesome-merge mochawesome-report-generator
```

**Cypress Configuration:**
```javascript
// cypress.config.js
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  e2e: {
    baseUrl: 'http://localhost:8000',
    
    // Reporter configuration
    reporter: 'mochawesome',
    reporterOptions: {
      reportDir: 'cypress/reports/mochawesome',
      overwrite: false,
      html: false,
      json: true,
      timestamp: 'mmddyyyy_HHMMss'
    },
    
    // Video and screenshot settings
    video: true,
    screenshotOnRunFailure: true,
    videosFolder: 'cypress/videos',
    screenshotsFolder: 'cypress/screenshots',
    
    setupNodeEvents(on, config) {
      // Implement node event listeners here
    }
  }
});
```

**Package.json Scripts:**
```json
{
  "scripts": {
    "start": "http-server -p 8000",
    "cy:open": "cypress open",
    "cy:run": "cypress run",
    "cy:run:chrome": "cypress run --browser chrome",
    "cy:run:firefox": "cypress run --browser firefox",
    "cy:run:edge": "cypress run --browser edge",
    "report:merge": "mochawesome-merge cypress/reports/mochawesome/*.json > cypress/reports/mochawesome-bundle.json",
    "report:generate": "marge cypress/reports/mochawesome-bundle.json -f index.html -o cypress/reports/html",
    "report:open": "open cypress/reports/html/index.html",
    "test": "npm run cy:run && npm run report:merge && npm run report:generate",
    "test:ci": "start-server-and-test start http://localhost:8000 cy:run"
  }
}
```

---

### **Cypress Dashboard Integration**

**Setup Cypress Dashboard:**
```bash
# Login to Cypress Dashboard
npx cypress open

# Get your project ID and record key from dashboard.cypress.io
```

**Record Tests:**
```bash
# Run and record tests
npx cypress run --record --key YOUR_RECORD_KEY

# Run with parallel execution
npx cypress run --record --key YOUR_RECORD_KEY --parallel --group "UI Tests"
```

**Benefits:**
- ✅ Test analytics dan insights
- ✅ Video recordings
- ✅ Screenshots on failure
- ✅ Test history dan trends
- ✅ Flaky test detection
- ✅ Parallel execution

---

### **Docker Support**

**Dockerfile:**
```dockerfile
FROM cypress/browsers:node18.12.0-chrome106-ff106

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy project files
COPY . .

# Run tests
CMD ["npm", "run", "test:ci"]
```

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8000:8000"
    command: npm run start
    
  cypress:
    image: cypress/browsers:node18.12.0-chrome106-ff106
    depends_on:
      - web
    environment:
      - CYPRESS_baseUrl=http://web:8000
    working_dir: /e2e
    volumes:
      - ./:/e2e
    command: npm run cy:run
```

**Run with Docker:**
```bash
# Build and run
docker-compose up --abort-on-container-exit

# Run specific browser
docker-compose run cypress npm run cy:run:chrome
```

---

### **Notifications Setup**

**Slack Integration:**
```javascript
// cypress/plugins/slack.js
const axios = require('axios');

async function sendSlackNotification(results) {
  const webhook = process.env.SLACK_WEBHOOK;
  
  const message = {
    text: `Cypress Test Results`,
    blocks: [
      {
        type: 'section',
        text: {
          type: 'mrkdwn',
          text: `*Test Run Complete*\n✅ Passed: ${results.totalPassed}\n❌ Failed: ${results.totalFailed}\n⏱️ Duration: ${results.totalDuration}ms`
        }
      }
    ]
  };
  
  await axios.post(webhook, message);
}

module.exports = { sendSlackNotification };
```

**Email Notifications:**
```javascript
// cypress/plugins/email.js
const nodemailer = require('nodemailer');

async function sendEmailReport(results) {
  const transporter = nodemailer.createTransport({
    service: 'gmail',
    auth: {
      user: process.env.EMAIL_USER,
      pass: process.env.EMAIL_PASS
    }
  });
  
  const mailOptions = {
    from: 'cypress@example.com',
    to: 'team@example.com',
    subject: `Cypress Test Report - ${results.totalPassed}/${results.totalTests} Passed`,
    html: `
      <h2>Test Results</h2>
      <p>Passed: ${results.totalPassed}</p>
      <p>Failed: ${results.totalFailed}</p>
      <p>Duration: ${results.totalDuration}ms</p>
    `
  };
  
  await transporter.sendMail(mailOptions);
}

module.exports = { sendEmailReport };
```

---

### **Environment-Specific Configurations**

**cypress.env.json (gitignored):**
```json
{
  "baseUrl": "http://localhost:8000",
  "apiUrl": "http://localhost:4000",
  "username": "testuser",
  "password": "testpass"
}
```

**Environment Files:**
```javascript
// cypress.config.js
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  e2e: {
    baseUrl: process.env.CYPRESS_BASE_URL || 'http://localhost:8000',
    env: {
      apiUrl: process.env.API_URL || 'http://localhost:4000',
      environment: process.env.ENVIRONMENT || 'development'
    }
  }
});
```

**CI Environment Variables:**
- `CYPRESS_BASE_URL` - Base URL for tests
- `CYPRESS_RECORD_KEY` - Cypress Dashboard key
- `SLACK_WEBHOOK` - Slack webhook URL
- `EMAIL_USER` - Email for notifications
- `EMAIL_PASS` - Email password

---

---

## 🗄️ Supabase Database Setup

### **Database Schema**

**Users Table:**
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  email TEXT UNIQUE NOT NULL,
  username TEXT UNIQUE NOT NULL,
  first_name TEXT,
  last_name TEXT,
  role TEXT DEFAULT 'user',
  avatar_url TEXT,
  is_active BOOLEAN DEFAULT true,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Enable Row Level Security
ALTER TABLE users ENABLE ROW LEVEL SECURITY;

-- Policy: Users can read all users
CREATE POLICY "Users can read all users"
  ON users FOR SELECT
  USING (true);

-- Policy: Users can update their own data
CREATE POLICY "Users can update own data"
  ON users FOR UPDATE
  USING (auth.uid() = id);
```

**Products Table:**
```sql
CREATE TABLE products (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  description TEXT,
  category TEXT NOT NULL,
  price DECIMAL(10, 2) NOT NULL,
  stock INTEGER DEFAULT 0,
  image_url TEXT,
  rating DECIMAL(3, 2) DEFAULT 0,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Enable RLS
ALTER TABLE products ENABLE ROW LEVEL SECURITY;

-- Policy: Anyone can read products
CREATE POLICY "Anyone can read products"
  ON products FOR SELECT
  USING (true);

-- Policy: Only admins can insert/update/delete
CREATE POLICY "Admins can manage products"
  ON products FOR ALL
  USING (
    EXISTS (
      SELECT 1 FROM users
      WHERE users.id = auth.uid()
      AND users.role = 'admin'
    )
  );
```

**Orders Table:**
```sql
CREATE TABLE orders (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id),
  total_amount DECIMAL(10, 2) NOT NULL,
  status TEXT DEFAULT 'pending',
  shipping_address JSONB,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE order_items (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  order_id UUID REFERENCES orders(id) ON DELETE CASCADE,
  product_id UUID REFERENCES products(id),
  quantity INTEGER NOT NULL,
  price DECIMAL(10, 2) NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Enable RLS
ALTER TABLE orders ENABLE ROW LEVEL SECURITY;
ALTER TABLE order_items ENABLE ROW LEVEL SECURITY;

-- Policy: Users can only see their own orders
CREATE POLICY "Users can see own orders"
  ON orders FOR SELECT
  USING (auth.uid() = user_id);
```

---

### **Supabase Client Setup**

**Install Supabase Client:**
```bash
npm install @supabase/supabase-js
```

**Initialize Supabase:**
```javascript
// js/supabase.js
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = 'YOUR_SUPABASE_URL';
const supabaseKey = 'YOUR_SUPABASE_ANON_KEY';

export const supabase = createClient(supabaseUrl, supabaseKey);
```

---

### **Authentication with Supabase**

**Login:**
```javascript
// js/auth.js
import { supabase } from './supabase.js';

export async function login(email, password) {
  const { data, error } = await supabase.auth.signInWithPassword({
    email,
    password
  });
  
  if (error) {
    throw new Error(error.message);
  }
  
  return data;
}

export async function register(email, password, userData) {
  // Sign up user
  const { data: authData, error: authError } = await supabase.auth.signUp({
    email,
    password
  });
  
  if (authError) {
    throw new Error(authError.message);
  }
  
  // Create user profile
  const { error: profileError } = await supabase
    .from('users')
    .insert([{
      id: authData.user.id,
      email,
      username: userData.username,
      first_name: userData.firstName,
      last_name: userData.lastName
    }]);
  
  if (profileError) {
    throw new Error(profileError.message);
  }
  
  return authData;
}

export async function logout() {
  const { error } = await supabase.auth.signOut();
  if (error) throw new Error(error.message);
}

export async function getCurrentUser() {
  const { data: { user } } = await supabase.auth.getUser();
  return user;
}
```

---

### **CRUD Operations**

**Users:**
```javascript
// js/users.js
import { supabase } from './supabase.js';

export async function getUsers() {
  const { data, error } = await supabase
    .from('users')
    .select('*')
    .order('created_at', { ascending: false });
  
  if (error) throw new Error(error.message);
  return data;
}

export async function createUser(userData) {
  const { data, error } = await supabase
    .from('users')
    .insert([userData])
    .select();
  
  if (error) throw new Error(error.message);
  return data[0];
}

export async function updateUser(id, updates) {
  const { data, error } = await supabase
    .from('users')
    .update(updates)
    .eq('id', id)
    .select();
  
  if (error) throw new Error(error.message);
  return data[0];
}

export async function deleteUser(id) {
  const { error } = await supabase
    .from('users')
    .delete()
    .eq('id', id);
  
  if (error) throw new Error(error.message);
}
```

**Products:**
```javascript
// js/products.js
import { supabase } from './supabase.js';

export async function getProducts(filters = {}) {
  let query = supabase
    .from('products')
    .select('*');
  
  if (filters.category) {
    query = query.eq('category', filters.category);
  }
  
  if (filters.minPrice) {
    query = query.gte('price', filters.minPrice);
  }
  
  if (filters.maxPrice) {
    query = query.lte('price', filters.maxPrice);
  }
  
  if (filters.search) {
    query = query.ilike('name', `%${filters.search}%`);
  }
  
  const { data, error } = await query;
  
  if (error) throw new Error(error.message);
  return data;
}

export async function createProduct(productData) {
  const { data, error } = await supabase
    .from('products')
    .insert([productData])
    .select();
  
  if (error) throw new Error(error.message);
  return data[0];
}
```

---

### **Real-time Subscriptions**

```javascript
// js/realtime.js
import { supabase } from './supabase.js';

export function subscribeToProducts(callback) {
  const subscription = supabase
    .channel('products-channel')
    .on('postgres_changes', 
      { event: '*', schema: 'public', table: 'products' },
      (payload) => {
        callback(payload);
      }
    )
    .subscribe();
  
  return subscription;
}

// Usage
subscribeToProducts((payload) => {
  console.log('Product changed:', payload);
  // Update UI
  refreshProductList();
});
```

---

## 🚀 Vercel Deployment

### **Setup Vercel**

**Install Vercel CLI:**
```bash
npm install -g vercel
```

**Login to Vercel:**
```bash
vercel login
```

**Deploy:**
```bash
# First deployment
vercel

# Production deployment
vercel --prod
```

---

### **Vercel Configuration**

**vercel.json:**
```json
{
  "version": 2,
  "builds": [
    {
      "src": "*.html",
      "use": "@vercel/static"
    },
    {
      "src": "api/**/*.js",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/api/(.*)",
      "dest": "/api/$1"
    },
    {
      "src": "/(.*)",
      "dest": "/$1"
    }
  ],
  "env": {
    "SUPABASE_URL": "@supabase-url",
    "SUPABASE_ANON_KEY": "@supabase-anon-key"
  }
}
```

---

### **Serverless API Routes (Optional)**

**api/users.js:**
```javascript
import { createClient } from '@supabase/supabase-js';

const supabase = createClient(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_ANON_KEY
);

export default async function handler(req, res) {
  if (req.method === 'GET') {
    const { data, error } = await supabase
      .from('users')
      .select('*');
    
    if (error) {
      return res.status(500).json({ error: error.message });
    }
    
    return res.status(200).json(data);
  }
  
  if (req.method === 'POST') {
    const { data, error } = await supabase
      .from('users')
      .insert([req.body])
      .select();
    
    if (error) {
      return res.status(500).json({ error: error.message });
    }
    
    return res.status(201).json(data[0]);
  }
  
  return res.status(405).json({ error: 'Method not allowed' });
}
```

---

### **Environment Variables**

**Add to Vercel Dashboard:**
```
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_KEY=your-service-key (for admin operations)
```

**Or via CLI:**
```bash
vercel env add SUPABASE_URL
vercel env add SUPABASE_ANON_KEY
```

---

### **Automatic Deployments**

**GitHub Integration:**
1. Connect Vercel to GitHub repository
2. Every push to `main` → Production deployment
3. Every PR → Preview deployment
4. Automatic rollback on failure

**Deployment Workflow:**
```
Developer Push → GitHub → GitHub Actions (Run Tests) → 
If Tests Pass → Vercel Deploy → Live Website
If Tests Fail → Block Deployment → Notify Developer
```

---

### **Custom Domain (Optional)**

**Add Custom Domain:**
```bash
vercel domains add yourdomain.com
```

**Configure DNS:**
```
Type: CNAME
Name: www
Value: cname.vercel-dns.com
```

---

## 🧪 Cypress Tests with Supabase

### **Test Database Setup**

**Create Test User:**
```javascript
// cypress/support/commands.js
Cypress.Commands.add('createTestUser', () => {
  return cy.task('supabase:createUser', {
    email: 'test@example.com',
    password: 'TestPass123!',
    username: 'testuser'
  });
});

Cypress.Commands.add('cleanupTestData', () => {
  return cy.task('supabase:cleanup');
});
```

**Cypress Tasks:**
```javascript
// cypress.config.js
const { createClient } = require('@supabase/supabase-js');

module.exports = defineConfig({
  e2e: {
    setupNodeEvents(on, config) {
      const supabase = createClient(
        process.env.SUPABASE_URL,
        process.env.SUPABASE_SERVICE_KEY // Use service key for admin operations
      );
      
      on('task', {
        async 'supabase:createUser'(userData) {
          const { data, error } = await supabase.auth.admin.createUser({
            email: userData.email,
            password: userData.password,
            email_confirm: true
          });
          
          if (error) throw error;
          
          // Create user profile
          await supabase.from('users').insert([{
            id: data.user.id,
            email: userData.email,
            username: userData.username
          }]);
          
          return data.user;
        },
        
        async 'supabase:cleanup'() {
          // Delete test data
          await supabase.from('users').delete().ilike('email', '%test%');
          await supabase.from('products').delete().ilike('name', '%test%');
          return null;
        }
      });
    }
  }
});
```

---

### **Testing with Real Database**

```javascript
describe('User Management with Supabase', () => {
  beforeEach(() => {
    cy.createTestUser();
    cy.visit('/login');
  });
  
  afterEach(() => {
    cy.cleanupTestData();
  });
  
  it('should create user in database', () => {
    cy.visit('/register');
    
    const userData = {
      email: 'newuser@test.com',
      password: 'TestPass123!',
      username: 'newuser'
    };
    
    cy.get('#email').type(userData.email);
    cy.get('#password').type(userData.password);
    cy.get('#username').type(userData.username);
    cy.get('button[type="submit"]').click();
    
    // Verify user in database
    cy.task('supabase:getUser', userData.email).then((user) => {
      expect(user).to.not.be.null;
      expect(user.username).to.equal(userData.username);
    });
  });
});
```

---

## 📊 CI/CD Best Practices

### **1. Test Optimization**
- ✅ Run tests in parallel
- ✅ Use test retries for flaky tests
- ✅ Cache dependencies
- ✅ Use Docker for consistent environment

### **2. Reporting**
- ✅ Generate HTML reports
- ✅ Archive screenshots and videos
- ✅ Send notifications on failure
- ✅ Track test trends

### **3. Security**
- ✅ Use secrets for sensitive data
- ✅ Don't commit credentials
- ✅ Use environment variables
- ✅ Rotate keys regularly

### **4. Monitoring**
- ✅ Track test execution time
- ✅ Monitor flaky tests
- ✅ Set up alerts for failures
- ✅ Review test coverage

---

---

## ♿ Accessibility Testing

### **Why Accessibility Matters**
- ✅ **Legal Requirement** - WCAG compliance
- ✅ **Better UX** - Helps all users
- ✅ **SEO Benefits** - Better search rankings
- ✅ **Portfolio Value** - Shows professional awareness

---

### **Cypress Accessibility Plugin**

**Install:**
```bash
npm install --save-dev cypress-axe axe-core
```

**Setup:**
```javascript
// cypress/support/e2e.js
import 'cypress-axe';

// Custom command for accessibility check
Cypress.Commands.add('checkA11y', (context, options) => {
  cy.injectAxe();
  cy.checkA11y(context, options, (violations) => {
    if (violations.length) {
      cy.task('log', `${violations.length} accessibility violations found`);
      violations.forEach((violation) => {
        cy.task('log', `${violation.id}: ${violation.description}`);
      });
    }
  });
});
```

**Cypress Config:**
```javascript
// cypress.config.js
module.exports = defineConfig({
  e2e: {
    setupNodeEvents(on, config) {
      on('task', {
        log(message) {
          console.log(message);
          return null;
        }
      });
    }
  }
});
```

---

### **Accessibility Tests**

**Basic Accessibility Check:**
```javascript
// cypress/e2e/accessibility/a11y-basic.cy.js
describe('Accessibility Tests', () => {
  beforeEach(() => {
    cy.visit('/');
    cy.injectAxe();
  });
  
  it('should have no accessibility violations on homepage', () => {
    cy.checkA11y();
  });
  
  it('should have no violations on login page', () => {
    cy.visit('/login');
    cy.checkA11y();
  });
  
  it('should have no violations on dashboard', () => {
    cy.login('admin', 'admin123');
    cy.checkA11y();
  });
});
```

**Specific Element Testing:**
```javascript
describe('Form Accessibility', () => {
  it('should have accessible form elements', () => {
    cy.visit('/register');
    cy.injectAxe();
    
    // Check specific form
    cy.checkA11y('form');
    
    // Check all inputs have labels
    cy.get('input').each(($input) => {
      const id = $input.attr('id');
      cy.get(`label[for="${id}"]`).should('exist');
    });
  });
  
  it('should have proper ARIA labels', () => {
    cy.visit('/products');
    cy.injectAxe();
    
    // Check buttons have accessible names
    cy.get('button').each(($btn) => {
      const ariaLabel = $btn.attr('aria-label');
      const text = $btn.text();
      
      expect(ariaLabel || text).to.not.be.empty;
    });
  });
});
```

**Keyboard Navigation:**
```javascript
describe('Keyboard Navigation', () => {
  it('should navigate with keyboard', () => {
    cy.visit('/');
    
    // Tab through focusable elements
    cy.get('body').tab();
    cy.focused().should('have.attr', 'href', '/login');
    
    cy.focused().tab();
    cy.focused().should('have.attr', 'href', '/register');
    
    // Enter key should activate
    cy.focused().type('{enter}');
    cy.url().should('include', '/register');
  });
  
  it('should have skip to main content link', () => {
    cy.visit('/');
    cy.get('body').tab();
    cy.focused().should('contain', 'Skip to main content');
  });
});
```

**Color Contrast:**
```javascript
describe('Color Contrast', () => {
  it('should have sufficient color contrast', () => {
    cy.visit('/');
    cy.injectAxe();
    
    cy.checkA11y(null, {
      rules: {
        'color-contrast': { enabled: true }
      }
    });
  });
});
```

---

### **Accessibility Checklist for Website**

**HTML Structure:**
- ✅ Semantic HTML5 elements (`<header>`, `<nav>`, `<main>`, `<footer>`)
- ✅ Proper heading hierarchy (h1 → h2 → h3)
- ✅ Skip to main content link
- ✅ Landmark regions with ARIA labels

**Forms:**
- ✅ All inputs have associated labels
- ✅ Error messages are announced
- ✅ Required fields marked with `aria-required`
- ✅ Form validation feedback

**Images:**
- ✅ All images have alt text
- ✅ Decorative images have `alt=""`
- ✅ Complex images have detailed descriptions

**Interactive Elements:**
- ✅ Keyboard accessible
- ✅ Focus indicators visible
- ✅ ARIA labels for icon buttons
- ✅ Tab order logical

**Color & Contrast:**
- ✅ WCAG AA contrast ratio (4.5:1)
- ✅ Don't rely on color alone
- ✅ Focus indicators visible

---

## 👁️ Visual Regression Testing

### **Why Visual Testing**
- ✅ Catch UI bugs automatically
- ✅ Cross-browser consistency
- ✅ Responsive design validation
- ✅ Prevent CSS regressions

---

### **Option 1: Percy.io (Recommended)**

**Setup:**
```bash
npm install --save-dev @percy/cli @percy/cypress
```

**Configuration:**
```javascript
// cypress/support/e2e.js
import '@percy/cypress';
```

**Percy Config:**
```yaml
# .percy.yml
version: 2
snapshot:
  widths:
    - 375   # Mobile
    - 768   # Tablet
    - 1280  # Desktop
  min-height: 1024
  percy-css: |
    /* Hide dynamic content */
    .timestamp { display: none; }
    .random-id { display: none; }
```

**Visual Tests:**
```javascript
// cypress/e2e/visual/visual-regression.cy.js
describe('Visual Regression Tests', () => {
  it('should match homepage snapshot', () => {
    cy.visit('/');
    cy.percySnapshot('Homepage');
  });
  
  it('should match login page snapshot', () => {
    cy.visit('/login');
    cy.percySnapshot('Login Page');
  });
  
  it('should match dashboard snapshot', () => {
    cy.login('admin', 'admin123');
    cy.percySnapshot('Dashboard - Admin View');
  });
  
  it('should match product catalog', () => {
    cy.visit('/products');
    cy.percySnapshot('Product Catalog', {
      widths: [375, 768, 1280]
    });
  });
  
  it('should match modal states', () => {
    cy.visit('/users');
    cy.get('#add-user-btn').click();
    cy.percySnapshot('Add User Modal');
  });
});
```

**Responsive Testing:**
```javascript
describe('Responsive Visual Tests', () => {
  const viewports = [
    { name: 'mobile', width: 375, height: 667 },
    { name: 'tablet', width: 768, height: 1024 },
    { name: 'desktop', width: 1280, height: 720 }
  ];
  
  viewports.forEach((viewport) => {
    it(`should match on ${viewport.name}`, () => {
      cy.viewport(viewport.width, viewport.height);
      cy.visit('/');
      cy.percySnapshot(`Homepage - ${viewport.name}`);
    });
  });
});
```

---

### **Option 2: Cypress Screenshot Comparison**

**Plugin:**
```bash
npm install --save-dev cypress-image-snapshot
```

**Setup:**
```javascript
// cypress/support/commands.js
import { addMatchImageSnapshotCommand } from 'cypress-image-snapshot/command';

addMatchImageSnapshotCommand({
  failureThreshold: 0.03, // 3% threshold
  failureThresholdType: 'percent',
  customDiffConfig: { threshold: 0.1 },
  capture: 'viewport'
});
```

**Tests:**
```javascript
describe('Screenshot Comparison', () => {
  it('should match baseline screenshot', () => {
    cy.visit('/');
    cy.matchImageSnapshot('homepage');
  });
  
  it('should match with custom threshold', () => {
    cy.visit('/dashboard');
    cy.matchImageSnapshot('dashboard', {
      failureThreshold: 0.05,
      failureThresholdType: 'percent'
    });
  });
});
```

---

### **CI/CD Integration**

**GitHub Actions with Percy:**
```yaml
# .github/workflows/visual-tests.yml
name: Visual Regression Tests

on: [push, pull_request]

jobs:
  percy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run Percy tests
        run: npx percy exec -- cypress run
        env:
          PERCY_TOKEN: ${{ secrets.PERCY_TOKEN }}
```

---

## 📊 Test Reporting Dashboard

### **Mochawesome Enhanced Reports**

**Install:**
```bash
npm install --save-dev mochawesome mochawesome-merge mochawesome-report-generator
```

**Custom Reporter:**
```javascript
// cypress/support/reporter.js
const addContext = require('mochawesome/addContext');

Cypress.on('test:after:run', (test, runnable) => {
  if (test.state === 'failed') {
    const screenshot = `screenshots/${Cypress.spec.name}/${runnable.parent.title} -- ${test.title} (failed).png`;
    addContext({ test }, screenshot);
  }
});
```

**Generate Report:**
```json
{
  "scripts": {
    "test:report": "cypress run --reporter mochawesome",
    "report:merge": "mochawesome-merge cypress/reports/mochawesome/*.json > cypress/reports/report.json",
    "report:generate": "marge cypress/reports/report.json -f index.html -o cypress/reports/html --inline",
    "report:open": "open cypress/reports/html/index.html"
  }
}
```

---

## 🎯 Complete Testing Strategy

### **Test Pyramid**

```
        /\
       /  \
      / E2E \          ← Cypress (UI + API)
     /______\
    /        \
   / Integration\      ← API Tests
  /____________\
 /              \
/  Unit Tests    \     ← (Not in scope)
/________________\
```

### **Testing Checklist**

**Functional Testing:**
- ✅ Login/Logout flows
- ✅ CRUD operations
- ✅ Form validations
- ✅ Search & filters
- ✅ Pagination
- ✅ Error handling

**Data-Driven Testing:**
- ✅ JSON fixtures
- ✅ CSV import
- ✅ Database queries

**API Testing:**
- ✅ GET/POST/PUT/DELETE
- ✅ Authentication
- ✅ Error responses
- ✅ Rate limiting

**Accessibility:**
- ✅ WCAG compliance
- ✅ Keyboard navigation
- ✅ Screen reader support
- ✅ Color contrast

**Visual Regression:**
- ✅ Cross-browser
- ✅ Responsive design
- ✅ Component states
- ✅ Dark mode

**Performance:**
- ✅ Page load times
- ✅ API response times
- ✅ Bundle size

---

## ✅ Deliverables

1. **Complete Website** - All pages functional
2. **Sample Cypress Tests** - Tests untuk setiap feature
3. **Page Object Models** - POM untuk semua pages
4. **Custom Commands** - Reusable commands
5. **Test Data** - Fixtures dan test data
6. **Documentation** - README dengan instructions
7. **Video Demo** - Screen recording demo website

---

## 📊 Success Criteria

- [ ] Website bisa run di localhost
- [ ] Semua fitur berfungsi dengan baik
- [ ] LocalStorage working untuk data persistence
- [ ] Responsive design (mobile & desktop)
- [ ] Sample Cypress tests passing
- [ ] Documentation lengkap
- [ ] Easy to modify dan extend

---

## 🎯 Next Steps

1. **Review Planning** - User review dan approval
2. **Build Website** - Implement semua pages
3. **Create Sample Tests** - Cypress tests untuk demo
4. **Documentation** - Setup guide dan usage
5. **Testing** - Verify semua functionality
6. **Delivery** - Push to repository

---

**Estimasi Waktu:** 4-6 jam untuk complete implementation

**Apakah planning ini sudah sesuai dengan kebutuhan Anda?** 🚀
