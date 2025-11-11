# Life OS Command Center - Claude.md

## Project Overview

**Life OS** is a personal command center dashboard for managing multiple life domains and projects. It's a fully self-contained, single-page web application (SPA) built with vanilla HTML/CSS/JavaScript—no frameworks, no build tools, no external dependencies.

**Current Version**: v2.0 (MVP - Tier 1 Complete)
**Live Demo**: https://jaydjohns.github.io/LifeOS_CommandCenter/
**Repository**: https://github.com/JaydJohns/LifeOS_CommandCenter

## Technology Stack

- **Frontend**: Vanilla JavaScript (ES6+), HTML5, CSS3
- **Storage**: Browser localStorage API (no backend)
- **Deployment**: GitHub Pages
- **Build Process**: None (single HTML file)
- **Testing**: Manual browser testing
- **Version Control**: Git/GitHub

## Architecture Overview

### Core Philosophy
All code is contained in a single `index.html` file (~160KB) combining HTML structure, CSS styling, and JavaScript logic. This approach ensures:
- Single file to deploy
- Easy offline usage
- Zero dependency management
- Long-term maintainability

### Data Flow
```
User Input → Event Handler → Business Logic → localStorage → UI Render
```

### State Management
- **Local State**: `currentModule`, `currentFilter`, `currentEditingTaskId`
- **Global State**: All data persisted in browser localStorage
- **Undo/Redo**: UndoRedoManager closure with dual stacks (max 50 states)

## Development Workflow & Agent Suite

**Active Agent Suite**: Dreamweavers Development Agent Suite (v1.0)
**Orchestration**: Workflow Orchestrator Agent (routing and coordination)
**See**: `claudeSubAgents.md` for complete agent capabilities and routing guide

### Agent Integration Pattern
- **Entry Point**: Workflow Orchestrator - intelligent task routing and workflow coordination
- **Core Development**: Software Development Agent for implementation and architecture
- **Quality Assurance**: Code Review & Analysis Agent for security, performance, and maintainability
- **Testing**: QA & Testing Strategy Agent for validation and test planning
- **Documentation**: Technical Documentation Agent for guides, API docs, and ADRs
- **Project Coordination**: Project Manager Agent for task breakdown and timeline management

### Current Workflow State
All multi-phase development tasks are coordinated through the Workflow Orchestrator, which:
- Routes tasks to appropriate specialized agents
- Manages phase dependencies and critical paths
- Tracks progress across agent handoffs
- Maintains context and ensures knowledge transfer
- Optimizes workflows based on task patterns

See `claudeSubAgents.md` for detailed agent descriptions and routing guidelines.

## Project Structure

### Single File Organization (`index.html`)

```
1. HTML Structure (lines 1-1623)
   - Meta tags & styles section
   - DOM elements for views
   - Modal dialogs for notes/export

2. JavaScript (lines 1625-end)
   - Constants & Configuration (STORAGE_KEYS, modules)
   - Storage Utilities (validation, error handling)
   - Undo/Redo Manager (state management)
   - Core Functions:
     * Dashboard rendering (renderDashboard)
     * Module detail view (renderDetailView)
     * Task operations (add, delete, edit, complete)
     * Global stats calculation (updateGlobalStats)
   - UI Functions (modals, themes, filters)
   - Event listeners & initialization
```

## Life Domains (9 Total)

1. **Annual Faculty Review (AFR)** - Faculty evaluation and self-assessment
2. **UXR Lab** - User research projects and studies
3. **Home** - Household management and operations
4. **Family** - Family relationships and activities
5. **PFW Teaching** - Course development and engagement
6. **PFW Service** - University committees and obligations
7. **Bellon Branch** - Branch operations and management
8. **Olde Oak Tree** - Property and asset management
9. **Health** - Personal wellness and fitness

## Key Features Implemented

