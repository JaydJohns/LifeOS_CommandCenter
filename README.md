# Life OS - Command Center

A personal command center dashboard for managing multiple life domains and projects. Think of it as your personal operating system for tracking work across all areas of your life.

![Status](https://img.shields.io/badge/status-MVP%20v2.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Dependencies](https://img.shields.io/badge/dependencies-0-brightgreen)

## Live Demo

🚀 **[Launch Life OS Command Center](https://jaydjohns.github.io/LifeOS_CommandCenter/)**

## Overview

Life OS is a fully self-contained, single-page web application that provides unified visibility into 9 life domains and 2 major projects. Built with vanilla HTML/CSS/JavaScript—no frameworks, no build tools, no external dependencies.

### Core Philosophy

Instead of juggling separate to-do lists for work, home, family, health, and side projects, Life OS gives you one unified dashboard where you can see everything at a glance, filter by priority/time, and track progress across all life areas simultaneously.

## Features

### ✨ Current (MVP - Tier 1)

- **9 Life Domains + 2 Projects**
  - Annual Faculty Review • UXR Lab • Home • Family
  - Teaching • Service • Bellon Branch • Olde Oak Tree • Health

- **Task Management with Metadata**
  - ✅ Checkboxes for completion status
  - 🎯 Priority levels (High/Medium/Low) with visual indicators
  - ⏱️ Time estimates in hours
  - 📅 Automatic timestamps on task creation

- **Smart Filtering**
  - View all tasks, today's tasks, this week's tasks, or only high-priority items
  - Real-time filter updates with active button states

- **Global Statistics Dashboard**
  - Tasks completed / total
  - Overall completion rate
  - Hours completed / total hours
  - High-priority task progress
  - Hour-based completion metrics

- **Data Persistence**
  - Browser localStorage for offline-first experience
  - No server, no cloud sync required
  - Each domain stores independently

- **Responsive Design**
  - Works on desktop, tablet, and mobile
  - Touch-friendly interface
  - JARVIS-inspired neon theme with glassmorphism effects

- **Form Validation**
  - Task description length checks (max 100 chars)
  - Time estimate range validation (0-1000 hours)
  - Required field enforcement

### 🎯 Planned (Tier 2-5)

**Tier 2 - Workflow Enhancements**
- Recurring tasks
- Notes & subtasks per task
- Markdown export
- Dark/light theme toggle

**Tier 3 - Accountability & Reflection**
- Weekly digest reports
- Browser notifications
- Heatmap calendar view

**Tier 4 - Intelligence**
- Performance analytics
- AI-powered suggestions

**Tier 5 - Sync & Integration**
- iCloud synchronization
- Apple Reminders sync
- GitHub Issues integration

## Getting Started

### Option 1: Live on Web (Easiest)
Simply visit: **[Life OS Command Center](https://jaydjohns.github.io/LifeOS_CommandCenter/)**

No installation needed. All data stored locally in your browser.

### Option 2: Self-Host
1. Clone this repository:
   ```bash
   git clone https://github.com/JaydJohns/LifeOS_CommandCenter.git
   cd LifeOS_CommandCenter
   ```

2. Open `life-os-command-center.html` in your browser (double-click the file)

3. (Optional) Serve with a local web server:
   ```bash
   # Python 3
   python -m http.server 8000

   # Node.js
   npx http-server
   ```
   Then visit `http://localhost:8000`

## Usage

### Dashboard View
1. Launch the application
2. See all 9 modules with progress bars at a glance
3. Click any module card to enter detail view

### Module Detail View
1. **View Tasks**: See all tasks with priority indicators and time estimates
2. **Check Off**: Click checkbox to mark tasks complete
3. **Filter**: Use filter buttons (All/Today/Week/High Priority)
4. **Add Task**:
   - Enter task description
   - Select priority level
   - Set time estimate in hours
   - Click "Execute Task"
5. **Track Progress**: Circular progress indicator updates in real-time

### Global Stats
Monitor your overall progress at the top of the dashboard:
- Total task completion rate
- Hours of work completed vs. planned
- High-priority task completion

## Customization

### Colors & Theme
Edit CSS variables at the top of `life-os-command-center.html` (lines 16-28):

```css
:root {
    --primary: #00d4ff;          /* Cyan glow - main accent */
    --secondary: #0099ff;        /* Blue - secondary accent */
    --accent: #ff006e;           /* Magenta - high priority */
    --success: #00ff88;          /* Green - low priority */
    --warning: #ffaa00;          /* Orange - medium priority */
    --bg-dark: #0a0e27;          /* Dark background */
    --bg-darker: #050810;        /* Darker background */
    --text-primary: #e0f7ff;     /* Light text */
    --text-secondary: #7dd3fc;   /* Dimmed text */
}
```

### Modules & Default Tasks
Edit the `modules` array in the JavaScript section (lines 934-944):

```javascript
const modules = [
    {
        id: 'custom',
        name: 'Your Module Name',
        icon: '🎨',
        description: 'Module description',
        storageKey: 'custom-domain',
        defaultTasks: ['Task 1', 'Task 2', 'Task 3']
    },
    // ... more modules
];
```

### Fonts
Update Google Fonts import at top of `<style>` section:
```html
@import url('https://fonts.googleapis.com/css2?family=Your+Font:wght@400;700&display=swap');
```

## Browser Support

✅ **Tested & Working**
- Chrome/Edge (latest)
- Firefox (latest)
- Safari (latest)
- Mobile browsers (iOS Safari, Chrome Mobile)

❌ **Not Supported**
- Internet Explorer (no ES6 support)
- Very old browser versions

**Required Features**
- ES6 JavaScript support
- CSS Grid & Custom Properties
- localStorage API
- Backdrop-filter (CSS)
- Conic-gradient (CSS)

## Data Storage

All data is stored in your browser's **localStorage** with these keys:

- `life-os-afr-tasks` - Annual Faculty Review tasks
- `life-os-uxr-tasks` - UXR Lab tasks
- `home-domain` - Home domain tasks
- `family-domain` - Family domain tasks
- `teaching-domain` - Teaching tasks
- `service-domain` - Service tasks
- `bellon-domain` - Bellon Branch tasks
- `oak-domain` - Olde Oak Tree tasks
- `health-domain` - Health tasks

**Each task is stored as:**
```json
{
  "taskId": {
    "completed": false,
    "priority": "high",
    "estimate": 2.5,
    "label": "TASK DESCRIPTION",
    "createdAt": 1699123456789
  }
}
```

### Exporting Your Data
Open browser DevTools (F12) → Console and run:
```javascript
// Export all data
const allData = {};
['life-os-afr-tasks', 'life-os-uxr-tasks', 'home-domain', 'family-domain',
 'teaching-domain', 'service-domain', 'bellon-domain', 'oak-domain', 'health-domain']
.forEach(key => {
  allData[key] = JSON.parse(localStorage.getItem(key));
});
console.log(JSON.stringify(allData, null, 2));
```

Copy the output and save as `backup.json`

## Offline Usage

Life OS is **100% offline capable**. Once loaded, the application works completely without internet:
- ✅ Create and edit tasks
- ✅ Check off completions
- ✅ View statistics
- ✅ Switch between modules
- ❌ No cloud sync (stored locally only)

## Multi-Device Sync

Currently, each device/browser has independent storage. To share data across devices:

1. **Export** data from Device A (see above)
2. **Import** by pasting into DevTools console on Device B:
   ```javascript
   const data = {...}; // your exported JSON
   Object.entries(data).forEach(([key, value]) => {
     localStorage.setItem(key, JSON.stringify(value));
   });
   location.reload();
   ```

Future versions will support automatic iCloud/cloud sync.

## Keyboard Shortcuts (Planned)

Coming in next update:
- `Cmd+K` / `Ctrl+K` - Command palette
- `Cmd+N` / `Ctrl+N` - New task
- `Cmd+E` / `Ctrl+E` - Export data
- `Cmd+?` / `Ctrl+?` - Help/Shortcuts

## Development

### Project Structure
```
LifeOS_CommandCenter/
├── life-os-command-center.html    # Main app (HTML/CSS/JS combined)
├── LIFE-OS-DOCUMENTATION.md       # Detailed feature documentation
└── README.md                       # This file
```

### No Build Process
This is vanilla JavaScript—no npm, webpack, or build tools needed. Edit the HTML file directly.

### Testing
1. Open in browser
2. Click through each module
3. Add tasks with different priorities/estimates
4. Use filters to verify functionality
5. Check browser console (F12) for any errors
6. Test localStorage persistence (reload page, data persists)

## Contributing

Found a bug? Have a feature idea?

1. Test in latest browser versions
2. Open an issue with details
3. Or submit a pull request!

Areas for contribution:
- Bug fixes
- Performance optimizations
- Accessibility improvements
- Documentation updates
- Feature implementations from Tier 2-5 roadmap

## License

MIT License - feel free to use, modify, and distribute.

## Architecture Decisions

**Why vanilla JavaScript?**
- Single HTML file is easier to share and deploy
- No dependency management overhead
- Works offline with zero build process
- Better for long-term maintainability

**Why localStorage?**
- No backend server required
- True offline-first experience
- Privacy-first (all data stays on user's device)
- Future versions will add cloud sync option

**Why this design?**
- JARVIS theme inspires focus and productivity
- All 9 domains visible at once = accountability
- Priority + time estimates = better time management
- Glassmorphism effects = modern, polished feel

## Roadmap

See `LIFE-OS-DOCUMENTATION.md` for detailed feature roadmap across 5 tiers.

**Current Version**: v2.0 (MVP - Tier 1 Complete)
**Next Release**: Tier 2 features (Q1 2025)

## Support

- 📖 Read `LIFE-OS-DOCUMENTATION.md` for detailed guides
- 🐛 Found a bug? Open an issue
- 💡 Have an idea? Create a discussion
- ❓ Questions? Check the FAQ in documentation

## Acknowledgments

- JARVIS UI inspiration
- Built for busy professionals managing multiple life domains
- Special thanks to all beta testers and feedback providers

---

**Life OS: Where productivity meets life balance.**

Stay organized. Stay focused. Stay balanced. 🚀
