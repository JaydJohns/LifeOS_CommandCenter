# Phase 3 Developer Quick Reference

Quick lookup guide for all Phase 3 functions and features.

## Phase 3a: Tag Hierarchy

### Core Functions

```javascript
// Get parent path of hierarchical tag
const parent = getTagParent("work/project-a");  // Returns "work"

// Get all direct children of parent
const children = getTagChildren("work");  // Returns array of child tags

// Validate tag hierarchy (circular refs, depth)
const result = validateTagHierarchy("work/project-a/task-1");
// Returns: { valid: true } or { valid: false, error: "..." }

// Create hierarchy, automatically creating parents
const created = createHierarchyFromPath("work/project-a/task-1");
// Returns: ["work", "work/project-a", "work/project-a/task-1"]

// Get full hierarchy chain
const chain = getTagHierarchy("work/project-a");
// Returns: ["work", "work/project-a"]

// Rename tag and all children atomically
renameTagHierarchy("work", "professional");
// All children updated: "professional/project-a", etc.

// Delete tag and children
deleteTagHierarchy("work", true);  // Include children

// Render collapsible tree in sidebar
const html = renderTagTree();

// Toggle parent expansion state
toggleTagExpander("work");

// Filter by wildcard pattern
const matches = filterByWildcard("work/*", ["work/project-a", "home/task"]);
// Returns: true (matches pattern)
```

### Data Structure

```javascript
// Tag object with hierarchy metadata
{
  name: "project-a",           // Leaf node name only
  path: "work/project-a",      // Full hierarchical path
  parent: "work",              // Parent path or null
  children: [],                // Child paths (reference)
  createdAt: 1731264000000,
  archived: false,
  lastUsedAt: 1731350400000
}
```

## Phase 3b: Tag Color Customization

### Color Conversion Functions

```javascript
// Convert hex to HSL
const hsl = hexToHSL("#FF00AA");
// Returns: { hue: 300, saturation: 100, lightness: 50 }

// Convert HSL to hex
const hex = hslToHex(300, 100, 50);  // Returns: "#FF00AA"

// Get contrast ratio (WCAG standard)
const ratio = getContrastRatio("#FF00AA");  // Returns: 5.2

// Validate contrast (WCAG AA = 4.5:1)
const validation = validateContrast("#FF00AA");
// Returns: { pass: true, ratio: "5.20", recommendation: "" }

// Get optimal text color for background
const textColor = getAutoTextColor("#FF00AA");  // Returns: "#000000" or "#FFFFFF"
```

### Color Management Functions

```javascript
// Get tag custom color (or auto-generated as fallback)
const color = getTagCustomColor("urgent");
// Returns: { hue: 0, saturation: 100, lightness: 50 } or auto-generated

// Set custom color for tag
setTagCustomColor("urgent", 0, 100, 50);

// Reset tag to auto-generated color
resetTagColor("urgent");

// Open color picker modal for tag
openColorPicker("urgent");

// Update color preview in real-time
updateColorPreview();  // Called while adjusting sliders

// Save color from picker
saveColorFromPicker();

// Apply preset palette
applyColorPreset("vibrant", 0);  // vibrant red
applyColorPreset("pastel", 3);   // pastel green
applyColorPreset("grayscale", 2); // gray
```

### Color Picker Modal Elements

```html
<!-- Required elements in DOM -->
<input id="hueSlider" type="range" min="0" max="360" value="200">
<input id="saturationSlider" type="range" min="0" max="100" value="85">
<input id="lightnessSlider" type="range" min="0" max="100" value="45">

<span id="hueValue">200°</span>
<span id="saturationValue">85%</span>
<span id="lightnessValue">45%</span>

<div id="colorPreview"></div>
<div id="contrastRatio">Contrast: 5.2:1 ✓</div>
```

### Preset Palettes

