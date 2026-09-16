# 📐 NOSTALGIA COLLECTIVE - Wireframes & User Flows

## INFORMATION ARCHITECTURE

```
┌─ Home / Landing
│
├─ Authenticated Routes
│  ├─ Dashboard
│  │  ├─ My Mixes (Created)
│  │  ├─ My Drawings (Saved)
│  │  └─ Collaborative Sessions (Hosted/Joined)
│  │
│  ├─ Create Flow
│  │  ├─ Sound Mixer
│  │  ├─ Drawing Canvas
│  │  ├─ Memory Note
│  │  └─ Publish/Share
│  │
│  ├─ Collaborative Draw Room
│  │  ├─ Shared Canvas
│  │  ├─ Video Feed (WebRTC)
│  │  ├─ Sound Playback (Synchronized)
│  │  ├─ Participant List
│  │  └─ Chat
│  │
│  ├─ Discovery / Gallery
│  │  ├─ Browse Memories
│  │  ├─ Filter (Decade, Region, Sound)
│  │  ├─ View Memory Detail
│  │  └─ Remix / Draw Together
│  │
│  ├─ Profile
│  │  ├─ My Memories
│  │  ├─ Achievements
│  │  └─ Settings
│  │
│  └─ Notifications
��     ├─ Collaboration invites
│     ├─ Comments/Likes
│     └─ New Similar Memories
│
└─ Unauthenticated Routes
   ├─ Landing Page
   ├─ Sign Up
   ├─ Sign In
   └─ Public Gallery (View only)
```

---

## WIREFRAME SCREENS

### 1. LANDING PAGE

```
┌──────────────────────────────────────────────────────┐
│  HEADER                                              │
│  [Logo] NOSTALGIA COLLECTIVE    [Sign In] [Sign Up] │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│                                                      │
│             HERO SECTION                            │
│                                                      │
│    Mix Sounds. Draw Memories. Connect Across Time   │
│                                                      │
│    [Large Vinyl Record Illustration with hands]     │
│                                                      │
│         [START CREATING] [EXPLORE GALLERY]          │
│                                                      │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│         HOW IT WORKS - 3 STEPS                       │
│                                                      │
│  🎵 MIX              🎨 DRAW           🎥 CONNECT   │
│  Select childhood    Add visual        Draw         │
│  sounds from our     context with      together     │
│  library            your memory       in real-time  │
│                                                      │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│     FEATURED MEMORIES - Carousel                     │
│                                                      │
│  [Memory Card 1]  [Memory Card 2]  [Memory Card 3]   │
│  [Drawing]        [Drawing]        [Drawing]        │
│  [Sounds]         [Sounds]         [Sounds]         │
│  [Meta]           [Meta]           [Meta]           │
│                                                      │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│           COMMUNITY STATS                            │
│                                                      │
│   234 Mixes Created  |  456 People  |  12 Live      │
│                          Connected      Sessions    │
│                                                      │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│  FOOTER: Links, Social, Newsletter signup           │
└──────────────────────────────────────────────────────┘
```

---

### 2. SOUND MIXER PAGE

```
┌──────────────────────────────────────────────────────┐
│  [Back] MIX YOUR SOUNDS            [Preview] [Next] │
└──────────────────────────────────────────────────────┘

┌─────────────────────┬──────────────────────────────┐
│  SOUND LIBRARY      │  YOUR MIX (Active)           │
│                     │                              │
│  🔔 School Bell     │  🔔 School Bell              │
│  🚌 Bus Conductor   │  [Volume: ▓▓▓░░]             │
│  📻 Radio           │  [Start: 0s] [End: 3s]       │
│  🌧️ Monsoon        │                              │
│  🛵 Scooter         │  🌧️ Monsoon Rain             │
│  📞 Telephone       │  [Volume: ▓▓░░░]             │
│  📝 Pencil Scratch  │  [Start: 1s] [End: 5s]       │
│  🏏 Bat & Ball      │                              │
│                     │  ❌ Remove   [+ Add Sound]   │
│  [Load More]        │                              │
│                     │  ┌──────────────────────┐    │
│                     │  │ PREVIEW              │    │
│                     │  │ [▶️ Play]  [⏸ Stop]  │    │
│                     │  │ 00:05 / 10:00        │    │
│                     │  │ Master Vol: ▓▓▓▓░    │    │
│                     │  └──────────────────────┘    │
│                     │                              │
│                     │  [Back] [Save Draft] [Next] │
└─────────────────────┴──────────────────────────────┘
```

