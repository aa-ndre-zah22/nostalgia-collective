# 🏗️ NOSTALGIA COLLECTIVE - Tech Stack & Architecture

## TECHNOLOGY STACK

### Frontend

#### Core Framework
```
React 18.x
  - Component-based UI
  - Fast re-renders with hooks
  - Context API for state management
  
Next.js 14.x (Optional enhancement)
  - Server-side rendering
  - API routes
  - Static generation for gallery
```

#### Visualization & Graphics
```
Three.js
  - Landing page 3D effects
  - Memory gallery animations
  
Canvas API
  - Drawing canvas implementation
  - Real-time rendering
  - Smooth brush strokes
  
Fabric.js (Alternative)
  - Advanced canvas drawing
  - Collaborative features
  - Better performance for shared canvas
```

#### Audio Processing
```
Web Audio API
  - Sound playback and mixing
  - Volume control
  - Real-time effects
  
Tone.js (Optional)
  - Advanced audio synthesis
  - Effects chains
  - Better timing control
```

#### Real-Time Communication
```
WebRTC
  - Peer-to-peer video calls
  - Audio stream management
  - Screen sharing (future)
  
Socket.io
  - Real-time canvas synchronization
  - Chat messaging
  - Presence indicators
  - Event broadcasting
```

#### UI Component Library
```
React Beautiful DnD
  - Drag-and-drop for sound reordering
  
Framer Motion
  - Smooth animations
  - Page transitions
  - Gesture controls
  
Radix UI / Shadcn UI
  - Accessible components
  - Customizable
  - A11y built-in
```

#### Styling
```
Tailwind CSS
  - Utility-first CSS
  - Responsive design
  - Dark mode support
  
CSS-in-JS (Styled Components / Emotion)
  - Dynamic theming
  - Component scoping
```

### Backend

#### Runtime & Framework
```
Node.js 18.x LTS
  - Event-driven architecture
  - Non-blocking I/O

Express.js or Hono
  - API endpoints
  - Middleware system
  - Request handling
```

#### Real-Time Server
```
Socket.io Server
  - Real-time event handling
  - Room management
  - Broadcast capabilities
  - Fallback support

Alternative: Pusher/Ably
  - Managed real-time service
  - Less infrastructure needed
```

#### Database
```
PostgreSQL
  - User accounts & profiles
  - Memory metadata (title, tags, dates)
  - Comments & likes
  - Relationships & queries
  
Advantage: ACID transactions, JSON support

Redis
  - Session management
  - Real-time session data
  - Caching
  - Pub/Sub for Socket.io
```

#### File Storage
```
AWS S3 or Google Cloud Storage
  - Store drawing images (PNG/WebP)
  - Audio files backup
  - User exports
  
Alternative: Cloudinary
  - Image optimization
  - CDN delivery
  - Automatic transformations
```

#### Authentication
```
Passport.js
  - Local strategy (email/password)
  - OAuth2 (Google, GitHub)
  - JWT tokens
  
Auth0 (Alternative)
  - Managed authentication
  - SAML support
  - Advanced security
```

#### APIs & Integrations
```
SendGrid or Mailgun
  - Transactional emails
  - Password reset
  - Notifications

Stripe (Future)
  - Premium features
  - Payment processing
  - Subscriptions
```

### DevOps & Deployment

#### Containerization
```
Docker
  - Containerize frontend & backend
  - Consistent environments
  - Easy scaling
```

#### Orchestration
```
Docker Compose (Development)
  - Multi-container setup
  - Local testing

Kubernetes (Production)
  - Container orchestration
  - Auto-scaling
  - Load balancing
```

#### CI/CD
```
GitHub Actions
  - Run tests on push
  - Build Docker images
  - Deploy to production
  
Alternative: GitLab CI, CircleCI
```

#### Hosting Options

**Option 1: Traditional VPS**
```
- DigitalOcean Droplet
- Linode
- Hetzner
- Cost: $5-20/month starter
```

**Option 2: Serverless (Recommended for MVP)**
```
- Vercel (Frontend)
- Railway / Render (Backend)
- Firebase (Backend + Real-time)
- Cost: Free tier → $10-30/month
```

