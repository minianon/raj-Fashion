# Raj Fashion: Low-Level Design

For the big picture see the [High-Level Design](hld.md).

## 1. Product model

| Field | Example |
|---|---|
| `id` | 1 |
| `name` | Product title |
| `category` | `men`, `women` or `kid` |
| `image` | Imported image asset |
| `new_price`, `old_price` | Sale and original price |

## 2. Cart (`ShopContext`)

| Member | Behavior |
|---|---|
| `cartItems` | `{ [productId]: quantity }`, initialised to 0 for every product |
| `addToCart(id)` | +1 |
| `removeFromCart(id)` | −1 |
| `getTotalCartAmount()` | Σ `new_price × quantity` for items in the cart |
| `getTotalCartItems()` | Σ quantities (shown on the navbar cart icon) |
| `clearCart()` | Resets every quantity to 0 |

## 3. Component tree

```mermaid
flowchart TB
  Router --> Navbar
  Router --> Shop --> Hero & Popular & Offers & NewCollections & NewsLetter
  Router --> ShopCategory --> Item
  Router --> Product --> Breadcrum & ProductDisplay & DescriptionBox & RelatedProducts
  Router --> Cart --> CartItems
  Router --> LoginSignup
  Router --> Footer
```

## 4. Checkout

```mermaid
sequenceDiagram
  actor U as Shopper
  participant P as ProductDisplay
  participant C as ShopContext
  participant CI as CartItems
  U->>P: ADD TO CART
  P->>C: addToCart(id)
  U->>CI: open /cart
  CI->>C: read cartItems, getTotalCartAmount()
  U->>CI: PROCEED TO CHECKOUT
  CI-->>U: "Order placed successfully!"
  CI->>C: clearCart()
```

The cart is not persisted, so it resets on page reload; the promo box shows "Invalid promo code!" because no codes are defined.
