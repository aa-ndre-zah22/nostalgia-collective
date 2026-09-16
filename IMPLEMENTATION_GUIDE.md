# 🚀 NOSTALGIA COLLECTIVE - Implementation Guide

## Phase 1: Project Setup (Week 1-2)

### Step 1.1: Initialize Frontend Project

```bash
# Create Next.js project with TypeScript
npx create-next-app@latest nostalgia-collective-web --typescript --tailwind

cd nostalgia-collective-web

# Install core dependencies
npm install react three @react-three/fiber @react-three/drei
npm install socket.io-client
npm install framer-motion
npm install zustand (state management)
npm install react-hot-toast
npm install axios
npm install react-beautiful-dnd
```

### Directory Structure

```
nostalgia-collective-web/
├── app/
│   ├── layout.tsx
│   ├── page.tsx (Landing)
│   ├── auth/
│   │   ├── login/
│   │   └── register/
│   ├── dashboard/
│   │   ├── page.tsx
│   │   ├── my-mixes/
│   │   └── my-drawings/
│   ├── create/
│   │   ├── mixer/
│   │   ├── canvas/
│   │   └── publish/
│   ├── gallery/
│   │   ├── page.tsx
│   │   └── [id]/
│   └── room/
│       └── [sessionId]/
├── components/
│   ├── Header.tsx
│   ├── SoundButton.tsx
│   ├── DrawingCanvas.tsx
│   ├── VideoRoom.tsx
│   ├── MemoryCard.tsx
│   └── ...
├── lib/
│   ├── api.ts
│   ├── socket.ts
│   ├── store.ts (Zustand)
│   └── utils.ts
├── styles/
│   └── globals.css
├── public/
│   └── sounds/
│       ├── school-bell.mp3
│       ├── bus-conductor.mp3
│       └── ...
└── .env.local
```

### Step 1.2: Initialize Backend Project

```bash
# Create backend directory
mkdir nostalgia-collective-api
cd nostalgia-collective-api

# Initialize Node.js project
npm init -y

# Install dependencies
npm install express cors dotenv
npm install socket.io
npm install pg (PostgreSQL driver)
npm install redis
npm install jsonwebtoken bcryptjs
npm install multer (file uploads)
npm install axios
npm install helmet
npm install express-validator
```

### Backend Directory Structure

```
nostalgia-collective-api/
├── src/
│   ├── index.js (Entry point)
│   ├── config/
│   │   ├── database.js
│   │   ├── redis.js
│   │   └── socket.js
│   ├── routes/
│   │   ├── auth.js
│   │   ├── users.js
│   │   ├── memories.js
│   │   ├── comments.js
│   │   ├── sounds.js
│   │   ├── sessions.js
│   │   └── upload.js
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── memoryController.js
│   │   ├── sessionController.js
│   │   └── ...
│   ├── models/
│   │   ├── User.js
│   │   ├── Memory.js
│   │   ├── Session.js
│   │   └── ...
│   ├── middleware/
│   │   ├── auth.js
│   │   ├── errorHandler.js
│   │   └── validation.js
│   ├── services/
│   │   ├── authService.js
│   │   ├── fileService.js
│   │   └── socketService.js
│   └── utils/
│       ├── logger.js
│       └── constants.js
├── .env
└── package.json
```

### Step 1.3: Database Setup

```bash
# Create PostgreSQL database
createdb nostalgia_collective

# Create .env file in backend
cat > .env << EOF
DATABASE_URL=postgresql://username:password@localhost:5432/nostalgia_collective
REDIS_URL=redis://localhost:6379
JWT_SECRET=your-secret-key-here
PORT=5000
NODE_ENV=development
EOF

# Run migrations (we'll create these next)
npm run migrate
```

### Step 1.4: Setup Docker Compose for Local Development

```yaml
# docker-compose.yml
version: '3.8'

services:
  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: nostalgia_collective
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

```bash
# Start services
docker-compose up -d

# Check status
docker-compose ps
```

---

## Phase 2: Backend Implementation (Week 3-4)

### Step 2.1: Database Migrations

```javascript
// src/migrations/001_initial_schema.js
const pool = require('../config/database');

