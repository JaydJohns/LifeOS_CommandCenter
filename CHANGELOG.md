# Life OS - Changelog

All notable changes to this project will be documented in this file.

## [v2.1.1] - November 11, 2025

### Security & Stability Fixes

#### Fix: State Management Race Condition in createProject()

**Problem**: Undo/Redo state was saved AFTER storage operations, creating inconsistent state if save failed.

**Solution**: Moved `UndoRedoManager.saveState()` to BEFORE data modifications (line 2524)
- Ensures undo state always matches storage state
- Prevents orphaned undo entries
- Improves reliability of undo/redo system

**Impact Level**: CRITICAL - State management consistency

#### Fix: XSS Vulnerability in Project Button Handlers

**Problem**: Project IDs and storage keys were embedded directly in inline onclick handlers, creating potential for DOM injection.

**Solution**: Replaced inline onclick handlers with event delegation pattern (lines 3166-3192)
- Changed from: `onclick="deleteProject('${id}', '${key}')"`
- Changed to: `data-project-id="${id}"` + `addEventListener('click', handler)`
- Separates data from code
- Follows security best practices
- No functional change to user experience

**Impact Level**: HIGH - Security/XSS prevention

#### Fix: Null Pointer Exception in renameProjectUI()

**Problem**: Prompt cancellation (returns null) caused `.trim()` to throw TypeError.

**Solution**: Added explicit null check before trim (line 3333-3334)
- Detects user cancellation: `if (newName === null) return;`
- Prevents error when user clicks Cancel
- Provides graceful exit from rename operation

**Impact Level**: HIGH - Error handling/UX

### Testing
- ✅ Project creation with undo/redo tested and working
- ✅ Project deletion with new event handlers tested and working
- ✅ Project rename with cancel button tested and working
- ✅ All console output clean, no errors reported

**Files Changed**: index.html
**Commit Message**: "Fix: Critical security, stability, and error handling improvements for v2.1.1"

---

## [v2.1.0] - November 10, 2025

### Bug Fixes

#### Fix: Repair data corruption in project creation and display

**Problem**: A critical data corruption issue was preventing task creation and project display. When old task data coexisted with new project-based data, the projects object would become corrupted with mixed properties, causing:
- Tasks to fail creating with "STORAGE FULL" errors
- Projects to not display in UI despite being created
- Empty projects to be hidden from view
- Mixed-format data to not migrate properly

**Root Cause**: Three interrelated issues in the data layer:
1. `migrateToProjects()` didn't detect when old task properties were mixed into the projects object
2. `getStorageData()` was validating project-based structures with old task validation logic
3. `renderDetailView()` was hiding projects that had zero tasks

**Solution**:
1. **Enhanced migrateToProjects()** (lines 2316-2476)
   - Added detection for mixed-format data where old task properties are mixed into projects object
   - Extracts and separates corrupted properties from valid projects
   - Validates that projects object only contains project IDs (keys starting with 'proj_' or 'general')
   - Reorganizes data into proper structure: `{projects: {proj_id: {...}, general: {...}}}`
   - Wraps orphaned old tasks in special "General" project for preservation
   - Includes emergency reset for severely corrupted data

2. **Fixed getStorageData()** (lines 2174-2180)
   - Added check for project-based structure BEFORE old task validation
   - If data has `projects` key with only that key present, returns immediately without validation
   - Only applies old task validation to flat structures
   - Prevents new project data from being corrupted by legacy validation logic

3. **Fixed renderDetailView()** (line 3124)
   - Removed `|| totalTasks === 0` condition from projects display check
   - Changed: `if (projects.length === 0 || totalTasks === 0)`
   - To: `if (projects.length === 0)`
   - Projects now display immediately after creation, even before adding tasks
   - Improves user feedback and prevents premature hiding of projects

**Testing**:
- Projects create successfully without errors
- Projects display with correct names and metadata
- Multiple projects can coexist in same storage
- Tasks add to projects correctly
- Data persists after page reload
- Migration runs automatically on corrupted data without user intervention
- Empty projects (0 tasks) display correctly

**Backward Compatibility**: ✅ Fully backward compatible
- Old flat task structures still work
- Existing project data unaffected
- Automatic migration runs transparently on load
- No breaking changes to API or data structures

**Impact Level**: CRITICAL - High priority fix
**Files Changed**: index.html
**Commit Message**: "Fix: Repair data corruption in project creation and display"

---

## [v2.0.0] - October 2024

### Features

#### Initial MVP Release
- 9 Life Domains + 2 Major Projects module structure
- Task management with priority levels (High/Medium/Low)
- Time estimates in hours
- Completion checkboxes and progress tracking
- Smart filtering (All/Today/This Week/High Priority)
- Global statistics dashboard
- Browser localStorage persistence
- Responsive design with JARVIS-inspired theme
- Dark/Light theme toggle
- Recurring tasks (daily/weekly/monthly)
- Notes and subtasks per task
- Markdown export functionality
- Undo/Redo with keyboard shortcuts (Cmd+Z, Cmd+Shift+Z)
- Weekly digest reports
- Browser notifications
- Heatmap calendar view
- Performance analytics dashboard
- Data export/import capabilities

### Infrastructure

- Single HTML file deployment
- Zero external dependencies
- 100% offline capable
- GitHub Pages hosting
- Browser localStorage API for data persistence

---

## Version History

| Version | Date | Status | Notes |
|---------|------|--------|-------|
| v2.1.1 | Nov 11, 2025 | Released | Security, stability, and error handling fixes |
| v2.1.0 | Nov 10, 2025 | Released | Critical data corruption fix |
| v2.0.0 | Oct 2024 | Released | Initial MVP with all Tier 1-5 features |

---

## Known Issues

None currently. Last updated: November 10, 2025

---

## Development Notes

### Data Structure Evolution

The application has evolved through two data storage formats:

**Old Format** (flat structure):
```javascript
{
  "taskId": {
    "label": "task",
    "completed": false,
    "priority": "high",
    // ... other properties
  },
  "taskId2": { ... }
}
```

**New Format** (project-based):
```javascript
{
  "projects": {
    "general": {
      "id": "general",
      "name": "General",
      "tasks": { /* tasks */ }
    },
    "proj_abc123": {
      "id": "proj_abc123",
      "name": "Project Name",
      "tasks": { /* tasks */ }
    }
  }
}
```

v2.1.0 includes robust migration logic to handle any data format combination.

---

**Last Updated**: November 11, 2025
**Current Version**: v2.1.1
**Status**: Stable - All tests passing