**Option 3: Cloud PaaS**
```
- AWS Elastic Beanstalk
- Google App Engine
- Azure App Service
- Cost: $10-50/month
```

### Testing & Quality

#### Testing Libraries
```
Jest
  - Unit & integration tests
  - Snapshot testing
  
React Testing Library
  - Component testing
  - User interaction simulation
  
Cypress
  - End-to-end testing
  - Visual testing
```

#### Code Quality
```
ESLint
  - JavaScript linting
  
Prettier
  - Code formatting
  
TypeScript
  - Type safety
  - Better IDE support
  - Fewer runtime errors
```

---

## SYSTEM ARCHITECTURE

### High-Level Overview

```
┌─────────────────────────────────────────────────────┐
│                   CLIENT LAYER                      │
│  ┌──────────────┐    ┌──────────────┐              │
│  │   React App  │───▶│  Three.js    │              │
│  │              │    │  Canvas API  │              │
│  │  - Sound Mixer    │              │              │
│  │  - Drawing Canvas │  WebGL       │              │
│  │  - Draw Room      │  Rendering   │              │
│  │  - Gallery        │              │              │
│  └────────────────────────────────────┘            │
│           ▼           ▼          ▼                  │
│  ┌───────────────────────────────────┐            │
│  │     Communication Layer           │            │
│  │  ├─ WebSocket (Socket.io)        │            │
│  │  ├─ WebRTC (Video/Audio)         │            │
│  │  ├─ Web Audio API                │            │
│  │  └─ REST API (HTTP)              │            │
│  └───────────────────────────────────┘            │
└─────────────────────────────────────────────────────┘
                      ▼
┌─────────────────────────────────────────────────────┐
│               SERVER LAYER (Node.js)                │
│  ┌──────────────────────────────────────────────┐  │
│  │  API Router (Express)                        │  │
│  │  ├─ Auth endpoints                           │  │
│  │  ├─ Memory CRUD                              │  │
│  │  ├─ User profile                             │  │
│  │  ├─ Gallery/Discovery                        │  │
│  │  └─ Upload handlers                          │  │
│  └──────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────┐  │
│  │  Real-Time Server (Socket.io)                │  │
│  │  ├─ Canvas sync                              │  │
│  │  ├─ Chat rooms                               │  │
│  │  ├─ Presence tracking                        │  │
│  │  └─ Event broadcasting                       │  │
│  └──────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────┐  │
│  │  Business Logic & Services                   │  │
│  │  ├─ Auth service                             │  │
│  │  ├─ Memory service                           │  │
│  │  ├─ Discovery service                        │  │
│  │  └─ File upload service                      │  │
│  └──────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
                      ▼
┌─────────────────────────────────────────────────────┐
│               DATA LAYER                            │
│  ┌──────────────┐    ┌──────────────┐              │
│  │ PostgreSQL   │    │    Redis     │              │
│  │              │    │              │              │
│  │ - Users      │    │ - Sessions   │              │
│  │ - Memories   │    │ - Cache      │              │
│  │ - Likes      │    │ - Pub/Sub    │              │
│  │ - Comments   │    │              │              │
│  │ - Tags       │    │              │              │
│  └──────────────┘    └──────────────┘              │
│  ┌──────────────────────────────────┐              │
│  │    Cloud Storage (S3/GCS)        │              │
│  │    - Drawing images              │              │
│  │    - Audio files                 │              │
│  │    - User exports                │              │
│  └──────────────────────────────────┘              │
└─────────────────────────────────────────────────────┘
```

### Database Schema

