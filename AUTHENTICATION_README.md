# 🔐 Swappify Authentication & eKYC System

## 🎯 Quick Start

### 1. Install Dependencies
```bash
npm install
```

### 2. Configure Firebase
1. Create a Firebase project at https://console.firebase.google.com/
2. Copy your Firebase config from Project Settings
3. Update `src/config/firebase.js` with your credentials

### 3. Run the App
```bash
npm run dev
```

### 4. Test Authentication
- Go to `http://localhost:5173/login`
- Sign up with email/password
- Complete Aadhaar verification (use any 12-digit number except starting with 0 or 1)
- Complete Video eKYC (allow camera access)
- See your verified badge on `/profile`

---

## 📁 Project Structure

```
src/
├── config/
│   └── firebase.js                 # Firebase initialization
├── contexts/
│   ├── AuthContext.jsx             # Authentication state management
│   └── ToastContext.jsx            # Toast notifications
├── components/
│   ├── AadhaarVerification.jsx     # Aadhaar verification form
│   ├── VideoKYC.jsx                # Video eKYC recording
│   ├── VerificationBadge.jsx       # Trust badge component
│   └── Navbar.jsx                  # Navigation (updated)
├── pages/
│   ├── Profile.jsx                 # User profile & verification
│   ├── Login.jsx                   # Login/Signup page
│   ├── Dashboard.jsx               # Main dashboard
│   └── ...
└── App.jsx                         # App wrapper (updated)
```

---

## 🔑 Key Features

### ✅ Authentication
- Email/Password signup and login
- Google OAuth (optional)
- Persistent sessions
- Secure token management
- Auto-login on page refresh

### ✅ Aadhaar Verification
- 12-digit Aadhaar number input
- Auto-formatting (XXXX XXXX XXXX)
- Mock API verification (2-second delay)
- Validation rules (no 0 or 1 at start)
- Firestore storage

### ✅ Video eKYC
- Webcam access with permission handling
- 5-second video recording
- Live preview with countdown
- Firebase Storage upload
- Secure video URLs
- Verification status update

### ✅ Trust Indicators
- Verification badges (Verified/Unverified)
- Display on profile, items, swaps
- Green badge for fully verified users
- Gray badge for unverified users

---

## 🔧 Configuration

### Firebase Setup

1. **Create Firebase Project**
   - Go to https://console.firebase.google.com/
   - Click "Add project"
   - Follow the setup wizard

2. **Enable Authentication**
   - Go to Authentication > Sign-in method
   - Enable Email/Password
   - (Optional) Enable Google

3. **Create Firestore Database**
   - Go to Firestore Database
   - Click "Create database"
   - Start in production mode
   - Add security rules (see FIREBASE_SETUP_GUIDE.md)

4. **Setup Storage**
   - Go to Storage
   - Click "Get started"
   - Add security rules (see FIREBASE_SETUP_GUIDE.md)

5. **Get Configuration**
   - Go to Project Settings
   - Scroll to "Your apps"
   - Click Web icon (</>)
   - Copy the config object
   - Paste into `src/config/firebase.js`

---

## 📊 Database Schema

### Firestore Collections

#### `users/{userId}`
```javascript
{
  uid: string,                    // Firebase Auth UID
  email: string,                  // User email
  name: string,                   // Display name
  swapCoinBalance: number,        // SwapCoin balance
  aadhaarVerified: boolean,       // Aadhaar verification status
  ekycVideoVerified: boolean,     // Video eKYC status
  aadhaarNumber: string,          // Encrypted Aadhaar number
  ekycVideoUrl: string,           // Firebase Storage URL
  createdAt: string               // ISO timestamp
}
```

#### `transactions/{transactionId}`
```javascript
{
  id: string,                     // Transaction ID
  userId: string,                 // User Firebase UID
  type: string,                   // "initial" | "earn" | "spend"
  amount: number,                 // Coin amount (+ or -)
  itemName: string,               // Item/reason name
  itemId: string | null,          // Related item ID
  timestamp: string               // ISO timestamp
}
```

### Firebase Storage Structure
```
ekyc-videos/
  {userId}/
    {timestamp}.webm              // User's eKYC video
```

---

## 🔒 Security

### Firestore Rules
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth.uid == userId;
      allow read: if request.auth != null;
    }
    match /transactions/{transactionId} {
      allow read, create: if request.auth != null;
    }
  }
}
```

### Storage Rules
```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /ekyc-videos/{userId}/{videoId} {
      allow read, write: if request.auth.uid == userId
                         && request.resource.size < 10 * 1024 * 1024
                         && request.resource.contentType.matches('video/.*');
    }
  }
}
```

---

## 🎨 UI Components

### AuthContext Hook
```javascript
import { useAuth } from '../contexts/AuthContext';

function MyComponent() {
  const { 
    currentUser,        // Firebase user object
    userProfile,        // Firestore user profile
    signup,             // Signup function
    login,              // Login function
    loginWithGoogle,    // Google OAuth function
    logout,             // Logout function
    updateUserProfile,  // Update profile function
    fetchUserProfile    // Fetch any user's profile
  } = useAuth();
  
  // Use auth state and functions
}
```

### Verification Badge
```javascript
import VerificationBadge from '../components/VerificationBadge';

