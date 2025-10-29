# Quick Start Guide - Fixed Version

## Setup Instructions

### 1. Setup Backend Environment

```bash
cd urgent-vue-go/backend
node setup-env.js
```

This creates the `.env` file with a secure JWT secret.

### 2. Update MongoDB Connection (if needed)

Edit `backend/.env` and update `MONGODB_URI`:

```
MONGODB_URI=mongodb://localhost:27017/ambulance-booking
```

For MongoDB Atlas (cloud):

```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/ambulance-booking
```

### 3. Start Backend Server

```bash
cd urgent-vue-go/backend
npm install  # if not already installed
npm start
```

Backend runs on http://localhost:5000

### 4. Start Frontend Server

```bash
cd urgent-vue-go
npm install  # if not already installed
npm run dev
```

Frontend runs on http://localhost:8080

## What Was Fixed

✅ **Login Error**: Missing JWT_SECRET environment variable - Now auto-generated  
✅ **Data Not Persisting**: Frontend was using local state only - Now saves to MongoDB  
✅ **Data Disappearing**: Data was in memory only - Now fetched from database on reload

## Testing

1. Go to http://localhost:8080/register
2. Create a new account
3. Login with your credentials
4. Click "Request Ambulance" and create a booking
5. Refresh the page (F5) - Booking should still be there!
6. Open MongoDB Compass and check the `bookings` collection - Data is saved!

## Troubleshooting

**Backend won't start:**

```bash
# Check if .env exists
ls backend/.env

# If not, create it
cd backend
node setup-env.js
```

**"Cannot connect to database" error:**

- Make sure MongoDB is running
- Update MONGODB_URI in backend/.env with correct connection string

**"Login failed" after registration:**

- Make sure backend/.env file exists
- Restart backend server: `npm start`
- Check backend console for errors

**Bookings not showing:**

- Open browser DevTools (F12) → Console tab
- Look for API errors
- Check Network tab to see if requests are being made

## Files Changed

- ✅ `src/lib/api.ts` - New API helper
- ✅ `src/pages/PatientDashboard.tsx` - Now uses database
- ✅ `src/contexts/AuthContext.tsx` - Updated to use API
- ✅ `backend/setup-env.js` - Environment setup script
- ✅ `backend/.env.example` - Template for environment variables
- ✅ `backend/ENV_SETUP.md` - Setup documentation
- ✅ `FIXES_APPLIED.md` - Detailed list of all fixes

## Next Steps

1. Set up MongoDB (local or Atlas)
2. Run `node setup-env.js` in backend folder
3. Start both servers
4. Test the application!

For detailed information, see `FIXES_APPLIED.md`