```sql
-- Users Table
CREATE TABLE users (
  id UUID PRIMARY KEY,
  username VARCHAR(100) UNIQUE NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  password_hash VARCHAR(255),
  profile_image_url TEXT,
  bio TEXT,
  favorite_decade VARCHAR(50),
  country VARCHAR(100),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP
);

-- Memories Table
CREATE TABLE memories (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id),
  title VARCHAR(255) NOT NULL,
  description TEXT,
  drawing_image_url TEXT,
  sounds JSONB, -- [{ soundId, startTime, endTime, volume }]
  decade VARCHAR(50),
  region VARCHAR(100),
  tags JSONB, -- ["childhood", "summer", "school"]
  visibility ENUM ('public', 'friends', 'private'),
  allow_remixing BOOLEAN DEFAULT true,
  allow_collaboration BOOLEAN DEFAULT true,
  likes_count INT DEFAULT 0,
  views_count INT DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP
);

-- Comments Table
CREATE TABLE comments (
  id UUID PRIMARY KEY,
  memory_id UUID REFERENCES memories(id),
  user_id UUID REFERENCES users(id),
  content TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Likes Table
CREATE TABLE likes (
  id UUID PRIMARY KEY,
  memory_id UUID REFERENCES memories(id),
  user_id UUID REFERENCES users(id),
  created_at TIMESTAMP,
  UNIQUE(memory_id, user_id)
);

-- Collaborative Sessions Table
CREATE TABLE sessions (
  id UUID PRIMARY KEY,
  host_user_id UUID REFERENCES users(id),
  title VARCHAR(255),
  memory_id UUID REFERENCES memories(id), -- Optional
  canvas_image_url TEXT,
  session_key VARCHAR(100) UNIQUE,
  status ENUM ('waiting', 'active', 'completed'),
  max_participants INT DEFAULT 4,
  created_at TIMESTAMP DEFAULT NOW(),
  ended_at TIMESTAMP
);

-- Session Participants Table
CREATE TABLE session_participants (
  id UUID PRIMARY KEY,
  session_id UUID REFERENCES sessions(id),
  user_id UUID REFERENCES users(id),
  joined_at TIMESTAMP,
  left_at TIMESTAMP,
  role ENUM ('host', 'participant')
);

-- Sounds Library Table
CREATE TABLE sounds (
  id UUID PRIMARY KEY,
  name VARCHAR(100) UNIQUE NOT NULL,
  emoji CHAR(2),
  audio_url TEXT,
  description TEXT,
  category VARCHAR(50), -- "school", "transport", "nature", etc
  duration_ms INT
);

-- Memory Remixes Table
CREATE TABLE remixes (
  id UUID PRIMARY KEY,
  original_memory_id UUID REFERENCES memories(id),
  remix_memory_id UUID REFERENCES memories(id),
  created_at TIMESTAMP
);
```

### API Endpoints

```
=== Authentication ===
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/refresh
POST   /api/auth/logout
GET    /api/auth/me

=== Users ===
GET    /api/users/:id
PUT    /api/users/:id
GET    /api/users/:id/memories
GET    /api/users/:id/profile

=== Memories ===
GET    /api/memories                    (Gallery with filters)
GET    /api/memories/:id
POST   /api/memories                    (Create new)
PUT    /api/memories/:id                (Update)
DELETE /api/memories/:id
POST   /api/memories/:id/like
DELETE /api/memories/:id/like

=== Comments ===
GET    /api/memories/:id/comments
POST   /api/memories/:id/comments
DELETE /api/comments/:id

=== Collaborative Sessions ===
POST   /api/sessions                    (Create room)
GET    /api/sessions/:id
PUT    /api/sessions/:id/participants   (Join/Leave)
POST   /api/sessions/:id/canvas         (Save canvas)

=== Sounds Library ===
GET    /api/sounds                      (All sounds)
GET    /api/sounds/:category

=== Discovery ===
GET    /api/discover/trending
GET    /api/discover/similar/:memoryId
GET    /api/discover/by-decade/:decade
GET    /api/discover/by-region/:region

=== Upload ===
POST   /api/upload/drawing
POST   /api/upload/export
```

### Socket.io Events

```javascript
// Canvas Synchronization
socket.on('canvas:draw', (data) => {
  // { sessionId, x, y, color, size, userId }
  // Broadcast to all in room
});

socket.on('canvas:clear', (sessionId) => {
  // Clear shared canvas
});

socket.on('canvas:undo', (sessionId) => {
  // Undo last action
});

// Presence & Participants
socket.on('user:joined', (data) => {
  // { sessionId, userId, username }
});

socket.on('user:left', (sessionId, userId) => {
  // User left session
});

socket.on('cursor:move', (data) => {
  // { sessionId, userId, x, y }
  // Show cursor position
});

// Chat
socket.on('message:send', (data) => {
  // { sessionId, userId, text, timestamp }
});

// Audio Sync
socket.on('audio:play', (data) => {
  // { sessionId, soundMixId, timestamp }
  // Keep all clients in sync
});

socket.on('audio:pause', (sessionId) => {
  // Pause for all
});

socket.on('audio:sync', (data) => {
  // Server broadcasts current playback position
});
```

