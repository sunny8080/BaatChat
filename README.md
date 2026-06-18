# BaatChat — Full Project Plan

## Tech Stack
| Layer | Technology |
|---|---|
| Frontend | React 18, Vite, TailwindCSS, React Router v6 |
| Backend | Node.js, Express.js |
| Database | MongoDB + Mongoose |
| Realtime | Socket.io |
| Video/Audio | WebRTC (peer-to-peer) |
| Auth | JWT + Google OAuth + Facebook OAuth |
| File Uploads | Cloudinary (images, files, voice) |
| Push Notifications | Web Push API + service worker |
| Deployment (FE) | Vercel |
| Deployment (BE) | Railway or Render |

---

## Folder Structure

### Frontend (`/client`)
```
client/
├── public/
│   ├── favicon.svg
│   └── manifest.json
├── src/
│   ├── assets/              # Logo, icons
│   ├── components/
│   │   ├── ui/              # Button, Input, Avatar, Badge, Modal
│   │   ├── chat/            # ChatWindow, MessageBubble, MessageInput, EmojiPicker
│   │   ├── sidebar/         # ConversationList, ConversationItem, SearchBar
│   │   ├── call/            # VideoTile, CallControls, CallTimer
│   │   └── shared/          # Navbar, Loader, ProtectedRoute, Toasts
│   ├── pages/
│   │   ├── Home.jsx         # Landing page / Chat Dashboard
│   │   ├── Auth.jsx         # Sign in / Sign up
│   │   ├── Call.jsx         # Call screen
│   │   ├── Settings.jsx     # Profile settings
│   │   ├── CallHistory.jsx  # Call logs
│   │   └── ResetPassword.jsx
│   ├── hooks/
│   │   ├── useSocket.js
│   │   ├── useWebRTC.js
│   │   ├── useAuth.js
│   │   └── useMediaStream.js
│   ├── context/
│   │   ├── AuthContext.jsx
│   │   ├── SocketContext.jsx
│   │   └── CallContext.jsx
│   ├── store/               # Zustand stores
│   │   ├── chatStore.js
│   │   ├── callStore.js
│   │   └── userStore.js
│   ├── services/            # Axios API calls
│   │   ├── authService.js
│   │   ├── chatService.js
│   │   ├── callService.js
│   │   └── userService.js
│   ├── utils/
│   │   ├── formatTime.js
│   │   ├── formatFileSize.js
│   │   └── constants.js
│   ├── App.jsx
│   └── main.jsx
```

### Backend (`/server`)
```
server/
├── config/
│   ├── db.js                # MongoDB connection
│   ├── cloudinary.js
│   └── passport.js          # Google + Facebook OAuth
├── controllers/
│   ├── authController.js
│   ├── userController.js
│   ├── chatController.js
│   ├── messageController.js
│   └── callController.js
├── middleware/
│   ├── auth.js              # JWT verify middleware
│   ├── upload.js            # Multer + Cloudinary
│   └── errorHandler.js
├── models/
│   ├── User.js
│   ├── Chat.js
│   ├── Message.js
│   └── Call.js
├── routes/
│   ├── authRoutes.js
│   ├── userRoutes.js
│   ├── chatRoutes.js
│   ├── messageRoutes.js
│   └── callRoutes.js
├── socket/
│   ├── index.js             # Socket.io init
│   ├── chatHandlers.js      # message, typing, read
│   ├── callHandlers.js      # offer, answer, ICE
│   └── presenceHandlers.js  # online/offline
├── utils/
│   ├── generateToken.js
│   ├── sendEmail.js         # Reset password email
│   └── webpush.js
└── server.js
```

---

## MongoDB Models

### User
```js
{ username, name, email, phone, password (hashed),
  avatar, status, isOnline, lastSeen,
  googleId, facebookId,
  pushSubscription, resetToken, resetTokenExpiry }
```

### Chat
```js
{ isGroup, name, avatar, members: [userId],
  admins: [userId], lastMessage, lastActivity }
```

### Message
```js
{ chat, sender, type: [text|image|file|audio|voice],
  content, fileUrl, fileName, fileSize,
  readBy: [userId], deletedFor: [userId],
  isDeleted, createdAt }
```

### Call
```js
{ caller, receiver, chat,
  type: [audio|video|audio+video],
  status: [missed|answered|rejected|ended],
  startedAt, endedAt, duration }
```

---

## Pages Breakdown

### 1. Home Page (`/`)
- **Logged out:** Landing page with hero, features, CTA
- **Logged in:** Full chat dashboard
  - Left sidebar: search, conversation list, new chat button
  - Center: active chat window (messages, input, media)
  - Right: contact info panel (optional, collapsible)

