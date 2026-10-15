# NjikoTrade AI

Build a mobile-first web app called NjikoTrade — a social trade-journaling and AI-graded trade analysis platform. Dark theme, neon-green accent (#00FF88 or similar), same visual language as a modern fintech/social app: rounded cards, bottom tab nav, credit-based economy.

Core concept: Users share trade setups (forex/crypto/stocks) to a community feed, get their trades graded by an AI review engine, follow top traders ranked by a leaderboard, and spend credits to unlock analysis and premium journals. This is an educational/analytics platform — it does not execute trades, hold funds, or manage portfolios.

1. AUTH SCREENS

Sign In: email + password, green "Sign in" button, tagline "Sign in to unlock vetted trade insights"

Sign Up: display name (optional), email, password, "Create account — 500 free credits"

Both screens show a fixed disclaimer above the footer: "NjikoTrade AI is an informational trade-journaling and analytics platform. We do not execute trades, manage funds, or guarantee returns. All analysis is for educational purposes only."

Footer: Terms of Service + Privacy Policy links

2. BOTTOM NAV (5 tabs)
   Feed · Scan · Leaderboard · Wallet · Profile

3. FEED SCREEN

Header: logo, credit balance badge (⚡ 752 CR), icon buttons

"Community Feed" title, "Shared picks vetted by [AI name]"

Search bar: "Search trader or pair..."

Top Traders horizontal scroll: avatar, handle, follower count, win-rate %, Follow button

Filter chips: Trending / Top Traders / Latest, then All / Following / Unlocked / Single / Multi

Market filter: Forex / Crypto / Stocks / Indices

Trade cards: trader avatar+handle, status badge (PENDING/WIN/LOSS/OPEN), instrument tag (FX/CRYPTO), pair name, timeframe, entry/SL/TP preview (locked rows with 🔒 for non-followers), confidence score ring (e.g. 88%), "Unlock Full Setup for X Credits" CTA, like/comment counts

4. SCAN SCREEN ("AI Trade Review Studio")

Market selector: Forex / Crypto / Stocks / Indices / Auto-Detect

Drop zone: "Drop chart screenshot or trade journal entry — PNG, JPG · up to 3"

Trade type selector: Single / Scalp / Swing / Position, each showing credit cost

"Analyze Trade with [AI name] · 100 CR" primary CTA

5. LEADERBOARD SCREEN

Title: "Trader Leaderboard — Ranked by risk-adjusted return. Verified traders show win rate + max drawdown across 10+ closed trades."

Tabs: This Week / This Month / All-Time

Ranked list: avatar, badge icon (trophy/medal for top 3), handle, win rate %, ROI, max drawdown % (new column vs. NjikoBet — critical for trading credibility), trade count

6. WALLET SCREEN

Balance card: "In-App Credits — X CREDITS", conversion note

Withdrawable Cash card, "Payouts Disabled" toggle state

Creator Monetization Eligibility: Followers progress bar, Settled Trades count, Win Rate %

Quick Buy tiles: credit packages with Naira pricing

Transaction history: creator rewards, scan costs, top-ups

7. PROFILE SCREEN

Avatar, handle, stats row: Trades / Followers / Following

Stat cards: Win Rate, ROI, Max Drawdown (in place of/alongside "Streak")

Edit Profile button

Shared Trades list: same card format as feed, with Download option

8. SETTINGS SCREEN

Account: Profile details, Email, Password

Preferences: Theme, Notifications, Language

Legal & Compliance: Terms of Service, Privacy Policy, "Trading & Analytics Disclaimer — Educational use only"

Danger Zone: Delete Account & Data

9. ADMIN PANEL (separate route, gated)

Tabs: Metrics · Users · Trades · Provider · AI · Settings

Metrics: Total Users, Credits Purchased, Trades Scanned, Shared to Feed, Top Creators leaderboard

Users: search bar, list with avatar/name/email, credit balance (+/- adjust buttons), "Custom" tag, "Make Admin" button

Trades: bulk select, filter by Won/Lost/Void/Pending, list rows showing trader, trade type (Single/Multi), pick count, timestamp, score, action icons (settle win/loss/void/pending)

Provider: market data source card — name, endpoint, plan, subscription status, requests today/limit, expiry date, "Re-check Key" + "Run Settlement Tick" buttons, cache hit rate / API calls / auto-settled / errors stats, recent ticks log

AI: "Active Master Prompt" card showing version name + last updated + LIVE badge; Prompt Versions list with View/Edit/Activate per version, "+ New Version" button

Settings:

Paystack Configuration: environment toggle (Test/Live), public key, secret key (masked, admin-only)

Ads Configuration: global toggle, publisher ID, banner/in-feed ad slot IDs, placement toggles (Show in Trade Feed / Show on Free Picks)

Monetization: enabled toggle, creator revenue share %, min cash payout (₦)

DATA MODELS TO SCAFFOLD

users (id, email, handle, avatar, credits, followers, following, win_rate, roi, max_drawdown, is_admin)

trades (id, user_id, market, pair, timeframe, entry, sl, tp, trade_type, status, score, shared, created_at)

credits_transactions (id, user_id, type, amount, description, created_at)

ai_prompts (id, version_label, content, char_count, active, created_at)

settlement_ticks (id, timestamp, window, results_count, source)

This project was built with [Lovable](https://lovable.dev).

**Live app**: https://ai-market-journal.lovable.app

## Build with Lovable

Continue developing this project in the [Lovable editor](https://lovable.dev/projects/5656a9fe-4d5b-4bb1-a744-75e065db4953).

- **Ship faster**: describe what you want to build and Lovable handles the code.
- **Stay in sync**: every change made in Lovable is committed straight to this repository.
- **Full ownership**: this code is yours. Push to `main` on GitHub and your changes sync back into Lovable, ready for your next prompt.

## Development

Prefer working locally? You need Node.js and npm — [install with nvm](https://github.com/nvm-sh/nvm#installing-and-updating).

```sh
git clone <this-repository-url>
cd <repository-name>
npm i
npm run dev
```

## Android Google sign-in setup

NjikoTrade uses Lovable's managed Google OAuth. The native app cannot send the
managed provider directly to a custom URL scheme, so the approved HTTPS bridge
returns the completed session to Android through the app deep link.

- OAuth redirect URL to allow in **Cloud → Users → Auth Settings → Redirect URLs**:
  `https://njikotrade.com/auth/native-callback`
- Android deep link consumed by the installed app:
  `com.njikotrade.app://auth-callback`
- Android application ID: `com.njikotrade.app`

The Android intent filter is already declared in
`android/app/src/main/AndroidManifest.xml`. After changing web or Capacitor
dependencies, publish the website first, then rebuild the native project:

```sh
npm install
npm run sync:android
npx cap open android
```

Build and install a fresh APK/AAB from Android Studio. Uninstall an older APK
before testing if it was built before the deep-link intent filter was added.
