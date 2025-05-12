---
layout: page
title: Dark Mode
nav_order: 5
permalink: /docs/technical/dark-mode/
---

# Dark Mode Implementation

This guide outlines the dark mode implementation for the Prisoner's Dilemma game.

## Features

- 🌓 Toggle between light and dark themes
- 💾 Persistent user preference (saved in localStorage)
- 🎨 Automatic detection of system preference
- 🔄 Smooth transitions between themes
- 📱 Responsive design in both themes
- 🛡️ Type-safe error handling with Result pattern

## Components

### 1. Dark Mode Service (`dark-mode.service.ts`)

The service manages dark mode state and persistence using the Result pattern:

```typescript
// Initialize the service
const darkModeService = new DarkModeService();

// Initialize dark mode (sets up initial theme)
const initResult = darkModeService.initializeDarkMode();
if (initResult.isSuccess()) {
  const currentTheme = initResult.getValue(); // Theme.LIGHT or Theme.DARK
}

// Toggle between themes
const toggleResult = darkModeService.toggleTheme();
if (toggleResult.isSuccess()) {
  const newTheme = toggleResult.getValue();
}

// Get current theme
const themeResult = darkModeService.getCurrentTheme();
if (themeResult.isSuccess()) {
  const currentTheme = themeResult.getValue();
}

// Set specific theme
const setResult = darkModeService.setTheme(Theme.DARK);

// Listen for system theme changes
const cleanup = darkModeService.listenForSystemThemeChanges();
// Call cleanup() when done listening
```

### 2. Dark Mode Toggle Component (`dark-mode-toggle.ts`)

A reusable toggle button component:

```html
<dark-mode-toggle></dark-mode-toggle>
```

Features:
- Sun/moon icon that changes based on theme
- Smooth transition animations
- Accessibility-friendly with ARIA labels
- Emits `theme-changed` custom event
- Error state handling with visual feedback

### 3. Updated Components

All components have been updated with dark mode support:

- `game-app.ts` - Main application wrapper with dark mode toggle
- `player-form.ts` - Registration form with dark theme
- `connection-manager.ts` - Connection interface with dark theme
- `connection-form.ts` - Connection form with dark theme
- `connection-list.ts` - Connection list with dark theme

## Theme Enum

```typescript
enum Theme {
  LIGHT = 'light',
  DARK = 'dark',
}
```

## Error Handling

The service uses the Result pattern for error handling:

```typescript
enum ThemeErrorType {
  STORAGE_ERROR = 'STORAGE_ERROR',
  INVALID_THEME = 'INVALID_THEME',
}

class ThemeError extends Error {
  constructor(public readonly type: ThemeErrorType, message: string);
}
```

## Tailwind Configuration

The Tailwind configuration has been updated to enable class-based dark mode:

```javascript
module.exports = {
  darkMode: 'class', // Enable class-based dark mode
  // ... rest of config
}
```

## CSS Classes

Dark mode uses Tailwind's dark variant:

```html
<!-- Background colors -->
<div class="bg-white dark:bg-gray-800">

<!-- Text colors -->
<p class="text-gray-900 dark:text-white">

<!-- Border colors -->
<div class="border-gray-200 dark:border-gray-600">

<!-- Hover states -->
<button class="hover:bg-gray-100 dark:hover:bg-gray-700">
```

## Implementation Details

### 1. Preventing Flash of Unstyled Content

The `index.html` includes a script that runs immediately to apply dark mode before the page renders:

```html
<script>
  // Prevent FOUC (Flash of Unstyled Content) by setting theme early
  const theme = localStorage.getItem('prisonersDilemma_theme');
  if (theme === 'dark' || (!theme && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
    document.documentElement.classList.add('dark');
  }
</script>
```

### 2. System Preference Detection

The service automatically detects the user's system preference:

```typescript
const prefersDarkMode = window.matchMedia('(prefers-color-scheme: dark)').matches;
const theme = prefersDarkMode ? Theme.DARK : Theme.LIGHT;
```

### 3. Persistence

User preference is saved to localStorage with the key `prisonersDilemma_theme`:

```typescript
localStorage.setItem('prisonersDilemma_theme', theme);
```

### 4. System Theme Change Listener

The service can listen for system theme changes:

```typescript
const cleanup = darkModeService.listenForSystemThemeChanges();
// The service will automatically update when system preference changes
```

## Adding Dark Mode to New Components

1. Import Tailwind styles:
```typescript
import tailwindStyles from '../tailwind-output.css?inline';
```

2. Add dark variants to classes:
```typescript
render() {
  return html`
    <div class="bg-white dark:bg-gray-800 text-gray-900 dark:text-white">
      <!-- Component content -->
    </div>
  `;
}
```

3. Use consistent color patterns:
- Backgrounds: `bg-white` → `dark:bg-gray-800`
- Text: `text-gray-900` → `dark:text-white` or `dark:text-gray-100`
- Secondary text: `text-gray-600` → `dark:text-gray-300` or `dark:text-gray-400`
- Borders: `border-gray-200` → `dark:border-gray-600`
- Hover states: `hover:bg-gray-100` → `dark:hover:bg-gray-700`

## Color Palette

The dark mode uses a consistent color scheme:

- **Backgrounds**: 
  - Body: `dark:bg-gray-900`
  - Primary: `dark:bg-gray-800`
  - Secondary: `dark:bg-gray-700`

- **Text**:
  - Primary: `dark:text-white` or `dark:text-gray-100`
  - Secondary: `dark:text-gray-300`
  - Muted: `dark:text-gray-400`

- **Borders**:
  - Primary: `dark:border-gray-600`
  - Secondary: `dark:border-gray-700`

- **Interactive Elements**:
  - Primary button: `dark:bg-blue-700 dark:hover:bg-blue-600`
  - Success button: `dark:bg-green-700 dark:hover:bg-green-600`
  - Danger button: `dark:bg-red-700 dark:hover:bg-red-600`

## Custom Events

The `dark-mode-toggle` component emits a custom event when the theme changes:

```typescript
// Listen for theme changes
element.addEventListener('theme-changed', (event: CustomEvent) => {
  const newTheme = event.detail.theme; // Theme.LIGHT or Theme.DARK
});
```

## Testing

To test dark mode:

1. Click the toggle button in the UI
2. Verify all components update their colors
3. Refresh the page and ensure the preference persists
4. Change system preference and verify automatic detection works
5. Check for smooth transitions between themes

## Browser Support

Dark mode is supported in all modern browsers:
- Chrome 76+
- Firefox 67+
- Safari 12.1+
- Edge 76+

## Best Practices

1. Always provide both light and dark variants for colors
2. Test both themes thoroughly
3. Ensure sufficient contrast in both modes
4. Use semantic color names when possible
5. Keep transitions smooth but not distracting
6. Consider accessibility in both themes
7. Handle errors gracefully using the Result pattern
8. Clean up event listeners when components unmount

## Error Handling Examples

```typescript
// Initialize with error handling
const initResult = darkModeService.initializeDarkMode();
if (initResult.isFailure()) {
  console.error('Failed to initialize dark mode:', initResult.getError().message);
  // Fallback behavior
}

// Toggle with error handling
const toggleResult = darkModeService.toggleTheme();
if (toggleResult.isFailure()) {
  const error = toggleResult.getError();
  if (error.type === ThemeErrorType.STORAGE_ERROR) {
    // Handle storage errors (localStorage disabled, etc.)
  }
}
```

## Troubleshooting

### Dark mode not persisting
- Check if localStorage is enabled in the browser
- Verify the `prisonersDilemma_theme` key is being set
- Look for storage errors in the console

### Flash of wrong theme
- Ensure the initialization script is in the `<head>` tag
- Check that it runs before any CSS is loaded
- Verify the script uses the correct localStorage key

### Components not updating
- Verify Tailwind's dark mode is set to 'class'
- Ensure all components use the `dark:` variant
- Check that the `dark` class is being added to the root element
- Confirm the component is listening for theme changes if needed

### Error handling issues
- Check Result pattern usage in error conditions
- Verify error types are being handled appropriately
- Ensure graceful fallbacks are in place