```javascript
// Vibrant (8 colors)
{ h: 0, s: 100, l: 50 }    // Red
{ h: 30, s: 100, l: 50 }   // Orange
{ h: 60, s: 100, l: 50 }   // Yellow
{ h: 120, s: 100, l: 50 }  // Green
{ h: 180, s: 100, l: 50 }  // Cyan
{ h: 240, s: 100, l: 50 }  // Blue
{ h: 270, s: 100, l: 50 }  // Purple
{ h: 330, s: 100, l: 50 }  // Pink

// Pastel (8 colors, same hues but lighter)
{ h: 0, s: 70, l: 75 }     // Light red
// ... etc

// Grayscale (5 shades)
{ h: 0, s: 0, l: 100 }     // White
{ h: 0, s: 0, l: 75 }      // Light gray
{ h: 0, s: 0, l: 50 }      // Gray
{ h: 0, s: 0, l: 25 }      // Dark gray
{ h: 0, s: 0, l: 0 }       // Black
```

## Phase 3c: Bulk Tag Operations

### Selection Management

```javascript
// Global state
let selectedTaskIds = new Set();  // Tracks selected task IDs

// Toggle single task selection
toggleTaskSelection("task_123");
toggleTaskSelection("task_123", true);  // Force select
toggleTaskSelection("task_123", false); // Force deselect

// Select all tasks in current module
selectAllTasks();

// Deselect all tasks
deselectAllTasks();

// Update UI (call after selection changes)
updateBulkOperationsUI();
```

### Bulk Operations

```javascript
// Bulk add tags
bulkAddTags(["feature-request", "v2.0"]);
// Respects 3-tag max, skips if already present
// Returns: true/false, shows count

// Bulk remove tags
bulkRemoveTags(["wontfix"]);
// Removes from all selected tasks
// Returns: true/false, shows count

// Bulk replace tags
bulkReplaceTags("project-a", "project-b");
// Replaces old tag with new, skips if conflict
// Returns: true/false, shows count

// Bulk clear all tags
bulkClearTags();
// Removes ALL tags from all selected tasks
// Requires confirmation
// Returns: true/false

// Open bulk operation modal
openBulkModal("add");    // Add Tag modal
openBulkModal("remove"); // Remove Tag modal
openBulkModal("replace"); // Replace Tag modal
openBulkModal("clear");  // Clears tags immediately
```

### HTML Elements Required

```html
<!-- Checkbox column in task list -->
<input type="checkbox"
  onchange="toggleTaskSelection('${taskId}')"
  class="task-checkbox">

<!-- Bulk operations panel -->
<div id="bulkOperationsPanel" style="display: none;">
  <span id="bulkSelectionCounter">5 tasks selected</span>
  <button onclick="openBulkModal('add')">Add Tag</button>
  <button onclick="openBulkModal('remove')">Remove Tag</button>
  <button onclick="openBulkModal('replace')">Replace Tag</button>
  <button onclick="bulkClearTags()">Clear All Tags</button>
</div>

<!-- Bulk operation modals -->
<div id="bulkAddModal" class="modal">
  <!-- Tag selection UI -->
</div>
<div id="bulkRemoveModal" class="modal">
  <!-- Tag selection UI -->
</div>
<div id="bulkReplaceModal" class="modal">
  <!-- Old tag selector + New tag input -->
</div>
```

## Phase 3d: AI Tag Suggestions

### Suggestion Engine

```javascript
// Singleton object (auto-initialized on page load)
TagSuggestionEngine

// Initialize engine (called automatically)
TagSuggestionEngine.initialize();

// Build pattern matrix from existing tasks
TagSuggestionEngine.buildPatternMatrix();

// Get suggestions based on context
const suggestions = TagSuggestionEngine.getSuggestions(
  title = "Fix kitchen cabinet",
  description = "Door hinge broken, needs replacement",
  priority = "high",
  existingTags = ["home"]
);
// Returns: [
//   { name: "repairs", score: 0.85, confidence: "high" },
//   { name: "diy", score: 0.62, confidence: "medium" },
//   ...
// ]

// Update pattern after adding tags
TagSuggestionEngine.updatePattern(["home", "repairs", "diy"]);

// Clear learned patterns
TagSuggestionEngine.clearPatterns();

// Persist to localStorage (called automatically)
TagSuggestionEngine.persist();
```

