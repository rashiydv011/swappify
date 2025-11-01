# 🔐 Firestore Rules - Allow Public User Count

## 🎯 Goal

Allow **anyone (even guests)** to read the user count on the Home page, while keeping user data secure.

---

## 📋 Required Firestore Rules

### **Update Your Firestore Rules:**

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Users collection
    match /users/{userId} {
      // ✅ Allow ANYONE to read (for counting users on homepage)
      allow read: if true;
      
      // ✅ Allow users to create their own document during signup
      allow create: if request.auth != null && request.auth.uid == userId;
      
      // ✅ Allow users to update only their own document
      allow update: if request.auth != null && request.auth.uid == userId;
      
      // ❌ Don't allow deleting user documents
      allow delete: if false;
    }
    
    // Add other collections here...
  }
}
```

---

## 🔧 How to Update Rules

### **Step 1: Go to Firebase Console**
```
1. Open Firebase Console (https://console.firebase.google.com)
2. Select your project
3. Click "Firestore Database" in left menu
4. Click "Rules" tab at the top
```

### **Step 2: Update Rules**
```
1. Copy the rules above
2. Paste into the rules editor
3. Click "Publish"
4. Wait for confirmation
```

### **Step 3: Test**
```
1. Refresh your Swappify home page
2. Should show actual user count ✅
3. Even when logged out!
```

---

## 🔒 Security Considerations

### **What This Allows:**
- ✅ Anyone can **read** user documents (for counting)
- ✅ Anyone can see user names, emails (public profiles)
- ✅ Authenticated users can create/update their own profile

### **What This Prevents:**
- ❌ Users can't modify other users' data
- ❌ Users can't delete any user documents
- ❌ Unauthenticated users can't create/update data

---

## 🎨 Alternative: Count-Only Access

If you want to **hide user details** but still show the count:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Stats collection (public read)
    match /stats/userCount {
      allow read: if true;
      allow write: if false;  // Only Cloud Functions can write
    }
    
    // Users collection (private)
    match /users/{userId} {
      // Only authenticated users can read
      allow read: if request.auth != null;
      allow create: if request.auth != null && request.auth.uid == userId;
      allow update: if request.auth != null && request.auth.uid == userId;
      allow delete: if false;
    }
  }
}
```

Then use a Cloud Function to update the count:
```javascript
// Cloud Function (Firebase Functions)
exports.updateUserCount = functions.firestore
  .document('users/{userId}')
  .onCreate(async (snap, context) => {
    const statsRef = admin.firestore().doc('stats/userCount');
    await statsRef.set({ count: admin.firestore.FieldValue.increment(1) }, { merge: true });
  });
```

---

## 🧪 Test Public Access

### **Test in Browser Console:**
```javascript
// Open browser console (F12)
// Run this code:

import { collection, getDocs } from 'firebase/firestore';
import { db } from './config/firebase';

const usersSnapshot = await getDocs(collection(db, "users"));
console.log('User count:', usersSnapshot.size);

// Should show count without errors ✅
```

---

## ✅ Recommended Rules (Simple & Secure)

For your use case, I recommend **Option 1** (public read):

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read: if true;  // Public profiles
      allow create: if request.auth != null && request.auth.uid == userId;
      allow update: if request.auth != null && request.auth.uid == userId;
      allow delete: if false;
    }
  }
}
```

**Why?**
- ✅ Simple to implement
- ✅ Works immediately
- ✅ User profiles are meant to be public anyway
- ✅ No Cloud Functions needed
- ✅ Real-time count updates

---

## 🚀 Apply Rules Now

```bash
1. Go to Firebase Console
2. Firestore Database → Rules
3. Copy the recommended rules above
4. Click "Publish"
5. Refresh your app
6. User count should appear! ✅
```

**This will allow the Home page to show the real user count even for visitors!** 🎉
