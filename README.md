# Campus Event Hub

A full-stack event management system for campus events with team registration, bookmarks, and society management features.

## Prerequisites

- Node.js (v14 or higher)
- MongoDB (local or Atlas)
- Git

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/Honest-Observer/Campus-Event-Hub.git
cd Campus-Event-Hub
```

### 2. Backend Setup

```bash
cd backend
npm install
```

Create a `.env` file in the `backend` folder with:

```
MONGODB_URI=mongodb://localhost:27017/campus-event-hub
JWT_SECRET=your_secret_key_here
PORT=5000
```

Start the backend server:

```bash
npm start
```

Backend will run on `http://localhost:5000`

### 3. Frontend Setup

Open a new terminal and navigate to frontend:

```bash
cd frontend
npm install
```

Create a `.env` file in the `frontend` folder with:

```
VITE_API_URL=http://localhost:5000/api
```

Start the frontend:

```bash
npm run dev
```

Frontend will run on `http://localhost:5173`

## Default Login

After setting up, create an account through the registration page. To test as a society head:

1. Register a new account
2. Go to MongoDB and change the user's `role` field from `student` to `society-head`
3. Login again to access event creation features

## Features

- **Students**: Browse events, register with teams, bookmark events, manage invitations
- **Society Heads**: Create events, manage registrations, customize registration forms
- **Team System**: Invite members, accept/decline invitations, manage team after registration
- **Bookmarks**: Save events for later viewing

## Tech Stack

- **Frontend**: React, Vite, TailwindCSS
- **Backend**: Node.js, Express
- **Database**: MongoDB
- **Authentication**: JWT

## Troubleshooting

- If MongoDB connection fails, ensure MongoDB is running locally or use MongoDB Atlas URI
- If ports are in use, change PORT in backend `.env` and VITE_API_URL in frontend `.env`
- Clear browser cache if you face login issues
