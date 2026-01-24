# iOS Apple Health Integration

This app supports saving heart rate measurements directly to Apple Health on iOS devices using iOS Shortcuts.

## One-Time Setup (5 minutes)

### Step 1: Create the Shortcut

1. Open the **Shortcuts** app on your iPhone/iPad
2. Tap the **"+"** button to create a new shortcut
3. Tap **"Add Action"**
4. Search for **"Log Health Sample"**
5. Select **"Log Health Sample"**
6. Configure the action:
   - **Type:** Heart Rate
   - **Value:** Select "Shortcut Input"
   - **Date:** Current Date
7. Tap **"Done"**
8. Name the shortcut: **"SaveHeartRate"** (exactly as written, case-sensitive!)

### Step 2: Grant Health Permissions

1. When you run the shortcut for the first time, iOS will ask for permission
2. Tap **"Allow"** to grant access to write heart rate data to Health

## How to Use

1. **Measure your heart rate** on the web app (tap in rhythm with your pulse)
2. When you see your BPM result, tap the **"Save to Health"** button (appears only on iOS)
3. iOS Shortcuts will open automatically
4. Confirm the action (may ask first time only)
5. Your heart rate is now saved in Apple Health! ✅

## Troubleshooting

### "Shortcut not found" error
- Make sure the shortcut is named exactly **"SaveHeartRate"** (case-sensitive)
- Check that the shortcut exists in the Shortcuts app

### Shortcut doesn't save to Health
- Open Settings → Privacy & Security → Health → Shortcuts
- Make sure "SaveHeartRate" has permission to write Heart Rate data

### Button doesn't appear
- This feature only works on iOS devices (iPhone/iPad)
- Make sure you've completed a measurement (you need a valid BPM value)

## Privacy

- The shortcut only receives the BPM number (e.g., "75")
- No other data is sent to Shortcuts
- Your heart rate data stays in your Apple Health app
- This integration is completely optional

## Alternative: Manual Entry

If you prefer not to use Shortcuts:
1. After measuring, remember your BPM value
2. Open the **Health** app
3. Tap **Browse** → **Heart** → **Heart Rate**
4. Tap **"Add Data"**
5. Enter your BPM value manually
