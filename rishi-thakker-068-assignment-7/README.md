# 🛒 E-Commerce Product & Shopping Cart API

A simple, file-based (JSON storage) REST API built with Node.js and Express — no database required.

## Setup

```bash
npm install
cp .env.example .env
npm run dev      # nodemon, auto-restarts on changes
# or
npm start        # plain node
```

Server runs at `http://localhost:3000`.

## Auth flow

This API uses cookie-based sessions (`express-session`). Use a client that persists
cookies between requests — e.g. **Postman** (cookie jar is automatic) or `curl -c/-b`.

```bash
# Register
curl -X POST http://localhost:3000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"alex","email":"alex@shop.com","password":"password123"}'

# Login (save cookies to a file)
curl -c cookies.txt -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"alex@shop.com","password":"password123"}'

# Use the saved cookie for authenticated routes
curl -b cookies.txt http://localhost:3000/api/cart
```

## Endpoints

### Auth
- `POST /api/auth/register`
- `POST /api/auth/login`
- `POST /api/auth/logout`

### Products
- `GET /api/products` — filters: `category`, `minPrice`, `maxPrice`, `inStock=true`, `sort=price_asc|price_desc|rating_desc|newest`
- `GET /api/products/:id`
- `POST /api/products`
- `PUT /api/products/:id`
- `DELETE /api/products/:id`

### Cart (requires login)
- `GET /api/cart`
- `POST /api/cart/items` — `{ "productId": "prod_101", "quantity": 2 }`
- `DELETE /api/cart/items/:productId`
- `POST /api/cart/checkout`

## Data

Sample products are pre-seeded in `data/products.json`. `data/users.json` and
`data/carts.json` start empty and fill up as you register users and use the cart.

## Testing checklist (matches the assignment's verification steps)

1. Products are pre-seeded (5 items, multiple categories). ✅
2. Register + login a user, confirm a session cookie is set.
3. Try adding more of a product than is in stock → expect `400` with an
   "Out of stock" style message.
4. Checkout → confirm `data/products.json` stock decreases and the cart empties.