**Interactions:**
- Click sound button → adds to mix (glow effect)
- Drag sound card → reorder mix
- Volume slider → real-time adjustment
- Play button → preview full mix
- Next → moves to Drawing Canvas

---

### 3. DRAWING CANVAS PAGE

```
┌──────────────────────────────────────────────────────┐
│  [Back] DRAW YOUR MEMORY              [Next] [Save] │
└──────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────┐
│                                                    │
│         [CANVAS AREA - 600x800px]                 │
│                                                    │
│         ┌─────────────────────────────┐           │
│         │                             │           │
│         │                             │           │
│         │                             │           │
│         │  [User can draw here]       │           │
│         │                             │           │
│         │                             │           │
│         │                             │           │
│         │                             │           │
│         └─────────────────────────────┘           │
│                                                    │
└────────────────────────────────────────────────────┘

┌─ TOOLBAR (Top) ─────────────────────────────────────┐
│  [🎨 Brush] [✏️ Pencil] [🖍️ Marker] [🪣 Fill]     │
│  [Undo ⟲] [Redo ⟳] [Clear 🗑️]                   │
└──────────────────────────────────────────────────────┘

┌─ COLOR PALETTE (Right) ────────────────────────────┐
│                                                    │
│  🟠 🟡 🔵 🟢 🔴 🟣 🟤 ⚫ ⚪  [Custom Color Picker] │
│                                                    │
│  Brush Size:  [━━━●━━━] 12px                      │
│                                                    │
│  Opacity:     [━━━━●━━] 80%                       │
│                                                    │
└──────────────────────────────────────────────────────┘

┌─ SOUND PLAYBACK (Bottom Left) ──────────────────────┐
│  🔊 Your Mix Playing                               │
│  [▶️ Play] [⏸ Pause] [⏹ Stop]                      │
│  Now playing: School Bell + Monsoon                │
│  00:45 / 10:00                                     │
└──────────────────────────────────────────────────────┘

[Back]  [Save Draft]  [Next →]
```

**Interactions:**
- Draw on canvas → real-time rendering
- Brush tool → freehand drawing
- Color picker → change drawing color
- Sound plays in background during drawing
- Auto-save every 30 seconds

---

### 4. MEMORY NOTE & PUBLISH PAGE

```
┌──────────────────────────────────────────────────────┐
│  [Back] SHARE YOUR MEMORY               [Publish] │
└───────────────────────────────────────���──────────────┘

┌────────────────────────────────────────────────────┐
│         PREVIEW (Left - 50%)                       │
│                                                    │
│  ┌─────────────────────────────────┐              │
│  │ [Drawing Preview]               │              │
│  │                                 │              │
│  │                                 │              │
│  │                                 │              │
│  │                                 │              │
│  └─────────────────────────────────┘              │
│                                                    │
│  🎵 Sounds: 🔔 School Bell, 🌧️ Monsoon          │
│  ❤️ 0  💬 0  🔗 Share                            │
│                                                    │
└────────────────────────────────────────────────────┘

┌───────��────────────────────────────────────────────┐
│         DETAILS (Right - 50%)                      │
│                                                    │
│  📝 Your Memory:                                   │
│  ┌────────────────────────────────┐               │
│  │ "Summer vacations at my        │               │
│  │  grandmother's place. The      │               │
│  │  school bell at the end of     │               │
│  │  recess and monsoon rains      │               │
│  │  bringing that earthy smell.   │               │
│  │  Simple times."                │               │
│  └────────────────────────────────┘               │
│                                                    │
│  🗓️ Decade: [1990s ▼]                            │
│  🌍 Region: [South Asia ▼]                        │
│  🏷️ Tags: [Childhood] [Summer] [Rain]            │
│                                                    │
│  🔒 Visibility: [Public ▼]                        │
│     Options: Public / Friends Only / Private      │
│                                                    │
│  ☑️ Allow Others to Draw Together                 │
│  ☑️ Allow Remixing Sounds                         │
│                                                    │
│  [← Back]  [Save Draft]  [Publish →]             │
│                                                    │
└────────────────────────────────────────────────────┘
```

---

### 5. COLLABORATIVE DRAW ROOM

