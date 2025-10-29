# 🚨 Driver Dashboard Debug Guide

## Issue: "Failed to fetch available bookings"

This error occurs when the driver dashboard can't connect to the backend API. Here's how to debug and fix it:

## 🔍 **Step 1: Check Backend Server**

Make sure your backend server is running:

```bash
cd urgent-vue-go/backend
npm start
```

You should see:

```
🚀 Server running on port 5000
✅ API Endpoints:
   - Driver: http://localhost:5000/api/driver
```

## 🔍 **Step 2: Check Driver Account Status**

The error usually means one of these issues:

### A) **Not Logged In as Driver**

- Make sure you're logged in with a driver account
- Check the browser console for authentication errors

### B) **Driver Not Approved**

- Driver accounts need admin approval to access the dashboard
- Login as admin and approve the driver in "Manage Users"

### C) **Wrong User Role**

- Make sure the account has role: "driver" (not "patient" or "admin")

## 🔍 **Step 3: Test Driver Routes**

I've added a test endpoint to help debug. Check the browser console for:

```
Driver routes test: { success: true, message: "Driver routes are working", user: {...} }
```

If you see an error here, it's an authentication/authorization issue.

## 🔍 **Step 4: Check Network Tab**

1. Open browser DevTools (F12)
2. Go to Network tab
3. Refresh the driver dashboard
4. Look for failed requests to `/api/driver/available-bookings`

Common error responses:

- **401 Unauthorized**: Not logged in or invalid token
- **403 Forbidden**: Wrong role or driver not approved
- **500 Server Error**: Backend issue

## 🔍 **Step 5: Verify Database Connection**

Test if the backend can connect to MongoDB:

```bash
curl http://localhost:5000/test-db
```

Should return:

```json
{
  "success": true,
  "message": "Database test successful",
  "connection_state": "connected",
  "user_count": 5
}
```

## 🔧 **Quick Fixes**

### Fix 1: Approve Driver Account

1. Login as admin
2. Go to "Manage Users" → "Drivers" tab
3. Find your driver account
4. Click "Approve" button

### Fix 2: Check Environment Variables

Make sure `.env` file has:

```
JWT_SECRET=your_jwt_secret
MONGODB_URI=your_mongodb_connection_string
```

### Fix 3: Restart Backend

```bash
cd urgent-vue-go/backend
# Stop server (Ctrl+C)
npm start
```

## 🧪 **Testing Steps**

1. **Register a driver account** (role: "Ambulance Driver")
2. **Login as admin** and approve the driver
3. **Login as the approved driver**
4. **Check driver dashboard** - should show available bookings

## 📊 **Expected Behavior**

When working correctly:

- Driver dashboard loads without errors
- Shows "Available Emergency Calls" section (if any pending requests)
- Shows "Current Assignments" section
- Driver can toggle availability status

## 🐛 **Common Issues**

| Error                                | Cause                       | Solution                      |
| ------------------------------------ | --------------------------- | ----------------------------- |
| "Failed to fetch available bookings" | Driver not approved         | Approve driver in admin panel |
| "Not authorized"                     | Wrong role or not logged in | Login with driver account     |
| "User not found"                     | Invalid token               | Logout and login again        |
| Network error                        | Backend not running         | Start backend server          |

## 🔄 **Complete Test Flow**

1. **Patient requests ambulance** → Status: "requested"
2. **Driver logs in** → Sees request in "Available Emergency Calls"
3. **Driver clicks "Accept"** → Request becomes assignment
4. **Driver updates status** → "On The Way" → "Completed"

The driver dashboard should now work properly! 🚑✅
