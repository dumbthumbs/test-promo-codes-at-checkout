---
name: Test promo codes at checkout
description: >-
  Use this when the user wants to know which of a few promo or discount codes
  lowers the price at an online store (Shopify, Target, Best Buy, Walmart, Nike,
  Macy's, and most others), without placing the order, or asks how much promo
  codes have saved them so far, or whether a saved gift card applies.
---
# Test promo codes at checkout

Try promo codes the way a shopper would, in the store's own promo box in the box browser. Report which codes lower the total and whether a saved gift card covers part of it. **Never place the order.**

## Ground rules (no gray areas)
- Use only the store's normal pages: the cart, the promo field, and the Apply button. Don't call hidden cart APIs, pull tokens out of page code, edit cookies, or script requests.
- Only try codes that come from the user, from the store's own site or emails, or from the user's Gmail (promo emails from that store). Don't spray codes from coupon-dump sites. You may suggest a short list from a reputable coupon page if the user asks, but label any such code "unverified".
- Go at human speed. Wait about 5 seconds between codes, and try no more than about 10 per store unless the user asks for more.
- If the store shows a captcha, a rate-limit message, or "too many attempts", stop and report how far you got. Don't retry and don't work around it.
- Don't create accounts, use someone else's account, or stack codes in ways the site doesn't offer. If a code is limited to first orders, new accounts, students, or the like, note that and skip it unless it applies to the user.
- Leave the user's existing cart alone. If items are already in it, list them and ask before removing anything. Add only what's needed for the test.
- Stop short of payment. Don't enter card details or submit anything.

## Inputs
- Store and product(s): a URL or name, plus variant (size or color) and quantity
- Codes to try: optional. If none are given, gather them from the sources allowed above
- Ship-to ZIP code if the total depends on it (take it from the vault; never guess)

## Steps
1. **Gather codes.** Remove duplicates and blanks. Note where each code came from.
2. **Check saved gift cards.** Look in `/workspace/promo-savings/gift-cards.json` for cards from this store, or from a gift card brand the store accepts. That file holds only the store, the last 4 digits, the last known balance, the date the balance was checked, and the name of the secret that holds the number and PIN. The number and PIN themselves are never in the file, the chat, or memory. If the user mentions a gift card that isn't saved, offer to save it as a secret, one secret request per card.
3. **Build the cart.** Add the item in the box browser and open the cart or checkout page that has the promo field. Some stores (Target, Walmart, Nike) show the field in the cart; others only show it at the first checkout step. Guest checkout is fine up to the shipping step. Record the baseline subtotal, shipping, tax if shown, and total.
4. **Try each code.** Type it, click Apply, and wait for the total to update. Record the store's own message (accepted, invalid, expired, minimum not met, item excluded) and the new total. Remove the code before trying the next one, unless the site allows only one at a time and replaces it anyway.
5. **Decide what counts as a hit.** A code is a hit only if the order total drops below baseline, or shipping drops to free. "Code accepted" with no price change is a miss.
6. **Apply a gift card.** Do this only if one matches and the user agrees. Use the store's own gift card field or balance checker, typing the number from its secret. Read the balance the store shows and update the file. Remove the card before leaving the page unless the user is checking out. Never redeem or spend a gift card without the user's yes on that specific order.
7. **Clean up.** Remove any applied code or gift card and any items you added. Leave the cart as you found it, unless the user wants the best code left applied.
8. **Report back.** Give a short table of code, result, saving, and any condition the store stated. Recommend the best one. Then add a line for any matching gift card, with its balance and what you'd still pay after using it. Say when a store needed sign-in or blocked testing. Ask before doing anything else.

## Savings tracker
Keep a running record of real savings in `/workspace/promo-savings/ledger.json` (create it if it is missing). It is a JSON array of entries with these fields: date (YYYY-MM-DD), store, items, code, baseline_total, final_total, saved, currency, and source (where the code came from).
- **Record a saving only once the order is actually placed with the code.** Confirm it from the order confirmation page or the store's receipt email. A test hit on its own isn't a saving. If the user places the order themselves, check their Gmail for the receipt and confirm the discount shown there before recording it.
- Saved = baseline total minus final total, using the store's own numbers. Count only the promo-code discount, not sale prices the item already had.
- Don't count gift card spending as savings. Instead, when a gift card was used, take the amount used off its balance in `gift-cards.json`.
- Skip duplicates: one entry per order number.
- **After recording,** tell the user what this order saved, then their running total. For example: "That code saved you $12.40. You've saved $86.15 across 7 orders with promo codes." Give a separate total for each currency if there is more than one.
- When asked "how much have I saved?", read the ledger and report the total, the number of orders, and the top stores.

## Store notes
- **Amazon:** there's no general promo field; codes apply at the final review step, which needs sign-in. Instead, report clippable coupons and Subscribe & Save on the product page, and test a code only if the user is signed in and asks. Amazon gift cards redeem to the account balance, so check that balance instead of the file.
- **Shopify stores and Shop app purchases:** the promo field is on the store's checkout page ("Discount code or gift card").
- **Stores that require sign-in to see the promo box:** stop and tell the user, and don't sign in unless they ask.

## Done when
Every code has a recorded result, any matching gift card has been flagged with its balance, the cart is back to how it started, and the user has the table. Nothing was purchased. Any confirmed saving is in the ledger and has been reported with the running total.