async function up() {
  await pool.query(`
    CREATE TABLE users (
      id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
      username VARCHAR(100) UNIQUE NOT NULL,
      email VARCHAR(100) UNIQUE NOT NULL,
      password_hash VARCHAR(255) NOT NULL,
      profile_image_url TEXT,
      bio TEXT,
      favorite_decade VARCHAR(50),
      country VARCHAR(100),
      created_at TIMESTAMP DEFAULT NOW(),
      updated_at TIMESTAMP DEFAULT NOW()
    );

    CREATE TABLE memories (
      id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
      user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
      title VARCHAR(255) NOT NULL,
      description TEXT,
      drawing_image_url TEXT,
      sounds JSONB,
      decade VARCHAR(50),
      region VARCHAR(100),
      tags JSONB,
      visibility VARCHAR(50) DEFAULT 'public',
      allow_remixing BOOLEAN DEFAULT true,
      allow_collaboration BOOLEAN DEFAULT true,
      likes_count INT DEFAULT 0,
      views_count INT DEFAULT 0,
      created_at TIMESTAMP DEFAULT NOW(),
      updated_at TIMESTAMP DEFAULT NOW()
    );

    CREATE INDEX idx_memories_user_id ON memories(user_id);
    CREATE INDEX idx_memories_tags ON memories USING GIN(tags);
    CREATE INDEX idx_memories_decade ON memories(decade);
  `);
}

async function down() {
  await pool.query(`DROP TABLE IF EXISTS memories; DROP TABLE IF EXISTS users;`);
}

module.exports = { up, down };
```

### Step 2.2: Authentication Service

```javascript
// src/services/authService.js
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');
const pool = require('../config/database');

class AuthService {
  async register(username, email, password) {
    const hashedPassword = await bcrypt.hash(password, 10);
    
    const result = await pool.query(
      'INSERT INTO users (username, email, password_hash) VALUES ($1, $2, $3) RETURNING id, email, username',
      [username, email, hashedPassword]
    );
    
    return result.rows[0];
  }

  async login(email, password) {
    const result = await pool.query(
      'SELECT * FROM users WHERE email = $1',
      [email]
    );
    
    const user = result.rows[0];
    if (!user) throw new Error('User not found');
    
    const validPassword = await bcrypt.compare(password, user.password_hash);
    if (!validPassword) throw new Error('Invalid password');
    
    const token = jwt.sign(
      { userId: user.id, email: user.email },
      process.env.JWT_SECRET,
      { expiresIn: '7d' }
    );
    
    return { token, user };
  }

  verifyToken(token) {
    return jwt.verify(token, process.env.JWT_SECRET);
  }
}

module.exports = new AuthService();
```

### Step 2.3: API Routes Setup

```javascript
// src/routes/memories.js
const express = require('express');
const router = express.Router();
const memoryController = require('../controllers/memoryController');
const { authenticate } = require('../middleware/auth');

router.get('/', memoryController.getAll);
router.get('/:id', memoryController.getById);
router.post('/', authenticate, memoryController.create);
router.put('/:id', authenticate, memoryController.update);
router.delete('/:id', authenticate, memoryController.delete);
router.post('/:id/like', authenticate, memoryController.like);
router.delete('/:id/like', authenticate, memoryController.unlike);

module.exports = router;
```

```javascript
// src/index.js
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
require('dotenv').config();

const app = express();

// Middleware
app.use(helmet());
app.use(cors());
app.use(express.json());

// Routes
app.use('/api/auth', require('./routes/auth'));
app.use('/api/users', require('./routes/users'));
app.use('/api/memories', require('./routes/memories'));
app.use('/api/sounds', require('./routes/sounds'));
app.use('/api/sessions', require('./routes/sessions'));

// Error handling
app.use((err, req, res, next) => {
  console.error(err);
  res.status(500).json({ error: err.message });
});

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### Step 2.4: Socket.io Setup for Real-Time Features

```javascript
// src/config/socket.js
const socketIo = require('socket.io');
const redis = require('redis');
const { createAdapter } = require('@socket.io/redis-adapter');

const redisClient = redis.createClient(process.env.REDIS_URL);

async function setupSocket(server) {
  const io = socketIo(server, {
    cors: { origin: process.env.FRONTEND_URL },
  });

  await redisClient.connect();
  io.adapter(createAdapter(redisClient, redisClient.duplicate()));

  io.on('connection', (socket) => {
    console.log('User connected:', socket.id);

    // Canvas drawing
    socket.on('canvas:draw', (data) => {
      socket.to(data.sessionId).emit('canvas:draw', data);
    });

    // Chat
    socket.on('message:send', (data) => {
      socket.to(data.sessionId).emit('message:new', data);
    });

    // Audio sync
    socket.on('audio:play', (data) => {
      socket.to(data.sessionId).emit('audio:play', data);
    });

    socket.on('disconnect', () => {
      console.log('User disconnected:', socket.id);
    });
  });

  return io;
}

module.exports = setupSocket;
```

