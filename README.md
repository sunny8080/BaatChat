<img width="1274" height="688" alt="image" src="https://github.com/user-attachments/assets/01dafd85-9c4e-4273-bc50-3cb88d674673" /># BaatChat — Full Stack project

BaatChat is a full-stack real-time chat application with a React frontend and Node.js backend. It supports personal and group messaging, authentication, online presence, notifications, and profile/chat management.

BaatChat is designed as a complete messaging workspace where users can sign in, find people, start private conversations, create groups, and keep chats synchronized across active sessions. The frontend handles the interactive chat experience with typed React components, protected routes, client-side stores, server-state caching, and browser notification support.

The backend exposes REST APIs for account, user, chat, group, message, upload, and notification workflows, while Socket.IO powers live message delivery, typing indicators, presence changes, unread counts, and message status updates. MongoDB stores users, chats, memberships, and messages, with support for personal-chat uniqueness, group membership history, soft-deleted chat visibility, and populated API responses.

The project is split into frontend and backend apps inside this monorepo so the full stack can be developed together, while each side can still be deployed or maintained independently.

## Live URL:

Production app: https://baatchat.online/

## GitHub Repositories

- Full-stack monorepo: https://github.com/sunny8080/BaatChat
- Frontend: https://github.com/sunny8080/BaatChat-Frontend
- Backend: https://github.com/sunny8080/BaatChat-Backend
- API Postman documentation: https://documenter.getpostman.com/view/19721099/2sBY4Jv2K2

## Tech Stack

### Frontend

- React 19
- TypeScript
- Vite
- Tailwind CSS
- React Router
- TanStack Query
- Zustand
- Socket.IO Client
- Axios
- Firebase Messaging

### Backend

- Node.js
- Express 5
- MongoDB and Mongoose
- Socket.IO
- JWT authentication
- Passport and Google auth support
- Firebase Admin
- Cloudinary and Multer
- Nodemailer, Mailtrap, and Mailgen
- Winston and Morgan logging

## Getting Started

Run the frontend and backend in separate terminals.

### Frontend

```bash
cd frontend
npm install
npm run dev
```

### Backend

```bash
cd backend
npm install
npm run dev
```

For production backend startup:

```bash
cd backend
npm start
```

Create the required environment files before starting the apps. The backend needs database, auth, email, upload, Firebase, and client URL configuration. The frontend needs API, socket, Google auth, and Firebase configuration.

## Project Structure

```text
.
├── backend/
│   ├── public/          # Static backend assets
│   ├── src/
│   │   ├── config/      # App, database, Firebase, and service configuration
│   │   ├── controllers/ # Request handlers for auth, chats, users, and uploads
│   │   ├── mails/       # Email templates and delivery helpers
│   │   ├── middlewares/ # Auth, validation, upload, and error middleware
│   │   ├── models/      # Database models
│   │   ├── routes/      # API route definitions
│   │   ├── socket/      # Socket.IO event handlers and realtime helpers
│   │   └── utils/       # Shared backend utilities
│   └── uploads/         # Runtime uploaded files
├── frontend/
│   ├── public/          # Static frontend assets
│   └── src/
│       ├── assets/      # Images, logos, and visual assets
│       ├── components/  # Reusable React UI components
│       ├── context/     # React context providers
│       ├── hooks/       # Custom React hooks
│       ├── pages/       # Route-level pages
│       ├── services/    # API and integration clients
│       ├── socket/      # Client Socket.IO setup
│       ├── types/       # Shared TypeScript types
│       ├── utils/       # Frontend helper functions
│       └── zustand/     # Client state stores
└── README.md
```

## Current Features

- User signup, login, logout, and protected routes
- Google authentication support
- Personal and group chats
- Real-time messaging with Socket.IO
- Online/offline presence and last-seen status
- Typing indicators
- Message status updates
- Push notification registration with Firebase Messaging
- User profile, username, contact, and password settings
- Profile and group avatar upload
- Group name, description, admin, member, leave, and delete flows
- Chat details panel with contact and group information
- File preview/download helpers for uploaded media

## Future Scope

- Audio calls
- Video calls (use SFU WebRTC architecture) (use mediasoup library)
- Screen Sharing
- Files section
- Reply to a message
- Reactions to chats/messages
- Implement Bloom Filter for checking username availability
- use Redis to store online users data
- Fetch contacts from gmail if user registered using gmail
- Provide feature to signup/login with Facebook, and fetch their facebook friends
- Update sent/delivered/read receipt
- Offline support (use Cached API and Indexed DB)
- update websocket to send and receive data in buffer/binary format instead of json format

## Preview Screenshots -
<img width="3024" height="1790" alt="image" src="https://github.com/user-attachments/assets/22de7c15-4a97-45ac-9444-c63455516f22" />

<img width="3024" height="1812" alt="image" src="https://github.com/user-attachments/assets/1945f990-edfc-4643-b244-a74bd1132b0c" />

<img width="3024" height="1804" alt="image" src="https://github.com/user-attachments/assets/844606c1-11da-42a8-8947-b0f83180e94a" />

<img width="3024" height="1782" alt="image" src="https://github.com/user-attachments/assets/178e57c8-5846-4a87-ad26-e0570d9cf18e" />

<img width="896" height="1398" alt="image" src="https://github.com/user-attachments/assets/c7f77e78-f737-4e20-b5f1-7db81ef7f65c" />

<img width="1138" height="1406" alt="image" src="https://github.com/user-attachments/assets/9d3aacd8-d6c7-41aa-8cd9-56ad5d19187e" />

<img width="3012" height="1782" alt="image" src="https://github.com/user-attachments/assets/0fbf3942-3e23-4fe8-8e04-7c7002082f08" />


