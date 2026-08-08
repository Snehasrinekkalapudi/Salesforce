# E-Commerce Architecture — Flows

## 1. Overall Application Flow

```text
User
  ↓
Product List
  ↓
Apex Controller
  ↓
SOQL
  ↓
Product__c
  ↓
Products returned to LWC
  ↓
Product Cards displayed
```

## 2. Product Loading Flow

```text
User opens application
        ↓
ProductList loads
        ↓
@wire calls getProducts()
        ↓
ProductController
        ↓
SOQL queries Product__c
        ↓
Products returned
        ↓
ProductList receives data
        ↓
Product Cards displayed
```

## 3. Loading State Flow

```text
User opens application
        ↓
Apex request starts
        ↓
Loading State displayed
        ↓
Apex response received
        ↓
Loading State ends
```

## 4. Success Flow

```text
Apex request
     ↓
Products returned
     ↓
ProductList receives data
     ↓
Products available
     ↓
Product Cards displayed
```

## 5. Empty State Flow

```text
Apex request
     ↓
Apex returns no products
     ↓
ProductList checks data
     ↓
No products found
     ↓
Empty State displayed
```

## 6. Error State Flow

```text
Apex request
     ↓
Error occurs
     ↓
ProductList receives error
     ↓
Error state stored
     ↓
Error message displayed
```

## 7. Product List → Product Card Flow

```text
ProductList
     ↓
Receives products
     ↓
Loops through products
     ↓
Passes one product
     ↓
ProductCard
     ↓
Displays product information
```

## 8. View Details Flow

```text
User
  ↓
Clicks View Details
  ↓
ProductCard
  ↓
productselect CustomEvent
  ↓
ProductList
  ↓
handleProductSelect()
  ↓
selectedProduct
  ↓
Selected Product displayed
```

## 9. Add to Cart Flow

```text
User
  ↓
Clicks Add to Cart
  ↓
ProductCard
  ↓
addtocart CustomEvent
  ↓
ProductList
  ↓
handleAddToCart()
  ↓
Check product in cart
  ↓
     ┌─────────────────┐
     │ Already exists? │
     └────────┬────────┘
          YES │     NO
              │      │
              ↓      ↓
        Increase    Add product
        quantity    quantity = 1
              │      │
              └──┬───┘
                 ↓
            Shopping Cart
```

## 10. Quantity Increase Flow

```text
User
  ↓
Clicks +
  ↓
handleIncrease()
  ↓
Find product in cart
  ↓
Increase quantity
  ↓
Cart updated
  ↓
UI re-renders
```

## 11. Quantity Decrease Flow

```text
User
  ↓
Clicks -
  ↓
handleDecrease()
  ↓
Find product in cart
  ↓
Decrease quantity
  ↓
Is quantity > 0?
  ↓
 ┌──────────────┐
 │              │
YES            NO
 │              │
 ↓              ↓
Keep item    Remove item
 │              │
 └──────┬───────┘
        ↓
   Cart updated
```

## 12. Complete E-Commerce User Flow

```text
User
 ↓
Open Product Portal
 ↓
Loading
 ↓
Products Retrieved
 ↓
Product List
 ↓
Product Card
 ↓
 ┌───────────────────┐
 │                   │
 ↓                   ↓
View Details      Add to Cart
 │                   │
 ↓                   ↓
Product Details   Shopping Cart
                     ↓
                Increase / Decrease
                     ↓
                  Quantity
                     ↓
                  Cart Total
                     ↓
                  Checkout
```
