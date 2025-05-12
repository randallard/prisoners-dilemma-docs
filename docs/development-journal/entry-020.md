---
layout: default
title: "Journal Entry #20"
parent: Development Journal
nav_order: [REVERSE_CHRONOLOGICAL_ORDER]
date: 2025-05-11
---

# Journal Entry #20 - Solving Dark Mode Styling Issues in Shadow DOM Components
{: .no_toc }

**Date:** May 11, 2025
{: .fs-5 }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Current Status

Today we tackled a perplexing issue with Tailwind CSS dark mode styling in our Lit components that was affecting the success message display. The issue specifically manifested with green color classes in dark mode, which was particularly evident in the connection form component. We successfully implemented a solution using CSS variables that provides consistent styling across both light and dark modes.

We still have several failing tests in our connection components that need to be resolved, specifically related to user stories #6 and #8 involving connection link management and display.

## Challenges Encountered

### Challenge 1: Shadow DOM Isolation Breaking Dark Mode Styling

**Description:** Our Lit components with Shadow DOM were experiencing inconsistent dark mode styling behaviors, with red and blue colors working properly but green color classes failing in dark mode.

**Impact:**
- Success messages displayed with improper contrast in dark mode
- Green text appeared white or light on light green backgrounds in dark mode
- Borders were barely visible or missing entirely
- User feedback for successful actions was compromised

**Root Cause:** Shadow DOM isolation was preventing some of Tailwind's dark mode variants from properly applying to green colors specifically. The issue appeared to be related to how green color classes were compiled or applied within the Shadow DOM context.

### Challenge 2: Inconsistent Solutions Across Components

**Description:** Initial attempts to resolve the issue led to component-specific workarounds that were not reusable or maintainable.

**Symptoms:**
- Different fixes for each component
- Inconsistent styling between similar feedback elements
- Increased technical debt with one-off solutions
- Poor maintainability for theme changes

### Challenge 3: Reconciling Tailwind Utility Approach with Component Needs

**Description:** Tailwind's utility-first approach conflicted with the need for consistent theme-aware styling across isolated Shadow DOM components.

**Challenges:**
- Tailwind dark mode relies on parent class selectors that don't penetrate Shadow DOM
- Inline style approach sacrificed some Tailwind benefits
- Custom class definitions required additional configuration
- Needed solution that maintained developer experience

## Resolution Process

### Step 1: Research and Analysis

We investigated multiple approaches:
1. Compared working (red, blue) vs. non-working (green) color implementations
2. Researched Shadow DOM limitations with Tailwind dark mode
3. Explored CSS variables as a potential solution
4. Tested various approaches to understand limitations

### Step 2: CSS Variables Implementation

We implemented a comprehensive solution:
1. Defined light and dark mode color variables at the `:root` level
2. Created variable mappings that change based on current theme
3. Updated component styling to reference these variables
4. Tested across different states and components

### Step 3: Documentation and Standardization

We documented the approach for future components:
1. Created a detailed technical guide for the pattern
2. Updated project plan to incorporate this technique
3. Standardized CSS variable naming conventions
4. Established best practices for variable usage

## Decisions

### Decision 1: Adopt CSS Variables for Theme-Sensitive Styling

**Context:** Need a reliable way to style Shadow DOM components that respond to theme changes.

**Options Considered:**
- Continue with inconsistent Tailwind dark mode classes
- Alternative library more compatible with Shadow DOM
- Use inline styles with detection logic
- Adopt CSS variables with theme-sensitive mappings

**Decision:** Implement CSS variables at root level with theme-specific mappings.

**Rationale:**
- CSS variables inherit through Shadow DOM boundaries
- Provides consistent behavior across all components
- Maintains theme awareness without complex selectors
- Allows for centralized theming control
- Compatible with Tailwind for other styling needs

### Decision 2: Standardize Variable Naming Convention

**Context:** Need consistent naming for theme-related CSS variables.

**Options Considered:**
- Function-based naming (e.g., `--error-bg`, `--success-text`)
- Color-based naming (e.g., `--green-bg-light`, `--red-text-dark`)
- Component-based naming (e.g., `--alert-bg`, `--button-text`)
- Hybrid approach with semantic and color variants

**Decision:** Adopt function-based primary naming with light/dark variants.

**Rationale:**
- Makes semantic purpose immediately clear
- Easier to update color values while maintaining meaning
- Aligns with design system thinking
- Reduces confusion when similar colors have different purposes
- Easy to extend with new semantic categories

### Decision 3: Implement in Core Components First

**Context:** Need to prioritize where to apply the CSS variable pattern.

**Options Considered:**
- Comprehensive application to all components
- As-needed application when issues arise
- Focus on feedback/message components first
- Only apply to new components going forward

