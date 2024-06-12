# Project Documentation

## Overview
This project involves displaying a list of products on a web page and allowing users to add items to a shopping cart. It also includes updating the cart quantity dynamically as items are added. The key components include HTML for displaying products, JavaScript for handling user interactions, and utility functions for various operations.

## File Structure
- **index.html**: Contains the structure of the web page.
- **data/**: 
  - **cart.js**: Contains the cart data and functions to manipulate the cart.
  - **products.js**: Contains the product data.
- **utils/**: 
  - **money.js**: Contains utility functions for handling money formatting.
- **styles/**: CSS files for styling the web page.

## Products Display
Products are displayed dynamically by generating HTML from product data and inserting it into the DOM.

### Code
```javascript
import {cart, addToCart} from '../data/cart.js';
import {products} from '../data/products.js';
import {formatCurrency} from './utils/money.js';

let productsHTML = '';

products.forEach((product) => {
  productsHTML += `
    <div class="product-container">
      <div class="product-image-container">
        <img class="product-image"
          src="${product.image}">
      </div>

      <div class="product-name limit-text-to-2-lines">
        ${product.name}
      </div>

      <div class="product-rating-container">
        <img class="product-rating-stars"
          src="${product.getStarsUrl()}">
        <div class="product-rating-count link-primary">
          ${product.rating.count}
        </div>
      </div>

      <div class="product-price">
        ${product.getPrice()}
      </div>

      <div class="product-quantity-container">
        <select>
          <option selected value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
          <option value="4">4</option>
          <option value="5">5</option>
          <option value="6">6</option>
          <option value="7">7</option>
          <option value="8">8</option>
          <option value="9">9</option>
          <option value="10">10</option>
        </select>
      </div>

      ${product.extraInfoHTML()}

      <div class="product-spacer"></div>

      <div class="added-to-cart">
        <img src="images/icons/checkmark.png">
        Added
      </div>

      <button class="add-to-cart-button button-primary js-add-to-cart"
      data-product-id="${product.id}">
        Add to Cart
      </button>
    </div>
  `;
});

document.querySelector('.js-products-grid').innerHTML = productsHTML;
```

## Adding Items to Cart
Items are added to the cart by attaching event listeners to the "Add to Cart" buttons. The cart quantity is updated dynamically.

### Code
```javascript
function updateCartQuantity() {
  let cartQuantity = 0;

  cart.forEach((cartItem) => {
    cartQuantity += cartItem.quantity;
  });

  document.querySelector('.js-cart-quantity')
    .innerHTML = cartQuantity;
}

document.querySelectorAll('.js-add-to-cart')
  .forEach((button) => {
    button.addEventListener('click', () => {
      const productId = button.dataset.productId;
      addToCart(productId);
      updateCartQuantity();
    });
  });
```

## Utility Functions
### formatCurrency
This function formats a number as currency.

### Code
```javascript
export function formatCurrency(value) {
  return `$${value.toFixed(2)}`;
}
```

## cart.js
Contains functions for manipulating the cart data.
### Code
```javascript
export let cart = [];

export function addToCart(productId) {
  const existingItem = cart.find(item => item.productId === productId);
  if (existingItem) {
    existingItem.quantity++;
  } else {
    cart.push({productId, quantity: 1});
  }
}
```

## products.js
Contains product data and methods for product operations.

### Code
```javascript
export const products = [
  // Product data here
];

products.forEach(product => {
  product.getStarsUrl = function() {
    return `images/stars/${this.rating.stars}.png`;
  };

  product.getPrice = function() {
    return formatCurrency(this.price);
  };

  product.extraInfoHTML = function() {
    // Generate extra info HTML if any
    return '';
  };
});
```

## Conclusion
This documentation provides an overview of the project, explaining how products are displayed, how items are added to the cart, and utility functions used. The provided code snippets and explanations should help in understanding and maintaining the project.
