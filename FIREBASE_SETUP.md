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
      }
    }
  }
}
```

### What do these rules do?

- **`.read": true`** - Anyone can read the counter values (needed to display them on the website)
- **`.write": "!data.exists() || newData.val() === data.val() + 1"`** - Only allows incrementing by 1
  - Prevents users from setting arbitrary values
  - Prevents decrementing the counter
  - Allows initialization if counter doesn't exist

### Step 3: Publish Rules

Click the **Publish** button to activate the new security rules.

## Rate Limiting (Optional)

For additional protection against abuse, you can:

1. Enable **App Check** in Firebase Console
2. Use **Cloud Functions** for server-side rate limiting
3. Monitor usage in Firebase Console > Usage and billing

## Database Structure

Your database will have this structure:

```
tepova-frekvence-default-rtdb/
└── counters/
    ├── visits: 1234
    └── measurements: 567
```

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
