# Minggu 7: DOM Manipulation & jQuery

## 📚 Pengantar

Cypress menggunakan jQuery di balik layar untuk DOM manipulation. Minggu ini kita akan belajar bagaimana bekerja dengan DOM elements menggunakan jQuery methods dalam Cypress tests.

---

## 1️⃣ Cypress Menggunakan jQuery

Setiap kali Cypress yields sebuah element, itu adalah jQuery object.

```javascript
describe('jQuery in Cypress', () => {
  it('should use jQuery methods', () => {
    cy.get('.user-list li').then(($items) => {
      // $items is a jQuery object
      
      // Get text
      const firstText = $items.first().text();
      const lastText = $items.last().text();
      
      // Get length
      const count = $items.length;
      
      // Check class
      const hasActive = $items.first().hasClass('active');
      
      // Get attribute
      const userId = $items.first().attr('data-user-id');
      
      // Filter
      const activeItems = $items.filter('.active');
      
      cy.log(`First: ${firstText}, Last: ${lastText}`);
      cy.log(`Total items: ${count}`);
      cy.log(`Active items: ${activeItems.length}`);
    });
  });
});
```

---

## 2️⃣ DOM Traversal

### find() - Find Descendants

```javascript
describe('find()', () => {
  it('should find child elements', () => {
    cy.get('.user-card').find('.user-name').should('be.visible');
    cy.get('.user-card').find('.user-email').should('contain', '@');
    
    // Multiple levels
    cy.get('.container').find('.section').find('.item').should('exist');
  });
});
```

---

### parent() - Find Parent

```javascript
describe('parent()', () => {
  it('should find parent element', () => {
    cy.get('.delete-button').parent().should('have.class', 'actions');
    
    // Verify button is inside specific container
    cy.get('.submit-btn')
      .parent()
      .should('have.class', 'form-footer');
  });
});
```

---

### parents() - Find All Ancestors

```javascript
describe('parents()', () => {
  it('should find all parent elements', () => {
    cy.get('.nested-item')
      .parents('.container')
      .should('have.length', 1);
  });
});
```

---

### siblings() - Find Siblings

```javascript
describe('siblings()', () => {
  it('should find sibling elements', () => {
    cy.get('.first-name').siblings('.last-name').should('exist');
    cy.get('.first-name').siblings('.email').should('contain', '@');
    
    // All siblings
    cy.get('.item').first().siblings().should('have.length.greaterThan', 0);
  });
});
```

---

### next() / prev() - Adjacent Siblings

```javascript
describe('next() and prev()', () => {
  it('should find next sibling', () => {
    cy.get('.item').first().next().should('have.class', 'item');
  });
  
  it('should find previous sibling', () => {
    cy.get('.item').last().prev().should('have.class', 'item');
  });
});
```

---

### closest() - Find Nearest Ancestor

```javascript
describe('closest()', () => {
  it('should find nearest ancestor', () => {
    cy.get('.edit-button')
      .closest('.user-card')
      .should('be.visible');
    
    cy.get('.nested-element')
      .closest('[data-testid="container"]')
      .should('exist');
  });
});
```

---

### within() - Scope Commands

```javascript
describe('within()', () => {
  it('should scope commands to element', () => {
    cy.get('.user-card').first().within(() => {
      cy.get('.user-name').should('contain', 'John');
      cy.get('.user-email').should('contain', '@');
      cy.get('.user-role').should('contain', 'Admin');
    });
  });
  
  it('should test table rows', () => {
    cy.get('table tbody tr').each(($row) => {
      cy.wrap($row).within(() => {
        cy.get('td').first().should('not.be.empty');
        cy.get('td').last().should('be.visible');
      });
    });
  });
});
```

---

## 3️⃣ Element Properties

### Checking Visibility

```javascript
describe('Element Visibility', () => {
  it('should check if element is visible', () => {
    cy.get('#submit-button').then(($btn) => {
      const isVisible = $btn.is(':visible');
      cy.log(`Button visible: ${isVisible}`);
      
      if (isVisible) {
        cy.wrap($btn).click();
      }
    });
  });
});
```

---

### Checking State

```javascript
describe('Element State', () => {
  it('should check element states', () => {
    cy.get('#submit-button').then(($btn) => {
      // Check if disabled
      const isDisabled = $btn.is(':disabled');
      
      // Check if checked (for checkboxes/radio)
      const isChecked = $btn.is(':checked');
      
      // Check if selected (for options)
      const isSelected = $btn.is(':selected');
      
      cy.log(`Disabled: ${isDisabled}`);
      cy.log(`Checked: ${isChecked}`);
    });
  });
});
```

---

### Getting Values

```javascript
describe('Getting Element Values', () => {
  it('should get various element properties', () => {
    cy.get('#username').then(($input) => {
      // Get value
      const value = $input.val();
      
      // Get text
      const text = $input.text();
      
      // Get HTML
      const html = $input.html();
      
      // Get attribute
      const placeholder = $input.attr('placeholder');
      const dataId = $input.attr('data-id');
      
      // Get CSS property
      const color = $input.css('color');
      const fontSize = $input.css('font-size');
      
      cy.log(`Value: ${value}`);
      cy.log(`Placeholder: ${placeholder}`);
    });
  });
});
```

