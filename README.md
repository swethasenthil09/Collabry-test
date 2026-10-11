# CYNEO — Full Stack Social Entertainment App

## Tech Stack
- **Frontend**: React 18 + React Router v6
- **Backend**: Java 17 + Spring Boot 3.2
- **Database**: MySQL 8
- **Auth**: JWT (Bearer tokens)
- **Real-time**: STOMP over SockJS (WebSocket)
- **Movie Data**: OMDb API

---

## Project Structure
```
cyneo/
├── database/
│   └── schema.sql              ← Run this first
├── backend/                    ← Spring Boot Maven project
│   ├── pom.xml
│   └── src/main/java/com/cyneo/
│       ├── model/              ← JPA Entities (12 models)
│       ├── repository/         ← Spring Data JPA (11 repos)
│       ├── dto/                ← Request/Response DTOs
│       ├── security/           ← JWT filter + UserDetailsService
│       ├── config/             ← SecurityConfig (CORS, JWT)
│       ├── service/            ← Business logic (7 services)
│       ├── controller/         ← REST endpoints (7 controllers)
│       └── websocket/          ← STOMP WebSocket config + handler
└── frontend/                   ← React app
    └── src/
        ├── context/            ← AuthContext
        ├── services/           ← API helpers + WebSocket + movie data
        ├── components/
        │   ├── Shared/         ← Navbar, Sidebar, BottomNav
        │   ├── Home/           ← MovieCard, MovieRow
        │   └── Feed/           ← PostCard, CreatePost
        └── pages/              ← All 9 pages
```

---

## Setup Instructions

### 1. Database
```sql
-- In MySQL client:
source database/schema.sql
```

### 2. Backend
Edit `backend/src/main/resources/application.properties`:
```properties
spring.datasource.password=YOUR_MYSQL_PASSWORD
jwt.secret=YOUR_SECRET_KEY_MIN_32_CHARS
```

```bash
cd backend
mvn spring-boot:run
# Runs on http://localhost:8080
```

### 3. Frontend
Edit `frontend/src/services/api.js`:
```js
const OMDB_KEY = 'YOUR_OMDB_API_KEY';
// Get a free key at: https://www.omdbapi.com/apikey.aspx
```

```bash
cd frontend
npm install
npm start
# Runs on http://localhost:3000
```

---

## API Endpoints

### Auth
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/auth/register | Register new user |
| POST | /api/auth/login | Login |

### Users
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/users/me | Get current user |
| GET | /api/users/:username | Get profile |
| PUT | /api/users/me | Update profile |
| DELETE | /api/users/me | Delete account |
| POST | /api/users/:id/follow | Toggle follow |
| GET | /api/users/search?q= | Search users |

### Posts
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/posts/feed | Get feed |
| GET | /api/posts/reels | Get reels |
| POST | /api/posts | Create post (multipart) |
| DELETE | /api/posts/:id | Delete post |
| POST | /api/posts/:id/like | Toggle like |
| GET | /api/posts/:id/comments | Get comments |
| POST | /api/posts/:id/comments | Add comment |

### Messages
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/messages/inbox | Get inbox |
| GET | /api/messages/conversation/:userId | Get chat |
| POST | /api/messages | Send message |

### Communities
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/communities | List all |
| POST | /api/communities | Create hive |
| POST | /api/communities/:id/join | Toggle join |
| GET | /api/communities/:id/channels | Get channels |
| POST | /api/communities/:id/channels | Create channel |

### WebSocket
```
WS endpoint: /ws (SockJS)
Send DM:     /app/chat.private
Send to channel: /app/chat.channel.{channelId}
Receive DM:  /user/{userId}/queue/messages
Receive channel: /topic/channel.{channelId}
```

---

## Features
- ✅ JWT Authentication (register/login/logout)
- ✅ 3-step signup with avatar + genre selection
- ✅ Netflix-style home with OMDb-powered genre rows
- ✅ Movie detail with YouTube trailer embed
- ✅ Like/watchlist/report on movies
- ✅ Instagram-style feed with posts + reels
- ✅ Like, comment, share on posts
- ✅ Real-time DM via WebSocket
- ✅ Community Hives with channels + real-time group chat
- ✅ User profiles with followers/following/watchlist
- ✅ Search (movies via OMDb + users)
- ✅ Notifications (follow/like/comment/message)
- ✅ Edit profile / delete account
- ✅ Responsive — desktop + tablet + mobile
- ✅ Black + Green Loki theme
