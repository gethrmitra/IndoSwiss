# GitHub Deployment Steps

## Step 1: Create Your GitHub Repository

1. Go to **https://github.com/new**
2. Fill in:
   - **Repository name**: `ecommerce-store`
   - **Description**: "Premium multi-brand watch ecommerce store"
   - **Visibility**: **Public** (required for free GitHub Pages)
3. Click **Create repository**

## Step 2: Push Your Code to GitHub

Run these commands in your terminal:

```bash
cd C:\Users\Sarath\ Narayan\ecommerce-store

# Verify git is set up
git config user.email
git config user.name

# Add the remote (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/ecommerce-store.git
git branch -M main
git push -u origin main
```

## Step 3: Enable GitHub Pages

1. Go to your GitHub repo
2. Click **Settings** → **Pages**
3. Under "Source":
   - Branch: `main`
   - Folder: `/ (root)`
4. Click **Save**
5. Wait 1-2 minutes

**Your store is now live at:**
```
https://YOUR_USERNAME.github.io/ecommerce-store/
```

- **Store**: https://YOUR_USERNAME.github.io/ecommerce-store/store.html
- **Admin**: https://YOUR_USERNAME.github.io/ecommerce-store/admin-master.html
- **Home**: https://YOUR_USERNAME.github.io/ecommerce-store/

---

# Add Payment Processing (Stripe)

## Why Stripe?

- Free to set up
- No monthly fees (only 2.9% + 30¢ per transaction)
- Works with GitHub Pages (client-side + webhooks)
- Handles taxes, refunds, invoicing
- PCI compliant (you never touch card data)

## Step 1: Create Stripe Account

1. Go to **https://stripe.com**
2. Sign up (free)
3. Go to **Developers** → **API Keys**
4. Copy your:
   - **Publishable Key** (public)
   - **Secret Key** (keep private!)

## Step 2: Add Payment Checkout to Store

Create a new file `checkout.html`:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Checkout — Chronos</title>
  <script src="https://js.stripe.com/v3/"></script>
  <style>
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
      max-width: 600px;
      margin: 40px auto;
      padding: 20px;
    }
    .form-group {
      margin-bottom: 20px;
    }
    label {
      display: block;
      font-weight: 600;
      margin-bottom: 8px;
    }
    input, select {
      width: 100%;
      padding: 12px;
      border: 1px solid #ddd;
      border-radius: 6px;
      font-size: 14px;
      box-sizing: border-box;
    }
    button {
      background: #b8860b;
      color: white;
      padding: 12px 24px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-weight: 600;
      width: 100%;
    }
    button:hover {
      background: #92660d;
    }
    .error {
      color: #dc2626;
      margin-top: 8px;
    }
    .success {
      color: #10b981;
      margin-top: 8px;
    }
  </style>
</head>
<body>
  <h1>⌚ Checkout</h1>
  
  <div id="cart-items"></div>
  <h3>Total: $<span id="total">0</span></h3>

  <form id="payment-form">
    <div class="form-group">
      <label>Email</label>
      <input type="email" id="email" required>
    </div>

    <div class="form-group">
      <label>Card Details</label>
      <div id="card-element"></div>
    </div>

    <button type="submit" id="submit-btn">Pay Now</button>
    <div id="error-message" class="error"></div>
    <div id="success-message" class="success"></div>
  </form>

  <script>
    // Replace with your Publishable Key
    const STRIPE_KEY = 'pk_test_YOUR_PUBLISHABLE_KEY_HERE';
    const stripe = Stripe(STRIPE_KEY);
    const elements = stripe.elements();
    const cardElement = elements.create('card');
    cardElement.mount('#card-element');

    // Load cart from localStorage
    const cart = JSON.parse(localStorage.getItem('cart') || '[]');
    const cartContainer = document.getElementById('cart-items');
    let total = 0;

    if (cart.length === 0) {
      cartContainer.innerHTML = '<p>Your cart is empty</p>';
    } else {
      cartContainer.innerHTML = cart.map(item => {
        const price = parseInt(item.price) || 100;
        total += price;
        return `<div style="padding: 12px; background: #f5f5f5; margin-bottom: 8px; border-radius: 6px;">
          <strong>${item.name}</strong> — $${price}
        </div>`;
      }).join('');
    }

    document.getElementById('total').textContent = total;

    // Handle form submission
    document.getElementById('payment-form').addEventListener('submit', async (e) => {
      e.preventDefault();

      const email = document.getElementById('email').value;
      const btn = document.getElementById('submit-btn');
      btn.disabled = true;
      btn.textContent = 'Processing...';

      try {
        // Create payment method
        const { paymentMethod, error } = await stripe.createPaymentMethod({
          type: 'card',
          card: cardElement,
          billing_details: { email }
        });

        if (error) {
          document.getElementById('error-message').textContent = error.message;
          btn.disabled = false;
          btn.textContent = 'Pay Now';
          return;
        }

        // Send to your backend (see next step)
        const response = await fetch('/api/payment', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            amount: total * 100,
            email,
            paymentMethodId: paymentMethod.id
          })
        });

        const result = await response.json();

        if (result.success) {
          localStorage.removeItem('cart');
          document.getElementById('success-message').textContent = 'Payment successful! Order #' + result.orderId;
          setTimeout(() => window.location.href = 'store.html', 2000);
        } else {
          document.getElementById('error-message').textContent = result.error;
          btn.disabled = false;
          btn.textContent = 'Pay Now';
        }
      } catch (err) {
        document.getElementById('error-message').textContent = err.message;
        btn.disabled = false;
        btn.textContent = 'Pay Now';
      }
    });
  </script>
