# Life OS Tag System - Phase 3 Specification

**Version**: 3.0 (Advanced Features)
**Status**: Planning & Requirements
**Date**: November 11, 2024
**Scope**: 5 Advanced Features for Tag Organization & Automation

---

## EXECUTIVE SUMMARY

Phase 3 enhances the tag system with hierarchical organization, visual customization, bulk operations, intelligent suggestions, and cloud synchronization. These features enable power users to organize complex workflows, automate tagging, and sync across devices.

**Build Timeline**: 12-15 days (dependent features)
**Development Effort**: ~95-110 hours
**Recommended Sequence**: Feature 1 → 2 → 3 → 4 → 5 (dependency order)

---

## FEATURE 1: TAG HIERARCHY (Parent/Child Tags)

### Description

Allows tags to be organized in a hierarchical structure using forward-slash notation (e.g., "work/urgent", "work/project-a"). Parent tags are automatically created and cascade down child tags. Users can expand/collapse tag groups in the sidebar and filter by parent or specific children.

### Key Requirements

- Parent/child relationship stored in tag metadata (e.g., `parent: "work"`, `path: "work/project-a"`)
- Automatic parent tag creation (no orphaned children)
- Recursive parent deletion removes all children with confirmation
- Sidebar displays as collapsible tree (expandable parents)
- Filter supports wildcard matching (e.g., "work/*" shows all work tasks)
- Task UI shows full hierarchy path "work / project-a" for clarity
- Backward compatible: flat tags remain flat unless manually organized

### User Workflow

1. User types tag with slash: "work/project-a"
2. System creates parent "work" if needed
3. Sidebar expands to show: `[+] work` → children list
4. User clicks parent to expand/collapse
5. User applies tag filter: "work/*" to show all work tasks
6. User selects "work/project-a" to filter specific subtask
7. User renames parent: all children update automatically (e.g., "work" → "professional")
8. User deletes parent: gets confirmation about children deletion

### Acceptance Criteria

