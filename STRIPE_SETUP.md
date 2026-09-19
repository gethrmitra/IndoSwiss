# Stripe Configuration Guide

## Getting Started with Stripe

### 1. Create a Stripe Account
- Visit https://stripe.com
- Sign up for a free account (test mode is free)
- Verify your email

### 2. Get Your API Keys
1. Log in to [Stripe Dashboard](https://dashboard.stripe.com)
2. Click on "Developers" in the left sidebar
3. Click on "API keys"
4. You'll see:
   - Publishable key (starts with `pk_test_`)
   - Secret key (starts with `sk_test_`)

### 3. Configure Environment Variables
Add to `.env.local`:
```
STRIPE_SECRET_KEY=sk_test_YOUR_KEY_HERE
STRIPE_PUBLISHABLE_KEY=pk_test_YOUR_KEY_HERE
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_YOUR_KEY_HERE
```

### 4. Setup Webhook (Important for Payment Confirmation)
1. Go to [Webhooks](https://dashboard.stripe.com/webhooks)
2. Click "Add endpoint"
3. For local testing:
   - Use Stripe CLI (recommended)
   - Or use ngrok/tunnel service
4. Endpoint URL: `http://localhost:3000/api/webhooks/stripe`
5. Select events:
   - `payment_intent.succeeded`
   - `payment_intent.payment_failed`
6. Get the webhook secret (starts with `whsec_`)
7. Add to `.env.local`:
```
STRIPE_WEBHOOK_SECRET=whsec_YOUR_SECRET_HERE
```

### 5. Using Stripe CLI for Local Webhook Testing

Install Stripe CLI:
```bash
# macOS
brew install stripe/stripe-cli/stripe

# Windows
choco install stripe-cli

# Linux
wget https://dl.stripe.com/stripe_cli_linux_x64.tar.gz
tar -zxf stripe_cli_linux_x64.tar.gz
```

Forward webhooks to local server:
```bash
stripe listen --forward-to localhost:3000/api/webhooks/stripe
```

This will output your webhook signing secret. Copy it to `.env.local`.

### 6. Test Cards

Use these test cards in development:

**Successful Payment:**
- Card: 4242 4242 4242 4242
- Exp: Any future date (e.g., 12/25)
- CVC: Any 3 digits (e.g., 123)

**Card Declined:**
- Card: 4000 0000 0000 0002
- Exp: Any future date
- CVC: Any 3 digits

**Requires Authentication:**
- Card: 4000 0025 0000 3155
- Exp: Any future date
- CVC: Any 3 digits

### 7. Test Payment Flow

1. Start the development server:
```bash
npm run dev
```

2. In another terminal, forward Stripe webhooks:
```bash
stripe listen --forward-to localhost:3000/api/webhooks/stripe
```

3. Visit http://localhost:3000
4. Add products to cart
5. Go to checkout
6. Use test card 4242 4242 4242 4242
7. Complete payment

Check the Stripe Dashboard to see the payment intent in real-time.

### 8. Monitoring Webhooks

In Stripe Dashboard:
1. Go to Developers > Webhooks
2. Click on your endpoint
3. Click "Events" tab to see all webhook deliveries
4. Click on an event to see details and response

### 9. Troubleshooting

**Webhook not receiving events:**
- Check if Stripe CLI is running
- Verify webhook URL is correct
- Check server logs for errors
- Verify webhook secret in `.env.local`

**Payment not going through:**
- Check Stripe Dashboard for payment intent status
- Verify API keys are correct
- Check browser console for errors
- Look at server logs

**CORS issues:**
- Ensure Stripe publishable key is in NEXT_PUBLIC_ env var
- Check that Stripe.js is loaded correctly

## Production Checklist

Before going live:
1. [ ] Switch to live API keys (remove "test" designation)
2. [ ] Update webhook endpoint to production URL
3. [ ] Setup HTTPS (required by Stripe)
4. [ ] Add fraud detection
5. [ ] Setup email notifications
6. [ ] Test all payment scenarios
7. [ ] Add error logging/monitoring
8. [ ] Review Stripe documentation for PCI compliance

## Resources

- [Stripe Documentation](https://stripe.com/docs)
- [Stripe React Integration](https://stripe.com/docs/stripe-js/react)
- [Payment Intents](https://stripe.com/docs/payments/accept-a-payment)
- [Webhooks Guide](https://stripe.com/docs/webhooks)
- [Testing Guide](https://stripe.com/docs/testing)
