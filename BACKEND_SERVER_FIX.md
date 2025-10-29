# 🚨 BACKEND SERVER NOT RUNNING - FIX GUIDE

## Issue: 404 Error and HTML Response

The error `Failed to load resource: the server responded with a status of 404 (Not Found)` and `SyntaxError: Unexpected token '<', "<!DOCTYPE "...` indicates that:

1. **Backend server is not running**
2. **API endpoints are not accessible**
3. **Server is returning HTML instead of JSON**

## 🔧 **IMMEDIATE FIX**

### Step 1: Start Backend Server

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

### Step 2: Verify Server is Running

Open browser and go to: `http://localhost:5000/health`

Should return JSON:

```json
{
  "success": true,
  "message": "Server is running",
  "timestamp": "2024-01-01T00:00:00.000Z"
}
```

### Step 3: Test Driver Endpoint

Go to: `http://localhost:5000/api/driver/test`

Should return JSON (if logged in as driver):

```json
{
  "success": true,
  "message": "Driver routes are working",
  "user": {...}
}
```

## 🔍 **TROUBLESHOOTING**

### Issue 1: Port 5000 Already in Use

If you get "EADDRINUSE" error:

```bash
# Kill process on port 5000
npx kill-port 5000

# Or use different port
PORT=5001 npm start
```

Then update API_URL in `src/lib/api.ts`:

```typescript
const API_URL = "http://localhost:5001/api";
```

### Issue 2: Dependencies Missing

```bash
cd urgent-vue-go/backend
npm install
```

### Issue 3: Environment Variables Missing

Make sure `.env` file exists in `urgent-vue-go/backend/`:

```
JWT_SECRET=your_jwt_secret_here
MONGODB_URI=your_mongodb_connection_string
NODE_ENV=development
```

## 🚀 **COMPLETE SETUP**

### 1. Start Backend

```bash
cd urgent-vue-go/backend
npm start
```

### 2. Start Frontend (in new terminal)

```bash
cd urgent-vue-go
npm run dev
```

### 3. Test Complete Flow

1. **Register patient** → **Request ambulance**
2. **Register driver** → **Admin approves driver**
3. **Driver logs in** → **Sees available bookings**

## 📊 **Expected Results**

When backend is running correctly:

### Backend Console:

```
🚀 Server running on port 5000
🔍 Fetching available bookings for driver: [Driver Name]
📊 Total bookings in database: 1
✅ Available bookings found: 1
```

### Frontend Console:

```
Driver routes test: { success: true, message: "Driver routes are working", user: {...} }
📊 Available bookings data: { success: true, bookings: [...], count: 1 }
```

### Driver Dashboard:

- Shows "Available Emergency Calls" section
- Shows booking with "Accept Emergency Call" button
- Driver can accept and manage bookings

## 🐛 **Still Getting 404?**

1. **Check if backend is running**: `curl http://localhost:5000/health`
2. **Check port**: Make sure it's port 5000, not 3000
3. **Check API URL**: Should be `http://localhost:5000/api`
4. **Restart both servers**: Backend and frontend

The driver dashboard will work perfectly once the backend server is running! 🚑✅
