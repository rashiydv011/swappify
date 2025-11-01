# 🗺️ Google Maps Integration Setup Guide

Complete guide to set up Google Maps for Swappify's location features.

---

## 📋 Prerequisites

- Google Cloud account (free tier available)
- Credit card (required for Google Cloud, but won't be charged on free tier)
- Firebase project already set up

---

## Step 1: Create Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Click "Select a project" → "New Project"
3. Enter project name: "Swappify" (or use existing Firebase project)
4. Click "Create"

---

## Step 2: Enable Required APIs

1. In Google Cloud Console, go to **APIs & Services** → **Library**
2. Enable these APIs:
   - **Maps JavaScript API** (for map display)
   - **Geocoding API** (for address lookup)
   - **Geolocation API** (optional, for better location accuracy)

For each API:
- Search for the API name
- Click on it
- Click "Enable"
- Wait for activation

---

## Step 3: Create API Key

1. Go to **APIs & Services** → **Credentials**
2. Click **"+ CREATE CREDENTIALS"** → **API key**
3. Copy the API key (starts with `AIza...`)
4. Click "Edit API key" to restrict it (recommended)

---

## Step 4: Restrict API Key (Security)

### Application Restrictions:
1. Select **"HTTP referrers (web sites)"**
2. Add your domains:
   ```
   http://localhost:*
   http://127.0.0.1:*
   https://your-domain.com/*
   https://your-project.vercel.app/*
   https://your-project.netlify.app/*
   https://your-project.web.app/*
   ```

### API Restrictions:
1. Select **"Restrict key"**
2. Choose these APIs:
   - Maps JavaScript API
   - Geocoding API
   - Geolocation API (if enabled)
3. Click **"Save"**

---

## Step 5: Add API Key to Your Project

1. Open your project folder
2. Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```

3. Edit `.env` and add your API key:
   ```env
   VITE_GOOGLE_MAPS_API_KEY=AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
   ```

4. **Important:** Never commit `.env` to Git!
   - `.env` should already be in `.gitignore`
   - Only commit `.env.example`

---

## Step 6: Test the Integration

1. Start your dev server:
   ```bash
   npm run dev
   ```

2. Go to **Add Item** page
3. You should see:
   - ✅ Map loads successfully
   - ✅ Your location marker appears
   - ✅ Address is displayed
   - ✅ You can drag the marker

If you see errors, check the browser console.

---

## 🐛 Troubleshooting

### Error: "This page can't load Google Maps correctly"

**Cause:** API key not set or invalid

**Solution:**
1. Check `.env` file has correct API key
2. Restart dev server (`npm run dev`)
3. Hard refresh browser (Ctrl+Shift+R)

### Error: "Google Maps JavaScript API error: RefererNotAllowedMapError"

**Cause:** Your domain not in allowed referrers

**Solution:**
1. Go to Google Cloud Console → Credentials
2. Edit your API key
3. Add `http://localhost:*` to HTTP referrers
4. Save and wait 5 minutes

### Error: "Geocoding API has not been used"

**Cause:** Geocoding API not enabled

**Solution:**
1. Go to APIs & Services → Library
2. Search "Geocoding API"
3. Click "Enable"

### Map shows but location not working

**Cause:** Browser location permission denied

**Solution:**
1. Click the location icon in browser address bar
2. Allow location access
3. Refresh page

---

## 💰 Pricing & Free Tier

### Google Maps Free Tier (Monthly):
- **$200 free credit** every month
- Maps JavaScript API: 28,000 loads
- Geocoding API: 40,000 requests
- More than enough for small to medium apps!

### Cost After Free Tier:
- Maps JavaScript API: $7 per 1,000 loads
- Geocoding API: $5 per 1,000 requests

### Tips to Stay in Free Tier:
- ✅ Restrict API key to your domains
- ✅ Cache geocoding results
- ✅ Only load maps when needed
- ✅ Set up billing alerts

---

## 🔒 Security Best Practices

### 1. Always Restrict Your API Key
```
✅ DO: Restrict to specific domains
❌ DON'T: Leave unrestricted
```

### 2. Never Expose API Key in Code
```javascript
// ❌ DON'T
const API_KEY = "AIzaSyXXXXXXXXXXXXXX";

// ✅ DO
const API_KEY = import.meta.env.VITE_GOOGLE_MAPS_API_KEY;
```

### 3. Use Environment Variables
```
✅ DO: Store in .env file
❌ DON'T: Hardcode in source files
```

### 4. Monitor Usage
- Set up billing alerts in Google Cloud
- Check usage regularly
- Watch for unusual spikes

---

## 📊 Monitoring Usage

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. **APIs & Services** → **Dashboard**
3. View API usage graphs
4. Set up alerts:
   - **Billing** → **Budgets & alerts**
   - Create budget: $10/month
   - Set alert at 50%, 90%, 100%

---

## 🚀 Deployment Considerations

### For Production:

1. **Update API Key Restrictions:**
   ```
   https://your-production-domain.com/*
   ```

2. **Add to Hosting Platform:**
   
   **Vercel:**
   ```bash
   Settings → Environment Variables
   Add: VITE_GOOGLE_MAPS_API_KEY
   ```

   **Netlify:**
   ```bash
   Site settings → Build & deploy → Environment
   Add: VITE_GOOGLE_MAPS_API_KEY
   ```

   **Firebase:**
   ```bash
   # Add to .env.production
   VITE_GOOGLE_MAPS_API_KEY=your_key
   ```

3. **Test on Production:**
   - Verify maps load
   - Check console for errors
   - Test location features

---

## 📝 Features Implemented

### ✅ Location Tagging (AddItem)
- User location detection
- Interactive map with draggable marker
- Address geocoding
- Location saved with item

### ✅ Distance Display (ItemCard)
- Calculate distance from user
- Show "X km away"
- Haversine formula for accuracy

### ✅ View on Map (ItemMapModal)
- Modal with item location
- Centered on item
- Animated marker
- Full address display

### ✅ Real-time Location Tracking (Future)
- Watch user location
- Update every 10 seconds
- Share with swap partner
- Privacy controls

---

## 🧪 Testing Checklist

- [ ] API key is set in `.env`
- [ ] Dev server restarted
- [ ] Maps load on Add Item page
- [ ] Location permission requested
- [ ] Marker appears on map
- [ ] Address is displayed
- [ ] Can drag marker
- [ ] Location saves with item
- [ ] Distance shows on item cards
- [ ] "View on Map" button works
- [ ] No console errors

---

## 📚 Additional Resources

- [Google Maps JavaScript API Docs](https://developers.google.com/maps/documentation/javascript)
- [Geocoding API Docs](https://developers.google.com/maps/documentation/geocoding)
- [React Google Maps API](https://react-google-maps-api-docs.netlify.app/)
- [Pricing Calculator](https://mapsplatformtransition.withgoogle.com/calculator)

---

## 🆘 Need Help?

### Common Issues:

1. **Map not loading?**
   - Check API key in `.env`
   - Verify APIs are enabled
   - Check browser console

2. **Location not detected?**
   - Allow browser location permission
   - Check HTTPS (required for geolocation)
   - Try different browser

3. **Billing concerns?**
   - Free tier is very generous
   - Set up billing alerts
   - Monitor usage dashboard

---

## ✅ Quick Setup Summary

```bash
# 1. Get API key from Google Cloud Console
# 2. Enable Maps JavaScript API & Geocoding API
# 3. Add to .env file
echo "VITE_GOOGLE_MAPS_API_KEY=your_key_here" >> .env

# 4. Restart dev server
npm run dev

# 5. Test on Add Item page
# 6. Done! 🎉
```

**Your location features are now ready!** 🗺️✨