</body>
</html>
```

## Step 3: Backend Integration (Node.js + Express)

For a real backend, create `server.js`:

```javascript
const express = require('express');
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);
const app = express();

app.use(express.json());

app.post('/api/payment', async (req, res) => {
  try {
    const { amount, email, paymentMethodId } = req.body;

    // Create payment intent
    const paymentIntent = await stripe.paymentIntents.create({
      amount,
      currency: 'usd',
      payment_method: paymentMethodId,
      confirm: true,
      receipt_email: email
    });

    if (paymentIntent.status === 'succeeded') {
      res.json({ success: true, orderId: paymentIntent.id });
    } else {
      res.json({ success: false, error: 'Payment failed' });
    }
  } catch (error) {
    res.json({ success: false, error: error.message });
  }
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

Install dependencies:
```bash
npm install express stripe dotenv
```

Create `.env`:
```
STRIPE_SECRET_KEY=sk_test_YOUR_SECRET_KEY_HERE
```

Run:
```bash
node server.js
```

## Step 4: Update Store to Add to Cart

In `store.html`, update the `addToCart` function:

```javascript
function addToCart(name) {
  const price = document.querySelector(`[data-product="${name}"]`).dataset.price;
  const cart = JSON.parse(localStorage.getItem('cart') || '[]');
  cart.push({ name, price, id: Date.now() });
  localStorage.setItem('cart', JSON.stringify(cart));
  document.getElementById('cartCount').textContent = cart.length;
  alert('Added to cart!');
}
```

Then update the "View Cart" button to link to `checkout.html`.

---

## Alternative: Use Stripe Hosted Checkout

**Simpler** — Stripe handles the entire checkout page:

```html
<form action="/create-checkout-session" method="POST">
  <button type="submit">Checkout</button>
</form>
```

Backend:
```javascript
app.post('/create-checkout-session', async (req, res) => {
  const session = await stripe.checkout.sessions.create({
    payment_method_types: ['card'],
    line_items: [
      {
        price_data: {
          currency: 'usd',
          product_data: { name: 'Watch' },
          unit_amount: 50000 // $500
        },
        quantity: 1
      }
    ],
    mode: 'payment',
    success_url: 'https://example.com/success',
    cancel_url: 'https://example.com/cancel'
  });

  res.redirect(303, session.url);
});
```

---

## Deploy Your Backend

Use **Vercel** (free tier):

1. Create account at **vercel.com**
2. Install Vercel CLI: `npm install -g vercel`
3. Deploy: `vercel`
4. Set environment variables in Vercel dashboard

Or use **Heroku** (free tier available):

```bash
heroku create your-app-name
heroku config:set STRIPE_SECRET_KEY=sk_test_...
git push heroku main
```

---

## Summary

**To go live:**
1. ✅ Push to GitHub
2. ✅ Enable GitHub Pages
3. ✅ Set up Stripe account
4. ✅ Add checkout page
5. ✅ Deploy backend (Vercel/Heroku)
6. ✅ Update store links

Your store is now a complete ecommerce platform! 🎉