- Tag registry stores parent-child relationships accurately
- Sidebar renders collapsible tree with proper indentation
- Wildcard filtering works (parent/* matches all children)
- Renaming parent updates all child paths atomically
- Deleting parent shows count of affected children before deletion
- Performance: hierarchy with 50+ parent tags renders in <100ms

### Estimated Dev Time

**22 hours**
- Data model update: 4h
- Sidebar tree rendering: 6h
- Filter logic & wildcard matching: 5h
- Rename/delete cascading: 4h
- Testing & edge cases: 3h

### Dependencies

- Phase 1 (MVP tags): Required
- Phase 2 (tag management): Required for rename/delete logic
- No external dependencies

### Risks

- Circular references if not validated (e.g., "a/b" with parent "a/b/c")
- Deep nesting performance (3+ levels) if not cached
- Migration of existing flat tags to hierarchical system

**Mitigation**:
- Validate parent path on creation (prevent circular refs)
- Limit nesting to 3 levels via validation
- Optional migration UI: "Organize into hierarchy"

---

## FEATURE 2: TAG COLOR CUSTOMIZATION

### Description

Users can assign custom colors to tags instead of auto-generated colors. Color picker (hue/saturation/lightness) in tag management modal with live preview. Supports preset palettes and manual hex input. Colors sync across all tags and tasks.

### Key Requirements

- HSL color picker (hue 0-360, saturation 0-100%, lightness 0-100%)
- Preset palettes: "Vibrant" (8 colors), "Pastel" (8 colors), "Grayscale" (5 colors)
- Manual hex color input with validation
- Accessibility: text color contrast check (WCAG AA standard)
- Color stored in tag metadata: `color: { hue: 200, saturation: 100, lightness: 50 }`
- Live preview in management modal
- Color persistence across page reloads
- Light/dark theme automatic contrast adjustment
- Export/import colors with tag configuration

### User Workflow

1. User opens tag management modal
2. User clicks color square next to tag name
3. Color picker modal opens with HSL sliders or preset buttons
4. User adjusts hue/saturation/lightness or selects preset
5. Live preview shows color in modal and task list
6. User clicks "Apply" - color saved to tag registry
7. All tasks with this tag update color immediately
8. User can copy color hex code for consistency

### Acceptance Criteria

- HSL picker correctly maps to CSS HSL colors
- Contrast ratio >= 4.5:1 for text on tag backgrounds (WCAG AA)
- Color updates atomic and synchronous across all tasks
- Preset palettes load instantly
- Hex input validates format (#RGB, #RRGGBB)
- Colors persist through browser refresh
- Works in both light and dark themes

### Estimated Dev Time

**18 hours**
- HSL color picker component: 7h
- Color storage & retrieval: 3h
- Contrast validation: 4h
- UI updates (task display): 3h
- Testing color accuracy: 1h

### Dependencies

- Phase 1 (MVP tags): Required
- Phase 2 (tag management modal): Required
- No external libraries (vanilla CSS/JS)

### Risks

- HSL to RGB conversion accuracy issues
- Color contrast fails in high-contrast themes
- Performance: updating colors on tasks with 100+ instances

**Mitigation**:
- Use standard HSL→RGB algorithms (tested)
- Automatic luminance adjustment if contrast fails
- Batch update colors: collect all tasks first, then render once

---

## FEATURE 3: BULK TAG OPERATIONS

### Description

Multi-select tasks in the detail view and apply bulk operations: add tags, remove tags, replace tags, delete tags. Includes checkbox UI in task list, action buttons above list, and undo/redo support for all bulk operations.

### Key Requirements

- Checkbox column in task list (left side) for multi-select
- Selection counter: "5 of 20 tasks selected"
- Bulk action buttons: Add Tag, Remove Tag, Replace Tag, Delete Tags
- Replace tag modal: "Replace 'old-tag' with 'new-tag' (affects 8 tasks)"
- Bulk operations atomic: all-or-nothing (no partial updates)
- Keyboard shortcut: Cmd+A to select all visible tasks
- Undo/redo support: single undo reverts entire bulk operation
- Filters applied to bulk: operate only on filtered tasks
- Confirmation dialog with impact summary before execution

### User Workflow

1. User opens module detail view
2. User clicks checkbox column header to select all (or individual checkboxes)
3. User sees: "8 tasks selected"
4. User clicks "Add Tag" button
5. Tag input dialog appears
6. User selects/types tag name(s)
7. User clicks "Add to 8 tasks"
8. Confirmation dialog: "Add 'feature-request' to 8 tasks?"
9. Tasks update immediately, undo button highlighted
10. User can undo: all 8 tasks revert instantly

### Acceptance Criteria

- Multi-select UI renders without performance impact on 100+ tasks
- Bulk operation affects correct task count (respects filters)
- Undo reverts entire bulk operation atomically
- Keyboard Cmd+A select/deselect works reliably
- Replace operation shows old vs. new tag clearly
- Operation completes within 500ms for 50+ tasks
- Clear "No tasks selected" state with appropriate button disabling

### Estimated Dev Time

**20 hours**
- Multi-select checkbox UI: 5h
- Bulk operation modals: 6h
- Atomic operations & validation: 5h
- Undo/redo integration: 3h
- Testing (various scenarios): 1h

### Dependencies

- Phase 1 (MVP tags): Required
- Undo/redo system (core): Required
- Feature 1 (tag hierarchy): Optional but recommended

### Risks

- Performance degradation with 200+ tasks if not optimized
- Undo history explosion (each task = 1 state) if not collapsed
- Complex filter interactions (e.g., "high-priority" + "work/*")

**Mitigation**:
- Virtualize task list (only render visible tasks)
- Store single undo state for entire bulk operation
- Test with large datasets (1000+ tasks)

---

## FEATURE 4: AI TAG SUGGESTIONS

### Description

As user types task title, system suggests relevant tags based on title keywords, task description, priority level, and historical tagging patterns. Suggestions appear as autocomplete dropdown or sidebar widget. Uses local pattern matching (no external API).

### Key Requirements

- Suggestion engine analyzes: title, description, priority, module type
- Pattern matching: keywords → frequently paired tags
- Learning: builds pattern matrix from existing task-tag relationships
- Exclude archived tags from suggestions
- Top 5 suggestions shown by relevance score
- No external API calls (all local processing)
- Threshold: minimum 2 historical occurrences to suggest
- Smart filtering: don't suggest tags already on task
- Works in task creation and edit modals

### User Workflow

1. User creates new task in "Home" module
2. User types title: "Fix kitchen cabinet door"
3. System analyzes title: keywords "fix", "kitchen", "cabinet"
4. Suggestion widget appears showing 5 relevant tags: "home", "repairs", "urgent", "diy", "kitchen"
5. User clicks suggested tag → adds it to task
6. If user types custom tag, suggestions update in real-time
7. System learns: "home + repairs + diy = common combination"
8. Future "Fix..." tasks will suggest same combination

### Acceptance Criteria

- Suggestions appear within 200ms of title input
- Relevance scoring matches actual task-tag patterns (>70% accuracy)
- Learning engine updates after every tag addition
- No duplicates in suggestion list
- Suggestions exclude already-added tags
- Top suggestions change based on priority (high-priority tasks suggest urgent/deadline tags)
- Graceful degradation if no patterns exist (show most-used tags)

### Estimated Dev Time

**24 hours**
- Pattern matrix builder: 7h
- Relevance scoring algorithm: 6h
- Suggestion UI (autocomplete): 5h
- Learning/persistence: 3h
- Testing & tuning: 3h

### Dependencies

- Phase 1 (MVP tags): Required
- Task data with title/description: Required
- No external dependencies

### Risks

- Poor suggestion quality if patterns are weak (new users)
- Privacy concern: local pattern analysis (mitigated by offline-only)
- Suggestion performance with 1000+ tasks (needs indexing)

**Mitigation**:
- Fallback to most-used tags if patterns weak
- Seed with common tag combinations (domain-specific)
- Lazy-build pattern matrix on first use
- Cache suggestions per task (don't recalculate on each keystroke)

---

## FEATURE 5: CLOUD SYNC (iCloud/Google Drive)

### Description

Optional cloud synchronization of tags and task data. Users authenticate via OAuth, then data syncs bidirectionally at configurable intervals (manual, hourly, daily). Conflict resolution via "latest-wins" or user choice. Works offline; syncs when connection restored.

### Key Requirements

- OAuth 2.0 integration: iCloud (if available) or Google Drive
- Sync targets: tag registry + all module task data
- Sync intervals: Manual, Every Hour, Daily, on-App-Load
- Conflict resolution: "Latest modification time wins" with confirmation for user choice
- Offline support: app works fully offline, queues sync on reconnect
- Sync status indicator: spinning icon when syncing, checkmark on success
- Error recovery: retry failed syncs with exponential backoff
- Data encryption: HTTPS only, no plaintext storage
- User privacy: users own their data, app cannot access without credentials
- Manual export/import: JSON file backup to file system

### User Workflow

1. User opens "Sync Settings" modal
2. User clicks "Connect to Google Drive"
3. Browser redirects to Google OAuth consent screen
4. User grants permission (access to app-specific folder only)
5. App receives access token, stores securely
6. User selects sync interval: "Every Hour"
7. Sync icon spins, data uploads to Drive
8. User continues working offline
9. Connection lost: icon shows "offline"
10. Connection restored: auto-sync triggers, icon shows success

### Acceptance Criteria

- OAuth flow completes in <10 seconds
- Sync completes for 100+ tasks in <5 seconds
- Offline mode works with all features (no errors)
- Conflict resolution shows clear UI ("Cloud has newer version")
- Access tokens refresh automatically without user intervention
- Sync history shows last sync time and status
- Failed syncs don't corrupt local data
- Manual export creates valid JSON file with full data
- Works in both light and dark themes

### Estimated Dev Time

**31 hours**
- OAuth setup (Google Drive API): 8h
- File upload/download logic: 8h
- Conflict detection & resolution: 6h
- Offline queuing & sync: 5h
- UI & status indicators: 3h
- Error handling & testing: 1h

### Dependencies

- Phase 1-2 (tag system core): Required
- Google Drive API setup (external): Required
- Network availability detection: Required

### Risks

- OAuth token expiry and refresh complexity
- Conflict resolution bugs causing data loss
- Large file uploads (>50MB) timeout
- User confusion about sync state
- Privacy implications (data on third-party servers)

**Mitigation**:
- Implement token refresh 5 minutes before expiry
- Sync only deltas (changed items) not full dataset
- File size limits enforced: warn if >10MB
- Clear status UI: "Last sync: 2 min ago", "Syncing...", "Sync failed"
- Legal: Privacy policy update, user consent flow

---

## OVERALL TIMELINE & PRIORITIZATION

### Sequential Build Schedule

```
Week 1 (Days 1-5):
  Feature 1: Tag Hierarchy
    Mon-Tue: Data model & tests (8h)
    Wed:     Sidebar rendering (6h)
    Thu:     Filter/wildcard logic (5h)
    Fri:     Testing & edge cases (3h)
  Total: 22h

Week 2 (Days 6-10):
  Feature 2: Color Customization
    Mon-Tue: HSL picker component (7h)
    Wed:     Color storage (3h)
    Thu:     Contrast validation (4h)
    Fri:     UI updates (3h + 1h testing)
  Total: 18h

Week 3 (Days 11-15):
  Feature 3: Bulk Operations (Part 1)
    Mon-Tue: Multi-select UI (5h)
    Wed-Thu: Bulk operation modals (6h)
    Fri:     Atomic ops & validation (4h)
  Total: 15h (continues to Week 4)

Week 4 (Days 16-20):
  Feature 3: Bulk Operations (Part 2)
    Mon:     Undo/redo integration (3h)
    Tue:     Testing & optimization (2h)
  Feature 4: AI Suggestions (Part 1)
    Wed-Thu: Pattern engine (13h)
    Fri:     Testing & tuning (3h)
  Total: 21h (continues to Week 5)

Week 5 (Days 21-25):
  Feature 4: AI Suggestions (Part 2)
    Mon-Tue: Continue pattern engine (11h)
    Wed-Thu: UI integration (5h)
    Fri:     Testing (3h)
  Feature 5: Cloud Sync (Part 1)
    Fri+:    OAuth setup (8h)
  Total: 19h (continues to Week 6)

Week 6+ (Days 26-35):
  Feature 5: Cloud Sync (Part 2)
    Complete file sync (8h)
    Conflict resolution (6h)
    Offline queuing (5h)
    UI & status (3h)
    Testing (1h)
  Total: 23h (overlaps with review/testing)

TOTAL DURATION: 12-15 days (full-time dev)
TOTAL EFFORT: 95-110 hours
```

### Prioritization (If Time-Constrained)

**Tier 1 (Must-Have - 8 days, 60h)**:
1. Tag Hierarchy (22h) - Unlocks organization
2. Color Customization (18h) - Improves UX
3. Bulk Operations (20h) - Increases efficiency

**Tier 2 (Should-Have - 4 days, 35h)**:
4. AI Suggestions (24h) - Automation benefit
5. Cloud Sync (31h) - Can defer to Phase 4

**Tier 3 (Nice-To-Have)**:
- Advanced analytics (co-occurrence patterns)
- Mobile-first sync optimizations
- Tag templates (pre-built hierarchies)

### Build Order Rationale

1. **Hierarchy first** - Other features depend on clean parent/child relationships
2. **Colors second** - UX improvement, no other dependencies
3. **Bulk ops third** - Requires hierarchy for wildcard filtering
4. **AI suggestions fourth** - Works with any tag structure
5. **Cloud sync last** - Most complex, can be external feature

---

## INTEGRATION NOTES

### Data Structure Changes

#### Tag Registry Evolution (life-os-tags-registry)

**Phase 2 Format**:
```javascript
{
  "tags": [
    {
      "name": "work",
      "createdAt": timestamp,
      "archived": false,
      "lastUsedAt": timestamp
    }
  ]
}
```

**Phase 3 New Format**:
```javascript
{
  "tags": [
    {
      "name": "work/project-a",
      "parent": "work",
      "path": "work/project-a",
      "createdAt": timestamp,
      "archived": false,
      "lastUsedAt": timestamp,
      // Feature 2: Color
      "color": {
        "hue": 200,
        "saturation": 100,
        "lightness": 50
      },
      // Feature 4: AI Learning
      "cooccurrencePatterns": {
        "urgent": 5,
        "deadline": 3
      }
    }
  ],
  // Feature 4: AI Pattern Matrix
  "patternMatrix": {
    "work": {
      "urgent": 8,
      "deadline": 6,
      "project-a": 5
    }
  },
  // Feature 5: Cloud Sync
  "syncMeta": {
    "lastSyncAt": timestamp,
    "cloudLastModified": timestamp,
    "localLastModified": timestamp,
    "syncEnabled": false,
    "provider": "google-drive" // or "icloud"
  }
}
```

#### Task Data Updates

Tasks now track tag assignment timestamps (for conflict resolution):

```javascript
{
  "id": "task_123",
  "label": "Fix kitchen cabinet",
  "tags": ["home", "repairs", "urgent"],
  "tagsMeta": {
    "home": { "addedAt": timestamp, "source": "manual|ai-suggestion|bulk-op" },
    "repairs": { "addedAt": timestamp, "source": "ai-suggestion" },
    "urgent": { "addedAt": timestamp, "source": "manual" }
  },
  // ... other fields
}
```

### Module Updates

- **renderDetailView()**: Add checkbox column, update task rendering for hierarchy
- **renderTagSidebar()**: Implement tree view for parents/children
- **getTagColor()**: Return custom color if exists, else generate
- **setStorageData()**: Validate parent relationships during save
- **filterTasks()**: Support wildcard patterns (parent/*)
- **UndoRedoManager**: Capture bulk operation as single state

### New Functions Required

```javascript
// Feature 1: Hierarchy
getTagParent(tagName)
getTagChildren(parentName)
validateTagHierarchy(tagPath)
renderTagTree(tags)
filterByWildcard(tagName, taskTags)
cascadeRenameChildren(oldParent, newParent)
cascadeDeleteChildren(parentName)

// Feature 2: Colors
openColorPicker(tagName)
hexToHSL(hexColor)
hslToHex(hue, saturation, lightness)
validateContrast(backgroundColor, textColor)
getAutoTextColor(backgroundColor)

// Feature 3: Bulk Operations
getSelectedTasks()
toggleTaskSelection(taskId)
selectAllTasks()
deselectAllTasks()
bulkAddTags(taskIds, tags)
bulkRemoveTags(taskIds, tags)
bulkReplaceTags(taskIds, oldTag, newTag)
bulkDeleteTasks(taskIds)

// Feature 4: AI Suggestions
buildPatternMatrix()
getTagSuggestions(taskTitle, taskDescription, priority)
calculateRelevanceScore(tag, context)
updatePatternMatrix(addedTags, taskContext)

// Feature 5: Cloud Sync
initializeGoogleDriveSync()
syncToCloud()
syncFromCloud()
resolveConflicts(cloudData, localData)
queueSyncOperation(operation)
processOfflineQueue()
```

---

## BACKWARD COMPATIBILITY

### Phase 1-2 Tags Remain Compatible

- Existing flat tags (no slash) continue working as-is
- Auto-generated colors (Phase 1) work alongside custom colors
- No forced migration: users opt-in to hierarchy
- Old format read-only: new features only on Phase 3 tags

### Migration Path (Optional)

Users can optionally organize existing tags into hierarchy:

```javascript
// Migration helper function
function suggestTagHierarchy() {
  // Analyzes existing tags and recommends groupings
  // E.g., "meeting", "meeting-plan", "meeting-notes" → "meeting/*"
  // User reviews suggestions and approves before applying
}
```

### Fallback Behavior

If Phase 3 features fail to load:
- System falls back to Phase 2 behavior
- App remains fully functional
- Colors revert to auto-generated
- Suggestions disabled
- Sync disabled
- Bulk operations unavailable but multi-select still works

---

## TECHNICAL APPROACH (High-Level)

### Architecture Principles

1. **Separation of Concerns**: Each feature in separate module-like section of code
2. **Pure Functions**: Calculations (pattern matrix, color conversion) are pure
3. **Atomic Operations**: Tag modifications either fully succeed or fully rollback
4. **Graceful Degradation**: Missing features don't break core functionality
5. **Local-First**: All data stays local until explicitly synced
6. **Caching**: Pattern matrix cached in memory, rebuilt on major changes

### State Management

```javascript
// New global state for Phase 3
const Phase3State = {
  selectedTasks: new Set(),           // Feature 3
  patternCache: {},                   // Feature 4 (invalidated on tag changes)
  syncInProgress: false,              // Feature 5
  offlineQueue: [],                   // Feature 5
  lastSyncTime: null,                 // Feature 5
  expandedParents: new Set()          // Feature 1
};
```

### Performance Optimizations

1. **Lazy load patterns**: Build pattern matrix only on Feature 4 activation
2. **Debounce suggestions**: Wait 300ms after typing before calculating
3. **Batch renders**: Collect all changes, render once (bulk operations)
4. **Cache validation**: Rebuild pattern matrix only on tag creation/deletion
5. **Virtualize task list**: Only render visible tasks (for selection UI)
6. **Sync deltas**: Upload only changed tasks, not full dataset

### Testing Strategy

- Unit tests: Pattern matrix accuracy, color conversion, hierarchy validation
- Integration tests: Bulk operations affect correct tasks, undo/redo works
- E2E tests: Full workflows (create hierarchy, bulk tag, sync)
- Performance tests: 1000+ tasks doesn't cause lag
- Compatibility tests: Phase 1-2 features still work with Phase 3

---

## SUMMARY TABLE

| Feature | Dev Time | Priority | Risk Level | Impact |
|---------|----------|----------|-----------|--------|
| 1. Hierarchy | 22h | P1 | Medium | High - enables organization |
| 2. Color | 18h | P1 | Low | Medium - improves UX |
| 3. Bulk Ops | 20h | P1 | Medium | High - increases efficiency |
| 4. AI Suggestions | 24h | P2 | Medium | Medium - automation benefit |
| 5. Cloud Sync | 31h | P2 | High | Low-Medium - optional feature |
| **Total** | **115h** | - | - | **Transforms tag system** |

---

## SUCCESS METRICS

Phase 3 will be considered successful when:

1. Tag hierarchy tested with 50+ parent/child relationships (no performance degradation)
2. 95%+ of custom colors meet WCAG AA contrast standards
3. Bulk operations on 50+ tasks complete in <500ms
4. AI suggestions show >70% relevance accuracy
5. Cloud sync handles 1000+ task sync in <10 seconds
6. All Phase 1-2 features remain fully functional
7. Undo/redo captures single undo state for bulk operations
8. Zero data loss during conflict resolution scenarios

---

## NEXT STEPS

1. **Review & Approve**: Stakeholder sign-off on Phase 3 scope
2. **Dependency Check**: Ensure Phase 1-2 complete and stable
3. **Environment Setup**: Google Drive API credentials (if pursuing Feature 5)
4. **Sprint Planning**: Create sprint tasks for each feature
5. **Begin Feature 1**: Start tag hierarchy implementation
6. **Documentation**: Update CLAUDE.md with Phase 3 features post-launch

---

**Document Version**: 1.0
**Last Updated**: November 11, 2024
**Status**: Ready for Development
**Owner**: Product Management