```
┌──────────────────────────────────────────────────────┐
│  [Back] DRAW TOGETHER: "90s Summer Vibes"      [✕] │
└──────────────────────────────────────────────────────┘

┌────────────────────────┬──────────────────────────┐
│  VIDEO FEED (Left)     │  SHARED CANVAS (Right)  │
│                        │                         │
│  ┌──────────────────┐  │  ┌───────────────────┐  │
│  │ [Your Camera]    │  │  │                   │  │
│  │                  │  │  │  [SHARED CANVAS]  │  │
│  │                  │  │  │                   │  │
│  │                  │  │  │                   │  │
│  └──────────────────┘  │  │                   │  │
│                        │  │                   │  │
│  👤 Sarah (Drawing)    │  │                   │  │
│  ┌──────────────────┐  │  └───────────────────┘  │
│  │ [Sarah's Camera] │  │                         │
│  │                  │  │  Toolbar:              │
│  │                  │  │  [🎨] [✏️] [🖍️]      │
│  │                  │  │  [Undo] [Redo] [Clear] │
│  └──────────────────┘  │                         │
│                        │  Colors: 🟠 🟡 🔵 🟢  │
│  👤 Mike (Muted)       │                         │
│  ┌──────────────────┐  │  Brush: [━━●━━] 12px   │
│  │ [Mike's Camera]  │  │                         │
│  │                  │  │  Master Vol: ▓▓▓▓░     │
│  │  🔇 Muted        │  │                         │
│  └──────────────────┘  │                         │
│                        │                         │
│ [🎤] [🎥] [Hang Up]   │  [Save Drawing] [Export]│
└────────────────────────┴──────────────────────────┘

┌──────────────────────────────────────────────────────┐
│  CHAT (Bottom)                                       │
│                                                      │
│  Sarah: This looks great! 🎨                        │
│  Mike: Let's add more colors                        │
│  You: The sound is perfect, so nostalgic ✨        │
│                                                      │
│  [Type message...]  [Send]  [Emoji] [Upload Image] │
└──────────────────────────────────────────────────────┘
```

**Features:**
- WebRTC video call (up to 4 participants)
- Synchronized canvas drawing (WebSocket)
- Shared sound playback (server-synced)
- Real-time cursor position of other drawers
- Chat alongside drawing
- Auto-save progress every 5 seconds

---

### 6. DISCOVERY / GALLERY PAGE

```
┌──────────────────────────────────────────────────────┐
│  [Logo] NOSTALGIA COLLECTIVE  [Profile] [Settings] │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│  FILTERS (Top Bar)                                   │
│                                                      │
│  [Decade: 1990s ▼] [Region: South Asia ▼]          │
│  [Sounds: All ▼] [Sort: Latest ▼] [Search]         │
│                                                      │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│           MEMORY GRID (Masonry Layout)              │
│                                                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │ [Drawing 1] │  │ [Drawing 2] │  │ [Drawing 3] │ │
│  │ By Sarah    │  │ By Raj      │  │ By Priya    │ │
│  │ 🔔 🌧️      │  │ 🚌 📻      │  │ 🛵 📞      │ │
│  │ ❤️234      │  │ ❤️89       │  │ ❤️156      │ │
│  │ "Summer..." │  │ "School..." │  │ "Evening..." │ │
│  └─────────────┘  └─────────────┘  └─────────────┘ │
│                                                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │ [Drawing 4] │  │ [Drawing 5] │  │ [Drawing 6] │ │
│  │ By Arun     │  │ By Meera    │  │ By Vikas    │ │
│  │ 📝 🏏      │  │ 🔔 🚌      │  │ 🌧️ 🎵     │ │
│  │ ❤️42       │  │ ❤️512      │  │ ❤️234      │ │
│  │ "Exams..." │  │ "Morning..." │  │ "Rainy..." │ │
│  └─────────────┘  └─────────────┘  └─────────────┘ │
│                                                      │
│  ┌─────────────┐  ┌─────────────┐                   │
│  │ [Drawing 7] │  │ [Drawing 8] │                   │
│  │ By Deepak   │  │ By Nisha    │                   │
│  │ 🛵 📻      │  │ 🚌 🌧️      │                   │
│  │ ❤️78       │  │ ❤️198      │                   │
│  │ "Ride..." │  │ "Commute..." │                   │
│  └─────────────┘  └─────────────┘                   │
│                                                      │
│              [Load More...]                         │
│                                                      │
└──────────────────────────────────────────────────────┘
```

