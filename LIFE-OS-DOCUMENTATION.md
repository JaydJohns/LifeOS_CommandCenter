# Life OS - Personal Operating System Documentation

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Philosophy & Design](#philosophy--design)
3. [Getting Started](#getting-started)
4. [Core Features](#core-features)
5. [Module Structure](#module-structure)
6. [Using the Dashboard](#using-the-dashboard)
7. [Data & Storage](#data--storage)
8. [GitHub Pages Deployment](#github-pages-deployment)
9. [Customization Guide](#customization-guide)
10. [Recommended Features](#recommended-features)

---

## Project Overview

**Life OS** is a modern, AI-inspired personal command center designed for educators, researchers, and busy professionals who need an integrated view of their work across multiple life domains.

### What It Does

Life OS provides a **single interface** for tracking and managing:
- **9 Life Domains**: Home, Family, PFW Teaching, PFW Service, Bellon Branch, Olde Oak Tree, Health
- **2 Special Projects**: Annual Faculty Review (AFR) and UXR Lab initiatives
- **Task progress** with visual indicators
- **At-a-glance dashboards** with drill-down capability

### Why It's Different

Unlike generic to-do apps, Life OS:
- ✅ Displays **all your domains at once** on a command center dashboard
- ✅ Uses **contextual progress tracking** (AFR and UXR have special prominence)
- ✅ Scales from **quick overview to deep work** in one click
- ✅ Works **completely offline** with local browser storage
- ✅ Looks **futuristic and engaging** (inspired by JARVIS/Iron Man UI)
- ✅ Requires **zero setup** — download, open, use

---

## Philosophy & Design

### Design Inspiration

Life OS adopts the aesthetic of advanced AI interfaces (think JARVIS from Iron Man):
- **Neon cyan/blue color scheme** with glowing effects
- **Monospace typography** (Orbitron + Space Mono fonts)
- **Glassmorphism panels** with sci-fi corners
- **Scan-line background** for digital ambiance
- **Circular progress indicators** instead of bars

This visual language creates an aspirational feeling—you're operating an advanced system, not just checking boxes.

### Core Philosophy

**Visibility + Accountability + Flexibility**

- **Visibility**: See all your domains at once. No hidden tabs or buried lists.
- **Accountability**: Progress rings and completion metrics keep you motivated.
- **Flexibility**: Add any task to any domain. Default structure, but not rigid.

---

## Getting Started

### Installation

1. **Download** the `life-os-command-center.html` file
2. **Save it locally** on your Mac (e.g., `~/Documents/life-os.html`)
3. **Double-click** to open in your browser

That's it. No installation, no dependencies, no internet required.

### First Time Setup

When you open Life OS for the first time:

1. **Dashboard Overview** appears with 9 modules:
   - 🎯 Annual Review (AFR)
   - 🧪 UXR Lab
   - 🏠 Home
   - 👨‍👩‍👧‍👦 Family
   - 📚 PFW Teaching
   - 🏛️ PFW Service
   - 🌳 Bellon Branch
   - 🏡 Olde Oak Tree
   - 💪 Health

2. **Each module shows**:
   - Progress (e.g., `0/6` tasks complete)
   - Mini progress bar
   - Brief description
   - "ENTER MODULE" prompt

3. **Click any module** to enter detail view and start adding tasks

---

## Core Features

### 1. Dashboard Overview

The home view displays all 9 modules as clickable cards.

**What you see:**
- Module icon and name
- Task progress (`completed/total`)
- Visual progress bar (cyan neon glow)
- Module description
- Hover effect with "ENTER MODULE" prompt

**Why this matters:**
You instantly see which areas need attention. If Home is `2/8` but Teaching is `6/6`, you know where to focus.

### 2. Module Detail View

Click any card to enter that module's full interface.

**What you see:**
- Large circular progress ring (0-360°)
- Completed/Total statistics
- Full checklist of all tasks
- Add new task input
- Back button to return to dashboard

**Task interactions:**
- Check off completed tasks
- Progress updates **instantly**
- New tasks auto-save to browser storage

### 3. Real-Time Progress Tracking

As you check tasks:
- ✅ Circular progress ring fills
- ✅ Percentage updates
- ✅ Completed count increments
- ✅ Dashboard card updates automatically
- ✅ All changes saved locally

### 4. Persistent Local Storage

All data is stored in your browser's `localStorage`:
- No cloud accounts needed
- Works offline
- Persists across sessions
- Private by default

**Data structure:**
```javascript
// Example stored data
{
  "life-os-afr-tasks": {
    "afr-0": false,      // Task not completed
    "afr-1": true,       // Task completed
    "afr-2023945": false // Custom added task
  }
}
```

---

## Module Structure

### Pre-populated Modules

**AFR (Annual Faculty Review)**
- Collect teaching evaluations
- Document service contributions
- Compile research activities
- Professional development summary
- Self-evaluation narrative
- Goal setting for next cycle

**UXR Lab (User Experience Research)**
- User research planning
- Participant recruitment
- Conduct interviews
- Data analysis
- Findings documentation
- Present results

### Domain Modules

**Home, Family, PFW Teaching, PFW Service, Bellon Branch, Olde Oak Tree, Health**

These start empty but accept unlimited custom tasks:
- Add anything relevant to that domain
- Tasks are stored independently
- Each domain can have different structures

**Example workflow for PFW Teaching:**
1. Enter Teaching module
2. Add: "Grade assignments for Data Structures"
3. Add: "Prepare next week's lecture slides"
4. Add: "Hold office hours review session"
5. Check off as completed
6. Dashboard shows `1/3` progress

---

## Using the Dashboard

### Daily Workflow

1. **Morning**: Open Life OS to see overview
   - Scan all 9 modules
   - Identify priorities (lowest % complete)
   - Plan your day

2. **During work**: Click modules as you work through them
   - Focus on one domain at a time
   - Check off completed tasks
   - See progress ring fill up (motivating!)

3. **Evening**: Quick review
   - See overall progress
   - Add tasks for tomorrow
   - Notice which domains are neglected

### Navigation

- **Dashboard → Detail**: Click any module card
- **Detail → Dashboard**: Click "← BACK" button
- **All changes auto-save**: No manual save needed

### Adding Tasks

In any module detail view:
1. Type task in the input field (e.g., "Review grant applications")
2. Click "EXECUTE" button
3. Task appears in checklist
4. Check off as you complete it

Tasks are automatically capitalized and stored.

---

## Data & Storage

### Where Your Data Lives

All data is stored in your browser's `localStorage` under these keys:

```
life-os-afr-tasks       // Annual Faculty Review
life-os-uxr-tasks       // UXR Lab
home-domain             // Home module
family-domain           // Family module
teaching-domain         // PFW Teaching
service-domain          // PFW Service
bellon-domain           // Bellon Branch
oak-domain              // Olde Oak Tree
health-domain           // Health module
```

### Data Format

Each module stores tasks as a JSON object:
```json
{
  "task-id-1": true,
  "task-id-2": false,
  "custom-task-123": true
}
```

The `true/false` represents completion status.

### Backing Up Your Data

To export your Life OS data:

1. Open DevTools (`Cmd + Option + I` on Mac)
2. Go to Console tab
3. Paste this:
   ```javascript
   JSON.stringify(localStorage)
   ```
4. Copy the output
5. Save to a text file for backup

To import data, reverse the process in the Console.

### Privacy

✅ **100% private** - data never leaves your device
✅ **No tracking** - no analytics or telemetry
✅ **No accounts** - no login required
✅ **No cloud sync** - all local storage

---

## GitHub Pages Deployment

Want to access Life OS from anywhere on your Mac or across devices?

### Setup Instructions

1. **Create GitHub repository** (if you don't have one):
   ```bash
   git init life-os
   cd life-os
   ```

2. **Add the HTML file**:
   ```bash
   cp ~/Downloads/life-os-command-center.html index.html
   git add index.html
   git commit -m "Initial Life OS dashboard"
   ```

3. **Push to GitHub**:
   ```bash
   git remote add origin https://github.com/your-username/life-os.git
   git branch -M main
   git push -u origin main
   ```

4. **Enable GitHub Pages**:
   - Go to repository Settings
   - Scroll to "Pages"
   - Select "Deploy from branch"
   - Choose `main` branch
   - Save

5. **Access your dashboard**:
   ```
   https://your-username.github.io/life-os/
   ```

### Important Note

**Each browser/device has separate storage**. If you open Life OS in different browsers, they won't share data. Consider:
- Using the same browser across devices
- Or manually exporting/importing data between devices
- Or see "Recommended Features" section for sync suggestions

---

## Customization Guide

### Changing Colors

Edit the CSS variables at the top of the `<style>` section:

```css
:root {
    --primary: #00d4ff;      /* Main cyan glow */
    --secondary: #0099ff;    /* Secondary blue */
    --accent: #ff006e;       /* Accent magenta */
    --success: #00ff88;      /* Green for status */
    --warning: #ffaa00;      /* Orange for warnings */
}
```

**Example**: Change primary from cyan to green:
```css
--primary: #00ff88;  /* Changes all cyan to green */
```

### Modifying Modules

To add a new domain or change existing ones, edit the `modules` array:

```javascript
const modules = [
    { 
        id: 'custom', 
        name: 'My Custom Domain', 
        icon: '🎨', 
        description: 'Custom description here', 
        storageKey: 'custom-domain', 
        defaultTasks: ['Task 1', 'Task 2'] 
    },
    // ... other modules
];
```

### Changing Default Tasks

For AFR or UXR, update the `defaultTasks` array:

```javascript
{ 
    id: 'afr', 
    name: 'Annual Review', 
    defaultTasks: [
        'NEW TASK 1',
        'NEW TASK 2',
        // ... replace or add
    ]
}
```

### Font Changes

Replace font imports at the top:

```css
@import url('https://fonts.googleapis.com/css2?family=FONT_NAME:wght@400;700&display=swap');
```

Then update CSS:
```css
body {
    font-family: 'FONT_NAME', monospace;
}
```

---

## Recommended Features

Life OS is designed as a **foundation** that can grow with your needs. Below are feature recommendations organized by impact and complexity, with specific implementation guidance.

---

### Tier 1: Quick Wins 🚀 (High Impact, Easy to Implement)

#### 1. **Task Priority Levels** ⭐ RECOMMENDED FIRST
Mark tasks as High/Medium/Low priority with visual indicators

**Features:**
- 🔴 High priority (red glow) — urgent, critical
- 🟡 Medium priority (yellow) — important but flexible
- 🟢 Low priority (green) — nice-to-have, can defer

**Visual changes:**
- Color-coded left border on tasks
- Sort by priority in detail view
- Dashboard shows "X high-priority tasks pending"

**Why this matters for you:**
- Teaching prep is critical; organizing desk isn't
- AFR has time-sensitive deadlines
- Helps you ignore low-impact tasks when overloaded

**Example workflow:**
```
PFW Teaching Module:
🔴 Grade midterm exams (High - due Friday)
🟡 Prepare next week's slides (Medium)
🟢 Update syllabus formatting (Low - can wait)
```

---

#### 2. **Task Time Estimates** ⭐ RECOMMENDED SECOND
Add realistic time per task for planning

**Features:**
- Quick select: "15 min" | "30 min" | "1 hour" | "2-4 hours" | "Full day"
- Show total time allocation per module
- Dashboard warning: "You have 12 hours of tasks in 8 hours available"

**Why this matters for you:**
- You're managing 9 life domains
- Helps prevent overcommitting
- Shows which weeks will be heavy

**Example:**
```
AFR Module tasks:
- Collect evaluations (1 hour)
- Write narrative (2-4 hours)
- Review goals (30 min)
→ Total: ~4 hours this week
```

---

#### 3. **Overall Stats Dashboard**
Display system-wide progress on the main overview

**What to show:**
- 📊 Overall completion % (all modules combined)
- 🔥 Active streak (days you've used Life OS)
- 📈 Trend indicator (↑ improving, ↓ declining)
- ⚠️ Neglected domains (lowest % complete)

**Why:**
- Motivating big-picture view
- Shows lifestyle balance at a glance
- Encourages daily engagement

**Example display:**
```
SYSTEM STATUS
Overall: 62% Complete
Streak: 12 days
Trend: ↑ +8% this week
⚠️ Health only 28% — attention needed
```

---

#### 4. **Time-Based Task Filtering**
Quickly show "Today", "This Week", "Overdue"

**How it works:**
- Tag tasks when created (today, this week, specific date)
- Filter buttons in detail view
- Focus on what's urgent

**Why:**
- Reduces cognitive load
- Quick prioritization without scanning everything
- Perfect for daily stand-ups

---

### Tier 2: Workflow Enhancements 🔧 (Medium Impact, Medium Effort)

#### 5. **Recurring Tasks** ⭐ RECOMMENDED THIRD
Automatically generate tasks on schedules

**Features:**
- Daily: "Morning review" appears every day
- Weekly: "Office hours" every Tuesday/Thursday
- Monthly: "Faculty meeting" on the 1st
- Custom: "Every 2 weeks" for biweekly projects

**Why this matters for you:**
- Faculty work is rhythmic and predictable
- Reduces manual re-entry of regular obligations
- Perfect for:
  - Office hours (weekly)
  - Department meetings (monthly)
  - Grading cycles (recurring)
  - Health check-ins (weekly)

**Example setup:**
```
PFW Service: "Attend committee meeting" (Monthly, 1st Tuesday)
Health: "Gym session" (3x per week: Mon/Wed/Fri)
PFW Teaching: "Office hours" (Weekly, Tue & Thu)
```

---

#### 6. **Notes & Subtasks per Task**
Click to expand any task for details and nested items

**Features:**
- Task notes: Add context without cluttering checklist
- Subtasks: Break "Grade assignments" into individual steps
- Links: Reference materials, URLs, contacts
- Quick preview on hover

**Why:**
- Some tasks require context
- Teaching prep needs topic details
- Meeting prep needs agenda items
- Helps avoid forgotten context

**Example:**
```
Task: Grade Database Assignments
Notes: Due by 5pm Friday for Monday review
Subtasks:
  □ ER Diagrams (Q1-Q4) — 20 min
  □ SQL Queries (Q5-Q8) — 30 min  
  □ Normalize scores and comment — 15 min
Link: https://blackboard.pfw.edu/...
```

---

#### 7. **Markdown Export**
Download any module as markdown file for your notes

**Features:**
- Export button in detail view
- Creates markdown with:
  ```markdown
  # AFR - Annual Faculty Review
  
  ## Completed ✅
  - [x] Collect teaching evaluations
  - [x] Document service contributions
  
  ## Pending
  - [ ] Write self-evaluation narrative
  - [ ] Set goals for next year
  
  Last updated: November 10, 2025
  ```
- Compatible with Obsidian, Notion, Apple Notes, etc.

**Why:**
- Integrates with your existing knowledge system
- Backup of your tasks
- Can share module status with advisor/committee

---

#### 8. **Dark/Light Theme Toggle**
Switch between neon dark and lighter modes

**Why:**
- Accessibility (some prefer lighter interfaces)
- Use light mode during day, dark at night
- Reduces eye strain based on preference

---

### Tier 3: Accountability & Reflection 📊 (Medium Impact, Requires Data Collection)

#### 9. **Weekly Digest Report** ⭐ TOP RECOMMENDATION FOR YOU
Auto-generated Sunday summary of your week

**What it shows:**
```
═══════════════════════════════════════
WEEKLY DIGEST — Week of Nov 3-9, 2025
═══════════════════════════════════════

MODULE PROGRESS
🎯 AFR              ████████░░ 80% (4/5 completed)
🧪 UXR Lab          ██░░░░░░░░ 20% (1/5 completed)
🏠 Home             ███████░░░ 70% (7/10 completed)
👨‍👩‍👧‍👦 Family          ██░░░░░░░░ 20% (1/5 completed) ⚠️
📚 PFW Teaching     █████████░ 90% (9/10 completed) ✅
🏛️ PFW Service      ███░░░░░░░ 30% (2/6 completed)
🌳 Bellon Branch    ░░░░░░░░░░ 0% (0/3 completed) ⚠️
🏡 Olde Oak Tree    ████░░░░░░ 40% (4/10 completed)
💪 Health           ████░░░░░░ 40% (2/5 completed) ⚠️

TRENDS & INSIGHTS
• Teaching is thriving! Up 15% from last week
• Family domain neglected — only 20% complete
• Health remains low — consider Tuesday gym commitment?
• AFR momentum good, stay focused on narrative

TIME ANALYSIS
Total time invested: 14 hours
Heaviest domains: Teaching (5h), AFR (4h)
Lightest domains: Bellon Branch (0h), Family (1h)

RECOMMENDATIONS FOR WEEK OF NOV 10-16
1. ✅ Maintain Teaching momentum (high productivity)
2. ⚠️ Schedule Family time — plan dinner together?
3. 🔥 Start Bellon Branch tasks — 3 pending
4. 💪 Commit to 3x gym sessions (Mon/Wed/Fri)
5. 🎯 Final push on AFR before deadline

═══════════════════════════════════════
```

**Why this is CRITICAL for you:**
- You manage 9 life domains — easy to ignore some
- Faculty life naturally emphasizes Teaching/Service
- Weekly check-in prevents "I haven't seen family in 3 weeks" moments
- Data-driven insights help rebalance
- Actionable recommendations for next week

**How to use it:**
- Set calendar reminder for Sunday 6pm
- Review for 5 minutes over coffee
- Plan next week based on recommendations

---

#### 10. **Browser Notifications & Daily Digest**
Passive reminders without checking the app

**Features:**
- Morning notification: "3 tasks pending today"
- Afternoon check-in: "You're 50% through today's tasks"
- Evening wrap-up: "Completed 5 tasks, 2 remaining"

**Optional email version:**
- Daily 6am summary to your inbox
- Shows urgent tasks for the day
- Can't miss important deadlines

**Why:**
- Keeps you accountable without obsessive checking
- Reduces decision fatigue
- Gentle nudge for neglected domains

---

#### 11. **Heatmap Calendar** 📅
Visual grid showing your activity patterns

**What it shows:**
```
November 2025
Mo Tu We Th Fr Sa Su
               1  2
 3  4  5  6  7  8  9   ← Week row (darker = more active)
10 11 12 13 14 15 16
17 18 19 20 21 22 23
24 25 26 27 28 29 30

Green = Highly active (8+ hours, 10+ tasks)
Yellow = Moderate (4-8 hours, 5-10 tasks)
Gray = Low activity (< 2 hours, 1-3 tasks)
White = No activity
```

**Why:**
- Spot patterns: "Always productive Mon-Wed, weekend slump"
- See neglected weeks
- Motivation to maintain streaks
- Identify work/life balance swings

---

### Tier 4: Intelligence & Insights 🧠 (Advanced Features, Algorithmic)

#### 12. **Performance Analytics Dashboard**
Data-driven insights into your patterns

**Metrics to track:**
- Module completion rates over time (is Teaching always 90%?)
- Domain correlations (when AFR > 80%, does Health drop?)
- Productivity timing (most active Mon-Wed?)
- Task completion velocity (how long does typical task take?)

**Example insights:**
```
🔍 PATTERNS DETECTED

You complete Teaching tasks 25% faster than other modules
Average: 15 min per Teaching task vs 40 min per Home task

When AFR completion rises above 70%, Health drops to 35%
→ Suggestion: Schedule gym during high-AFR weeks

You're most productive Tuesday-Thursday (88% completion)
Weekends see only 32% completion
→ Consider lighter planning for weekends?
```

**Why:**
- Reveals hidden patterns in your work style
- Helps optimize schedule
- Identifies when work-life balance tips

---

#### 13. **AI-Powered Suggestions**
Smart recommendations based on your behavior

**Examples:**
- "You haven't touched Bellon Branch in 8 days. Time to check in?"
- "Family is 25% below your average. Consider a family dinner?"
- "You've completed 15 AFR tasks in 2 days. Slow down, pace yourself?"
- "Health tasks show 40% completion. Weekly gym habit for 30 days?"

**Why:**
- Gentle accountability nudges
- Helps rebalance without guilt
- Learns your patterns over time

---

### Tier 5: Integration & Sync 🔗 (Advanced, Cross-Platform)

#### 14. **iCloud Sync**
Automatically sync data across Mac, iPhone, iPad

**Why:**
- Access dashboard on the go
- Same data everywhere
- Perfect for traveling or working from different devices
- No manual export/import

**Technical approach:** Service Workers + IndexedDB

---

#### 15. **Apple Reminders Sync**
Two-way integration with native Apple Reminders

**Why:**
- Unified with other reminders you use
- Get notifications on Mac, iPhone, Apple Watch
- Use Siri: "Hey Siri, add to PFW Teaching"

---

#### 16. **GitHub Issues Sync** (For Tech Folks)
Link to your GitHub repositories

**Why:**
- If you use GitHub for project management
- Auto-sync Teaching prep or research project issues
- Link to PRs, code reviews, etc.

---

---

## Recommended Implementation Roadmap

### **My Top 3 for You (Start Here)**

Based on your role as educator + researcher + manager of 9 life domains:

**#1: Task Priority Levels + Time Estimates**
- Implement together (one feature set)
- Helps you decide "what to do when"
- Essential for overloaded schedules
- Quick to build (~2 hours)

**#2: Recurring Tasks**
- Faculty work is rhythmic
- Eliminates manual re-entry
- Will save you hours per month
- Time to build: ~3 hours

**#3: Weekly Digest Report**
- The "killer app" for your 9-domain system
- Prevents neglecting entire life areas
- Data-driven rebalancing tool
- Time to build: ~4-5 hours

---

### **Suggested Timeline**

```
Week 1: Deploy to GitHub Pages + daily usage
Week 2: Add priority levels + time estimates
Week 3: Add recurring tasks
Week 4: Add markdown export
Week 5: Implement weekly digest report ← Key milestone!
Month 2: Add browser notifications
Month 3: Add heatmap calendar
Month 4+: Analytics dashboard, AI suggestions, sync
```

---

## Implementation Priority Guide

### **Phase 1: Foundation** (Current)
- ✅ Dashboard overview
- ✅ 9 modules with progress tracking
- ✅ Local persistent storage

### **Phase 2: Daily Usability** (Weeks 1-3)
- 🔴 Priority levels (High/Medium/Low)
- ⏱️ Time estimates
- 🔄 Recurring tasks

### **Phase 3: Intentional Living** (Weeks 3-5)
- 📝 Notes & subtasks
- 📤 Markdown export
- 📊 Weekly digest report ← The "aha!" moment

### **Phase 4: Accountability** (Month 2)
- 🔔 Notifications & reminders
- 📅 Heatmap calendar
- 📈 Overall stats dashboard

### **Phase 5: Intelligence** (Month 3+)
- 🧠 Analytics dashboard
- 💡 AI suggestions
- 🔗 Sync (iCloud/Apple Reminders)
- 📱 Mobile app (PWA)

---

## Why These Matter for Your Workflow

**You manage:**
- 7 life domains (Home, Family, Teaching, Service, Bellon, Oak, Health)
- 2 major projects (AFR, UXR Lab)
- Multiple roles (educator, researcher, manager, professor)

**The problem:**
- Easy to hyperfocus on Teaching (your strength)
- Health/Family/Home get neglected
- Annual Review sneaks up on you
- No reflection on overall balance

**The solution:**
- Priority levels + time estimates = realistic daily planning
- Recurring tasks = less decision fatigue
- Weekly digest = forced reflection + rebalancing
- This combination creates **intentional living**, not reactive scrambling

---

---

## Quick Reference

### Keyboard Shortcuts (Future)
- `Cmd+K` - Command palette
- `Cmd+N` - New task
- `Cmd+E` - Export data
- `Cmd+?` - Help

### URL Parameters (Future)
- `?module=afr` - Open AFR directly
- `?view=week` - Weekly view
- `?theme=light` - Light theme

### Browser Compatibility
- ✅ Safari (Mac)
- ✅ Chrome/Chromium
- ✅ Firefox
- ✅ Mobile browsers (responsive)

---

## Troubleshooting

### Data Not Saving?
1. Check if localStorage is enabled (Safari: Preferences > Privacy > Website data)
2. Clear cache and reload
3. Try a different browser to verify

### Progress Ring Not Updating?
1. Refresh the page (`Cmd+R`)
2. Check console for errors (`Cmd+Option+I`)

### Want to Reset Everything?
In browser console:
```javascript
localStorage.clear();
location.reload();
```

---

## Future Vision

Life OS is designed as a **foundation** for your personal operating system:

- 📊 **Current**: Command center dashboard
- 🔮 **Next**: AI suggestions based on patterns
- 🎯 **Later**: Connected to other tools (calendar, email, GitHub, etc.)
- 🚀 **Future**: Voice commands, smart scheduling, life insights

---

## Support & Customization

For specific feature requests or customizations, document:
- What problem does it solve?
- How would you use it daily?
- What's the expected outcome?

This helps prioritize what to build next.

---

## License & Credits

**Life OS v2.0**
- Design inspired by JARVIS (Marvel)
- Built with vanilla HTML/CSS/JavaScript
- 100% open-source, fully yours to modify

**Fonts:**
- Orbitron (Google Fonts)
- Space Mono (Google Fonts)

---

*Last updated: November 2025*

*Created for Jay @ Purdue Fort Wayne*