### Tier 1 (Complete - MVP)
- ✅ Task management with priority levels (High/Medium/Low)
- ✅ Time estimates in hours
- ✅ Completion checkboxes
- ✅ Smart filtering (All/Today/This Week/High Priority)
- ✅ Global statistics dashboard
- ✅ localStorage persistence
- ✅ Responsive design
- ✅ JARVIS-inspired theme

### Tier 2 (Complete)
- ✅ Dark/Light theme toggle
- ✅ Markdown export
- ✅ Recurring tasks (daily/weekly/monthly)
- ✅ Notes and subtasks per task

### Tier 3 (Complete)
- ✅ Weekly digest reports
- ✅ Browser notifications
- ✅ Heatmap calendar view

### Tier 4 (Complete)
- ✅ Performance analytics dashboard
- ✅ Trends and metrics

### Tier 5 (Complete)
- ✅ Sync and integration center
- ✅ Data portability

### Recent Additions
- ✅ Undo/Redo functionality with keyboard shortcuts (Cmd+Z/Cmd+Shift+Z)
- ✅ Comprehensive localStorage validation and error handling
- ✅ Automatic data repair for corrupted tasks
- ✅ Storage quota checking and error recovery

## Data Storage & Migration

### localStorage Keys
```javascript
{
  'life-os-afr-tasks': {...},           // Annual Faculty Review
  'life-os-uxr-tasks': {...},           // UXR Lab
  'home-domain': {...},                 // Home tasks
  'family-domain': {...},               // Family tasks
  'teaching-domain': {...},             // Teaching tasks
  'service-domain': {...},              // Service tasks
  'bellon-domain': {...},               // Bellon tasks
  'oak-domain': {...},                  // Olde Oak tasks
  'health-domain': {...}                // Health tasks
}
```

### Data Structure Evolution

The application supports two data formats with automatic migration between them.

#### Legacy Format (Flat Task Structure)
Used in earlier versions. Stored tasks directly at module level:

```javascript
{
  "taskId1": {
    "completed": false,
    "priority": "high",
    "estimate": 2.5,
    "label": "Task description",
    "createdAt": timestamp,
    "dueDate": timestamp,
    "recurrence": "none" | "daily" | "weekly" | "monthly",
    "notes": "optional notes",
    "subtasks": [
      { "label": "Subtask", "completed": false }
    ]
  },
  "taskId2": { ... }
}
```

#### New Format (Project-Based Structure)
Current format. Organizes tasks within projects:

```javascript
{
  "projects": {
    "general": {
      "id": "general",
      "name": "General",
      "createdAt": timestamp,
      "tasks": {
        "taskId1": { /* task object */ },
        "taskId2": { /* task object */ }
      }
    },
    "proj_abc123": {
      "id": "proj_abc123",
      "name": "Project Name",
      "createdAt": timestamp,
      "tasks": {
        "taskId3": { /* task object */ },
        "taskId4": { /* task object */ }
      }
    }
  }
}
```

### Automatic Migration

#### migrateToProjects() Function (Lines 2316-2476)

Handles automatic migration from old to new format. Runs on app initialization and handles:

**1. Mixed-Format Data Detection**
- Detects when old task properties (label, completed, priority, estimate, etc.) are mixed into the projects object
- Validates that projects object contains only valid keys (proj_*, 'general')
- Protects against data corruption

**2. Data Reorganization Paths**
- **Flat to Projects**: All old tasks wrapped in "General" project
- **Projects + Old Tasks**: Coexisting data properly separated
- **Corrupted Mixed Data**: Task properties extracted, projects preserved
- **Empty Data**: Initializes clean project structure
- **Already Migrated**: Detects and skips redundant migrations

**3. Data Preservation**
- No task data is lost
- Project IDs are preserved
- Corrupted properties are safely extracted
- Legacy tasks wrapped in special "General" project