**Card Interaction:**
- Hover: Lift effect, show action buttons
- Click: Expand to full detail view
- "Draw Together" button → Invite to collaborative session
- "Remix" button → Create new mix based on this one
- "Like" button → Heart icon fills

---

### 7. MEMORY DETAIL VIEW

```
┌──────────────────────────────────────────────────────┐
│  [← Back to Gallery]                  [⋯] [×]      │
└──────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│                                                     │
│            [DRAWING - Large View]                  │
│            (Clickable to zoom)                     │
│                                                     │
│            ┌──────────────────────────┐            │
│            │                          │            │
│            │                          │            │
│            │                          │            │
│            │                          │            │
│            │                          │            │
│            │                          │            │
│            │                          │            │
│            └──────────────────────────┘            │
│                                                     │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│ DETAILS & ACTIONS                                   │
│                                                     │
│ 👤 Created by Sarah • 🗓️ 2 hours ago              │
│ 🏷️ Tags: Childhood, Summer, 1990s, South Asia     │
│                                                     │
│ 📝 Memory:                                         │
│ "This takes me back to summer vacations at my      │
│  grandmother's place in Kerala. The school bell    │
│  marking the end of recess, running barefoot       │
│  through the house, and then monsoon rain          │
│  bringing that unmistakable earthy petrichor       │
│  smell. Simple times, now only in memory."         │
│                                                     │
│ 🎵 Sounds Used:                                    │
│  • 🔔 School Bell (0-3s)                          │
│  • 🌧️ Monsoon Rain (1-5s)                         │
│                                                    │
│  [▶️ Listen to Mix] [00:05 / 10:00]                │
│  Master Volume: ▓▓▓▓░                              │
│                                                     │
│ ────────────────────────────────────────────────    │
│                                                     │
│ ACTIONS:                                           │
│                                                     │
│ [❤️ 234 Like]  [💬 12 Comments]  [🔗 Share]       │
│                                                     │
│ [🎨 Draw Together] [🎵 Remix Sounds] [⭐ Save]    │
│                                                     │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│ COMMENTS SECTION                                    │
│                                                     │
│ 👤 Mike: "This is exactly how I remember it!      │
│          I also add the sound of old crickets"     │
│          ↳ ❤️ 3                                   │
│          ↳ Reply...                                │
│                                                     │
│ 👤 Priya: "Amazing! So nostalgic 🥺"               │
│          ↳ ❤️ 8                                   │
│          ↳ Reply...                                │
│                                                     │
│ 👤 Raj: "Can I add the sound of the ice-cream     │
│         vendor's bell to this?"                    │
│          ↳ ❤️ 2                                   │
│          ↳ Reply...                                │
│                                                     │
│ [Write a comment...]                               │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

### 8. USER DASHBOARD

```
┌──────────────────────────────────────────────────────┐
│  [☰] NOSTALGIA COLLECTIVE        [🔔] [👤] [⚙️]    │
└──────────────────────────────────────────────────────┘

┌───────────────┬────────────────────────────────────┐
│ LEFT SIDEBAR  │                                    │
│               │  DASHBOARD                         │
│ 🏠 Dashboard  │                                    │
│ 🎨 Create New │  👤 Welcome back, Sarah!           │
│ 🎵 My Mixes   │  Your Stats:                       │
│ 🎨 My Drawings│  • 12 Mixes Created               │
│ 🎥 Sessions   │  • 34 Drawings Made               │
│ 💫 Saved      │  • 8 Collaborative Sessions       │
│ 🔍 Discover   │  • 156 Total Likes Received       │
│ 👥 Community  │                                    │
│ ⚙️ Settings   │  ────────────────────────────────  │
│               │                                    │
│ 🚪 Log Out    │  RECENT ACTIVITY                   │
│               │                                    │
│               │  3h ago - You created "Monsoon   │
│               │           Memory Mix"             │
│               │                                    │
│               │  1d ago - Sarah liked your        │
│               │           drawing                 │
│               │                                    │
│               │  2d ago - You drew in              │
│               │           "90s Nostalgia Room"    │
│               │                                    │
│               │  ────────────────────────────────  │
│               │                                    │
│               │  UPCOMING SESSIONS                │
│               │                                    │
│               │  Tomorrow 7pm - "Childhood       │
│               │  Sounds Remix" by Mike            │
│               │  [Join] [Interested]              │
│               │                                    │
│               │  ────────────────────────────────  │
│               │                                    │
│               │  QUICK ACTIONS                    │
│               │                                    │
│               │  [+ Create New Mix]               │
│               │  [+ Invite Friends]               │
│               │  [Explore Gallery]                │
│               │                                    │
└───────────────┴────────────────────────────────────┘
```

---

### 9. MOBILE LAYOUT (320px width)

```
��───────────────────────────────┐
│ [☰] NOSTALGIA      [🔔] [👤]  │
└───────────────────────────────┘