### Suggestion Scoring Algorithm

```javascript
// Score = weighted sum of:
// 1. Keyword match (0.3 weight)
//    - Tag name contains keyword from title/description
// 2. Co-occurrence (0.5 weight)
//    - How often tag appears with already-added tags
// 3. Priority bonus (0.2 weight)
//    - "urgent" or "critical" tags suggested for high-priority tasks
// 4. Frequency (0.1 weight)
//    - How often tag is used overall

// Confidence levels:
// - high: score > 0.8
// - medium: 0.5 < score <= 0.8
// - low: score <= 0.5
```

### Integration with Task Modal

```javascript
// In task edit modal, after tags input:

// When user types title
function updateTaskTitle(title) {
  // ... update task ...

  // Get current tags
  const currentTags = getCurrentTaskTags();

  // Get suggestions
  const suggestions = TagSuggestionEngine.getSuggestions(
    title,
    document.getElementById('taskDescription').value,
    currentModule.priority,
    currentTags
  );

  // Render suggestions
  renderTagSuggestions(suggestions);
}

// HTML element required:
<div id="tagSuggestionsContainer"></div>
```

### Pattern Matrix Structure

```javascript
{
  "patternMatrix": {
    "home": {
      "repairs": 5,     // "home" and "repairs" appear together 5 times
      "diy": 3,
      "urgent": 2
    },
    "repairs": {
      "home": 5,
      "diy": 4,
      "plumbing": 2
    }
    // ... more tags
  },
  "coOccurrence": {
    // Not currently used, reserved for future enhancement
  },
  "lastUpdated": 1731350400000
}
```

## Phase 3e: Cloud Sync

### Cloud Sync Manager

```javascript
// Singleton object (auto-initialized on page load)
CloudSyncManager

// Check connection status
const connected = CloudSyncManager.getToken() !== null;

// Connect to Google Drive
CloudSyncManager.connectGoogleDrive();
// Prompts for token (development) or OAuth flow (production)

// Disconnect from cloud
CloudSyncManager.disconnectGoogleDrive();

// Get stored token
const token = CloudSyncManager.getToken();
// Returns: decrypted token or null

// Sync to cloud (upload)
await CloudSyncManager.syncToCloud();
// Returns: true/false
// Updates: lastSyncTime, updateSyncStatus()

// Sync from cloud (download)
await CloudSyncManager.syncFromCloud();
// Returns: true/false

// Queue operation for offline sync
CloudSyncManager.queueSyncOperation({
  type: "addTag",
  taskId: "task_123",
  tag: "urgent"
});

// Process offline queue when online
await CloudSyncManager.processOfflineQueue();
// Automatically called when connection restored

// Setup periodic sync
CloudSyncManager.setupPeriodicSync();
// Called automatically if syncInterval != "manual"

// Update UI status indicator
CloudSyncManager.updateSyncStatus("Syncing...");

// Persist configuration
CloudSyncManager.persist();
```

### Settings Functions

```javascript
// Open sync settings modal
openCloudSyncSettings();

// Close sync settings modal
closeCloudSyncSettings();

// Update status display in UI
updateCloudSyncStatus();

// Connect to Google Drive (UI wrapper)
connectCloudSync();

// Disconnect from Google Drive (UI wrapper)
disconnectCloudSync();

// Manually sync to cloud
await triggerManualSync();

// Manually download from cloud
await triggerManualDownload();

// Set sync interval
setSyncInterval("manual");   // One-time sync only
setSyncInterval("hourly");   // Sync every 60 minutes
setSyncInterval("daily");    // Sync at 2 AM
```

### HTML Elements Required

```html
<!-- Cloud sync modal -->
<div id="cloudSyncModal" class="modal">
  <div id="cloudSyncStatusDisplay">
    <!-- Auto-updated by updateCloudSyncStatus() -->
  </div>

  <button onclick="connectCloudSync()">Connect Google Drive</button>
  <button onclick="disconnectCloudSync()">Disconnect</button>
  <button onclick="triggerManualSync()">Sync Now</button>
  <button onclick="triggerManualDownload()">Download Latest</button>

  <select onchange="setSyncInterval(this.value)">
    <option value="manual">Manual</option>
    <option value="hourly">Hourly</option>
    <option value="daily">Daily</option>
  </select>

  <!-- Sync status indicator -->
  <span id="syncStatusIndicator">Ready</span>
</div>
```