**4. Emergency Reset**
```javascript
// If data is severely corrupted (parse error):
// 1. Removes corrupted data
// 2. Initializes clean project structure
// 3. Logs warning to console
// 4. User continues with blank slate
```

#### Data Validation: getStorageData() Function (Lines 2174-2180)

Prevents new project data from being corrupted by legacy validation:

```javascript
// Check if this is the new project-based structure
const isProjectStructure = parsed.projects &&
                          typeof parsed.projects === 'object' &&
                          Object.keys(parsed).length === 1;

if (isProjectStructure) {
  // New project-based structure - return as-is
  return parsed;
}

// Only apply old task validation to flat structures
// ... old validation logic
```

**Key Difference**:
- Legacy validation expects flat task objects with label/completed/priority
- Project structure has nested format with projects → tasks hierarchy
- Early return prevents misvalidating new structure with old rules

### The "General" Project

Special project that preserves old tasks during migration:

```javascript
{
  "id": "general",
  "name": "General",
  "createdAt": timestamp,
  "tasks": {
    // Contains all tasks migrated from legacy format
    "legacy_task": { /* task from old format */ }
  }
}
```

**Purpose**:
- Provides a home for orphaned old tasks
- Ensures no task data is lost during migration
- Users can reorganize tasks into projects later
- Acts as catch-all for corrupted task properties

### Migration Console Messages

When migration occurs, you'll see:
```
// Fixing mixed-format data
Fixing corrupted projects object for life-os-afr-tasks
Fixed corrupted projects object for life-os-afr-tasks

// Normal migration
Migrated tasks to projects for home-domain
Fixed mixed-format data for life-os-uxr-tasks, found 2 projects and 5 old tasks
```

### Troubleshooting Data Issues

**Issue**: "Projects aren't displaying"
- Check browser console for migration messages
- Run full page reload (Cmd+R)
- Clear cache if problem persists

**Issue**: "Tasks disappeared after update"
- Check console for "Emergency reset performed" warning
- Data was corrupted but should be auto-recovered
- Contact support with console logs

**Issue**: "Mixed data messages appearing"
- Normal during first load after v2.1 update
- Indicates old format being converted to new format
- Safe - all data is preserved
- Only happens once per module

## Storage Utilities

### Safe Retrieval: `getStorageData(key)`
- Checks if localStorage is available
- Parses JSON with error handling
- Validates task data structure
- Auto-repairs corrupted data
- Returns empty object on failure

### Safe Storage: `setStorageData(key, data)`
- Checks storage availability
- Validates data size (5MB limit)
- Catches QuotaExceededError
- Returns boolean success status
- Logs errors to console

### Validation: `isValidTask(task)`
- Checks required fields (label, completed, priority, estimate, createdAt)
- Validates field types
- Checks priority values
- Validates optional fields (recurrence, notes, subtasks)

## Undo/Redo System

### UndoRedoManager (Closure Pattern)
```javascript
UndoRedoManager.saveState(description) // Saves current state
UndoRedoManager.undo()                 // Restores previous state
UndoRedoManager.redo()                 // Restores undone state
UndoRedoManager.canUndo()              // Check if undo available
UndoRedoManager.canRedo()              // Check if redo available
UndoRedoManager.clear()                // Clear all history
```

### History Limits
- Max 50 states stored
- Each state captures all modules' data
- Oldest states removed when limit exceeded

### Keyboard Shortcuts
- **Undo**: `Cmd+Z` or `Ctrl+Z`
- **Redo**: `Cmd+Shift+Z` or `Ctrl+Shift+Z`
- **Redo (Alt)**: `Cmd+Y` or `Ctrl+Y`

## Important Functions

### Core Task Operations
- `addDetailTask()` - Create new task with validation
- `deleteTask(taskId)` - Remove task with confirmation
- `editTask(taskId)` - Modify task properties
- `updateModuleProgress(taskId)` - Handle checkbox completion
- `renderDetailView(module)` - Display tasks for module
- `renderDashboard()` - Show overview of all modules