┌───────────────────────────────┐
│   SOUND MIXER (Vertical)       │
│                               │
│   🔔 School Bell              │
│   [+]                         │
│                               │
│   🚌 Bus Conductor            │
│   [+]                         │
│                               │
│   📻 Radio                    │
│   [+]                         │
│                               │
│   [Scroll down for more]      │
│                               │
│   ────────────────────────    │
│   YOUR MIX                    │
│                               │
│   🔔 School Bell              │
│   Vol: ▓▓░  [Remove ✕]       │
│                               │
│   🌧️ Monsoon                 │
│   Vol: ▓▓▓░ [Remove ✕]       │
│                               │
│   Master: ▓▓▓░  [Play ▶️]    │
│                               │
│   [Back]  [Next]              │
│                               │
└───────────────────────────────┘

┌───────────────────────────────┐
│   DRAWING CANVAS (Vertical)   │
│                               │
│   ┌─────────────────────────┐ │
│   │                         │ │
│   │ [CANVAS]                │ │
│   │                         │ │
│   │                         │ │
│   └─────────────────────────┘ │
│                               │
│   [🎨] [✏️] [Undo] [Clear]  │
│                               │
│   Colors: 🟠 🟡 🔵 🟢 ⚫    │
│                               │
│   Size: [━●━] 12px            │
│                               │
│   🔊 School Bell              │
│   [▶️] 0:05 / 1:00            │
│                               │
│   [Back]  [Next]              │
│                               │
└───────────────────────────────┘
```

---

## USER FLOWS

### Flow 1: Create & Share Memory

```
Start
  ↓
[Sign In / Sign Up]
  ↓
[Dashboard] → Click "Create New"
  ↓
[Sound Mixer] → Select 2-3 sounds → Adjust volume
  ↓
[Preview Mix] → Sounds good? [Next]
  ↓
[Drawing Canvas] → Sketch memory → Preview
  ↓
[Memory Note] → Add text + tags + decade/region
  ↓
[Preview Card] → Looks good? [Publish]
  ↓
[Share Modal] → Copy link / Share on social
  ↓
✅ Memory Published!
  ↓
[Back to Dashboard] or [View in Gallery]
```

---

### Flow 2: Collaborative Drawing Session

```
User A (Host)
  ↓
[Gallery] → Find memory → [Draw Together]
  ↓
[Create Session] → Set title + invite link
  ↓
[Waiting Room] → Share link with friends
  ↓
User B joins via link
  ↓
User C joins via link
  ↓
[Draw Together Screen] → All see:
  - Shared canvas
  - Video feeds
  - Synchronized sound playback
  - Chat
  ↓
All draw together (15-30 mins)
  ↓
[Save Collaborative Drawing] → Save + Share
  ↓
✅ Session Complete
```

---

## RESPONSIVE DESIGN NOTES

- **Desktop (1200px+)**: Full layout with sidebars
- **Tablet (768px)**: Stack to 2-column, sidebar collapses
- **Mobile (320px)**: Full-screen stack, hamburger menu
- **Canvas scaling**: Responsive canvas size, touch-friendly brush size

---

## ACCESSIBILITY FEATURES

1. **Keyboard Navigation**: Tab through all controls
2. **Color Contrast**: WCAG AA compliant
3. **Screen Readers**: Alt text for all images, aria labels
4. **Focus States**: Clear 3px indigo outline
5. **Audio Descriptions**: Descriptions of drawing prompts
6. **Captions**: Video calls support live captions
7. **Reduced Motion**: Option to disable animations

---

## NEXT STEPS

1. Create Figma wireframes with actual dimensions
2. Build clickable prototype for user testing
3. Design mobile-first responsive layout
4. Create component interactions in Framer
5. Conduct user testing with 5-10 people
6. Iterate based on feedback
7. Create developer handoff specs