---

## 4️⃣ Iterating Elements

### each() - Iterate Over Elements

```javascript
describe('each()', () => {
  it('should iterate over elements', () => {
    cy.get('.product-card').each(($card, index) => {
      cy.wrap($card).within(() => {
        cy.get('.product-name').should('not.be.empty');
        cy.get('.product-price').should('be.visible');
        
        cy.log(`Checking product ${index + 1}`);
      });
    });
  });
  
  it('should verify list items', () => {
    const expectedItems = ['Item 1', 'Item 2', 'Item 3'];
    
    cy.get('.item-list li').each(($li, index) => {
      expect($li.text()).to.equal(expectedItems[index]);
    });
  });
});
```

---

## 5️⃣ Working with Text

### Getting and Verifying Text

```javascript
describe('Text Operations', () => {
  it('should get and verify text', () => {
    cy.get('.username').then(($el) => {
      const text = $el.text();
      const trimmedText = text.trim();
      
      expect(trimmedText).to.not.be.empty;
      expect(trimmedText).to.match(/^[a-zA-Z0-9]+$/);
    });
  });
  
  it('should extract numbers from text', () => {
    cy.get('.price').then(($price) => {
      const priceText = $price.text(); // "$29.99"
      const priceNumber = parseFloat(priceText.replace('$', ''));
      
      expect(priceNumber).to.be.greaterThan(0);
      cy.log(`Price: ${priceNumber}`);
    });
  });
});
```

---

## 🎯 Real-World Examples

### Example 1: Verify Table Data

```javascript
describe('Table Verification', () => {
  it('should verify table structure and data', () => {
    // Verify headers
    cy.get('table thead th').should('have.length', 4);
    
    // Verify each row
    cy.get('table tbody tr').each(($row, index) => {
      cy.wrap($row).within(() => {
        // First column should have user name
        cy.get('td').eq(0).should('not.be.empty');
        
        // Second column should have email
        cy.get('td').eq(1).should('contain', '@');
        
        // Third column should have role
        cy.get('td').eq(2).should('be.oneOf', ['Admin', 'User', 'Moderator']);
        
        // Fourth column should have actions
        cy.get('td').eq(3).find('button').should('exist');
      });
    });
  });
});
```

---

### Example 2: Complex Form Validation

```javascript
describe('Form Validation', () => {
  it('should validate all form fields', () => {
    cy.get('form').within(() => {
      // Find all required fields
      cy.get('[required]').each(($field) => {
        const fieldName = $field.attr('name');
        const fieldType = $field.attr('type');
        
        cy.log(`Validating ${fieldName} (${fieldType})`);
        
        // Verify field has label
        cy.get(`label[for="${$field.attr('id')}"]`).should('exist');
        
        // Verify field is visible
        expect($field.is(':visible')).to.be.true;
      });
      
      // Verify submit button is disabled initially
      cy.get('button[type="submit"]').should('be.disabled');
    });
  });
});
```

---

### Example 3: Dynamic Content Verification

```javascript
describe('Dynamic Content', () => {
  it('should verify dynamically loaded content', () => {
    cy.visit('/dashboard');
    
    // Wait for content to load
    cy.get('.widget').should('have.length.greaterThan', 0);
    
    // Verify each widget
    cy.get('.widget').each(($widget) => {
      cy.wrap($widget).within(() => {
        // Widget should have title
        cy.get('.widget-title').should('be.visible');
        
        // Widget should have content
        cy.get('.widget-content').should('exist');
        
        // Get widget type
        const widgetType = $widget.attr('data-widget-type');
        cy.log(`Widget type: ${widgetType}`);
        
        // Type-specific validation
        if (widgetType === 'chart') {
          cy.get('canvas').should('exist');
        } else if (widgetType === 'table') {
          cy.get('table').should('exist');
        }
      });
    });
  });
});
```

---

## 🎓 Ringkasan Minggu 7

### Yang Sudah Dipelajari:

✅ **jQuery Methods** - text(), val(), attr(), css(), hasClass()
✅ **DOM Traversal** - find(), parent(), siblings(), closest(), within()
✅ **Element Properties** - Checking visibility, state, getting values
✅ **Iteration** - each() untuk loop over elements
✅ **Text Operations** - Getting dan parsing text content

---

### Key Takeaways:

1. **Cypress yields jQuery objects** - Gunakan jQuery methods
2. **within()** sangat berguna untuk scope commands
3. **each()** perfect untuk iterating elements
4. **DOM traversal** memudahkan navigation

---

### Checklist Penguasaan:

- [ ] Comfortable dengan jQuery methods
- [ ] Menguasai DOM traversal
- [ ] Bisa check element properties
- [ ] Iterate elements dengan each()
- [ ] Extract dan parse text content

**Next Week:** Minggu 8 - Error Handling & Debugging! 🚀
