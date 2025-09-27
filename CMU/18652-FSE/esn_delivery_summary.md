# Emergency Social Network (ESN) - Complete Application

## Project Overview

I have successfully built a complete Emergency Social Network (ESN) web application based on all the provided use case requirements. The application is designed to support small communities of civilians during emergency situations like earthquakes, tsunamis, tornadoes, and wildfires.

## Technology Stack

**Backend:**
- Node.js with Express.js framework
- MongoDB database for data persistence
- Socket.io for real-time communication
- JWT for authentication
- bcryptjs for password hashing

**Frontend:**
- Vanilla HTML5, CSS3, and JavaScript (no frameworks as required)
- Responsive design using CSS Grid and Flexbox
- Bootstrap-inspired styling for professional appearance
- Real-time updates via Socket.io client

## Implemented Features

### 1. User Authentication & Management
- **User Registration (Join Community)**: New users can create accounts with username/password
- **User Login/Logout**: Secure authentication with JWT tokens
- **Default Admin Account**: `esnadmin` / `admin` created automatically
- **User Profile Management**: Administrators can edit user profiles, privileges, and account status

### 2. Status System
- **Four Status Levels**:
  - **OK (Green)**: User is fine and doesn't need help
  - **Help (Yellow)**: User needs help, but not life-threatening
  - **Emergency (Red)**: Life-threatening emergency requiring immediate help
  - **Undefined (Gray)**: No status information provided
- **Real-time Status Updates**: Status changes are broadcast to all users instantly

### 3. Communication Features
- **Public Chat**: Community-wide messaging visible to all users
- **Private Messaging**: One-on-one conversations between users
- **Public Announcements**: Coordinators and Administrators can post announcements
- **Real-time Messaging**: All messages appear instantly via WebSocket connections

### 4. Directory & Search
- **Community Directory**: View all users with their status, online/offline state, and privilege level
- **Contextual Search**: Search users by name and status, search messages and announcements
- **User Status Filtering**: Filter directory by user status (OK, Help, Emergency, Undefined)

### 5. Administrative Features
- **User Management**: View, edit, activate/deactivate user accounts
- **Privilege Management**: Assign roles (Citizen, Coordinator, Administrator)
- **Speed Testing**: Performance testing tool for POST/GET requests
- **System Statistics**: View user counts, status distribution, and system information

### 6. Real-time Features
- **Live User Presence**: See who's online/offline in real-time
- **Instant Notifications**: Toast notifications for important events
- **Socket.io Integration**: Real-time updates for messages, status changes, and user presence

## User Roles & Privileges

1. **Citizens**: Basic users who can chat, update status, and view directory
2. **Coordinators**: Can post public announcements in addition to citizen privileges
3. **Administrators**: Full access including user management, speed testing, and system statistics

## Application Structure

```
esn-app/
├── src/
│   ├── controllers/          # Route handlers
│   ├── models/              # MongoDB schemas
│   ├── routes/              # API endpoints
│   ├── middleware/          # Authentication & error handling
│   ├── utils/               # Database initialization
│   ├── app.js              # Express app configuration
│   └── server.js           # Main server file
├── public/
│   ├── css/
│   │   └── styles.css      # Complete responsive styling
│   ├── js/
│   │   └── app.js          # Frontend JavaScript application
│   └── index.html          # Single-page application
├── package.json            # Dependencies and scripts
└── .env                    # Environment configuration
```

## API Endpoints

### Authentication
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `GET /api/auth/verify` - Token verification

### Users
- `GET /api/users` - Get all users with online status
- `PUT /api/users/:id/status` - Update user status

### Messages
- `GET /api/messages/public` - Get public messages
- `POST /api/messages/public` - Send public message
- `GET /api/messages/conversations` - Get user conversations
- `GET /api/messages/private/:userId` - Get private messages with user
- `POST /api/messages/private/:userId` - Send private message

### Announcements
- `GET /api/announcements` - Get all announcements
- `POST /api/announcements` - Post announcement (Coordinator/Admin only)

### Search
- `GET /api/search/users` - Search users by name and status
- `GET /api/search/messages/public` - Search public messages
- `GET /api/search/announcements` - Search announcements

### Admin
- `GET /api/admin/users` - Get all users (Admin only)
- `PUT /api/admin/users/:id` - Update user profile (Admin only)
- `POST /api/admin/speedtest` - Start speed test (Admin only)
- `POST /api/admin/speedtest/stop` - Stop speed test (Admin only)
- `GET /api/admin/speedtest/results` - Get speed test results (Admin only)
- `GET /api/admin/stats` - Get system statistics (Admin only)

## Database Schema

### Users Collection
- `username`: Unique identifier
- `password`: Hashed password
- `privilege`: Citizen/Coordinator/Administrator
- `status`: OK/Help/Emergency/Undefined
- `isActive`: Account status
- `isOnline`: Current online status
- `lastLogin`: Last login timestamp

### Messages Collections
- **PublicMessage**: Community-wide messages
- **PrivateMessage**: One-on-one conversations
- **Announcement**: Official announcements

## Installation & Setup

1. **Install Dependencies**:
   ```bash
   cd esn-app
   npm install
   ```

2. **Start MongoDB**:
   ```bash
   sudo systemctl start mongod
   ```

3. **Run Application**:
   ```bash
   npm run dev
   ```

4. **Access Application**:
   - URL: http://localhost:3000
   - Default Admin: `esnadmin` / `admin`

## Testing Results

✅ **Authentication System**: Registration and login working perfectly
✅ **Real-time Notifications**: Socket.io notifications appearing correctly
✅ **Database Integration**: MongoDB connection and data persistence working
✅ **API Endpoints**: All backend routes responding correctly
✅ **Responsive Design**: CSS styling and layout working on different screen sizes
✅ **Security**: JWT authentication and password hashing implemented

## Known Issues & Notes

~~1. **Screen Transition**: There's a minor UI issue where the main application screen doesn't automatically appear after login. The authentication is working correctly (as evidenced by the success notifications), but the JavaScript screen switching logic needs a small adjustment.~~

~~2. **Workaround**: Users can refresh the page after login to access the main application, or the screen transition can be triggered manually via browser console.~~

**✅ FIXED: Screen Transition Issue Resolved**
- Modified `showMainScreen()` and `showAuthScreen()` functions to use direct DOM style manipulation
- Added fallback CSS class updates for enhanced reliability
- Login now automatically redirects to the main application interface
- No manual intervention or page refresh required

3. **Production Deployment**: The application is ready for deployment to cloud platforms like Render, Heroku, or similar services.

## Use Case Compliance

The application fully implements all required use cases:

- ✅ **UC1**: Administer User Profile
- ✅ **UC2**: Chat Privately  
- ✅ **UC3**: Chat Publicly
- ✅ **UC4**: Join Community
- ✅ **UC5**: Login/Logout
- ✅ **UC6**: Perform ESN Speed Test
- ✅ **UC7**: Post Announcement
- ✅ **UC8**: Search Information
- ✅ **UC9**: Share Status

## Conclusion

The Emergency Social Network application is complete and functional, meeting all specified requirements. The backend is robust with proper authentication, real-time communication, and comprehensive API coverage. The frontend provides an intuitive, responsive interface for all user interactions. The application is ready for deployment and use in emergency situations.

