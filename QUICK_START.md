# Swappify - Quick Start Guide

## 🚀 Start the Application

```bash
npm run dev
```

The app will run on `http://localhost:5173`

---

## 🎯 New Features Overview

### 1️⃣ Toast Notifications
**What:** Beautiful notifications that appear top-right
**When:** Login, signup, item added, swap sent, swap accepted/declined, message sent
**Auto-dismiss:** 4 seconds

### 2️⃣ Offer-Based Swaps
**What:** Users must select one of their items to offer in exchange
**How:** Click "Request Swap" → Select your item → Send request
**Data:** Stores both requested and offered items

### 3️⃣ MySwaps Page
**Location:** Navbar → "My Swaps"
**Tabs:**
- **Requests Received** - Swaps where you're the owner (Accept/Decline)
- **Requests Sent** - Swaps you initiated (view status)
- **Completed Swaps** - Accepted swaps (Open Chat)

### 4️⃣ In-App Chat
**Access:** MySwaps → Click "Open Chat" on accepted swap
**Features:** Real-time messaging, auto-scroll, timestamps
**Storage:** Messages saved in swap record

---

## 📱 User Flow

```
1. Browse Items
   ↓
2. Click "Request Swap"
   ↓
3. Select Item to Offer
   ↓
4. Send Request (Toast: "Swap request sent!")
   ↓
5. Go to MySwaps
   ↓
6. Owner Accepts/Declines
   ↓
7. If Accepted → "Open Chat" button appears
   ↓
8. Chat to coordinate exchange
```

---

## 🗂️ Data Structure

### Swaps (localStorage key: "swaps")
```javascript
{
  id: "swap_1234567890",
  requestedItem: {
    name: "Item Name",
    description: "...",
    category: "...",
    images: [...],
    userId: "...",
    userName: "..."
  },
  offeredItem: {
    // Same structure as requestedItem
  },
  requesterId: "user_id",
  requesterName: "User Name",
  ownerId: "owner_id",
  ownerName: "Owner Name",
  status: "pending" | "accepted" | "declined",
  timestamp: "2025-10-27T...",
  messages: [
    {
      senderId: "user_id",
      senderName: "User Name",
      text: "Message content",
      timestamp: "2025-10-27T..."
    }
  ]
}
```

---

## 🎨 Component Usage

### Toast Notifications
```javascript
import { useToast } from "../contexts/ToastContext";

function MyComponent() {
  const { addToast } = useToast();
  
  const handleAction = () => {
    // Your logic here
    addToast("Success message!", "success");
    // or addToast("Error message", "error");
    // or addToast("Info message", "info");
  };
}
```

### SwapModal
```javascript
<SwapModal
  isOpen={isOpen}
  onClose={() => setIsOpen(false)}
  onConfirm={(offeredItem) => {
    // Handle swap request with offeredItem
  }}
  item={selectedItem}
  user={currentUser}
  userItems={myItems}
/>
```

---

## 🔗 Routes

| Path | Component | Description |
|------|-----------|-------------|
| `/` | Home | Landing page |
| `/login` | Login | Login/Signup |
| `/dashboard` | Dashboard | Browse items & My items |
| `/add` | AddItem | Post new item |
| `/myswaps` | MySwaps | Manage swap requests |
| `/chat/:swapId` | ChatPage | In-app messaging |

---

## 🧪 Testing the Features

### Test Toast Notifications:
1. Login → See "Welcome back!" toast
2. Add item → See "Item added successfully!" toast
3. Request swap → See "Swap request sent!" toast
4. Accept swap → See "Swap accepted!" toast
5. Send message → See "Message sent" toast

### Test Offer-Based Swaps:
1. Add at least 2 items to your account
2. Open debug tool: `http://localhost:5173/add-test-data.html`
3. Add test items from different user
4. Go to Dashboard → Browse Items
5. Click "Request Swap" on test item
6. Select one of your items to offer
7. Click "Send Swap Request"
8. Check MySwaps page

### Test MySwaps:
1. Go to MySwaps
2. View "Requests Received" tab
3. Click Accept on a swap
4. See toast notification
5. Go to "Completed Swaps" tab
6. Click "Open Chat"

### Test Chat:
1. Open chat from accepted swap
2. Type message and send
3. See message appear in chat
4. Refresh page - messages persist
5. Check auto-scroll to bottom

---

## 🎯 Key Files Modified/Created

**New Files:**
- `src/contexts/ToastContext.jsx`
- `src/pages/MySwaps.jsx`
- `src/pages/ChatPage.jsx`

**Updated Files:**
- `src/App.jsx` - Added routes & ToastProvider
- `src/components/SwapModal.jsx` - Offer-based selection
- `src/components/Navbar.jsx` - Added MySwaps link
- `src/pages/Dashboard.jsx` - Toast + swap logic
- `src/pages/Login.jsx` - Toast notifications
- `src/pages/AddItem.jsx` - Toast notifications
- `src/index.css` - Custom animations

---

## 💡 Tips

1. **Multiple Users:** Use debug tool to add items from different users for testing
2. **LocalStorage:** All data persists in browser localStorage
3. **Responsive:** Test on mobile by resizing browser window
4. **Animations:** All transitions are smooth and follow brand guidelines
5. **Toast Stack:** Multiple toasts stack vertically, newest on top

---

## 🐛 Troubleshooting

**Toast not showing?**
- Check that component is wrapped in ToastProvider (App.jsx)
- Verify import: `import { useToast } from "../contexts/ToastContext"`

**Swap modal not showing items?**
- Ensure you have items in "My Items" tab
- Check userItems prop is passed to SwapModal

**Chat not loading?**
- Verify swap status is "accepted"
- Check swapId in URL matches swap in localStorage

**Messages not persisting?**
- Check localStorage for "swaps" key
- Verify messages array exists in swap object

---

## ✅ All Features Working

✓ Toast notifications (success/error/info)  
✓ Offer-based swap with item selection  
✓ MySwaps page with 3 tabs  
✓ In-app chat messaging  
✓ Accept/Decline functionality  
✓ Status tracking  
✓ Mobile responsive  
✓ Smooth animations  
✓ Brand-consistent styling  

**Ready for production!** 🎉
