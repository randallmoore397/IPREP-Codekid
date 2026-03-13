# 🚀 CodeKids — Beginner Web Learning Platform

**Designed & Developed by Mr. Randall A. Moore**
© 2026 Mr. Randall A. Moore. All rights reserved.

---

## 📖 Overview

CodeKids is an interactive, gamified web-based learning platform built for classroom use. It teaches complete beginners how to build websites by guiding them through three progressive stages — HTML, CSS, and JavaScript — using live code editors, quizzes, tasks, and reflection activities.

Student progress is saved in real time to Firebase Firestore, and teachers can monitor every student's activity through a dedicated admin dashboard.

---

## 📁 Project Files

| File | Description |
|------|-------------|
| `codekids.html` | Student-facing learning app |
| `admin.html` | Teacher dashboard (password protected) |
| `README.md` | This documentation file |

Both files are **standalone HTML files** — no build tools, no frameworks, no installation required. Just open in a browser or deploy to any static host.

---

## ✨ Features

### Student App (`codekids.html`)

**Welcome Screen**
- Student enters their full name and selects their grade (Grade 1–12)
- Both fields are required before the journey begins

**3 Progressive Learning Stages**

| Stage | Topic | Colour |
|-------|-------|--------|
| Stage 1 | 🟠 HTML — Structure & Markup | Orange |
| Stage 2 | 🔵 CSS — Styling & Layouts | Blue |
| Stage 3 | 🟡 JavaScript — Interactivity & Logic | Yellow |

Each stage includes:
- 2 guided lessons with syntax-highlighted code examples
- 2 live split-pane code editors (edit → run → preview instantly)
- 5 checkable tasks (10 XP each)
- **50 quiz questions** broken into 5 rounds of 10 (20 XP per correct answer)
- 2 reflection text areas

**Quiz System**
- 150 total questions across all three stages (50 per stage)
- Organised into 5 rounds of 10 per stage: ⭐ Round 1 → 🔥 Round 2 → 💡 Round 3 → 🚀 Round 4 → 🏆 Round 5
- Round tabs turn green when fully completed
- Live score bar shows answered count and correct count per round
- Prev / Next navigation between rounds

**XP & Gamification**
- XP bar in the top navigation (max 600 XP)
- Task completions, correct quiz answers, and stage unlocks award XP
- Confetti particle effects and Web Audio API sound effects
- Stage unlock modal with celebration animation
- Final certificate screen on completion

**Stage Gating**
- Each stage locks until the previous one is fully completed
- Complete button requires: all 5 tasks checked, all 50 quiz questions answered, and both reflections written (minimum 10 characters each)

**Firebase Sync**
- All progress auto-saves to Firestore in real time
- Sync status indicator in the top bar (syncing / saved / error)
- Students can return and pick up exactly where they left off
- Progress is stored by student name as the document ID

---

### Teacher Dashboard (`admin.html`)

**Login**
- Username: `teacher`
- Password: `codekids`

**Date-Based Activity View (Landing Screen)**
- Shows a card grid organised by date — one card per day students were active
- Each card displays the date, day name, active student count, and stage completion badges
- Today's card is highlighted with a green border and a **● Today** badge
- Click any date card to drill into that day's student activity

**Day Drill-Down**
- Shows all students active on the selected date
- Full searchable, filterable student table
- Filter by name or class/grade
- Click any student row to open their full detail panel

**Student Detail Panel (4 tabs)**
- **Overview** — XP progress bar and per-stage breakdown
- **HTML** — tasks, all 50 quiz answers (grouped by round), and reflections
- **CSS** — tasks, all 50 quiz answers (grouped by round), and reflections
- **JavaScript** — tasks, all 50 quiz answers (grouped by round), and reflections

**Stats Row**
- Total students enrolled
- Count of HTML / CSS / JS stage completions
- Average XP across all students

**CSV Export**
- Exports all student data to a timestamped `.csv` file
- Includes: Name, Class, XP, stage completions, task scores, quiz scores, all reflections, and last seen timestamp

**Real-Time Updates**
- Dashboard uses a Firestore `onSnapshot` listener — updates automatically when any student saves progress
- Manual ↻ Reload button available as a fallback

---

## 🔥 Firebase Setup

Both files connect to the same Firebase Firestore project. The config is already embedded in both files. If you ever need to change the project, update the `firebaseConfig` object in **both** files:

```javascript
const firebaseConfig = {
  apiKey:            "YOUR_API_KEY",
  authDomain:        "YOUR_PROJECT.firebaseapp.com",
  projectId:         "YOUR_PROJECT_ID",
  storageBucket:     "YOUR_PROJECT.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId:             "YOUR_APP_ID"
};
```

**Firestore Rules (required)**

In your Firebase console under Firestore → Rules, set:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

> ⚠️ These are open rules suitable for a controlled classroom environment. For a public deployment, add proper authentication rules.

**Firestore Data Structure**

Each student is stored as a document in the `students` collection, with their name as the document ID:

```
students/
  {studentName}/
    name          — string
    className     — string (Grade 1–12)
    xp            — number
    lastSeen      — timestamp
    stages/
      html/
        completed   — boolean
        tasks       — array [0,0,0,0,0]
        quiz        — map { qid: { sel, correct, answer } }
        reflections — array ["", ""]
      css/  (same structure)
      js/   (same structure)
```

---

## 🚀 Deployment

### Local (Testing)
Simply open `codekids.html` or `admin.html` directly in any modern browser. No server needed.

### Netlify (Recommended)
1. Create a free account at [netlify.com](https://netlify.com)
2. Drag and drop the project folder (containing both HTML files) onto the Netlify dashboard
3. Netlify will provide a public URL — share `codekids.html` with students and keep `admin.html` private

### Any Static Host
Both files are pure HTML/CSS/JavaScript with no server-side code. They will work on GitHub Pages, Vercel, Cloudflare Pages, or any web server.

---

## 🛠️ Technical Stack

| Technology | Purpose |
|------------|---------|
| HTML5 / CSS3 / Vanilla JavaScript | Core platform — zero dependencies |
| Firebase Firestore (v9 compat) | Real-time student data sync |
| Google Fonts (Nunito + Fira Code) | Typography |
| Web Audio API | Sound effects (built-in, no library) |
| CSS Custom Properties | Consistent theming |

No `npm`, no build step, no framework. Everything runs directly in the browser.

---

## 📊 XP Scoring System

| Action | XP Awarded |
|--------|-----------|
| Task completed | +10 XP |
| Quiz question correct | +20 XP |
| Max possible (all 3 stages) | 600 XP |

Tasks: 5 per stage × 3 stages × 10 XP = 150 XP
Quiz: 50 per stage × 3 stages × 20 XP = **3,000 XP possible from quiz**

> Note: The XP bar maxes out at 600 XP for display purposes, but students can accumulate beyond that through quiz completions.

---

## 🔐 Admin Credentials

| Field | Value |
|-------|-------|
| Username | `teacher` |
| Password | `codekids` |

> To change the password, edit the `CREDS` object in `admin.html`:
> ```javascript
> const CREDS = { user: 'teacher', pass: 'your-new-password' };
> ```

---

## 📋 Quiz Content Summary

### 🟠 HTML — 50 Questions
- **Round 1** (Q1–10): Core tags and fundamentals
- **Round 2** (Q11–20): Document structure and attributes
- **Round 3** (Q21–30): Forms and tables
- **Round 4** (Q31–40): Semantic HTML and metadata
- **Round 5** (Q41–50): Advanced concepts and best practices

### 🔵 CSS — 50 Questions
- **Round 1** (Q1–10): Selectors, properties, and basics
- **Round 2** (Q11–20): Box model — padding, margin, border
- **Round 3** (Q21–30): Flexbox and layout
- **Round 4** (Q31–40): Typography and colour
- **Round 5** (Q41–50): Selectors, animations, and advanced CSS

### 🟡 JavaScript — 50 Questions
- **Round 1** (Q1–10): Variables, strings, and fundamentals
- **Round 2** (Q11–20): Conditionals and loops
- **Round 3** (Q21–30): Functions and scope
- **Round 4** (Q31–40): Arrays and objects
- **Round 5** (Q41–50): DOM manipulation and events

---

## 🌐 Browser Compatibility

CodeKids works in all modern browsers:

- ✅ Google Chrome (recommended)
- ✅ Mozilla Firefox
- ✅ Microsoft Edge
- ✅ Safari (desktop and iOS)

JavaScript must be enabled. An internet connection is required for Firebase sync and Google Fonts.

---

## 📞 Support & Contact

For questions, issues, or feedback regarding this platform, contact:

**Mr. Randall A. Moore**
Platform Designer & Developer

---

*Unauthorised reproduction or distribution of this platform is strictly prohibited.*