---

## Phase 3: Frontend Implementation (Week 5-6)

### Step 3.1: Sound Mixer Component

```typescript
// components/SoundMixer.tsx
'use client';

import { useState } from 'react';
import SoundButton from './SoundButton';
import toast from 'react-hot-toast';

const SOUNDS = [
  { id: 'bell', emoji: '🔔', name: 'School Bell' },
  { id: 'bus', emoji: '🚌', name: 'Bus Conductor' },
  { id: 'rain', emoji: '🌧️', name: 'Monsoon Rain' },
  { id: 'scooter', emoji: '🛵', name: 'Scooter' },
  { id: 'radio', emoji: '📻', name: 'Radio' },
  { id: 'phone', emoji: '📞', name: 'Telephone' },
  { id: 'pencil', emoji: '📝', name: 'Pencil Scratch' },
  { id: 'cricket', emoji: '🏏', name: 'Cricket Bat' },
];

export default function SoundMixer() {
  const [selectedSounds, setSelectedSounds] = useState<any[]>([]);

  const handleSoundSelect = (sound: any) => {
    if (selectedSounds.find(s => s.id === sound.id)) {
      setSelectedSounds(selectedSounds.filter(s => s.id !== sound.id));
    } else {
      if (selectedSounds.length >= 5) {
        toast.error('Maximum 5 sounds allowed');
        return;
      }
      setSelectedSounds([...selectedSounds, { ...sound, volume: 100 }]);
    }
  };

  const handleVolumeChange = (id: string, volume: number) => {
    setSelectedSounds(
      selectedSounds.map(s => s.id === id ? { ...s, volume } : s)
    );
  };

  return (
    <div className="flex gap-8 p-8">
      {/* Sound Library */}
      <div className="flex-1">
        <h2 className="text-2xl font-bold mb-6">Select Sounds</h2>
        <div className="grid grid-cols-2 gap-4">
          {SOUNDS.map(sound => (
            <SoundButton
              key={sound.id}
              sound={sound}
              isSelected={selectedSounds.some(s => s.id === sound.id)}
              onSelect={() => handleSoundSelect(sound)}
            />
          ))}
        </div>
      </div>

      {/* Selected Mix */}
      <div className="w-80 bg-gray-50 rounded-lg p-6">
        <h3 className="text-xl font-bold mb-4">Your Mix</h3>
        {selectedSounds.length === 0 ? (
          <p className="text-gray-500">Select sounds to create your mix</p>
        ) : (
          <div className="space-y-4">
            {selectedSounds.map(sound => (
              <div key={sound.id} className="bg-white p-4 rounded-lg">
                <div className="flex justify-between items-center mb-2">
                  <span className="font-semibold">{sound.emoji} {sound.name}</span>
                  <button
                    onClick={() => handleSoundSelect(sound)}
                    className="text-red-500 hover:text-red-700"
                  >
                    ✕
                  </button>
                </div>
                <input
                  type="range"
                  min="0"
                  max="100"
                  value={sound.volume}
                  onChange={(e) => handleVolumeChange(sound.id, Number(e.target.value))}
                  className="w-full"
                />
                <div className="text-sm text-gray-500 mt-1">Volume: {sound.volume}%</div>
              </div>
            ))}
          </div>
        )}
      </div>
    </div>
  );
}
```

### Step 3.2: Drawing Canvas Component

