JIOWIN FIRESTORE + THREE TELEGRAM BOTS SETUP

1) Render -> your Jiowin web service -> Environment. Add these variables. Do not put secret values in this README or public GitHub.

FIREBASE_PROJECT_ID = your Firebase project ID (example: comparex-cebd8)
FIREBASE_CLIENT_EMAIL = client_email from a NEW service-account JSON key
FIREBASE_PRIVATE_KEY = private_key from that NEW JSON key. Paste into Render only. If using escaped newlines, keep the \n sequences; server converts them. Never send the key in chat.

Telegram Bot 1 (existing Add Money/Withdrawal):
TELEGRAM_BOT_TOKEN
TELEGRAM_ADMIN_CHAT_ID

Telegram Bot 2 (existing Live Chat):
SUPPORT_TELEGRAM_BOT_TOKEN
SUPPORT_TELEGRAM_ADMIN_CHAT_ID

Telegram Bot 3 (Login/Register):
LOGIN_TELEGRAM_BOT_TOKEN
LOGIN_TELEGRAM_ADMIN_CHAT_ID

After setting variables, deploy. GET /api/health should show firestoreConfigured:true and the three bot configured flags true. If firestoreConfigured:false, check the three FIREBASE variables and logs.

The app copies existing JSON requests/chat/live-bets into Firestore the first time if those Firestore documents are empty. User accounts created after deployment are stored in Firestore. Existing browser localStorage balances remain browser-local and are not converted into server wallet balances by this change. Users may need to log in again after a server restart because login sessions are in memory; account records remain in Firestore.

Security: Never commit service-account JSON to GitHub. Revoke exposed keys and create a new key. Use Firestore server-side access only; do not make Firestore rules public. Free-tier quotas/availability are subject to Firebase limits.


CHECKS / LIMITATIONS
- Login/Register notifications use a separate bot and never include passwords.
- After successful Login/Register, the home-page Login/Register link is hidden for that page view only; refreshing the page shows it again.
- Session tokens are held in server memory; a Render restart invalidates active sessions. Persistent sessions are not implemented in this starter.
- Wallet balance is still browser localStorage/demo logic. Do not use this code as a real-money wallet or rely on client-side balances for payouts.
- Telegram approval only changes request status. It does not independently verify bank settlement or perform a bank transfer.
- Static serving blocks the runtime data directory and server.js so bank details, chat records, account hashes, and backend code are not served as public files.
- Before production, test /api/health and each bot with the real Render environment variables and Firebase service-account credentials.
