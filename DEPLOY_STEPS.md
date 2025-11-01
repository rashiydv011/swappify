# 🚀 Deploy to Vercel - Step by Step

## ✅ **Vercel CLI Installed!**

Now let's deploy your app.

---

## 📦 **Method 1: Deploy from Root (Recommended)**

### **Step 1: Login to Vercel**
```bash
vercel login
```
- Opens browser
- Login with your Vercel account
- Return to terminal

### **Step 2: Deploy**
```bash
vercel
```
- Answer the prompts:
  - **Set up and deploy?** → Yes
  - **Which scope?** → Your account
  - **Link to existing project?** → No
  - **Project name?** → swappify (or your choice)
  - **In which directory?** → ./ (press Enter)
  - **Override settings?** → No

### **Step 3: Deploy to Production**
```bash
vercel --prod
```

**Done!** Your site is live! 🎉

---

## 📦 **Method 2: Deploy Only Dist Folder**

If you want to deploy only the built files:

### **Step 1: Navigate to dist**
```bash
cd dist
```

### **Step 2: Deploy**
```bash
vercel --prod
```

### **Step 3: Return to root**
```bash
cd ..
```

---

## 🔐 **Add Environment Variables**

After deployment:

### **Option A: Via Vercel Dashboard**
1. Go to: https://vercel.com/dashboard
2. Click your project
3. Settings → Environment Variables
4. Add each variable from your `.env` file
5. Click Save

### **Option B: Via CLI**
```bash
vercel env add VITE_FIREBASE_API_KEY
# Paste your value when prompted
# Select: Production, Preview, Development

# Repeat for all variables:
vercel env add VITE_FIREBASE_AUTH_DOMAIN
vercel env add VITE_FIREBASE_PROJECT_ID
vercel env add VITE_FIREBASE_STORAGE_BUCKET
vercel env add VITE_FIREBASE_MESSAGING_SENDER_ID
vercel env add VITE_FIREBASE_APP_ID
```

### **Step 4: Redeploy**
```bash
vercel --prod
```

---

## 🌐 **Your Site is Live!**

Vercel will show you the URL:
```
✓ Production: https://swappify-xyz.vercel.app
```

---

## 🔄 **For Future Updates**

```bash
# 1. Make changes to your code

# 2. Deploy
vercel --prod

# That's it! Vercel builds and deploys automatically
```

---

## 📊 **Check Deployment Status**

```bash
# List all deployments
vercel ls

# View project info
vercel inspect
```

---

## 🎯 **Troubleshooting**

### **If build fails on Vercel:**
Deploy the dist folder instead:
```bash
npm run build
cd dist
vercel --prod
cd ..
```

### **If login doesn't work:**
- Make sure browser opens
- Check popup blockers
- Try different browser

### **If deployment is slow:**
- First deployment takes longer
- Subsequent deploys are faster

---

## ✅ **Quick Commands**

```bash
# Login
vercel login

# Deploy preview
vercel

# Deploy production
vercel --prod

# View logs
vercel logs

# Remove project
vercel remove
```

---

## 🎉 **Success!**

Your Swappify platform is now live on the internet! 🌍✨

**Share your URL with friends and start swapping!** 🎊
