# 🚨 Ambulance Request Not Appearing - Debug Guide

## Issue: Patient requests ambulance but it doesn't appear in driver dashboard

This is a common issue that can have several causes. Let's debug step by step:

## 🔍 **Step 1: Verify Complete Flow**

### A) **Create a Test Booking**

1. **Login as a patient**
2. **Go to Patient Dashboard**
3. **Click "Request Ambulance"**
4. **Fill out the form:**
   - Emergency Level: High/Medium/Low
   - Address: Any address
   - Medical Notes: Optional
5. **Submit the request**
6. **Check browser console** for any errors

### B) **Check Backend Logs**

Look at your backend console for:

```
📝 Registration attempt: {...}
✅ User created successfully: ...
🔍 Fetching available bookings for driver: [Driver Name]
📊 Total bookings in database: X
📊 Booking statuses: [...]
✅ Available bookings found: Y
```

## 🔍 **Step 2: Check Database**

### A) **Test Database Connection**

```bash
curl http://localhost:5000/test-db
```

### B) **Check if Bookings Exist**

Look at the backend console logs when driver dashboard loads. You should see:

- Total bookings count
- Booking statuses
- Available bookings count

## 🔍 **Step 3: Check Driver Account**

### A) **Driver Must Be Approved**

1. **Login as admin**
2. **Go to "Manage Users" → "Drivers"**
3. **Find your driver account**
4. **Make sure it shows "Approved" status**

### B) **Driver Must Be Available**

1. **Login as driver**
2. **Check availability toggle** - should be ON
3. **Refresh the dashboard**

## 🔍 **Step 4: Check Frontend Console**

Open browser DevTools (F12) and look for:

### A) **Driver Routes Test**

```
Driver routes test: { success: true, message: "Driver routes are working", user: {...} }
```

### B) **Data Fetching**

```
📊 Assignments data: { success: true, assignments: [...], count: X }
📊 Available bookings data: { success: true, bookings: [...], count: Y }
```

### C) **Network Requests**

Check Network tab for:

- `GET /api/driver/test` - Should return 200
- `GET /api/driver/available-bookings` - Should return 200
- `GET /api/driver/assignments` - Should return 200

## 🔍 **Step 5: Common Issues & Solutions**

### Issue 1: **No Bookings in Database**

**Symptoms:** Backend logs show "Total bookings in database: 0"
**Solution:**

- Make sure patient actually submitted a request
- Check if booking creation failed
- Verify patient is logged in

### Issue 2: **Bookings Exist But Wrong Status**

**Symptoms:** Backend logs show bookings but status is not "requested"
**Solution:**

- Check booking creation process
- Verify status is set to "requested" when created

### Issue 3: **Driver Not Approved**

**Symptoms:** Driver routes test fails or returns 403
**Solution:**

- Approve driver in admin panel
- Make sure driver role is correct

### Issue 4: **Driver Not Available**

**Symptoms:** Driver is approved but no bookings show
**Solution:**

- Toggle availability to "Available"
- Refresh dashboard

## 🔧 **Quick Test Steps**

### Test 1: **Create Fresh Booking**

1. **Logout all users**
2. **Register new patient** → **Login as patient** → **Request ambulance**
3. **Register new driver** → **Login as admin** → **Approve driver**
4. **Login as approved driver** → **Check dashboard**

### Test 2: **Check Backend Logs**

When driver dashboard loads, you should see:

```
🔍 Fetching available bookings for driver: [Driver Name]
📊 Total bookings in database: 1
📊 Booking statuses: [{ id: "...", status: "requested", patient: "Patient Name" }]
✅ Available bookings found: 1
```

### Test 3: **Check Frontend Data**

In browser console, you should see:

```
📊 Available bookings data: { success: true, bookings: [{ _id: "...", status: "requested", patient: {...} }], count: 1 }
```

## 🚨 **Emergency Debug Commands**

### Check All Bookings (Backend)

Add this to your backend controller temporarily:

```javascript
const allBookings = await Booking.find().populate(
  "patient",
  "name email phone"
);
console.log("ALL BOOKINGS:", JSON.stringify(allBookings, null, 2));
```

### Check Driver Status (Frontend)

Add this to your driver dashboard:

```javascript
console.log("Current user:", user);
console.log("Is available:", isAvailable);
console.log("Available bookings:", availableBookings);
```

## 📊 **Expected Flow**

1. **Patient requests ambulance** → Status: "requested"
2. **Backend logs:** "Total bookings in database: 1"
3. **Driver logs in** → Sees "Available Emergency Calls" section
4. **Driver clicks "Accept"** → Status: "assigned"
5. **Booking moves to "Current Assignments"**

## 🐛 **Still Not Working?**

If the issue persists:

1. **Check browser console** for specific error messages
2. **Check backend console** for database queries
3. **Verify all steps** in the test flow
4. **Try creating a completely fresh booking**

The debugging logs I added will show exactly what's happening at each step! 🔍✅
