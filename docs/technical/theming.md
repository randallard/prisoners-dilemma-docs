---
layout: page
title: Theming
nav_order: 5
permalink: /docs/technical/theming/
---

# CSS Variables for Theming in Shadow DOM
{: .no_toc }

This document outlines our approach to using CSS variables for consistent theming in Shadow DOM components, focusing on solving the challenges of dark mode compatibility.

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Overview

Web Components with Shadow DOM provide excellent encapsulation but present challenges for global theming systems like Tailwind CSS's dark mode. This is especially problematic for feedback components that need to maintain proper contrast ratios and visual hierarchy in different themes.

Our solution leverages CSS Custom Properties (variables) to create a semantic theming system that properly penetrates Shadow DOM boundaries while maintaining component encapsulation.

## The Problem

When using Tailwind CSS with Shadow DOM components, we encountered several issues:

1. **Selector Isolation**: Tailwind's dark mode relies on parent selectors (`.dark`) that don't penetrate Shadow DOM
2. **Inconsistent Behavior**: Some color classes (like green) failed in dark mode while others (red, blue) worked
3. **Contrast Issues**: Success messages became invisible or hard to read in dark mode
4. **Maintainability Challenges**: Component-specific fixes created maintenance overhead

## The Solution: CSS Variables

CSS Custom Properties (variables) inherit through Shadow DOM boundaries, making them ideal for theming. Our approach:

1. Define semantic color variables at the root level
2. Create mappings that change based on current theme
3. Reference these variables in component styles
4. Maintain encapsulation while enabling global theme control

## Implementation

### 1. Define Global CSS Variables

Add these variable definitions to your global CSS (e.g., `index.css`):

```css
:root {
  /* Light mode semantic variables */
  --success-bg-light: #ecfdf5;
  --success-border-light: #10b981;
  --success-text-light: #065f46;
  
  --error-bg-light: #fee2e2;
  --error-border-light: #ef4444;
  --error-text-light: #b91c1c;
  
  --info-bg-light: #eff6ff;
  --info-border-light: #3b82f6;
  --info-text-light: #1e40af;
  
  --warning-bg-light: #fffbeb;
  --warning-border-light: #f59e0b;
  --warning-text-light: #92400e;
  
  /* Dark mode semantic variables */
  --success-bg-dark: rgba(6, 95, 70, 0.2);
  --success-border-dark: #059669;
  --success-text-dark: #34d399;
  
  --error-bg-dark: rgba(153, 27, 27, 0.2);
  --error-border-dark: #dc2626;
  --error-text-dark: #f87171;
  
  --info-bg-dark: rgba(29, 78, 216, 0.2);
  --info-border-dark: #2563eb;
  --info-text-dark: #60a5fa;
  
  --warning-bg-dark: rgba(146, 64, 14, 0.2);
  --warning-border-dark: #d97706;
  --warning-text-dark: #fbbf24;
}

/* Light mode default mapping */
:root {
  --success-bg: var(--success-bg-light);
  --success-border: var(--success-border-light);
  --success-text: var(--success-text-light);
  
  --error-bg: var(--error-bg-light);
  --error-border: var(--error-border-light);
  --error-text: var(--error-text-light);
  
  --info-bg: var(--info-bg-light);
  --info-border: var(--info-border-light);
  --info-text: var(--info-text-light);
  
  --warning-bg: var(--warning-bg-light);
  --warning-border: var(--warning-border-light);
  --warning-text: var(--warning-text-light);
}

/* Dark mode mapping */
.dark {
  --success-bg: var(--success-bg-dark);
  --success-border: var(--success-border-dark);
  --success-text: var(--success-text-dark);
  
  --error-bg: var(--error-bg-dark);
  --error-border: var(--error-border-dark);
  --error-text: var(--error-text-dark);
  
  --info-bg: var(--info-bg-dark);
  --info-border: var(--info-border-dark);
  --info-text: var(--info-text-dark);
  
  --warning-bg: var(--warning-bg-dark);
  --warning-border: var(--warning-border-dark);
  --warning-text: var(--warning-text-dark);
}
```

### 2. Use Variables in Lit Component Templates

Apply these variables in your Lit component rendering methods:

```javascript
private _renderSuccessMessage() {
  return this.successMessage ? html`
    <div class="success-message" style="
      background-color: var(--success-bg); 
      border: 2px solid var(--success-border);
      color: var(--success-text);
      padding: 0.75rem 1rem;
      margin-bottom: 1rem;
      border-radius: 0.375rem;
      position: relative;"
      role="alert">
      <strong class="font-bold">Success:</strong>
      <span class="block sm:inline">${this.successMessage}</span>
      <button 
        @click=${this._dismissMessage}
        class="dismiss-button absolute top-0 bottom-0 right-0 px-4 py-3"
        style="color: var(--success-text);"
      >
        <span class="sr-only">Dismiss</span>
        <svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
        </svg>
      </button>
    </div>
  ` : '';
}
```

### 3. Alternative: Define Variables in Component Styles

For even more component-specific control, you can include the variables in your component's static styles:

```javascript
static styles = css`
  ${unsafeCSS(tailwindStyles)}
  
  :host {
    /* Component-specific overrides */
    --component-success-bg: var(--success-bg);
    --component-success-border: var(--success-border);
    --component-success-text: var(--success-text);
  }
  
  .success-message {
    background-color: var(--component-success-bg);
    border: 2px solid var(--component-success-border);
    color: var(--component-success-text);
    padding: 0.75rem 1rem;
    margin-bottom: 1rem;
    border-radius: 0.375rem;
    position: relative;
  }
  
  .dismiss-button {
    color: var(--component-success-text);
  }
`;
```

## Best Practices

### Variable Naming Conventions

Our naming convention follows a semantic-first approach:

1. **Primary Purpose**: Start with semantic meaning (e.g., `--success`, `--error`, `--warning`, `--info`)
2. **Element Role**: Indicate the role (e.g., `-bg`, `-text`, `-border`)
3. **Theme Variation**: Add theme suffix when defining variations (e.g., `-light`, `-dark`)

Examples:
- `--success-bg-light`: Background color for success messages in light mode
- `--error-text-dark`: Text color for error messages in dark mode

### Usage Guidelines

1. **Use Semantic Variables**: Reference semantic variables (`--success-bg`) rather than theme-specific ones (`--success-bg-light`)
2. **Component-Specific Overrides**: When a component needs unique styling, define component-specific variables that reference global ones
3. **Maintain Border Visibility**: Always use `border: 2px solid` for sufficient visibility in dark mode
4. **Preserve Accessibility**: Test color contrast in both light and dark modes
5. **Combine with Tailwind**: Use Tailwind for layout and non-theme-sensitive styling

### When to Use This Approach

Use CSS variables for theme-sensitive styling when:

1. Component uses Shadow DOM and needs theme awareness
2. Element requires different styling based on theme
3. UI element conveys status or feedback
4. Consistent color scheme is needed across components
5. Tailwind's dark mode variant is not functioning as expected

Continue using Tailwind classes for:
1. Layout (width, height, padding, margin)
2. Typography size and weight
3. Non-theme-sensitive styling
4. Components that don't use Shadow DOM

## Testing

### Visual Testing

1. **Test Both Themes**: Always verify components in both light and dark modes
2. **Check Color Contrast**: Ensure WCAG-compliant contrast ratios in all states
3. **Verify Border Visibility**: Confirm borders are visible in both themes
4. **Test All Status Types**: Check success, error, warning, and info states

### Automated Testing

Add these tests to your test suite:

```javascript
it('should apply correct styles in dark mode', async () => {
  // Set dark mode
  document.documentElement.classList.add('dark');
  
  // Create component with success message
  element.successMessage = 'Test success message';
  await element.updateComplete;
  
  // Get computed styles
  const successElement = element.shadowRoot.querySelector('.success-message');
  const styles = window.getComputedStyle(successElement);
  
  // Test background, border, and text colors
  expect(styles.backgroundColor).to.include('rgb(6, 95, 70)'); // Dark mode success bg
  expect(styles.borderColor).to.include('rgb(5, 150, 105)'); // Dark mode success border
  expect(styles.color).to.include('rgb(52, 211, 153)'); // Dark mode success text
  
  // Reset dark mode
  document.documentElement.classList.remove('dark');
});
```

## Examples

### Success Message

```javascript
private _renderSuccessMessage() {
  return this.successMessage ? html`
    <div class="success-message" style="
      background-color: var(--success-bg); 
      border: 2px solid var(--success-border);
      color: var(--success-text);
      padding: 0.75rem 1rem;
      margin-bottom: 1rem;
      border-radius: 0.375rem;
      position: relative;"
      role="alert">
      <strong class="font-bold">Success:</strong>
      <span class="block sm:inline">${this.successMessage}</span>
      <button @click=${this._dismissSuccess} 
              class="dismiss-button absolute top-0 bottom-0 right-0 px-4 py-3"
              style="color: var(--success-text);">
        <span class="sr-only">Dismiss</span>
        <svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
        </svg>
      </button>
    </div>
  ` : '';
}
```

