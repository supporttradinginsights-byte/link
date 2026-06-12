# Trading Insights — Referral Deep Links

> **Free domain solution** using GitHub Pages for referral links and Android App Links.
> No domain purchase needed! ✅

---

## 🔗 Referral Link Format

After setup, your referral links will be:

```
https://YOUR-USERNAME.github.io/trading-insights-links/r/ABCD1234
```

Example share link a user will send:
```
https://johndoe.github.io/trading-insights-links/r/XYZ789
```

---

## ⚙️ Setup — Step by Step

### Step 1: GitHub pe repo banayein

1. GitHub.com pe login karein
2. **New Repository** click karein
3. Name dein: `trading-insights-links`
4. **Public** rakhein (GitHub Pages free mein sirf public repo pe kaam karta hai)
5. Create karo

### Step 2: Yeh files upload karein

```
trading-insights-links/
├── index.html            ← Play Store download page
├── 404.html              ← Referral redirect (IMPORTANT - ye hi magic hai)
├── .well-known/
│   └── assetlinks.json   ← Android App Links ke liye
└── README.md
```

> ⚠️ `.well-known` folder hidden hota hai (dot se start). GitHub pe manually create karein ya zip se upload karein.

### Step 3: GitHub Pages enable karein

1. Repository → **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: `main` → folder: `/ (root)`
4. **Save** karein
5. 1-2 minute wait karein
6. URL milega: `https://YOUR-USERNAME.github.io/trading-insights-links`

### Step 4: SHA256 Fingerprint fill karein

`assetlinks.json` mein apna fingerprint daalein:

```bash
# Keystore se fingerprint nikaalein:
keytool -list -v -keystore your-key.jks -alias your-alias
# Ya Google Play Console → Setup → App Signing → SHA-256 certificate fingerprint
```

Phir `.well-known/assetlinks.json` edit karein:
```json
"sha256_cert_fingerprints": [
  "AB:CD:EF:12:34:..."   ← yahan real fingerprint
]
```

### Step 5: Backend .env update karein

```env
# Server pe yeh set karein:
SERVER_URL=http://155.133.23.X:PORT
GOOGLE_PLAY_PACKAGE_NAME=com.premium.trading_insights.signals
REFERRAL_FALLBACK_URL=https://YOUR-USERNAME.github.io/trading-insights-links
```

### Step 6: Flutter app update karein

`AndroidManifest.xml` mein GitHub Pages domain add karein:

```xml
<intent-filter android:autoVerify="true">
    <action android:name="android.intent.action.VIEW"/>
    <category android:name="android.intent.category.DEFAULT"/>
    <category android:name="android.intent.category.BROWSABLE"/>
    <data
        android:scheme="https"
        android:host="YOUR-USERNAME.github.io"
        android:pathPrefix="/trading-insights-links/r/"/>
</intent-filter>
```

---

## 🔄 How it Works

```
User taps referral link
        │
        ▼
https://username.github.io/trading-insights-links/r/ABC123
        │
        ├── App INSTALLED? ──► Android App Links intercepts
        │                       → app_links stream fires in Flutter
        │                       → code auto-fill hota hai ✅
        │
        └── App NOT installed? ─► GitHub 404.html fires
                                   → JavaScript reads /r/ABC123
                                   → Redirects to Play Store:
                                     ?referrer=ref%3DABC123
                                   → User installs app
                                   → First launch → code auto-fill ✅
```

---

## ✅ Referral Link Generate Karna (Flutter mein)

Flutter app mein referral share karte waqt:

```dart
// Pehle wala (IP wala — ab nahi use karna):
// final link = 'http://155.133.23.X:PORT/r/${user.referralCode}';

// Naya (GitHub Pages — sahi tarika):
final link = 'https://YOUR-USERNAME.github.io/trading-insights-links/r/${user.referralCode}';

Share.share('Trading Insights join karein! Meri referral link: $link');
```

---

## ❓ FAQ

**Q: Kya yeh free hai?**
A: Haan, GitHub Pages bilkul free hai public repos ke liye.

**Q: Kya baad mein real domain add kar sakte hain?**
A: Haan! GitHub Pages Settings mein "Custom domain" daal dein — bas `CNAME` file add hogi, baaki sab same rahega.

**Q: API server wahi 155.133.23.x pe rahega?**
A: Haan. Sirf referral links GitHub pe host honge. API server abhi bhi wahi kaam karega.