---

## DEPLOYMENT ARCHITECTURE

### Development Environment
```
Local Machine
  ├─ Frontend (npm start)
  ├─ Backend (npm run dev)
  ├─ PostgreSQL (Docker)
  └─ Redis (Docker)
```

### Production Environment

#### Option 1: Vercel + Railway (Recommended for MVP)

```
Frontend (Vercel)
  ├─ Next.js static export
  ├─ CDN delivery
  ├─ Automatic deployments
  └─ Custom domain

Backend (Railway)
  ├─ Node.js server
  ├─ Docker container
  ├─ Automatic scaling
  └─ Database included

Real-time (Socket.io on Railway)
  ├─ WebSocket server
  ├─ Sticky sessions
  └─ Redis for Pub/Sub
```

#### Option 2: AWS (Enterprise)

```
Frontend (CloudFront + S3)
  ├─ React build
  ├─ CDN distribution
  └─ SSL/HTTPS

Backend (EC2 + RDS)
  ├─ Node.js servers (ALB)
  ├─ Auto-scaling group
  ├─ PostgreSQL RDS
  └─ ElastiCache (Redis)

Storage (S3)
  └─ Drawing images & audio files
```

#### Option 3: Docker Compose (Self-hosted)

```
VPS (DigitalOcean/Linode)
  ├─ Frontend (Nginx)
  ├─ Backend (Node.js)
  ├─ PostgreSQL
  ├─ Redis
  └─ SSL (Let's Encrypt)
```

---

## PERFORMANCE OPTIMIZATION

### Frontend
- **Code splitting**: Lazy load pages with React.lazy()
- **Image optimization**: WebP format, lazy loading
- **Bundle analysis**: Use webpack-bundle-analyzer
- **Service Worker**: Offline support, caching

### Backend
- **Caching**: Redis for frequently accessed data
- **Database indexing**: Index on user_id, memory_id, tags
- **Query optimization**: Use EXPLAIN ANALYZE
- **Connection pooling**: pgbouncer for PostgreSQL

### Network
- **CDN**: CloudFlare or AWS CloudFront
- **Compression**: gzip, brotli
- **HTTP/2**: Server-push assets
- **WebSocket**: Binary frames for real-time

---

## SECURITY CONSIDERATIONS

```
1. Authentication
   - JWT with refresh tokens
   - Secure cookies (HttpOnly, Secure, SameSite)
   - CSRF protection

2. Authorization
   - Role-based access control (RBAC)
   - Resource-level permissions
   - Validate ownership before updates

3. Data Protection
   - Hash passwords with bcrypt
   - Encrypt sensitive data at rest
   - HTTPS everywhere
   - Content Security Policy (CSP)

4. Input Validation
   - Sanitize user inputs
   - Validate file uploads
   - Rate limiting on API endpoints
   - CORS configuration

5. Infrastructure
   - Firewall rules
   - DDoS protection
   - Regular security audits
   - Dependency scanning (npm audit)
```

---

## MONITORING & LOGGING

```
Monitoring
  - Datadog or New Relic
  - Track API response times
  - Database query performance
  - Server resource usage

Logging
  - Winston or Bunyan
  - Structured JSON logs
  - ELK stack (Elasticsearch, Logstash, Kibana)
  - Log retention: 30 days

Error Tracking
  - Sentry or Rollbar
  - Capture exceptions
  - Source maps
  - Team notifications
```

---

## SCALABILITY ROADMAP

### Phase 1 (MVP - 1,000 users)
- Single Node.js server
- PostgreSQL on managed hosting
- Redis for sessions
- CloudFront for CDN

### Phase 2 (Scaling - 10,000 users)
- Load balancer
- Multiple Node.js instances
- Database read replicas
- Redis cluster
- Separate WebSocket server

### Phase 3 (Growth - 100,000+ users)
- Kubernetes cluster
- Microservices architecture
- Message queue (RabbitMQ/Kafka)
- Elasticsearch for search
- Analytics service