### Error Message

```javascript
private _renderErrorMessage() {
  return this.errorMessage ? html`
    <div class="error-message" style="
      background-color: var(--error-bg); 
      border: 2px solid var(--error-border);
      color: var(--error-text);
      padding: 0.75rem 1rem;
      margin-bottom: 1rem;
      border-radius: 0.375rem;
      position: relative;"
      role="alert">
      <strong class="font-bold">Error:</strong>
      <span class="block sm:inline">${this.errorMessage}</span>
      <button @click=${this._dismissError} 
              class="dismiss-button absolute top-0 bottom-0 right-0 px-4 py-3"
              style="color: var(--error-text);">
        <span class="sr-only">Dismiss</span>
        <svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
        </svg>
      </button>
    </div>
  ` : '';
}
```

## Common Issues and Solutions

### Issue: Variables Not Inheriting in Shadow DOM

**Solution:** Ensure variables are defined at `:root` level, not on `body` or other elements.

### Issue: Inconsistent Dark Mode Detection

**Solution:** Verify that the `.dark` class is being applied to the `html` or `:root` element, not to `body`.

### Issue: Dynamic Theme Changes Not Reflected

**Solution:** 
1. Ensure your dark mode toggle is correctly adding/removing the `.dark` class
2. Add a small delay after theme changes using `setTimeout` to ensure CSS updates

### Issue: Poor Contrast in Specific Components

**Solution:** Create component-specific overrides in the component's `static styles`:

```javascript
static styles = css`
  :host {
    --success-bg: rgba(6, 95, 70, 0.3); /* Stronger background for this component */
    --success-text: #10b981; /* Brighter text for better contrast */
  }
`;
```

## Migration Guide

### From Direct Tailwind Classes

If you're currently using direct Tailwind classes like:

```html
<div class="bg-green-100 dark:bg-green-900/30 border-2 border-green-400 dark:border-green-600 text-green-700 dark:text-green-300">
  Success message
</div>
```

Change to:

```html
<div style="
  background-color: var(--success-bg); 
  border: 2px solid var(--success-border);
  color: var(--success-text);">
  Success message
</div>
```

### From Inline Styles

If you're using direct color values:

```html
<div style="
  background-color: #ecfdf5; 
  border: 2px solid #10b981;
  color: #065f46;">
  Success message
</div>
```

Change to:

```html
<div style="
  background-color: var(--success-bg); 
  border: 2px solid var(--success-border);
  color: var(--success-text);">
  Success message
</div>
```

## Advanced Topics

### Extending for Custom Themes

The CSS variable approach makes it easy to support multiple theme variations:

```css
/* Add new theme-specific variables */
:root {
  /* High Contrast theme variables */
  --success-bg-high-contrast: #ffffff;
  --success-border-high-contrast: #047857;
  --success-text-high-contrast: #000000;
}

/* Add theme selector */
.high-contrast {
  --success-bg: var(--success-bg-high-contrast);
  --success-border: var(--success-border-high-contrast);
  --success-text: var(--success-text-high-contrast);
}
```

### Integration with Design Systems

For design system integration, expand the variable structure to include:

1. **Color Tokens**: Base colors defined by design system
2. **Semantic Tokens**: Functional meanings mapped to color tokens
3. **Component Tokens**: Component-specific mappings of semantic tokens

```css
:root {
  /* Color tokens */
  --color-green-50: #f0fdf4;
  --color-green-500: #10b981;
  --color-green-900: #064e3b;
  
  /* Semantic tokens - light */
  --success-bg-light: var(--color-green-50);
  --success-border-light: var(--color-green-500);
  --success-text-light: var(--color-green-900);
  
  /* Mappings remain the same */
  --success-bg: var(--success-bg-light);
  /* etc. */
}
```

## References

- [MDN: Using CSS custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)
- [Lit: Styles documentation](https://lit.dev/docs/components/styles/)
- [Web Components: Using shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM)
- [Tailwind CSS: Dark Mode](https://tailwindcss.com/docs/dark-mode)
- [Styling Web Components Using a Shared Style Sheet](https://css-tricks.com/styling-a-web-component/)
- [CSS Shadow Parts](https://developer.mozilla.org/en-US/docs/Web/CSS/::part)