```typescript
// components/DrawingCanvas.tsx
'use client';

import { useRef, useEffect, useState } from 'react';

export default function DrawingCanvas() {
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const [isDrawing, setIsDrawing] = useState(false);
  const [color, setColor] = useState('#FF9D5C');
  const [brushSize, setBrushSize] = useState(5);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;

    const ctx = canvas.getContext('2d');
    if (!ctx) return;

    ctx.fillStyle = 'white';
    ctx.fillRect(0, 0, canvas.width, canvas.height);
  }, []);

  const startDrawing = (e: React.MouseEvent) => {
    setIsDrawing(true);
    const canvas = canvasRef.current;
    const rect = canvas?.getBoundingClientRect();
    if (!rect) return;

    const ctx = canvas?.getContext('2d');
    if (!ctx) return;

    ctx.beginPath();
    ctx.moveTo(
      e.clientX - rect.left,
      e.clientY - rect.top
    );
  };

  const draw = (e: React.MouseEvent) => {
    if (!isDrawing) return;

    const canvas = canvasRef.current;
    const rect = canvas?.getBoundingClientRect();
    const ctx = canvas?.getContext('2d');
    if (!canvas || !rect || !ctx) return;

    ctx.strokeStyle = color;
    ctx.lineWidth = brushSize;
    ctx.lineCap = 'round';
    ctx.lineJoin = 'round';

    ctx.lineTo(
      e.clientX - rect.left,
      e.clientY - rect.top
    );
    ctx.stroke();
  };

  const stopDrawing = () => {
    setIsDrawing(false);
  };

  return (
    <div className="flex flex-col gap-4 p-8">
      <div className="flex gap-4 items-center">
        <label className="flex items-center gap-2">
          Color:
          <input
            type="color"
            value={color}
            onChange={(e) => setColor(e.target.value)}
            className="w-12 h-12 cursor-pointer"
          />
        </label>

        <label className="flex items-center gap-2">
          Brush Size:
          <input
            type="range"
            min="1"
            max="50"
            value={brushSize}
            onChange={(e) => setBrushSize(Number(e.target.value))}
            className="w-32"
          />
          <span>{brushSize}px</span>
        </label>

        <button
          onClick={() => {
            const canvas = canvasRef.current;
            const ctx = canvas?.getContext('2d');
            if (ctx && canvas) {
              ctx.fillStyle = 'white';
              ctx.fillRect(0, 0, canvas.width, canvas.height);
            }
          }}
          className="px-4 py-2 bg-red-500 text-white rounded-lg hover:bg-red-600"
        >
          Clear
        </button>
      </div>

      <canvas
        ref={canvasRef}
        width={600}
        height={800}
        onMouseDown={startDrawing}
        onMouseMove={draw}
        onMouseUp={stopDrawing}
        onMouseLeave={stopDrawing}
        className="border-2 border-gray-300 rounded-lg cursor-crosshair bg-white"
      />
    </div>
  );
}
```

### Step 3.3: Memory Gallery Component

```typescript
// components/MemoryGallery.tsx
'use client';

import { useEffect, useState } from 'react';
import axios from 'axios';
import MemoryCard from './MemoryCard';

export default function MemoryGallery() {
  const [memories, setMemories] = useState([]);
  const [filter, setFilter] = useState('all');
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetchMemories();
  }, [filter]);

  const fetchMemories = async () => {
    try {
      setLoading(true);
      const response = await axios.get('/api/memories', {
        params: { filter },
      });
      setMemories(response.data);
    } catch (error) {
      console.error('Error fetching memories:', error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="p-8">
      <div className="mb-8 flex gap-4">
        <button
          onClick={() => setFilter('all')}
          className={`px-4 py-2 rounded-lg ${
            filter === 'all' ? 'bg-amber-500 text-white' : 'bg-gray-200'
          }`}
        >
          All Memories
        </button>
        <button
          onClick={() => setFilter('trending')}
          className={`px-4 py-2 rounded-lg ${
            filter === 'trending' ? 'bg-amber-500 text-white' : 'bg-gray-200'
          }`}
        >
          Trending
        </button>
        <button
          onClick={() => setFilter('recent')}
          className={`px-4 py-2 rounded-lg ${
            filter === 'recent' ? 'bg-amber-500 text-white' : 'bg-gray-200'
          }`}
        >
          Recent
        </button>
      </div>

      {loading ? (
        <p>Loading memories...</p>
      ) : (
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {memories.map(memory => (
            <MemoryCard key={memory.id} memory={memory} />
          ))}
        </div>
      )}
    </div>
  );
}
```

---

## Phase 4: Real-Time Features (Week 7-8)

### Step 4.1: Collaborative Drawing Room

