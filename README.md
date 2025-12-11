# Little Soul Stories - Setup Instructions

## 🚨 Important: Firebase Configuration Required

Your `index.html` and `admin.html` files have been fixed to remove placeholder global variables, but you still need to add your own Firebase credentials for the app to work.

## Step 1: Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Add project" or select an existing project
3. Follow the setup wizard to create your project

## Step 2: Enable Firestore Database

1. In your Firebase project, click "Firestore Database" in the left sidebar
2. Click "Create database"
3. Choose "Start in test mode" for now (you can secure it later)
4. Select a Cloud Firestore location (choose one closest to your users)
5. Click "Enable"

## Step 3: Enable Authentication

1. Click "Authentication" in the left sidebar
2. Click "Get started"
3. Go to the "Sign-in method" tab
4. Click on "Anonymous" and enable it
5. Click "Save"

## Step 4: Register Your Web App

1. In Project Settings (gear icon), scroll to "Your apps"
2. Click the web icon (</>)
3. Register your app with a nickname (e.g., "Little Soul Stories")
4. Click "Register app"
5. Copy the `firebaseConfig` object

## Step 5: Update Your HTML Files

### In `index.html` (around line 132-141):

Replace the placeholder config:
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY_HERE",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

With your actual Firebase config from Step 4.

### In `admin.html` (around line 151-160):

Do the same - replace the placeholder config with your actual Firebase credentials.

**Important:** Make sure both files use the EXACT SAME Firebase config!

## Step 6: Deploy Your Site

You can deploy using:

### Option A: GitHub Pages
1. Go to your repository Settings
2. Click "Pages" in the left sidebar
3. Under "Source", select "main" branch
4. Click "Save"
5. Your site will be live at `https://shaggyyyz.github.io/Little-Soul-Stories/`

### Option B: Firebase Hosting
1. Install Firebase CLI: `npm install -g firebase-tools`
2. Run `firebase login`
3. Run `firebase init hosting`
4. Select your Firebase project
5. Set public directory to `.` (current directory)
6. Configure as single-page app: No
7. Run `firebase deploy`

## Step 7: Test Your App

1. Open `index.html` in your browser (or visit your deployed URL)
2. You should see the "Little Soul Stories" storefront
3. Open `admin.html` to access the admin dashboard
4. Click "Initialize Default Data" to seed the database with sample books
5. Go back to `index.html` and the books should now appear!

## Firestore Rules (Optional but Recommended)

For better security, update your Firestore rules:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /artifacts/little-soul-stories/public/data/{document=**} {
      // Allow anyone to read
      allow read: if true;
      // Allow authenticated users to write
      allow write: if request.auth != null;
    }
  }
}
```

## Troubleshooting

### "No stories found yet"
- Make sure you've added your Firebase credentials to both files
- Check the browser console for errors (F12)
- Make sure Firestore is enabled in your Firebase project
- Try clicking "Initialize Default Data" in the admin panel

### "Could not verify user"
- Make sure Anonymous authentication is enabled in Firebase
- Check that your Firebase config is correct

### Books added in admin don't show in index
- Make sure both files use the same Firebase config
- Make sure the `appId` is set to `"little-soul-stories"` in both files
- Check the browser console for errors

## What Changed?

I've fixed both files to:
1. ✅ Remove `JSON.parse(__firebase_config)` and replace with proper config object structure
2. ✅ Remove `typeof __app_id !== 'undefined'` conditional and use fixed `appId = "little-soul-stories"`
3. ✅ Remove `signInWithCustomToken` and use only `signInAnonymously` for simpler auth
4. ✅ Both files now use identical Firebase paths for data sync

---

**Need help?** Check the Firebase docs at https://firebase.google.com/docs/web/setup
