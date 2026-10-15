<!-- LOVABLE:BEGIN -->

> [!IMPORTANT]
> This project is connected to [Lovable](https://lovable.dev). Avoid rewriting
> published git history — force pushing, or rebasing/amending/squashing commits
> that are already pushed — as it rewrites history on Lovable's side and the
> user will likely lose their project history.
>
> Commits you push to the connected branch sync back to Lovable and show up in
> the editor, so keep the branch in a working state.

<!-- LOVABLE:END -->

## Ad network files (do not delete)

`public/sw.js` is the Monetag site-verification / push service worker
(zone 11822747, domain 3nbf4.com). Monetag requires it to stay permanently at
the site root (`njikotrade.com/sw.js`) — removing it after verification can
break their ad tag. Never delete, rename, or reformat its contents.

Note: `public/sw.js` and `public/push-sw.js` both want root scope. If Monetag's
tag registers `/sw.js`, the browser will hand root control to whichever
registration happens last, which can silence NjikoTrade's own push alerts.

## Rewarded web ads

Mount the Adsterra banner script in a modal-only srcdoc iframe so it executes in its own document; keep the claim countdown in the parent app.

## Crypto checkout

USDT subscriptions use NOWPayments direct payments at the server catalogue's USD price and explicit live USDT network tickers; render provider-issued amount/address/QR in the app, and keep the signed IPN as the only credit trigger. Legacy top-up invoices remain separate so subscription checkout never opens a hosted wrapper.

## Native Google sign-in

Use Lovable's managed OAuth helper directly from the sign-in button on both web and Android; direct Supabase Google OAuth has no managed secret and fails before Google opens. Android OAuth must return first to the allowlisted HTTPS bridge `https://njikotrade.com/auth/native-callback`, which launches the package-targeted `com.njikotrade.app://auth-callback` deep link. A single root-level Capacitor App listener owns warm and cold deep-link delivery, establishes and verifies the session, persists it, and navigates to the feed; MainActivity must not consume or rewrite the credential-bearing intent. Do not add a second callback consumer, an intermediate start page, or a `_system` window target.

## Premium subscriptions

Plans are one-off payments per period (NGN→Korapay, USD→Paystack, USDT→NOWPayments; Wallet sells plans only, no credit packs) that grant `subscription_credits` on activation; short plans never refill, the yearly plan refills via `subscription_next_refill_at` checked by the 00:00 UTC cron, which touches only subscription columns, and spend RPCs draw subscription credits before the wallet — keeps purchased/wallet metrics untouched.
