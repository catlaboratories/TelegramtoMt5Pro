Telegram to MT5 Pro
by AtlasLabs.uk  ·  Jeanette Abou Khalil
Version 1.14  ·  For MetaTrader 5

Overview
Telegram to MT5 Pro is a professional signal copier that automatically copies trading signals from any Telegram channel or group directly to your MetaTrader 5 account — in real time.
The system consists of two components working together:
The Expert Advisor (this EA) — installed on your MT5 chart to execute trades
The companion desktop app (TelegramMT5Pro.exe) — connects to Telegram and bridges signals to the EA

You can connect multiple Telegram providers to multiple MT5 accounts simultaneously. Every setting is configurable per account and per provider, giving you full granular control over how signals are copied.
ℹ Due to MQL5 restrictions, the EA cannot connect to Telegram directly. The companion desktop app must be running on the same PC as MetaTrader 5.

System Requirements
Operating System	Windows 10 or higher (Windows 11 recommended)
MetaTrader	MetaTrader 5 (any broker)
Companion App	TelegramMT5Pro.exe — must run on the same PC
Telegram Account	Required for connecting to channels/groups
.NET / Runtime	No additional runtime needed — app is self-contained
WebRequests	EA requires http://127.0.0.1 whitelisted in MT5 Options → Expert Advisors

Quick Setup (5 Steps)
Step 1 — Whitelist the EA in MetaTrader
Open MetaTrader 5 → Tools → Options → Expert Advisors tab.
Check "Allow WebRequests for listed URL"
Add: http://127.0.0.1
Click OK and restart MetaTrader

Step 2 — Launch the Companion App
Run TelegramMT5Pro.exe. On first launch, go to Settings and enter your Telegram API credentials:
API ID and API Hash — obtain from https://my.telegram.org/apps
Phone number (with country code, e.g. +44...)
Click Save, then Connect — enter the verification code sent to your Telegram app

Step 3 — Add an MT5 Account
In the app, click "+ Account" and name your account. Then open Account Settings → Port tab and set:
Account Number — your MT5 login number
Bridge Port — default 5000 (use 5001, 5002 etc. for additional accounts)

Step 4 — Add Signal Providers
Go to the Channels page in the app. Click "+ Add Provider" under your account. A list of your Telegram channels and groups will appear. Search and select the one you want to copy signals from.
Each provider has its own settings for risk, SL/TP, filters, and copy rules.

Step 5 — Attach the EA to a Chart
Drag TelegramMT5Pro onto any chart. In the EA Inputs, set:
Port — must match the Bridge Port set in the app for this account
Magic Number — unique number to identify EA orders (default: 20250101)
The EA panel will appear on the chart showing STATUS: RUNNING when connected successfully.

Full Feature List
Signal Detection & Parsing
Auto-detects BUY/SELL direction from message text
Parses entry price, range entries (More buy/More sell), SL, and up to 7 TP levels
Supports custom buy/sell keywords per provider (override built-in detection)
Handles "More buy" and "More sell" as additional entry prices for range orders
Symbol auto-detection from message text with broker prefix/suffix applied
Custom symbol mappings (e.g. GOLD → XAUUSD, BITCOIN → BTCUSD)
Signal type detection: New Trade, Modify Order, Close All, Partial Close, Break-Even, TP Hit
Message edit → auto convert to Modify Order signal
Message delete → auto close matching positions

Order Execution Modes
Market order — executes immediately at current price
Pending order — BUY LIMIT / SELL LIMIT / BUY STOP / SELL STOP based on entry vs current price
Range orders — places multiple orders across entry range at configurable step
Range + Multi-TP — places exactly N orders (one per TP) auto-spaced across range, each with its own TP
Multi-TP mode — one order per TP level at current price
Per-entry mode — one order per explicit entry price with sequential TPs
Single order — one order with TP1
Max positions cap — stops placing new orders when limit is reached
Pending order expiry — auto-cancel pending orders after X minutes

Stop Loss & Take Profit
SL from signal — uses the SL price in the signal
Fixed SL — override with a fixed pip value regardless of signal
Manual SL fallback — apply manual pips when signal has no SL
SL sanity check — detects mistyped SL values (e.g. 6900 instead of 69000) and falls back safely
Up to 7 TP levels parsed from signal
Fixed TP override — use configured pip value instead of signal TP
Open TP with trailing — no fixed TP, trail instead

