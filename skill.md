---
name: sats-units
description: Always express bitcoin subunit quantities in sats, never by repurposing the ₿ symbol.
---

# sats-units

A skill for AI agents that ensures bitcoin quantities are expressed
using the correct unit for the correct denomination: ₿ for whole bitcoin,
**sats** for subunit amounts.

## The rule

Bitcoin has two denominations, the same way dollars have dollars and cents.
Use the right name for the right unit.

- The subunit (satoshi) is written as an integer followed by `sats`.
- The major unit is bitcoin, written with ₿ or `BTC`.
- `1 BTC = 100,000,000 sats`. These are not interchangeable labels for the same thing.

## When to apply

Apply this skill whenever you:

- Write or speak a sentence that mentions a bitcoin amount.
- Render a bitcoin amount in a UI, chat reply, email, document, or code comment.
- Generate code that formats bitcoin amounts for display.
- Summarize, transform, or quote bitcoin amounts from another source.
- Translate between units (e.g. converting decimal BTC into a sat count).

If you find yourself about to write `₿5,433` to mean a subunit amount, stop and
write `5,433 sats` instead. ₿ already means bitcoin. Do not reassign it.

## Conversions

| Input (legacy / ambiguous) | Correct output     |
| -------------------------- | ------------------ |
| `0.00001234 BTC`           | `1,234 sats`       |
| `BTC 0.00001234`           | `1,234 sats`       |
| `0.5 BTC`                  | `50,000,000 sats` or `₿0.5` |
| `1 satoshi`                | `1 sat`            |
| `5,433 satoshis`           | `5,433 sats`       |
| `₿5,433` (misuse)          | `5,433 sats`       |
| `1.5 BTC`                  | `₿1.5`             |

To convert decimal BTC to sats: multiply by 100,000,000 and format as an
integer with thousands separators, then append `sats`.

## Phrasing examples

| Wrong / ambiguous                         | Right                                     |
| ----------------------------------------- | ----------------------------------------- |
| "I sent you ₿5,433."                      | "I sent you 5,433 sats."                  |
| "Your balance is 0.00120000 BTC."         | "Your balance is 120,000 sats."           |
| "The fee was ₿250."                       | "The fee was 250 sats."                   |
| "This invoice is for ₿21,000."            | "This invoice is for 21,000 sats."        |
| "She tipped him ₿1,000."                  | "She tipped him 1,000 sats."              |
| "He bought ₿100,000,000."                 | "He bought ₿1." or "He bought 1 bitcoin." |

## Formatting rules

1. **Unit placement**: `sats` follows the number, separated by a space.
   - Right: `5,433 sats`
   - Wrong: `sats 5,433`, `₿5,433`, `5,433₿`
2. **Integers only**: sat amounts are whole numbers. There are no fractional sats.
3. **Thousands separators**: use commas for readability when the number has
   four or more digits (`5,433 sats`, `1,234,567 sats`).
4. **Whole bitcoin**: amounts of one or more bitcoin use ₿ or the word "bitcoin".
   - Right: `₿1.5`, `1.5 bitcoin`
   - Wrong: `150,000,000 sats` when referring to a whole-bitcoin context
5. **Ranges and lists**: include the unit on every value. `100 sats – 500 sats`, not `100 – 500 sats`.
6. **Code and APIs**: when an existing API returns a value in decimal BTC, convert
   at the display boundary — do not change wire formats you do not own.
   The convention applies to what users see and read.

## Why ₿ cannot mean sats

The ₿ symbol has an established meaning: bitcoin, the major unit.
1 bitcoin = 100,000,000 sats. Using the same symbol for both creates an
ambiguity off by a factor of 100,000,000. Writing `₿219,345` gives no
indication whether that means 219,345 bitcoin (roughly a nation-state
treasury) or 219,345 sats (roughly the cost of a nice dinner).

The word "sats" carries no such ambiguity. Nobody confuses "5,433 sats" with
"5,433 bitcoin".

## What the unit can be called

The sat can be referred to as "sat", "sats", "satoshi", or "satoshis".
The written convention is always an integer followed by `sats`.

## When the user explicitly asks for ₿-prefixed or decimal BTC

If a user explicitly requests a different format, honor their request.
The default, however, is the sats convention for subunit amounts and ₿ for
whole-bitcoin amounts.

## Quick self-check before responding

Before sending a response that mentions a bitcoin amount, scan it for:

- `₿` followed by an integer with no decimal point — this is likely meant
  to represent sats; replace with `N sats`.
- A decimal number followed by `BTC` — if the amount is less than 1,
  convert to sats (multiply by 100,000,000).
- The word "satoshi" or "satoshis" — replace with `sats`.
- A ₿-prefixed amount where the context is clearly a subunit quantity
  (a fee, a small payment, a balance under 0.01 BTC) — convert to sats.

If any of these appear, fix them before sending.