### Sync Data Format

```javascript
{
  "version": "3.0",
  "timestamp": 1731350400000,
  "data": {
    "life-os-afr-tasks": { /* all AFR tasks */ },
    "life-os-uxr-tasks": { /* all UXR tasks */ },
    // ... all other modules
    "life-os-tags-registry": { /* tag registry with Phase 3 metadata */ }
  }
}
```

### Configuration Storage

```javascript
// localStorage['life-os-sync-config']
{
  "syncEnabled": true,
  "syncInterval": "hourly",
  "lastSyncTime": 1731350400000,
  "offlineQueue": [
    {
      "type": "addTag",
      "taskId": "task_123",
      "tag": "urgent",
      "queuedAt": 1731350380000
    }
  ]
}
```

## localStorage Keys Summary

```javascript
// Phase 3a: Hierarchy
'life-os-expanded-tags'
// { "work": true, "work/project-a": false, ... }

// Phase 3b: Colors
// Colors stored in 'life-os-tags-registry' tag objects
// { color: { hue, saturation, lightness } }

// Phase 3d: AI Suggestions
'life-os-tag-suggestions'
// { patternMatrix: {...}, coOccurrence: {...}, lastUpdated: ... }

// Phase 3e: Cloud Sync
'life-os-sync-config'
// { syncEnabled, syncInterval, lastSyncTime, offlineQueue }

'life-os-google-token'
// Base64-encoded Google Drive token (if connected)
```

## Integration Checklist

Before deploying Phase 3 features:

- [ ] Add color picker modal HTML
- [ ] Add bulk operations panel HTML
- [ ] Add tag suggestions container to task modal
- [ ] Add cloud sync modal HTML
- [ ] Add task selection checkboxes to task list
- [ ] Call `TagSuggestionEngine.initialize()` on app load
- [ ] Call `CloudSyncManager.initialize()` on app load
- [ ] Update `renderTagSidebar()` to call `renderTagTree()`
- [ ] Update `addTagToTask()` to call `TagSuggestionEngine.updatePattern()`
- [ ] Add keyboard shortcut: Cmd+A to select/deselect all
- [ ] Add Cmd+Click to select range of tasks
- [ ] Update tag filter logic to support wildcards

## Performance Tips

```javascript
// Lazy-build pattern matrix only when needed
if (!TagSuggestionEngine.patternMatrix.work) {
  TagSuggestionEngine.buildPatternMatrix();
}

// Debounce suggestions while typing (300ms)
let suggestionTimeout;
function onTitleInput() {
  clearTimeout(suggestionTimeout);
  suggestionTimeout = setTimeout(() => {
    renderTagSuggestions(
      TagSuggestionEngine.getSuggestions(...)
    );
  }, 300);
}

// Batch color updates
const colorUpdates = [];
tasks.forEach(task => {
  task.color = computeNewColor(task);
  colorUpdates.push(task);
});
// Save all at once
setStorageData(storageKey, { projects });
// Render once
renderDetailView(currentModule);
```

## Debugging

```javascript
// Enable debugging
localStorage.setItem('phase3-debug', 'true');

// Check state
console.log('Tags:', getAllTags());
console.log('Hierarchy:', getTagChildren('work'));
console.log('Color:', getTagCustomColor('urgent'));
console.log('Patterns:', TagSuggestionEngine.patternMatrix);
console.log('Sync:', CloudSyncManager.syncEnabled);

// Reset everything
localStorage.removeItem('life-os-expanded-tags');
localStorage.removeItem('life-os-tag-suggestions');
localStorage.removeItem('life-os-sync-config');
localStorage.removeItem('life-os-google-token');
```

---

**Last Updated**: November 11, 2024
**Status**: Complete
**Lines of Reference**: ~500