Trailing Stop & Break-Even
Trailing stop — activates after N pips profit, trails by N pips distance
Break-even — moves SL to entry + offset after N pips profit
Auto break-even on TP1 hit — moves remaining positions to BE automatically
All running on 2-second timer — no missed ticks

Partial Close
Flat partial close — close X% of all positions when TP1 hits
Per-TP partial close — close different % at each TP level (TP1/TP2/TP3/TP4+)
Close remaining pending orders when any TP hits
TP hit counter per symbol — correctly tracks which TP level was hit
Uses OnTradeTransaction for instant detection — no polling delay

Lot Size & Risk Management
Fixed lot — use a specific lot size
Fixed money — risk a fixed dollar amount per trade
% of Balance — risk a percentage of account balance
% of Equity — risk a percentage of account equity
Growth lot sizing — increase lot size as balance grows
Symbol tick factor correction for indices and crypto
Currency factor correction for non-USD account base currencies
MaxAffordableLot — never exceeds 90% of free margin
Minimum lot fallback — uses SYMBOL_VOLUME_MIN if calculated lot is 0

Copy Filters (per provider)
Copy New Trades — enable/disable copying new signals
Copy Long (BUY) — enable/disable copying buy signals
Copy Short (SELL) — enable/disable copying sell signals
Copy Modify Orders — enable/disable copying edits to existing trades
Copy Close Orders — enable/disable copying close signals
Copy Partial Closes — enable/disable partial close signals
Copy Break-Even signals — enable/disable BE signals
Ignore Incomplete signals — skip signals with no SL and no TP

Daily / Weekly / Monthly Limits (Equity Guard)
Daily loss limit — stop all trading when daily loss exceeds threshold
Weekly loss limit — stop all trading for the week
Monthly loss limit — stop all trading for the month
Daily profit target — stop trading once daily profit target hit
Weekly profit target — stop trading once weekly profit target hit
Balance and equity tracked separately — resets automatically each period

Trade Limits
Max open trades at any time
Max trades per minute
Max trades per hour
Max trades per day
Max trades per week
Max trades per month
Counters reset automatically at the correct time boundary

Time Filters
Allowed trading days — enable/disable individual days of the week (all 7 configurable)
Blackout time ranges — block trading during specific hours (e.g. Mon 22:00 → Tue 06:00)
Auto-close all positions at a configured day and time (e.g. Friday 22:00)
All times based on your PC local time

News Filter (Forex Factory)
Fetches upcoming high/medium/low impact news from Forex Factory
Configurable minutes before and after each impact level
Blocks new trades during news windows
Currency-specific filtering (e.g. only filter USD news)
1-hour fetch cooldown to avoid rate limiting
Automatically disabled in Strategy Tester

Notifications
Telegram bot notifications — send alerts to your own Telegram chat
Mobile App notifications — via MT5 mobile app using your MQL5 ID
Email notifications — via MetaTrader email configuration
Configurable events: Trade Open, Trade Close, Order Cancel, Order Modify
Test Notification button — verify Telegram bot is working instantly

EA Dashboard Panel
A professional dark-themed panel renders on the chart showing:
STATUS — RUNNING (green) / BRIDGE OFFLINE (red) / DAILY LIMIT (orange) etc.
PORT — active bridge port number
POSITIONS — count of all open positions (EA + manual), with EA count in brackets
P&L — live floating profit/loss of all open positions (green = profit, red = loss)
MAGIC — the EA magic number
Branding: AtlasLabs.uk, Jeanette Abou Khalil
Embedded logo — no external files needed
Chart style applied automatically: dark background, no grid, gold/green/red candles
All chart settings restored when EA is removed

Live Account Dashboard (in App)
Balance and Equity displayed live from EA
Total Profit (closed + open)
Drawdown % live
Profit Today / This Week / This Month
Open Trades count
Account login number, type (Demo/Real), leverage, broker
Account Balance History chart — updates every 3 seconds with area chart
Settings and Delete buttons always visible (never hidden)

Multi-Account Support
Add unlimited MT5 accounts, each on a separate port
Each account has its own settings, providers, and equity limits
Each provider has its own risk, SL/TP, and copy filter settings
All settings saved locally in JSON — no cloud dependency
Configuration reloads instantly in the EA without restart (config version system)

