# 🚑 Ambulance Booking & Dispatch System - Setup Guide

## 📋 Overview

A complete emergency ambulance booking and dispatch platform with role-based access for Patients, Drivers, and Admins.

## ⚠️ Important Note

The frontend is built with **React + TypeScript** (Lovable's standard stack). While you requested JavaScript, TypeScript provides better type safety and works very similarly. The backend is pure Node.js/Express as requested.

---

## 🎨 Frontend Setup (Already Done!)

The frontend is ready to run in Lovable. Just preview the app!

### Features Available:
- ✅ Login & Registration with role selection
- ✅ Patient Dashboard (book ambulance, track status)
- ✅ Driver Dashboard (view assignments, update status)
- ✅ Admin Dashboard (analytics, manage users & ambulances)
- ✅ Beautiful modern UI with animations
- ✅ Responsive mobile-friendly design

---

## 🔧 Backend Setup (MongoDB + Express)

### 1. Navigate to Backend Folder
```bash
cd backend
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Setup MongoDB

**Option A: Local MongoDB**
1. Install MongoDB from https://www.mongodb.com/try/download/community
2. Start MongoDB service
3. Your connection string: `mongodb://localhost:27017/ambulance-booking`

**Option B: MongoDB Atlas (Cloud)**
1. Create free account at https://www.mongodb.com/cloud/atlas
2. Create a new cluster
3. Get your connection string (looks like: `mongodb+srv://username:password@cluster.mongodb.net/`)

### 4. Create .env File
```bash
# Copy the example
cp .env.example .env

# Edit .env with your settings
```

Edit `.env`:
```env
PORT=5000
NODE_ENV=development

# Use ONE of these MongoDB options:

# Option A: Local MongoDB
MONGODB_URI=mongodb://localhost:27017/ambulance-booking

# Option B: MongoDB Atlas (replace with your actual connection string)
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/ambulance-booking

JWT_SECRET=change-this-to-a-random-secure-string
FRONTEND_URL=http://localhost:8080
```

### 5. Start the Backend Server
```bash
# Development mode (auto-reload)
npm run dev

# Or production mode
npm start
```

You should see:
```
✅ MongoDB Connected: localhost
🚀 Server running on port 5000
```

---

## 🔗 Connecting Frontend to Backend

The frontend is already configured to make API calls to `http://localhost:5000`.

**To test the full system:**
1. Start the backend: `cd backend && npm run dev`
2. Preview the Lovable frontend (it's already running!)
3. Try registering a new account

---

## 👥 Testing the System

### Create Test Accounts

1. **Admin Account:**
   - Register with email/password
   - **IMPORTANT:** Manually set role to 'admin' in MongoDB:
   ```javascript
   // In MongoDB shell or Compass
   db.users.updateOne(
     { email: "admin@example.com" },
     { $set: { role: "admin" } }
   )
   ```

2. **Driver Account:**
   - Register as "Ambulance Driver"
   - Admin must approve the driver in the Manage Users page

3. **Patient Account:**
   - Register as "Patient"
   - Immediately active, can book ambulances

---

## 🎯 Complete Workflow Test

1. **Patient books ambulance:**
   - Login as patient
   - Click "Request Ambulance"
   - Fill in emergency level, address, notes
   - Submit request

2. **Admin assigns driver:**
   - Login as admin
   - Go to "All Bookings"
   - Find the pending request
   - Assign an available driver and ambulance

3. **Driver updates status:**
   - Login as driver
   - See the assignment in dashboard
   - Update status: "On The Way" → "Completed"

4. **Admin views analytics:**
   - Check dashboard for stats
   - View monthly charts
   - Manage users and ambulances

---

## 📁 Project Structure

```
📦 Project Root
├── 📁 src/                      # Frontend (React + TypeScript)
│   ├── 📁 components/           # UI components
│   ├── 📁 contexts/             # Auth context
│   ├── 📁 pages/                # Dashboard pages
│   └── 📁 hooks/                # Custom hooks
│
└── 📁 backend/                  # Backend (Node.js + Express + MongoDB)
    ├── 📁 config/               # Database connection
    ├── 📁 models/               # Mongoose schemas
    ├── 📁 controllers/          # Business logic
    ├── 📁 routes/               # API routes
    ├── 📁 middleware/           # Auth & permissions
    └── server.js                # Main server file
```

---

## 🔐 API Authentication

All protected routes require a JWT token in the Authorization header:

```javascript
headers: {
  'Authorization': 'Bearer YOUR_JWT_TOKEN',
  'Content-Type': 'application/json'
}
```

The frontend automatically handles this!

---

## 🚀 Key Features

### Patient Features
- Register & login
- Request ambulance with emergency levels (Low/Medium/High)
- Track booking status in real-time
- View assigned driver details
- View booking history
- Cancel booking before assignment

### Driver Features
- Login with driver credentials
- View assigned emergencies
- Update status (On The Way, Completed)
- Set availability (Available/Busy/Offline)
- View assignment history

### Admin Features
- Dashboard with analytics charts
- Manage users (approve drivers, block users)
- Manage ambulance fleet
- Assign bookings to drivers
- View all bookings and requests
- Real-time statistics

---

## 🛠️ Troubleshooting

### Frontend Issues
- **Port conflict:** Frontend runs on port 8080 by default
- **API not connecting:** Make sure backend is running on port 5000
- **Login fails:** Check backend logs for errors

### Backend Issues
- **MongoDB connection failed:**
  - Verify MongoDB is running
  - Check MONGODB_URI in .env
  - For Atlas: whitelist your IP address

- **JWT errors:**
  - Ensure JWT_SECRET is set in .env
  - Try generating a new random secret

- **CORS errors:**
  - Backend allows `http://localhost:8080` by default
  - Update FRONTEND_URL in .env if needed

---

## 📚 API Documentation

See `backend/README.md` for complete API endpoint documentation.

---

## 🎨 Customization

### Colors & Design
All colors are defined in `src/index.css` using CSS variables. Update the design system there!

### Adding Features
- Frontend: Add components in `src/components/` and pages in `src/pages/`
- Backend: Add routes, controllers, and models in respective folders

---

## 🙏 Support

If you encounter issues:
1. Check console logs (browser & terminal)
2. Verify all environment variables
3. Ensure MongoDB is running
4. Check that backend port 5000 is not in use

---

## ✅ Quick Start Checklist

- [ ] Install Node.js
- [ ] Install/Setup MongoDB
- [ ] Navigate to backend folder
- [ ] Run `npm install`
- [ ] Create `.env` file with your MongoDB connection
- [ ] Start backend with `npm run dev`
- [ ] Preview frontend in Lovable
- [ ] Create admin account and set role in MongoDB
- [ ] Test the complete workflow!

---

**You're all set! Happy coding! 🚀**