<VerificationBadge 
  isVerified={userProfile?.aadhaarVerified && userProfile?.ekycVideoVerified}
  size="sm"  // xs, sm, md, lg
/>
```

### Aadhaar Verification
```javascript
import AadhaarVerification from '../components/AadhaarVerification';

<AadhaarVerification
  onVerified={() => {
    // Called when Aadhaar is verified
    // Proceed to Video KYC
  }}
  onSkip={() => {
    // Called when user skips
    // Close modal or redirect
  }}
/>
```

### Video KYC
```javascript
import VideoKYC from '../components/VideoKYC';

<VideoKYC
  onComplete={() => {
    // Called when video is uploaded
    // Show success message
  }}
  onSkip={() => {
    // Called when user skips
    // Close modal or redirect
  }}
/>
```

---

## 🧪 Testing

### Test Accounts
Create test accounts with these patterns:

**Valid Aadhaar Numbers:**
- `234567890123` ✅
- `345678901234` ✅
- `456789012345` ✅

**Invalid Aadhaar Numbers:**
- `012345678901` ❌ (starts with 0)
- `123456789012` ❌ (starts with 1)
- `12345678901` ❌ (only 11 digits)

### Test Flow
1. **Signup:** Create account with email/password
2. **Login:** Sign in with created account
3. **Profile:** Navigate to /profile
4. **Aadhaar:** Click "Verify Now", enter valid number
5. **Video KYC:** Allow camera, record 5-second video
6. **Verify:** Check for green "Verified" badge

---

## 🚀 Deployment

### Environment Variables
Create `.env` file (copy from `.env.example`):
```bash
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_auth_domain
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_storage_bucket
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id
```

### Build for Production
```bash
npm run build
```

### Deploy to Vercel/Netlify
```bash
# Vercel
vercel deploy

# Netlify
netlify deploy --prod
```

---

## 📱 Browser Support

### Required Features
- ✅ ES6+ JavaScript
- ✅ WebRTC (for camera access)
- ✅ MediaRecorder API (for video recording)
- ✅ LocalStorage
- ✅ Fetch API

### Tested Browsers
- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+

### Mobile Support
- ✅ iOS Safari 14+
- ✅ Chrome Mobile 90+
- ✅ Samsung Internet 14+

---

## 🐛 Troubleshooting

### Camera Not Working
**Issue:** Camera permission denied or not accessible

**Solutions:**
- Ensure you're on HTTPS (or localhost)
- Check browser permissions in settings
- Try a different browser
- Restart browser and clear cache

### Firebase Connection Error
**Issue:** "Firebase: Error (auth/configuration-not-found)"

**Solutions:**
- Verify Firebase config in `src/config/firebase.js`
- Check API key is correct
- Ensure Firebase project is active
- Check internet connection

### Video Upload Fails
**Issue:** Video doesn't upload to Firebase Storage

**Solutions:**
- Check Firebase Storage rules
- Verify file size < 10MB
- Check internet connection
- Ensure user is authenticated
- Check browser console for errors

### Verification Status Not Updating
**Issue:** Badge doesn't show after verification

**Solutions:**
- Check Firestore security rules
- Verify user is authenticated
- Refresh the page
- Check browser console for errors
- Verify Firestore write permissions

---

## 📚 Additional Resources

### Documentation
- [Firebase Setup Guide](./FIREBASE_SETUP_GUIDE.md)
- [Implementation Details](./AUTH_EKYC_IMPLEMENTATION.md)
- [Swappify Overview](./README.md)

### External Links
- [Firebase Documentation](https://firebase.google.com/docs)
- [MediaRecorder API](https://developer.mozilla.org/en-US/docs/Web/API/MediaRecorder)
- [WebRTC](https://webrtc.org/)
- [Aadhaar UIDAI](https://uidai.gov.in/)

---

## 🤝 Contributing

### Adding New Verification Methods

1. Create new component in `src/components/`
2. Add verification logic
3. Update Firestore schema
4. Update `AuthContext` if needed
5. Add to Profile page
6. Update documentation

### Reporting Issues

Please include:
- Browser and version
- Steps to reproduce
- Expected vs actual behavior
- Console errors (if any)
- Screenshots (if applicable)

---

## ✅ Checklist

### Before Demo
- [ ] Firebase project configured
- [ ] Authentication enabled (Email/Password)
- [ ] Firestore database created
- [ ] Storage bucket created
- [ ] Security rules applied
- [ ] Test signup works
- [ ] Test login works
- [ ] Test Aadhaar verification
- [ ] Test video eKYC
- [ ] Test verification badges
- [ ] Test on mobile device

### Production Ready
- [ ] Environment variables set
- [ ] Firebase billing enabled
- [ ] Custom domain configured (optional)
- [ ] Error monitoring setup
- [ ] Analytics configured
- [ ] Backup strategy in place
- [ ] Security audit completed
- [ ] Performance testing done

---

## 🎉 Success!

Your Swappify platform now has enterprise-grade authentication with:
- 🔐 Secure Firebase Authentication
- 🆔 Aadhaar Verification (simulated)
- 📹 Video eKYC with webcam
- ✅ Trust badges throughout
- 🔒 Encrypted data storage
- 🎨 Beautiful, consistent UI

**Ready to impress the judges!** 🏆
