# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

**나의 버킷 리스트** is a lightweight, client-side personal goal tracking web application. It's a vanilla JavaScript project with no build tools, frameworks, or external dependencies (except Tailwind CSS via CDN).

- **Type**: Static HTML/CSS/JavaScript web app
- **Browser Storage**: LocalStorage API (no backend required)
- **Code Size**: ~600 lines total
- **Architecture**: Simple 3-layer structure (UI → App Logic → Data Storage)

## Running the Application

Choose one of these methods:

### Direct Browser (Simplest)
```bash
# Simply open index.html in any modern browser
# File Explorer: double-click index.html
# Command line: start index.html  (Windows) or open index.html  (Mac)
```

### Live Server (Recommended for Development)
```bash
# VS Code + Live Server extension
# Right-click index.html → "Open with Live Server"
```

### Python Simple Server
```bash
python -m http.server 8000
# Then visit http://localhost:8000
```

**Note**: No build step, no npm install, no webpack. Just open and use.

## Architecture

The codebase uses **Separation of Concerns** across three layers:

### Layer 1: Storage Layer (`js/storage.js`)
- **BucketStorage** object - handles all LocalStorage operations
- **Responsibilities**: load/save data, CRUD operations, filtering, statistics
- **Pattern**: Singleton-like object with pure data functions
- **Key methods**:
  - `load()` / `save()` - LocalStorage I/O with error handling
  - `addItem()` / `updateItem()` / `deleteItem()` - data mutations
  - `toggleComplete()` - complete status toggling
  - `getStats()` - calculate total/completed/progress/completion_rate
  - `getFilteredList(filter)` - return filtered items (all/active/completed)

### Layer 2: Application Logic (`js/app.js`)
- **BucketListApp** class - manages UI state and interactions
- **Responsibilities**: DOM manipulation, event handling, screen rendering
- **Pattern**: ES6 class with initialization and event binding
- **Key methods**:
  - `init()` - setup: cache DOM elements, bind events, render
  - `cacheElements()` - store references to all used DOM nodes for performance
  - `bindEvents()` - attach all event listeners (form, filters, modals)
  - `render()` - update entire UI based on current state
  - `createBucketItemHTML()` - generate list item markup with proper escaping

### Layer 3: Presentation Layer (`index.html`, `css/styles.css`)
- **HTML**: Template structure, semantic sections, form elements
- **CSS**: Tailwind utilities + custom animations/responsive design
- **Tailwind**: via CDN, complemented by `styles.css` for complex styles

### Data Flow
```
User Interaction (click, type)
  ↓
Event Handler (app.js)
  ↓
Storage Operation (storage.js: addItem, updateItem, etc.)
  ↓
render() called
  ├→ updateStats() from storage
  ├→ getFilteredList() from storage
  └→ recreate all list items HTML and insert into DOM
```

## Key Code Patterns

### Data Persistence
- All state lives in LocalStorage under key `'bucketList'`
- Data is a JSON array of item objects: `{id, title, completed, createdAt, completedAt}`
- IDs are timestamp-based: `Date.now().toString()`
- Every mutation calls `BucketStorage.save()` automatically

### DOM Rendering
- No virtual DOM or framework - direct innerHTML manipulation
- Re-render entire list on any change (acceptable for this scale)
- List items generated via template strings in `createBucketItemHTML()`
- **Important**: All user input is escaped via `escapeHtml()` to prevent XSS

### Event Handling
- Most events use `addEventListener()` (form submit, filter buttons, modal)
- Dynamic list item buttons use inline `onclick` attributes (necessary since they're recreated on render)
- Modal state: `editingId` property tracks which item is being edited

### Filtering
- Current filter state stored in `app.currentFilter` ('all' | 'active' | 'completed')
- `getFilteredList(filter)` returns the appropriate subset
- Filter buttons get active class for visual feedback

## Important Files

| File | Purpose | Lines | Key Points |
|------|---------|-------|-----------|
| `index.html` | HTML structure | 131 | No build required, Tailwind CDN link, semantic sections |
| `js/storage.js` | Data management | 144 | All persistence logic, no side effects on DOM |
| `js/app.js` | UI logic | 260 | Class-based, event-driven, renders on every state change |
| `css/styles.css` | Custom styles | 153 | Animations, responsive tweaks, dark mode (prepared) |
| `README.md` | User documentation | 179 | Feature list, usage guide, customization tips |

## Common Tasks

### Add a New Feature
1. Identify which layer it belongs in (Storage, App Logic, or Presentation)
2. For data changes: add method to `BucketStorage`
3. For UI changes: modify `index.html` and `createBucketItemHTML()`
4. For interaction: add event handler in `bindEvents()` and call `render()`
5. Test in browser - no tests exist, verify manually

### Fix a Bug
- Most bugs are in `app.js` (event handling) or `storage.js` (data logic)
- Check browser console for errors
- Verify data is saved to LocalStorage (F12 → Application tab)
- Test filter states and empty list scenarios

### Style Changes
- Use Tailwind utility classes in HTML first
- Add custom CSS in `styles.css` only for complex animations/responsive hacks
- Test dark mode (already has CSS media query support)

### Add Data Field
1. Add field to item object creation in `BucketStorage.addItem()`
2. Update `createBucketItemHTML()` to display it
3. Update any methods that access items (filter, stats, etc.)

## Browser Support

- Chrome, Firefox, Safari, Edge (latest versions)
- Requires: ES6 classes, template literals, arrow functions, LocalStorage API
- Optional: CSS Grid, Flexbox, modern animations

## Security Notes

- **XSS Prevention**: `escapeHtml()` sanitizes all user input before display
- **No external data**: All data stays in browser LocalStorage
- **No authentication**: Single-user, local device only
- **CSRF not applicable**: No server/forms to external domains

## Future Improvements (from README)

- Categories/tags
- Image attachments
- Detailed notes
- Due dates
- Priority levels
- Import/export (JSON)
- Dark mode toggle UI
- Drag-and-drop sorting

See README.md for full feature roadmap.

## Testing Strategy

No test framework exists. Testing is manual:
- Open in browser, test each feature (add, complete, filter, edit, delete)
- Check localStorage persistence (open DevTools → Application tab)
- Test mobile responsiveness (DevTools device emulation)
- Test filter states (all/active/completed)
- Test edge cases (empty list, special characters in title)

## Performance Considerations

- Lightweight: entire app loads instantly, no build step
- Full re-render on state change is fast (list rarely exceeds 50 items)
- LocalStorage is synchronous (acceptable for this scale)
- No animations block interactions (all use CSS transitions)

## Development Tips

1. **LocalStorage debugging**: Open DevTools → Application → LocalStorage → check `bucketList` entry
2. **Hot reload**: Just refresh the page (no build process needed)
3. **Element inspection**: Use F12 to inspect generated HTML for list items
4. **CSS tweaking**: Changes in `styles.css` take effect immediately on refresh
5. **Responsive testing**: DevTools device emulation or manually resize browser
