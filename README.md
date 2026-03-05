# Cashflow Stacking — Calculator & Tracker

Personal financial freedom calculator with Google Sheets sync for cross-device data persistence.

## Quick Setup (5 minutes)

### 1. Google Cloud Setup

You need a Google Cloud project with the Sheets API enabled.

**Enable the API:**
- Go to [Google Cloud Console](https://console.cloud.google.com)
- Select your project
- Go to **APIs & Services → Library**
- Search "Google Sheets API" → click **Enable**

**Create OAuth Credentials:**
- Go to **APIs & Services → Credentials**
- Click **+ Create Credentials → OAuth client ID**
- Application type: **Web application**
- Name: `Cashflow Tracker`
- **Authorized JavaScript origins:** `https://YOUR-GITHUB-USERNAME.github.io`
- **Authorized redirect URIs:** `https://YOUR-GITHUB-USERNAME.github.io`
- Click Create → copy the **Client ID**

**OAuth Consent Screen** (if not already configured):
- Go to **APIs & Services → OAuth consent screen**
- User type: **External**
- App name: `Cashflow Tracker`
- Add your email as a **Test user**
- Add scope: `https://www.googleapis.com/auth/spreadsheets`

### 2. Create a Google Sheet

- Create a new blank Google Sheet
- Copy the **Sheet ID** from the URL: `https://docs.google.com/spreadsheets/d/SHEET_ID_HERE/edit`

### 3. Configure the App

Open `index.html` and find the `SHEETS_CONFIG` section near the top:

```javascript
const SHEETS_CONFIG = {
  CLIENT_ID: 'YOUR_OAUTH_CLIENT_ID_HERE',  // ← paste your OAuth Client ID
  SHEET_ID: 'YOUR_GOOGLE_SHEET_ID_HERE',   // ← paste your Sheet ID
  ...
};
```

### 4. Deploy to GitHub Pages

1. Create a new GitHub repository (can be public — your data is private in Google Sheets)
2. Push the `index.html` file to the repo
3. Go to **Settings → Pages**
4. Source: **Deploy from a branch**
5. Branch: `main`, folder: `/ (root)`
6. Your app will be live at `https://YOUR-USERNAME.github.io/REPO-NAME/`

### 5. Use It

1. Open your GitHub Pages URL
2. Go to **Settings → Data Management**
3. Click **Sign In with Google**
4. The app will create 3 tabs in your Google Sheet: `Settings`, `Flips`, `Actuals`
5. Data now syncs across all devices where you sign in!

## How It Works

- **Signed in:** Data saves to both Google Sheets (sync) and localStorage (backup)
- **Not signed in:** Data saves to localStorage only (device-specific)
- **Google Sheet structure:**
  - `Settings` tab: key-value pairs for all configuration
  - `Flips` tab: one row per tracked flip with all details
  - `Actuals` tab: one row per month with personal cash, withdrawals, notes

## Features

- 📊 Dashboard with freedom date, cashflow trajectory chart
- 🔄 Flip tracking with index fund tax calculations
- 📋 Detail table with actuals vs forecast (export to CSV)
- ⚙️ Configurable flip structure, 401k integration
- 🎯 Self-sustaining freedom calculation
- 💾 Export/Import JSON backup
- 📱 Works on phone and desktop