```typescript
// components/DrawingRoom.tsx
'use client';

import { useEffect, useRef, useState } from 'react';
import { io } from 'socket.io-client';
import DrawingCanvas from './DrawingCanvas';
import VideoFeed from './VideoFeed';

export default function DrawingRoom({ sessionId }: { sessionId: string }) {
  const socketRef = useRef<any>(null);
  const [participants, setParticipants] = useState<any[]>([]);
  const [messages, setMessages] = useState<any[]>([]);

  useEffect(() => {
    socketRef.current = io(process.env.NEXT_PUBLIC_API_URL);

    socketRef.current.emit('session:join', { sessionId });

    socketRef.current.on('participants:update', (data) => {
      setParticipants(data);
    });

    socketRef.current.on('message:new', (msg) => {
      setMessages(prev => [...prev, msg]);
    });

    return () => {
      socketRef.current.disconnect();
    };
  }, [sessionId]);

  const sendMessage = (text: string) => {
    socketRef.current.emit('message:send', {
      sessionId,
      text,
      timestamp: new Date(),
    });
  };

  return (
    <div className="flex h-screen gap-4">
      {/* Drawing Canvas */}
      <div className="flex-1">
        <DrawingCanvas />
      </div>

      {/* Video Feeds & Chat */}
      <div className="w-80 bg-gray-100 rounded-lg p-4 flex flex-col">
        {/* Video Feeds */}
        <div className="flex-1 space-y-2 mb-4">
          {participants.map(participant => (
            <VideoFeed
              key={participant.id}
              participant={participant}
            />
          ))}
        </div>

        {/* Chat */}
        <div className="border-t pt-4">
          <div className="h-40 overflow-y-auto mb-2">
            {messages.map((msg, idx) => (
              <div key={idx} className="text-sm mb-2">
                <strong>{msg.user}:</strong> {msg.text}
              </div>
            ))}
          </div>
          <input
            type="text"
            placeholder="Send message..."
            onKeyPress={(e) => {
              if (e.key === 'Enter') {
                sendMessage(e.currentTarget.value);
                e.currentTarget.value = '';
              }
            }}
            className="w-full px-3 py-2 border rounded-lg"
          />
        </div>
      </div>
    </div>
  );
}
```

---

## Phase 5: Deployment (Week 9-10)

### Step 5.1: Deploy Frontend to Vercel

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel
```

Create `vercel.json`:

```json
{
  "env": {
    "NEXT_PUBLIC_API_URL": "@api_url"
  }
}
```

### Step 5.2: Deploy Backend to Railway

```bash
# Login to Railway
railway login

# Initialize project
railway init

# Deploy
railway up
```

Create `railway.toml`:

```toml
[build]
builder = "dockerfile"

[deploy]
startCommand = "npm run start"
```

### Step 5.3: Database Migration in Production

```bash
# Connect to production database
railway variables set DATABASE_URL

# Run migrations
npm run migrate:prod
```

---

## Phase 6: Testing & Optimization (Week 11)

### Step 6.1: Unit Tests

```typescript
// __tests__/SoundMixer.test.tsx
import { render, screen } from '@testing-library/react';
import SoundMixer from '@/components/SoundMixer';

describe('SoundMixer', () => {
  it('renders sound buttons', () => {
    render(<SoundMixer />);
    expect(screen.getByText('School Bell')).toBeInTheDocument();
  });
});
```

### Step 6.2: E2E Tests

```typescript
// cypress/e2e/create-memory.cy.ts
describe('Create Memory Flow', () => {
  it('user can create a memory', () => {
    cy.visit('/');
    cy.contains('Start Creating').click();
    cy.get('[data-testid="sound-button"]').first().click();
    cy.contains('Next').click();
  });
});
```

---

## Phase 7: Launch & Monitoring (Week 12)

### Setup Monitoring

```bash
# Install Sentry
npm install @sentry/nextjs

# Initialize
npx @sentry/wizard@latest -i nextjs
```

### Analytics Setup

```bash
# Install Posthog or Mixpanel
npm install posthog-js

# Initialize tracking
```

---

## Quick Start Commands

```bash
# Development
npm run dev

# Build
npm run build

# Test
npm run test

# Deploy
npm run deploy

# Database
npm run migrate
npm run seed
```

---

## Troubleshooting

### WebSocket Connection Issues
- Check CORS in backend
- Verify Socket.io client/server versions match
- Check firewall/proxy settings

### Canvas Drawing Lag
- Reduce drawing frequency (throttle)
- Use requestAnimationFrame
- Optimize canvas rendering

### Database Performance
- Add indexes on frequently queried columns
- Use connection pooling
- Monitor slow queries

