---
layout: page
title: Dark Mode Testing
nav_order: 5
permalink: /docs/technical/dark-mode-testing/
---

# Testing Dark Mode

This document explains how to test the dark mode functionality in the Prisoner's Dilemma game.

## Test Structure

The dark mode testing suite consists of several types of tests:

1. **Unit Tests** - Test DarkModeService with Result pattern
2. **Component Tests** - Test dark-mode-toggle component
3. **Integration Tests** - Test interaction between components
4. **Visual Tests** - Ensure visual consistency across themes

## Running Tests

### Unit Tests (Vitest)
```bash
npm run test:unit
# or watch mode
npm run test:unit:watch
# or with coverage
npm run test:unit:coverage
```

### Component Tests (Web Test Runner)
```bash
npm run test:components
# or watch mode
npm run test:components:watch
```

### All Tests
```bash
npm test
```

## Test Files

### Core Test Files

1. **`dark-mode.service.test.ts`** (Unit Test - Vitest)
   - Tests the DarkModeService functionality
   - Covers initialization with Result pattern
   - Tests theme management and persistence
   - Tests system preference detection
   - Validates error handling with ThemeError
   - Tests system theme change listener

2. **`dark-mode-toggle.test.ts`** (Component Test - Web Test Runner)
   - Tests the DarkModeToggle component
   - Covers rendering in both themes
   - Tests user interaction and toggle functionality
   - Tests accessibility attributes
   - Validates error states and disabled behavior
   - Tests custom event emission
   - Tests lifecycle and cleanup

## Key Test Scenarios

### Service Tests (Result Pattern)

1. **Initialization**
   ```typescript
   test('should initialize with saved theme')
   test('should initialize with system preference when no saved theme')
   test('should default to light theme when no system preference')
   test('should handle localStorage errors gracefully')
   ```

2. **Theme Management**
   ```typescript
   test('should return saved theme from localStorage')
   test('should detect theme from document class')
   test('should default to light theme when no indicators')
   ```

3. **Theme Setting**
   ```typescript
   test('should set dark theme successfully')
   test('should set light theme successfully')
   test('should reject invalid theme values')
   test('should handle localStorage errors')
   ```

4. **Theme Toggle**
   ```typescript
   test('should toggle from light to dark')
   test('should toggle from dark to light')
   test('should handle getCurrentTheme errors')
   ```

5. **System Theme Listener**
   ```typescript
   test('should set up listener and return cleanup function')
   test('should update theme when system preference changes')
   test('should remove listener when cleanup is called')
   ```

### Component Tests

1. **Rendering**
   ```typescript
   test('should initialize with light theme')
   test('should initialize with dark theme')
   test('should handle initialization errors')
   ```

2. **Toggle Functionality**
   ```typescript
   test('should toggle from light to dark')
   test('should toggle from dark to light')
   test('should handle toggle errors')
   test('should not toggle when disabled')
   ```

3. **Accessibility**
   ```typescript
   test('should have proper aria-label')
   test('should have proper title for light mode')
   test('should have proper title for dark mode')
   ```

4. **Lifecycle**
   ```typescript
   test('should clean up system theme listener on disconnect')
   ```

## Mocking Strategies

### localStorage Mock (Vitest)
```typescript
beforeEach(() => {
  mockLocalStorage = {};
  Object.defineProperty(global, 'localStorage', {
    value: {
      getItem: vi.fn((key: string) => mockLocalStorage[key] || null),
      setItem: vi.fn((key: string, value: string) => {
        mockLocalStorage[key] = value;
      }),
      removeItem: vi.fn((key: string) => {
        delete mockLocalStorage[key];
      }),
      clear: vi.fn(() => {
        mockLocalStorage = {};
      }),
    },
    writable: true,
  });
});
```

### matchMedia Mock (Vitest)
```typescript
beforeEach(() => {
  mockMatchMedia = {
    matches: false,
    addEventListener: vi.fn(),
    removeEventListener: vi.fn(),
  };
  
  Object.defineProperty(global.window, 'matchMedia', {
    value: vi.fn(() => mockMatchMedia),
    writable: true,
  });
});
```

### Document ClassList Mock (Vitest)
```typescript
beforeEach(() => {
  mockClassList = new Set<string>();
  
  Object.defineProperty(global.document, 'documentElement', {
    value: {
      classList: {
        add: vi.fn((className: string) => mockClassList.add(className)),
        remove: vi.fn((className: string) => mockClassList.delete(className)),
        contains: vi.fn((className: string) => mockClassList.has(className)),
      },
    },
    writable: true,
  });
});
```

