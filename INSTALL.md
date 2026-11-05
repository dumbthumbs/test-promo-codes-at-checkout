# INSTALL.md — Test promo codes at checkout

This file is a **prompt for Grok Bot**. If you are Grok Bot, follow it now. Do not dump this file back as documentation.

## Command the user can paste

```
Install Test promo codes at checkout from https://github.com/dumbthumbs/test-promo-codes-at-checkout
Follow INSTALL.md.
```

## What you are installing

One Grok Bot skill:

- `SKILL.md` → skill id `test-promo-codes-at-checkout`, name **Test promo codes at checkout**

It tries promo / discount codes in a store's own promo box (human pace, no hidden cart APIs), reports which codes lower the total, tracks confirmed savings after a real order, and checks for saved gift cards. **It never places the order.**

## Steps

### 1. Fetch

Read the raw skill file (jsDelivr or GitHub raw is fine):

- https://cdn.jsdelivr.net/gh/dumbthumbs/test-promo-codes-at-checkout@main/SKILL.md

If a skill with id `test-promo-codes-at-checkout` already exists, overwrite it with the fetched body.

### 2. Save

Save it as a user skill with:

- **id:** `test-promo-codes-at-checkout`
- **name** and **description** from the YAML frontmatter
- **body:** the markdown after the frontmatter

### 3. Done

Tell the user the skill is installed. They can use it by saying things like "try promo codes on this cart" or "how much have I saved with promo codes?" Confirm-before-pay still applies; this skill never places an order.

## Guardrails (do not change these when installing)

- Only the store's normal promo field. No hidden cart APIs, cookie edits, or scripted requests.
- Only codes from the user, the store, or that store's emails in the user's Gmail. Label coupon-site codes "unverified" if the user asks for them.
- About 5 seconds between codes; about 10 codes max unless the user asks for more.
- Stop on captcha or rate-limit. Don't work around it.
- Never enter card details or submit payment.