### 2. Auth Page (`/auth`)
- Default → `/auth?mode=signin`
- Sign In: username/password, Google, Facebook
- Sign Up: username, name, email, phone, password + social auth
- Animated toggle between signin/signup
- Form validation + error toasts

### 3. Call Screen (`/call/:id`)
- Full screen video tiles (self + remote)
- Controls: mute mic, toggle camera, screen share, end call
- Audio-only mode: avatar shown instead of video
- Call timer
- Incoming call overlay (ring + accept/reject)

### 4. Settings (`/settings`)
- Update avatar (upload to Cloudinary)
- Update name, status message
- Change password (old → new)
- Notification preferences
- Account danger zone (delete account)

### 5. Call History (`/calls`)
- List of all past calls
- Filter: All / Missed / Incoming / Outgoing
- Icons for audio vs video calls
- Tap to call back

### 6. Reset Password (`/reset-password/:resetToken`)
- New password + confirm password
- Token validation on load
- Success → redirect to signin

---

## Socket.io Events

### Chat
| Event | Direction | Purpose |
|---|---|---|
| `join:chat` | Client → Server | Join a chat room |
| `send:message` | Client → Server | Send a message |
| `receive:message` | Server → Client | Receive new message |
| `typing:start` | Client → Server | User started typing |
| `typing:stop` | Client → Server | User stopped typing |
| `message:read` | Client → Server | Mark messages as read |
| `message:delete` | Client → Server | Delete / unsend message |

### Calls (WebRTC Signaling)
| Event | Direction | Purpose |
|---|---|---|
| `call:initiate` | Client → Server | Start a call |
| `call:incoming` | Server → Client | Notify receiver |
| `call:accept` | Client → Server | Accept call |
| `call:reject` | Client → Server | Reject call |
| `call:end` | Client → Server | End call |
| `webrtc:offer` | Client → Server | SDP offer |
| `webrtc:answer` | Client → Server | SDP answer |
| `webrtc:ice` | Client → Server | ICE candidate |

### Presence
| Event | Direction | Purpose |
|---|---|---|
| `user:online` | Server → Client | User came online |
| `user:offline` | Server → Client | User went offline |

---

## REST API Endpoints

### Auth
```
POST   /api/auth/signup
POST   /api/auth/signin
POST   /api/auth/signout
GET    /api/auth/google
GET    /api/auth/facebook
POST   /api/auth/forgot-password
POST   /api/auth/reset-password/:token
POST   /api/auth/refresh-token
```

### Users
```
GET    /api/users/me
PUT    /api/users/me
PUT    /api/users/me/avatar
GET    /api/users/search?q=
```

### Chats
```
GET    /api/chats
POST   /api/chats               # create 1-on-1
POST   /api/chats/group         # create group
GET    /api/chats/:id
PUT    /api/chats/:id           # update group info
DELETE /api/chats/:id/leave     # leave group
```

### Messages
```
GET    /api/messages/:chatId
POST   /api/messages/:chatId
DELETE /api/messages/:messageId
```

### Calls
```
GET    /api/calls
POST   /api/calls
PATCH  /api/calls/:id
```

---

## Key npm Packages

### Frontend
```
react-router-dom, axios, socket.io-client,
zustand, tailwindcss, framer-motion,
emoji-picker-react, react-hook-form,
react-dropzone, date-fns, react-hot-toast
```

### Backend
```
express, mongoose, socket.io,
jsonwebtoken, bcryptjs, passport,
passport-google-oauth20, passport-facebook,
multer, cloudinary, nodemailer,
web-push, cors, dotenv, helmet, morgan
```

---

## Environment Variables

### Frontend (`.env`)
```
VITE_API_URL=https://your-backend.railway.app
VITE_SOCKET_URL=https://your-backend.railway.app
VITE_GOOGLE_CLIENT_ID=
VITE_FACEBOOK_APP_ID=
```

### Backend (`.env`)
```
PORT=5000
MONGO_URI=
JWT_SECRET=
JWT_REFRESH_SECRET=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
FACEBOOK_APP_ID=
FACEBOOK_APP_SECRET=
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
EMAIL_HOST=
EMAIL_USER=
EMAIL_PASS=
CLIENT_URL=https://baatchat.online
VAPID_PUBLIC_KEY=
VAPID_PRIVATE_KEY=
```

---

## Development Order (Recommended)

1. Project setup (Vite + Tailwind + Express + MongoDB)
2. Auth system (JWT + Google + Facebook)
3. User model + settings page
4. Chat model + conversation list + 1-on-1 messaging
5. Socket.io real-time messaging
6. Group chat
7. File / image / voice message uploads
8. WebRTC calls (audio → video → screen share)
9. Call history page
10. Push notifications
11. Landing page (Home logged out)
12. Polish + deployment# BaatChat