### Storage Functions
- `getStorageData(key)` - Safe data retrieval
- `setStorageData(key, data)` - Safe data storage
- `isStorageAvailable()` - Check localStorage availability
- `validateAllStorage()` - Validate all modules

### Notes & Subtasks
- `openNotesModal(taskId)` - Show notes/subtasks dialog
- `addSubtask()` - Create subtask
- `deleteSubtask(index)` - Remove subtask
- `toggleSubtask(index)` - Mark subtask complete/incomplete
- `saveTaskDetails()` - Save notes and subtasks

### UI Functions
- `updateGlobalStats()` - Calculate and display global metrics
- `setFilter(filterType)` - Apply task filter
- `toggleTheme()` - Switch dark/light theme
- `exportModuleMarkdown()` - Export tasks as markdown

### Undo/Redo Functions
- `undoAction()` - Trigger undo
- `redoAction()` - Trigger redo
- `updateUndoRedoButtons()` - Sync button UI state

## Development Guidelines

### Code Style
- Vanilla JavaScript (no frameworks)
- Single responsibility functions
- Consistent indentation (4 spaces)
- Descriptive variable names
- Comments for complex logic

### Adding New Features
1. **Keep it in one file**: Don't split code unless essential
2. **Update modules array**: Add to `modules` constant if adding new domain
3. **Use safe storage**: Always use `getStorageData()`/`setStorageData()`
4. **Add error handling**: Wrap operations in try-catch
5. **Save undo state**: Call `UndoRedoManager.saveState()` before mutations
6. **Update buttons**: Call `updateUndoRedoButtons()` after state changes

### Validation Pattern
```javascript
// 1. Get safe data
const tasks = getStorageData(module.storageKey);

// 2. Save undo state BEFORE mutations
UndoRedoManager.saveState('Action description');

// 3. Perform operation
// ... modify tasks object

// 4. Safe save
if (!setStorageData(module.storageKey, tasks)) {
  console.error('Failed to save');
  return;
}

// 5. Update UI
renderDetailView(module);
updateUndoRedoButtons();
```

### Error Handling Pattern
```javascript
try {
  // Operation code
} catch(error) {
  console.error('Error message:', error);
  // Graceful fallback
}
```

## Running & Testing

### Local Development
1. Open `index.html` directly in browser (no build process needed)
2. Or serve with: `python -m http.server 8000`
3. Visit `http://localhost:8000`

### Testing Checklist
- [ ] Create tasks in multiple modules
- [ ] Test completion checkboxes
- [ ] Apply filters (All/Today/Week/High Priority)
- [ ] Edit task properties
- [ ] Delete tasks
- [ ] Add notes and subtasks
- [ ] Test undo/redo (Cmd+Z, Cmd+Shift+Z)
- [ ] Toggle theme (dark/light)
- [ ] Export markdown
- [ ] Reload page (verify localStorage persistence)
- [ ] Test on mobile (responsive design)

### Browser Support
- ✅ Chrome/Edge (latest)
- ✅ Firefox (latest)
- ✅ Safari (latest)
- ✅ Mobile browsers
- ❌ Internet Explorer (no ES6 support)

### Required Browser Features
- ES6 JavaScript support
- CSS Grid & Custom Properties
- localStorage API
- Backdrop-filter
- Conic-gradient
- Fetch API (for potential future features)

## CSS Variables (Customization)

Located at top of `<style>` section:
```css
:root {
  --primary: #00d4ff;          /* Cyan glow */
  --secondary: #0099ff;        /* Blue */
  --accent: #ff006e;           /* Magenta (high priority) */
  --success: #00ff88;          /* Green (low priority) */
  --warning: #ffaa00;          /* Orange (medium priority) */
  --bg-dark: #0a0e27;          /* Dark background */
  --bg-darker: #050810;        /* Darker background */
  --text-primary: #e0f7ff;     /* Light text */
  --text-secondary: #7dd3fc;   /* Dimmed text */
}
```