### Component Test Mocking (Web Test Runner)
```typescript
class MockDarkModeService extends DarkModeService {
  private mockTheme: Theme = Theme.LIGHT;
  private mockError: ThemeError | null = null;
  
  setMockTheme(theme: Theme): void {
    this.mockTheme = theme;
  }
  
  setMockError(error: ThemeError | null): void {
    this.mockError = error;
  }
  
  initializeDarkMode(): Result<Theme, ThemeError> {
    if (this.mockError) {
      return Result.failure(this.mockError);
    }
    return Result.success(this.mockTheme);
  }
  
  toggleTheme(): Result<Theme, ThemeError> {
    if (this.mockError) {
      return Result.failure(this.mockError);
    }
    this.mockTheme = this.mockTheme === Theme.LIGHT ? Theme.DARK : Theme.LIGHT;
    return Result.success(this.mockTheme);
  }
}
```

## Testing with Result Pattern

The DarkModeService uses the Result pattern for error handling:

```typescript
// Test successful operations
test('should initialize with saved theme', () => {
  mockLocalStorage['prisonersDilemma_theme'] = Theme.DARK;
  
  const result = darkModeService.initializeDarkMode();
  
  expect(result.isSuccess()).toBe(true);
  expect(result.getValue()).toBe(Theme.DARK);
});

// Test error conditions
test('should handle localStorage errors gracefully', () => {
  vi.spyOn(localStorage, 'getItem').mockImplementation(() => {
    throw new Error('Storage error');
  });
  
  const result = darkModeService.initializeDarkMode();
  
  expect(result.isFailure()).toBe(true);
  expect(result.getError().type).toBe(ThemeErrorType.STORAGE_ERROR);
});
```

## Best Practices

1. **Type Safety**
   - Use TypeScript types for all mocks
   - Leverage the Result pattern for error testing
   - Type custom events properly

2. **Isolation**
   - Mock all browser APIs (localStorage, matchMedia, etc.)
   - Reset mocks between tests
   - Clean up event listeners

3. **Coverage**
   - Test both success and error paths
   - Cover all theme states
   - Test edge cases (no localStorage, etc.)

4. **Component Testing**
   - Wait for `updateComplete` after state changes
   - Test accessibility attributes
   - Verify custom event emissions

5. **Service Testing**
   - Test all Result scenarios (success/failure)
   - Verify error types are correct
   - Test system preference integration

## Common Issues

### Tests Failing After Implementation Changes

1. **Result Pattern Changes**
   - Update tests to use `isSuccess()`/`isFailure()`
   - Check error types match `ThemeErrorType`
   - Verify Result values are correct type

2. **Theme Enum Changes**
   - Use `Theme.LIGHT`/`Theme.DARK` instead of booleans
   - Update mock implementations
   - Check localStorage key name

3. **Event Changes**
   - Verify custom event names match
   - Check event detail structure
   - Update event listener tests

### Debugging Tests

1. **Vitest UI**
   ```bash
   npm run test:unit:ui
   ```

2. **Console Debugging**
   ```typescript
   // Add to test
   console.log('Current theme:', result.getValue());
   console.log('Error:', result.getError());
   ```

3. **Mock Verification**
   ```typescript
   expect(localStorage.setItem).toHaveBeenCalledWith(
     'prisonersDilemma_theme',
     Theme.DARK
   );
   ```

### Test Structure Examples

```typescript
// Service test structure
describe('DarkModeService', () => {
  let darkModeService: DarkModeService;
  
  beforeEach(() => {
    darkModeService = new DarkModeService();
    // Setup mocks
  });
  
  describe('initializeDarkMode', () => {
    test('should handle success case', () => {
      // Test implementation
    });
    
    test('should handle error case', () => {
      // Test implementation
    });
  });
});

// Component test structure
describe('DarkModeToggle', () => {
  let el: DarkModeToggle;
  let mockService: MockDarkModeService;
  
  beforeEach(async () => {
    mockService = new MockDarkModeService();
    el = await fixture<DarkModeToggle>(html`<dark-mode-toggle></dark-mode-toggle>`);
    el.darkModeService = mockService;
    await el.updateComplete;
  });
  
  describe('toggle functionality', () => {
    test('should toggle theme', async () => {
      // Test implementation
    });
  });
});
```