**Decision:** Implement in feedback components first, then expand to all theme-sensitive elements.

**Rationale:**
- Addresses most visible user experience issues first
- Provides immediate value for critical feedback paths
- Allows testing pattern before wider adoption
- Creates clear examples for other component updates
- Balances immediate fixes with systematic improvement

## Implementation Details

### Key Components Updated

1. **CSS Variable Definitions**:
   - Added to global index.css
   - Included light and dark mode variables
   - Created semantic mappings
   - Ensured proper inheritance

2. **ConnectionFormComponent**:
   - Updated success message rendering
   - Replaced direct Tailwind classes with CSS variable references
   - Maintained accessible contrast ratios
   - Ensured consistent styling across themes

3. **Documentation**:
   - Created technical guide for the pattern
   - Updated project standards
   - Added to design system documentation
   - Created reusable examples

### Sample CSS Variable Structure

```css
:root {
  /* Light mode semantic variables */
  --success-bg-light: #ecfdf5;
  --success-border-light: #10b981;
  --success-text-light: #065f46;
  
  /* Dark mode semantic variables */
  --success-bg-dark: rgba(6, 95, 70, 0.2);
  --success-border-dark: #059669;
  --success-text-dark: #34d399;
}

/* Light mode default mapping */
:root {
  --success-bg: var(--success-bg-light);
  --success-border: var(--success-border-light);
  --success-text: var(--success-text-light);
}

/* Dark mode mapping */
.dark {
  --success-bg: var(--success-bg-dark);
  --success-border: var(--success-border-dark);
  --success-text: var(--success-text-dark);
}
```

## Lessons Learned

### Lesson 1: Shadow DOM Requires Different Styling Strategies

**Insight:** Standard CSS class patterns don't always work as expected within Shadow DOM.

**Example:** Tailwind's dark mode variant mechanism couldn't reliably target green color classes within Shadow DOM components.

**Takeaway:** Shadow DOM requires style approaches that leverage inheritance (like CSS variables) rather than relying on parent-child selectors.

### Lesson 2: Test All Color Variations Explicitly

**Insight:** Color systems can behave inconsistently in different contexts.

**Observation:** We only discovered the issue with green colors after implementing red for errors and blue for other UI elements.

**Takeaway:** Test each color palette explicitly in both light and dark modes, especially for critical feedback components.

### Lesson 3: CSS Variables Provide Superior Shadow DOM Compatibility

**Insight:** CSS variables offer a more reliable approach for theming Shadow DOM components.

**Impact:**
- CSS variables inherit through Shadow DOM boundaries
- Provides consistent behavior across components
- Creates centralized theme control
- Simplifies component implementation

**Takeaway:** Prioritize CSS variables for theme-sensitive styling in Shadow DOM components.

## Impact on Project

### Positive Outcomes

1. **Improved Feedback UX**: Proper contrast and visibility for success messages
2. **Consistent Theming**: Reliable theme switching across all components
3. **Maintainable Pattern**: Centralized theme control for easier updates
4. **Reduced Technical Debt**: Standard approach rather than component-specific fixes
5. **Developer Experience**: Clear pattern for implementing theme-aware components

### Technical Improvements

- Success messages now properly visible in dark mode
- Green color styling works reliably across all components
- Border visibility improved in all states
- Theme transitions maintain consistent styling
- Reduced debug time for theme-related styling issues

## Next Actions

1. Address failing tests in connection components:
   - Fix `ConnectionFormComponent` test for "displays a clear visual indication when a new link has been generated"
   - Resolve `ConnectionListComponent` tests for pending connection management
   - Implement missing functionality for copying and resharing pending connection links
2. Fix failing test for dark mode toggle in `GameApp` component
3. Apply the CSS variable pattern to all feedback/message components
4. Create a comprehensive set of semantic color variables for the entire application
5. Document the pattern in the style guide
6. Update existing components using the new pattern
7. Add theme-related visual testing to the test suite

## References & Resources

- [CSS Variables with Lit Elements](https://lit.dev/docs/components/styles/#using-css-custom-properties)
- [Shadow DOM Styling Best Practices](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM#styling_with_css)
- [Tailwind CSS with Shadow DOM Discussion](https://github.com/tailwindlabs/tailwindcss/discussions/1935)
- [CSS Custom Properties Through Shadow DOM](https://www.w3.org/TR/css-variables-1/#using-variables)
- [Styling Web Components](https://css-tricks.com/styling-a-web-component/)

---

**Hours Logged:** 3.5

**Tags:** #shadow-dom #dark-mode #css-variables #theme-system #lit-element #tailwind-css #developer-experience #user-experience #styling #web-components