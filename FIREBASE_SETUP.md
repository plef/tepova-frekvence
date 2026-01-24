# Firebase Setup Instructions

## Security Rules Configuration

To protect your Firebase Realtime Database from abuse, you need to configure security rules.

### Step 1: Go to Firebase Console

1. Open https://console.firebase.google.com/
2. Select your project: **tepova-frekvence**
3. Click on **Realtime Database** in the left menu
4. Click on the **Rules** tab

### Step 2: Update Security Rules

Replace the existing rules with the following configuration:

```json
{
  "rules": {
    "counters": {
      ".read": true,
      "visits": {
        ".write": "!data.exists() || newData.val() === data.val() + 1"
      },
      "measurements": {
        ".write": "!data.exists() || newData.val() === data.val() + 1"
      },
      "measurementsByCountry": {
        "$country": {
          ".write": "!data.exists() || newData.val() === data.val() + 1"
        }
      },
      "totalBpmByCountry": {
        "$country": {
          ".write": "!data.exists() || (newData.val() >= data.val() && newData.val() - data.val() <= 220)"
        }
      }
    },
    "measurements": {
      ".read": true,
      "$timestamp": {
        ".write": "!data.exists()",
        ".validate": "newData.hasChildren(['bpm', 'country', 'timestamp']) &&
                      newData.child('bpm').isNumber() &&
                      newData.child('bpm').val() >= 30 &&
                      newData.child('bpm').val() <= 220 &&
                      newData.child('country').isString() &&
                      newData.child('timestamp').isNumber()"
      }
    }
  }
}
```

### What do these rules do?

**Counters:**
- **`.read": true`** - Anyone can read the counter values (needed to display them on the website)
- **`visits` and `measurements`** - Only allows incrementing by 1
  - Prevents users from setting arbitrary values
  - Prevents decrementing the counter
- **`measurementsByCountry/$country`** - Per-country measurement counters (increment +1 only)
- **`totalBpmByCountry/$country`** - Sum of all BPM values per country
  - Allows increment up to +220 (max reasonable BPM)
  - Prevents arbitrary values and decrementing

**Measurements collection:**
- **Write once only** - Each measurement can only be written once (`!data.exists()`)
- **Validation** - Ensures all required fields are present:
  - `bpm`: Number between 30-220
  - `country`: String (country code)
  - `timestamp`: Number (Unix timestamp)

### Step 3: Publish Rules

Click the **Publish** button to activate the new security rules.

## Built-in Rate Limiting

The application has built-in protection against abuse:

1. **Per-session limit:** Maximum 5 measurements per 30-minute session
2. **Total limit:** Maximum 10 million measurements (~1 GB storage)
3. **Data retention:** 1 year (measurements older than 1 year won't be saved)
4. **Country detection:** Using ipapi.co (1000 requests/day free tier)

## Additional Protection (Optional)

For even more protection, you can:

1. Enable **App Check** in Firebase Console
2. Use **Cloud Functions** for server-side rate limiting
3. Monitor usage in Firebase Console > Usage and billing

## Database Structure

Your database will have this structure:

```
tepova-frekvence-default-rtdb/
├── counters/
│   ├── visits: 1234
│   ├── measurements: 567
│   ├── measurementsByCountry/
│   │   ├── CZ: 120
│   │   ├── US: 89
│   │   ├── DE: 45
│   │   └── unknown: 12
│   └── totalBpmByCountry/
│       ├── CZ: 8450
│       ├── US: 6230
│       └── DE: 3150
└── measurements/
    ├── 1706123456789/
    │   ├── bpm: 75
    │   ├── country: "CZ"
    │   └── timestamp: 1706123456789
    └── 1706123567890/
        ├── bpm: 82
        ├── country: "US"
        └── timestamp: 1706123567890
```

## Privacy & Data Collection

The application stores the following data:
- ✅ **BPM values** (30-220)
- ✅ **Country code** (e.g., "CZ", "US", "DE")
- ✅ **Timestamp** (Unix timestamp in milliseconds)

The application does NOT store:
- ❌ IP addresses
- ❌ Names or personal identifiers
- ❌ User accounts or authentication data
- ❌ Any other personal information

**Data retention:** 1 year
**GDPR compliance:** Country code is not considered personal data under GDPR

## Monitoring

To monitor your database usage:

1. Go to Firebase Console
2. Select **Realtime Database**
3. Check the **Usage** tab
4. Monitor reads/writes and storage

## Free Tier Limits

- **Simultaneous connections:** 100
- **Storage:** 1 GB
- **Downloads:** 10 GB/month
- **Uploads:** Unlimited

Your app should easily fit within these limits.
