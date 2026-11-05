# Test promo codes at checkout

A [Grok Bot](https://grok.x.ai/) skill that tries promo codes the way a shopper would: in the store's own discount box, at human pace, with no gray-area cart tricks.

## What it does

- Tries codes on Shopify, Target, Best Buy, Walmart, Nike, Macy's, and most other stores with a normal promo field
- Pulls candidate codes from you, that store's emails in Gmail, and the store's own pages
- Reports which codes actually lower the total
- Tracks confirmed savings after a real order (ask "how much have I saved?")
- Checks for a saved gift card balance for that store
- **Never places the order**

## Install

Paste this to your Grok Bot:

```
Install Test promo codes at checkout from https://github.com/dumbthumbs/test-promo-codes-at-checkout
Follow INSTALL.md.
```

## Use

Once installed, say something like:

- "Try promo codes on this Target cart"
- "Any codes for this Nike order?"
- "How much have I saved with promo codes?"

## Rules baked in

- Only the store's normal promo field (no hidden APIs)
- Human pace (~5 seconds between codes)
- Stop on captcha or rate limits
- No payment, no order submit

## License

MIT