Settings Reference
Account Settings → Symbol Tab
Prefix	Text added before symbol name (e.g. "m" → XAUUSDm). Applied by app, not EA.
Suffix	Text added after symbol name (e.g. ".a" → XAUUSD.a).
Symbol Mappings	Map provider symbol names to broker names. E.g. GOLD → XAUUSD, BITCOIN → BTCUSD. Case-insensitive.

Account Settings → Port Tab
Account Number	Your MT5 login number. Auto-populated once EA connects.
Server	Your broker server name (optional, for reference only).
Bridge Port	Port the app and EA communicate on. Default 5000. Use 5001, 5002 for more accounts.

Account Settings → News Tab
High Impact	Block trading N mins before and after high-impact news events.
Medium Impact	Same for medium-impact events.
Low Impact	Same for low-impact events (usually left at 0).
ℹ News filter uses Forex Factory calendar. Make sure your PC clock is accurate.

Account Settings → EA Config Tab
This tab mirrors all EA trading settings. Changes save and reload in the EA within 20 seconds automatically. Key sections:
Risk Mode	Fixed Lot / Fixed Money / % Balance / % Equity
Fixed Lot	Lot size when using Fixed Lot mode
Risk %	Risk per trade when using % Balance or % Equity mode
SL Mode	"signal" uses SL from message, "fixed" uses configured pip value
Fixed SL Pips	SL in pips when SL mode is Fixed or as fallback
TP Mode	"signal" uses TP from message, "fixed" uses configured pip value
Max Positions	Maximum concurrent open positions managed by EA
Slippage	Maximum allowed slippage in points for market orders
Use Trailing	Enable trailing stop on open positions
Trail Activate	Pips in profit before trailing activates
Trail Distance	How many pips behind price the SL trails
Use Break-Even	Enable automatic break-even on open positions
BE Activate	Pips in profit to move SL to break-even
BE Offset	Extra pips above entry to set SL at break-even
Partial Close	Enable partial close when TP1 is hit
Partial %	Percentage of position to close at TP1
Per-TP Partial	Use different % for each TP level instead of flat %
TP1–TP4 %	Partial close % at each TP level when per-TP mode is on
Close on TP Hit	Cancel remaining pending orders when any TP hits
Range Orders	Place multiple orders across entry range
Range Step	Price step between range orders (in price units)
Range Cap	Cap orders at Max Positions setting
Range Auto-Step	Auto-calculate step from range ÷ max positions
Range Same TP	Give all range orders TP1 instead of sequential TPs
Multi-TP	One order per TP level (Range+MultiTP: auto-spaces N orders for N TPs)
Pending Expiry	Cancel pending orders after X minutes (0 = never)
Tick Factors	Correction for indices/crypto (e.g. XAUUSD=100,US30=10)
Currency Factors	Correction for non-USD pairs (e.g. HK50=0.18)
Max Daily Loss	Daily loss limit in $ — EA stops trading (0 = off)
Max Daily Profit	Daily profit target in $ — EA stops trading (0 = off)
Enable Logging	Write detailed trade log to MT5 Experts tab
Log Level	Info / Debug / Error — controls verbosity

Account Settings → Equity Tab
Daily Loss Limit	Stop trading if account loses more than this amount in one day
Weekly Loss Limit	Stop trading if account loses more than this amount in one week
Monthly Loss Limit	Stop trading if account loses more than this amount in one month
Daily Profit Target	Stop trading once daily profit reaches this amount
Weekly Profit Target	Stop trading once weekly profit reaches this amount

Account Settings → Limits Tab
Max Open Trades	Maximum total open trades at any time (0 = unlimited)
Max Per Minute	Maximum new trades in any 60-second window
Max Per Hour	Maximum new trades in any 60-minute window
Max Per Day	Maximum new trades per calendar day
Max Per Week	Maximum new trades per calendar week
Max Per Month	Maximum new trades per calendar month

Account Settings → Time Tab
Allowed Days	Check/uncheck days when trading is allowed (all 7 days configurable)
Blackout Ranges	Add time ranges to block trading (e.g. Fri 22:00 → Mon 00:00 for weekend)
Auto Close	Automatically close all positions at a specific day and time

