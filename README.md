## Awesome Cart JS

A simple shopping cart front end javascript library. Awc does not have a backend, instead it relies on adapters to fetch products and place orders.
The goal of Awc is to quickly deploy a shopping cart front end with little to no changes to your existing website code base while maintaining modern cart flexibility on any page you would like to sell a product from.

### Getting Started

On its own awcjs doesn't do much, the source includes a demo adapter to show how
setting up a simple cart works. Currently there is only one other adapter which
works with awcjs(AWC for ErpNext), however build adapters should not be too difficult
a task if you know your backends well enough.

### Building a Catalog page

Awc requires at least one empty element in your page which it will use to feed
products listing.

So, somewhere in your page add an empty element and give it an ID:

```html
<div id="products"></div>
```

Next include the library close to the bottom of your page:

```html
<script src="js/awc.standalone.js" type="text/javascript"></script>
```

Finally define the product feed and bootstrap the cart.

```html
<script type="text/javascript">

  // create a cart instance
  var cart = new awc.AwesomeCart()

  // A product feed is an automated way of fetching product listings from a
  // previously defined data store. The default data store is the included
  // DemoStoreaAdapter class
  cart.newProductFeed('products', {
    container: '#products',
    product_template: cart.template('product_template.html') // define a handlebars template location
  })

  // kickoff the cart logic
  cart.bootstrap()
</script>
```

### Product Templates

AWC uses the handlebars templating library to ease building of product listings.
You can simply reference a template using awc.getTemplate() or cart.template() functions to define what
template to use when a product feed is being refreshed.

As of this time the template context is the same object as received from the store adapter:

```json
{
  "sku": "Product SKU",
  "name": "Product Name",
  "description": "Product Description",
  "price": 100,
  "tags": ["tag1", "tag2", "etc..."],
  "details": {}
}
```

### adapters

If you don't provide an adapter instance during the creation of awcjs a DemoStoreAdapter instance will be created and assigned to your cart instance.

Currently only AWC for ErpNext adapter exists. The Adapter API is quite small and easy
to extend, so new carts or existing backends should be relatively easy to hook up.

TODO: Explain the adapter API concepts

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### awc

### StoreAdapter

**Extends EventEmitter**

The base store adapter class. All adapters are required to extend from this class.

#### fetchProducts

Implements feting products from a backend.

**Parameters**

-   `tags`  
-   `terms`  
-   `start`  
-   `limit`  

#### fetchCartSession

Implements session fetching from a backend

#### sessionAction

Session actions sent to a backend.
Expected actions are:

-   addToCart
-   removeFromCart

**Parameters**

-   `action`  
-   `data`  

#### loadTemplate

This is an optional method to implement js based templates from your own
backend. The returning value should be a BlueBird promise.

**Parameters**

-   `name`  

#### validate

Used when the cart needs to be validate before checkout.
This method can be used as an opportunity to further modify cart data and
submit checkout request to the server on validation.
Return null on success or any other object with error information.

### Template

**Extends EventEmitter**

Handlebars wrapper to add custom features to our templates

### DemoStoreaAdapter

**Extends StoreAdapter**

A demo store adapter with hardcoded products to demonstrate how adapters work.

### filters

Array, gets the active filters for this feed.

Returns **any** Array

### filters

Sets the active filters for this feed. The feed is immediately
refreshed with this property is changed.

**Parameters**

-   `filters`  Array

### ProductFeed

**Extends Feed**

ProductFeed manages updating product listing elements on the live webpage.

#### constructor

Instantiates a product feed

**Parameters**

-   `cart`  string     The AwesomeCart instance
-   `name`  string     The feed name
-   `options`  object  Feed options
-   `params` **...any** 

### CartFeed

**Extends Feed**

CartFeed manages updating cart listing elements on the live webpage.

#### constructor

Instantiates a product feed

**Parameters**

-   `cart`  string     The AwesomeCart instance
-   `name`  string     The feed name
-   `options`  object  Feed options
-   `params` **...any** 

### AwesomeCart

**Extends EventEmitter**

The main cart class. All managing of shopping cart happens here.

#### totals

Returns stored totals. These values should be set and calculated by the store adapter.

#### totalItems

Returns total line items in the cart.

Returns **any** int

#### totalCount

Returns sum of all qty values in cart.

Returns **any** int

#### items

Returns the list of line items in the cart.

Returns **any** Array

#### newProductFeed

Adds a new managed product feed. Product feeds automate fetching and displaying
product listings.

**Parameters**

-   `name`  string     The feed name.
-   `options`  object  An object defining the feed properties and behaviour.

#### updateUI

Updates click event references and overall UI handling

#### addToCart

Adds a product to the cart by its sku and qty amount.

**Parameters**

-   `sku`  string      The product sku to track in the cart.
-   `qty`  int         The product qty to add to cart.
-   `options`  object  Customization options associated with product

#### removeFromCartBySKU

Removes a qty of skus in the cart

**Parameters**

-   `sku`  string  The product sku to remove in the cart.
-   `qty`  int     The product qty to remove to cart.

#### bootstrap

Kickstarts the cart logic. This causes session data to be downloaded while
the cart is populated from a past session. Also feeds are initialized here
to start consuming product listing and cart data.

### utils

#### uuid

Low quality guid using Math.random.
Only use if you are sure you don't need high quality randomness