## Modules Configuration

Edit the `modules` array to customize:
```javascript
const modules = [
  {
    id: 'custom',
    name: 'Module Name',
    icon: '🎨',
    description: 'Module description',
    storageKey: 'custom-domain',
    defaultTasks: ['Task 1', 'Task 2']
  },
  // ... more modules
];
```

## Recent Development History

### Session 1: Undo/Redo Implementation
- Added UNDO/REDO buttons to header
- Created UndoRedoManager with dual stacks (max 50 states)
- Integrated keyboard shortcuts (Cmd+Z, Cmd+Shift+Z, Cmd+Y)
- Integrated saveState() into all task operations
- Commit: `20e6301` - "Integrate undo/redo state management"

### Session 2: Storage Validation & Error Handling
- Created comprehensive storage utility functions
- Added data validation with automatic repair
- Implemented quota checking
- Wrapped all task operations in try-catch
- Updated all major functions to use safe storage
- Commit: `eec0245` - "Add comprehensive localStorage validation"

## Pending Features

### Tier 2-5 Planned
- [ ] Keyboard shortcuts (Cmd+K command palette, Cmd+N new task, etc.)
- [ ] URL parameters (?module=, ?view=, ?theme=)
- [ ] Comprehensive test suite
- [ ] User onboarding guide
- [ ] iCloud/cloud sync
- [ ] Apple Reminders integration

## Common Patterns

### Getting Module Data
```javascript
const tasks = getStorageData(currentModule.storageKey);
const taskIds = Object.keys(tasks);
```

### Iterating Tasks
```javascript
taskIds.forEach(id => {
  const task = tasks[id];
  if (typeof task === 'object' && task.label) {
    // Process task
  }
});
```

### Updating UI After Data Change
```javascript
renderDetailView(currentModule);  // Refresh current view
renderDashboard();                // Update overview
updateGlobalStats();              // Recalculate stats
updateUndoRedoButtons();          // Sync undo/redo
```

## Troubleshooting

### localStorage Full Error
- User should export data and clear browser cache
- App continues to work with in-memory data until refresh
- Check browser console for `QuotaExceededError`

### Data Corruption Detected
- Automatic repair runs when data is loaded
- Check browser console for `Repaired corrupted data` messages
- Original data is removed, repaired version saved

### Storage Unavailable
- App returns empty objects and continues functioning
- Check if browser is in private/incognito mode
- Check if localStorage is disabled in browser settings

## Performance Notes

- Single HTML file (~160KB) loads in milliseconds
- localStorage operations are synchronous but fast for task data volumes
- Undo/Redo history limited to 50 states to prevent memory bloat
- Dashboard rendering optimized for 9 modules

## Security Considerations

- All data stored locally (no server/cloud by default)
- No authentication required
- No external API calls
- Safe from XSS (no user input rendered as HTML)
- Data never leaves user's device (privacy-first)

## File Locations

- **Main Application**: `index.html` (single file)
- **Documentation**: `README.md`, `LIFE-OS-DOCUMENTATION.md`
- **Repository**: `.git/` (Git version control)
- **Deployed**: GitHub Pages (`gh-pages` branch handled by GitHub)

## Resources

- **GitHub Issues**: Report bugs or request features
- **Documentation**: See `LIFE-OS-DOCUMENTATION.md` for detailed feature specs
- **README**: See `README.md` for user guide and setup instructions

## Next Steps for Development

1. Implement keyboard shortcuts (Cmd+K, Cmd+N, Cmd+E, Cmd+?)
2. Add URL parameter support for deep linking
3. Create comprehensive test suite
4. Develop user onboarding tutorial
5. Implement cloud sync capabilities

---

**Last Updated**: November 10, 2024
**Version**: 2.0 (MVP)
**Status**: Active Development