Account Settings → Notify Tab
Email Notifications	Sends email via MetaTrader. Configure in MT5 → Tools → Options → Email.
Mobile App Notifications	Push notifications to MT5 mobile app. Configure MQL5 ID in MT5 → Tools → Options → Notifications.
Telegram Notifications	Send alerts to your Telegram chat via a bot you create.
Bot Token	Token from @BotFather — create a bot and copy the token
Chat ID	Your Telegram chat ID (use @userinfobot to find yours)
Test Button	Send a test message immediately to verify Telegram bot is working
Events	Choose which events trigger notifications: Trade Open, Close, Cancel, Modify

Provider Settings → Risk Tab
Risk Mode	Fixed Lot / Fixed Money / % Balance / % Equity — per provider override
Fixed Lot	Specific lot size for this provider
Slippage	Max slippage for this provider's trades in points
Partial Close %	Default partial close percentage for this provider

Provider Settings → Copy Tab
Copy New Trades	On/Off — copy new trade signals from this provider
Copy Long (BUY)	On/Off — copy buy signals
Copy Short (SELL)	On/Off — copy sell signals
Copy Modify Orders	On/Off — copy when provider edits a message (modify SL/TP)
Copy Close Orders	On/Off — copy close all signals
Copy Partial Closes	On/Off — copy partial close signals
Copy Break-Even	On/Off — copy break-even signals
Ignore Incomplete	Skip signals that have neither SL nor TP

Provider Settings → Advanced (EA Config) Tab
When set at provider level, these override the account-level EA Config for signals from this provider specifically. Includes all range order settings, multi-TP settings, pending expiry, and custom keyword detection.

Range Orders — How They Work
Range orders are the most powerful feature for prop firm challenge management and scaling into positions. Here is exactly how each mode works:
Range Only (Multi-TP OFF)
Signal: BUY 70000, More buy 69900, TP 70500, SL 69000
With step = 50: places orders at 69900, 69950, 70000 — all with TP1 = 70500
With "Assign all TPs to each range order" ON: each order gets TP1 only
With "Distribute TPs sequentially" ON: order1=TP1, order2=TP2, order3=TP3

Range + Multi-TP (the recommended mode)
Signal: BUY 70000, More buy 69900, TP 70500, TP 70600, TP 70700, SL 69000
Ignores step setting. Places EXACTLY one order per TP, auto-spaced across range:
Order 1 @ 69900 → TP1 = 70500
Order 2 @ 69950 → TP2 = 70600
Order 3 @ 70000 → TP3 = 70700
To use: check both "Split range entry into ladder orders" AND "One order per TP level"

Multi-TP Only (Range OFF)
Places one order per TP at the current market price. Each has its own TP level and shared SL.

EA Panel Reference
STATUS	RUNNING (green) = connected and working
BRIDGE OFFLINE (red) = app not running or wrong port
CONNECTING (yellow) = waiting for first response
DAILY LIMIT (orange) = equity/trade limit reached
NOT WHITELISTED (red) = add http://127.0.0.1 to MT5 WebRequests
PORT	The bridge port this EA is listening on
POSITIONS	Total open positions. Format: "5 (3 EA)" means 5 total, 3 placed by this EA.
P&L	Live floating profit/loss of ALL open positions. Green = profit, Red = loss.
MAGIC	The magic number configured for this EA instance

Troubleshooting
BRIDGE OFFLINE	Make sure TelegramMT5Pro.exe is running. Check port matches in both EA and app.
NOT WHITELISTED	Add http://127.0.0.1 to MT5 → Tools → Options → Expert Advisors → Allow WebRequests.
Signal received but no trade	Check EA log in MT5 Experts tab. Common causes: symbol not found, SL too tight, max positions reached, daily limit hit, news filter active.
Invalid stops error	SL or TP is too close to entry for this broker. Increase fixed SL pips or check signal SL value.
Settings not applying	EA reloads config every 20 seconds. Wait or remove/re-add EA.
Search not finding channels	Telegram must be connected (green status in app). Try scrolling — all chats load on connect.
Balance chart not showing	Chart fills after EA POSTs stats. Start the EA and wait 10–20 seconds for first data.
Double account tabs	Update to latest version — fixed in v1.14.

Telegram to MT5 Pro  ·  AtlasLabs.uk
© 2026 Jeanette Abou Khalil  ·  All rights reserved
