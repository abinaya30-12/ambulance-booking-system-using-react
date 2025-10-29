# 🚨 MongoDB Connection Fix Guide

## Issue: `querySrv ECONNREFUSED _mongodb._tcp.cluster0.m4yexej.mongodb.net`

This error means your backend can't connect to MongoDB Atlas. Here's how to fix it:

## 🔧 **Step 1: Check MongoDB Atlas Dashboard**

1. **Go to**: https://cloud.mongodb.com/
2. **Login** with your credentials
3. **Select your project** and cluster
4. **Check cluster status** - should be "Running"

## 🔧 **Step 2: Verify Connection String**

1. **Click "Connect"** on your cluster
2. **Choose "Connect your application"**
3. **Copy the connection string**
4. **Make sure it matches** what's in your `.env` file

## 🔧 **Step 3: Check Network Access (IP Whitelist)**

1. **Go to "Network Access"** in left sidebar
2. **Click "Add IP Address"**
3. **Add your current IP** or use `0.0.0.0/0` for development
4. **Save changes**

## 🔧 **Step 4: Check Database User**

1. **Go to "Database Access"** in left sidebar
2. **Make sure user `arjunpandi` exists**
3. **Check password** - make sure it's correct
4. **Verify user has read/write permissions**

## 🔧 **Step 5: Test Connection**

### Option A: Test in MongoDB Atlas

1. **Go to "Browse Collections"**
2. **Try to view your database**
3. **Should show collections if connected**

### Option B: Test with MongoDB Compass

1. **Download MongoDB Compass**
2. **Connect using your connection string**
3. **Should connect successfully**

## 🔧 **Step 6: Alternative - Use Local MongoDB**

If Atlas continues to have issues, use local MongoDB:

### Install MongoDB locally:

```bash
# Windows (using Chocolatey)
choco install mongodb

# Or download from: https://www.mongodb.com/try/download/community
```

### Update .env file:

```env
JWT_SECRET=your_jwt_secret_here
MONGODB_URI=mongodb://localhost:27017/ambulance-booking
NODE_ENV=development
```

## 🔧 **Step 7: Restart Backend**

After fixing the connection:

```bash
cd urgent-vue-go/backend
# Stop server (Ctrl+C)
npm start
```

Should see:

```
✅ MongoDB Connected: cluster0-shard-00-02.m4yexej.mongodb.net
📅 Connected at: 2025-10-28T03:35:20.231Z
🗄️ Database: ambulance-booking
```

## 🚨 **Common Issues**

| Issue                   | Solution                          |
| ----------------------- | --------------------------------- |
| IP not whitelisted      | Add your IP to Network Access     |
| Wrong password          | Reset password in Database Access |
| Cluster paused          | Resume cluster in Atlas dashboard |
| Network timeout         | Check internet connection         |
| Wrong connection string | Copy fresh string from Atlas      |

## 🧪 **Test After Fix**

Once connected, test the driver dashboard:

1. **Register a patient** → **Request ambulance**
2. **Register a driver** → **Admin approves driver**
3. **Driver logs in** → **Should see available bookings**

The driver dashboard will work perfectly once MongoDB is connected! 🚑✅
