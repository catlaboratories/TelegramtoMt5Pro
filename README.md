# Telegram to MT5 Pro

**by AtlasLabs.uk · Jeanette Abou Khalil**  
**Version 1.14 · For MetaTrader 5**

## Overview

Telegram to MT5 Pro is a professional signal copier that automatically copies trading signals from any Telegram channel or group directly to your MetaTrader 5 account in real time.

The system consists of two components working together:

1. **The Expert Advisor (EA)** — installed on your MT5 chart to execute trades  
2. **The companion desktop app (`TelegramMT5Pro.exe`)** — connects to Telegram and bridges signals to the EA

You can connect multiple Telegram providers to multiple MT5 accounts simultaneously. Every setting is configurable per account and per provider, giving full granular control over how signals are copied.

> Due to MQL5 restrictions, the EA cannot connect to Telegram directly. The companion desktop app must be running on the same PC as MetaTrader 5.

---

## System Requirements

- **Operating System:** Windows 10 or higher (Windows 11 recommended)
- **MetaTrader:** MetaTrader 5 (any broker)
- **Companion App:** `TelegramMT5Pro.exe` must run on the same PC
- **Telegram Account:** Required for connecting to channels/groups
- **Runtime:** No additional runtime needed — app is self-contained
- **WebRequests:** EA requires `http://127.0.0.1` whitelisted in MT5 Options → Expert Advisors

---

## Quick Setup

### Step 1 — Whitelist the EA in MetaTrader

Open MetaTrader 5 → **Tools** → **Options** → **Expert Advisors**

- Check **Allow WebRequests for listed URL**
- Add: `http://127.0.0.1`
- Click **OK** and restart MetaTrader

### Step 2 — Launch the Companion App

Run `TelegramMT5Pro.exe`

On first launch:

- Go to **Settings**
- Enter your Telegram API credentials
- Add your **API ID** and **API Hash** from `my.telegram.org/apps`
- Enter your phone number with country code
- Click **Save**
- Click **Connect**
- Enter the verification code sent to your Telegram app

### Step 3 — Add an MT5 Account

In the app:

- Click **+ Account**
- Name your account
- Open **Account Settings** → **Port**
- Set:
  - **Account Number** = your MT5 login number
  - **Bridge Port** = default `5000`  
    Use `5001`, `5002`, etc. for additional accounts

### Step 4 — Add Signal Providers

- Go to the **Channels** page
- Click **+ Add Provider** under your account
- Search and select the Telegram channel or group you want to copy

Each provider has its own settings for risk, SL/TP, filters, and copy rules.

### Step 5 — Attach the EA to a Chart

Drag **TelegramMT5Pro** onto any MT5 chart.

In EA Inputs, set:

- **Port** — must match the Bridge Port from the app
- **Magic Number** — unique number for EA orders  
  Default: `20250101`

If connected successfully, the EA panel shows:

**STATUS: RUNNING**

---

## Main Features

### Signal Detection and Parsing

- Auto-detects BUY and SELL direction from message text
- Parses entry price, range entries, SL, and up to 7 TP levels
- Supports custom buy/sell keywords per provider
- Handles “More buy” and “More sell” additional entries
- Symbol auto-detection from message text
- Custom symbol mappings such as:
  - `GOLD → XAUUSD`
  - `BITCOIN → BTCUSD`
- Detects:
  - New Trade
  - Modify Order
  - Close All
  - Partial Close
  - Break-Even
  - TP Hit
- Message edit converts to Modify Order
- Message delete closes matching positions

### Order Execution Modes

- Market order
- Pending order
- Range orders
- Range + Multi-TP
- Multi-TP mode
- Per-entry mode
- Single order
- Max positions cap
- Pending order expiry

### Stop Loss and Take Profit

- SL from signal
- Fixed SL override
- Manual SL fallback
- SL sanity check
- Up to 7 TP levels parsed
- Fixed TP override
- Open TP with trailing

### Trailing Stop and Break-Even

- Trailing stop
- Break-even
- Auto break-even on TP1 hit
- All actions run on 2-second timer

### Partial Close

- Flat partial close
- Per-TP partial close
- Close remaining pending orders on TP hit
- TP hit counter per symbol
- Instant detection via `OnTradeTransaction`

### Lot Size and Risk Management

- Fixed lot
- Fixed money
- % of Balance
- % of Equity
- Growth lot sizing
- Tick factor correction
- Currency factor correction
- MaxAffordableLot protection
- Minimum lot fallback

### Copy Filters

Per provider:

- Copy New Trades
- Copy Long (BUY)
- Copy Short (SELL)
- Copy Modify Orders
- Copy Close Orders
- Copy Partial Closes
- Copy Break-Even signals
- Ignore Incomplete signals

### Equity Guard

- Daily loss limit
- Weekly loss limit
- Monthly loss limit
- Daily profit target
- Weekly profit target
- Automatic resets by period

### Trade Limits

- Max open trades
- Max trades per minute
- Max trades per hour
- Max trades per day
- Max trades per week
- Max trades per month

### Time Filters

- Allowed trading days
- Blackout time ranges
- Auto-close all positions at a configured time
- All times based on PC local time

### News Filter

- Forex Factory upcoming news filter
- High, medium, low impact handling
- Minutes before and after news
- Currency-specific filtering
- 1-hour fetch cooldown
- Automatically disabled in Strategy Tester

### Notifications

- Telegram bot notifications
- Mobile app notifications
- Email notifications
- Configurable event triggers
- Test notification button

### EA Dashboard Panel

Displays:

- STATUS
- PORT
- POSITIONS
- P&L
- MAGIC
- Branding
- Embedded logo
- Automatic chart styling
- Restores chart settings when removed

### Live Account Dashboard in App

Displays:

- Balance
- Equity
- Total profit
- Drawdown %
- Profit today / week / month
- Open trades count
- Broker and leverage info
- Balance history chart
- Settings and delete buttons

### Multi-Account Support

- Unlimited MT5 accounts on separate ports
- Separate settings per account
- Separate settings per provider
- Local JSON storage
- Instant config reload without restart

---

## Range Orders

### Range Only
Places multiple orders across the entry range using configured step.

### Range + Multi-TP
Recommended mode.

Places exactly one order per TP, automatically spaced across the range.

### Multi-TP Only
Places one order per TP at current market price.

---

## Troubleshooting

### BRIDGE OFFLINE
Make sure `TelegramMT5Pro.exe` is running. Check that the port matches in both the EA and the app.

### NOT WHITELISTED
Add `http://127.0.0.1` in MT5 → Tools → Options → Expert Advisors → Allow WebRequests.

### Signal received but no trade
Check the MT5 **Experts** tab. Common causes:
- symbol not found
- SL too tight
- max positions reached
- daily limit hit
- news filter active

### Invalid stops error
SL or TP is too close to entry for the broker. Increase fixed SL pips or verify the signal SL value.

### Settings not applying
EA reloads config every 20 seconds. Wait briefly or remove and re-attach the EA.

### Search not finding channels
Make sure Telegram is connected in the app. Try scrolling so all chats load.

### Balance chart not showing
Wait 10 to 20 seconds for the EA to post the first data.

### Double account tabs
Update to latest version.

---

## Download

Download the latest desktop bridge from the **Releases** section of this repository.

---

## Copyright

**Telegram to MT5 Pro · AtlasLabs.uk**  
© 2026 Jeanette Abou Khalil · All rights reserved
