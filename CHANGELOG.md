# Changelog

All notable changes to this project are documented here.  
Started: **February 2025** | Current: **v12 (April 2026)**

---

## [v12] — April 2026 — Stability & Performance

**Theme: Production hardening — zero crashes, faster responses**

- `safeJSONParse()` — Claude JSON parsing never crashes silently on malformed output
- `fetchWithRetry()` — 3 attempts with 600ms backoff on all Claude API calls
- `quickIntentCheck()` — rule-based regex pre-filter before Claude API; resolves ~60% of messages with zero latency and zero API cost
- `LockService` — prevents race conditions when multiple family members send messages simultaneously
- Fixed: `getRecentExpenses()` now correctly reads top-down (newest entry always at `DATA_START_ROW`)
- Fixed: `last_logged_<chatId>` now stored per chat, enabling instant correction after multi-item expense logging
- Fixed: Duplicate `isRefundMessage()` removed — stricter version kept
- Fixed: `NO` / `N` now correctly cancels delete/edit flow
- Fixed: Refunds now count as member activity for inactivity reminder logic

---

## [v11] — March 2026 — Smart Corrections

**Theme: Fixing logged entries without re-entering**

- Interactive correction flow — "category is wrong" / "user galat hai" triggers guided fix
- `last_logged` pattern — every `logToSheet()` stores last entry per chat with 10-minute expiry
- Claude extracts correction field + value from natural language ("category should be Transport")
- `/edit [n]` — pick from last N entries, then edit any field interactively
- `/delete [n]` — pick from last N entries to delete
- Category uncertainty flag — logs immediately if Claude is unsure, notifies user to correct if needed
- State cleanup on NO/cancel — all stale edit/correction states cleared atomically

---

## [v10] — March 2026 — Queries & Analytics

**Theme: Ask the bot anything about your spending**

- `handleQuery()` — Claude Haiku answers free-form spending questions in natural language
- `/trend` — category-wise spending trend across last 3 months
- `/find [keyword]` — search across all logged expenses
- `/who [member]` — member-wise spending breakdown
- `/summary [month]` — any specific month e.g. `/summary feb`
- Month shortcuts — `/jan`, `/feb` ... `/dec` as direct commands
- `detectMasterIntent()` — routes expense / query / correction / config to correct handler
- Hindi + Hinglish query support ("feb ka kitna kharch hua")

---

## [v9] — February 2026 — Budget & Category Alerts

**Theme: Proactive alerts so you never overspend silently**

- Budget alerts at 80% / 90% / 99% — fires once per level per month
- Category-specific spending alerts (e.g. Shopping > 6% of monthly budget)
- Alert deduplication — same alert never fires twice in a month
- Excluded categories from budget calculation: `Indore`, `SIP`, `HOME LOAN EMI`, `Sierra Payment` — investments and fixed costs don't inflate alert triggers
- `/budget` — live budget status with days left and daily allowance
- `/top5` — highest 5 expenses of the month

---

## [v8] — February 2026 — Reminders

**Theme: Never forget to log an expense**

- Morning reminder — configurable daily nudge (e.g. 7 AM IST)
- Night reminder — end-of-day logging reminder
- Inactivity reminder — fires if no expense logged in N days (once per day, deduplicated)
- `setupTriggersV2()` — GAS time-based triggers for all scheduled jobs
- `/setreminder`, `/showreminders`, `/deletereminder` commands
- Reminder config via chat — "reminder morning 7 baje set karo"

---

## [v7] — February 2026 — Monthly PDF Report

**Theme: Shareable monthly summary**

- `/monthlyreport` — generates a formatted PDF of the month's expenses
- PDF created via Google Docs, stored in Drive, link sent to Telegram
- Category-wise and member-wise breakdown in report
- `authorizeDrive()` — one-time Drive permission setup

---

## [v6] — January 2026 — Refunds & Negative Amounts

**Theme: Track money coming back**

- Refund detection — "refund", "cashback", "reversal" keywords trigger negative amount logging
- Negative amounts auto-reduce all totals, summaries, and budget calculations
- `isRefundMessage()` + `parseRefund()` — dedicated refund parsing pipeline
- Multi-item refunds supported in single message

---

## [v5] — January 2026 — Image & PDF Parsing

**Theme: Log expenses from bills and bank statements**

- Photo upload → Claude Vision reads receipt/bill and extracts expenses
- PDF upload → text extracted, parsed for expense entries
- Bank SMS parsing — paste a debit SMS, bot extracts amount, merchant, date automatically
- `handleMedia()` — unified handler for photo and document uploads
- `isBankSMS()` + `parseBankSMS()` — dedicated SMS format detection

---

## [v4] — January 2026 — Config Management

**Theme: Customize the bot without touching code**

- Add custom categories via chat — "add category Vacation"
- Add family members via chat — "add member Rahul"
- Add payment types via chat
- Set monthly budget via chat — "budget 300000 set karo"
- Set category spending limits via chat
- `loadPersistedConfig()` — Script Properties merged into CONFIG on every request
- `/listconfig` — shows current active config
- `/addcategory`, `/addmember`, `/addtransaction` slash commands

---

## [v3] — December 2025 — Edit Existing Entries

**Theme: Fix mistakes without re-logging**

- Interactive edit flow — search by keyword, pick entry, edit any field
- Fields editable: amount, description, category, date, user
- `applyEditDirectly()` — writes correction to exact sheet row
- Duplicate detection — scans top 50 rows before logging new expense
- YES/NO confirmation flow for duplicate entries
- State machine in Script Properties — edit/delete flow persists across messages

---

## [v2] — November 2025 — Claude AI Integration & Rich Confirmations

**Theme: Stop typing structured data — just talk**

- Claude Haiku integrated for expense parsing
- Natural language input — Hindi, Hinglish, English all supported
- Multi-item expenses — log multiple expenses in one message
- Telegram confirmation message after every log — amount, category, member, payment type, date
- `parseExpenseWithClaude()` — primary parser; `parseExpenseRegex()` as fallback
- Smart category matcher — number, exact, partial, and word-level matching
- `/summary` — monthly breakdown by category and member
- `/today`, `/week` — quick time filters
- `/members` — list family members

---

## [v1] — February 2025 — Foundation

**Theme: Get something working**

- Basic Telegram bot setup via Google Apps Script webhook
- Manual expense logging — structured text input only
- Google Sheets as database — one row per expense
- Columns: Date, Amount, Description, Payment Type, Category, Member
- `logToSheet()` — inserts at top row, newest entries always first
- `/help` command
- Deployed as GAS Web App, webhook set manually

---

## Stats

| Metric | Value |
|---|---|
| Development period | Feb 2025 → Apr 2026 (14 months) |
| Total versions | 12 |
| Lines of code (v12) | ~2,600 |
| Claude API calls saved by quickIntentCheck | ~60% |
| Languages understood | English, Hindi, Hinglish |
| Hosting cost | ₹0 |
