---
layout: page
title: Project Plan & Status
nav_order: 2
permalink: /docs/project-plan-and-status/
---

# Prisoner's Dilemma App: Project Plan & Status

This document serves as a living record of our project plan, current status, and progress towards completing the Prisoner's Dilemma application.

## Current Status

**Phase**: Core Implementation (Connection UI Components - Red Phase)  
**Current TDD Phase**: RED - Writing tests for Connection UI Components  
**Last Updated**: May 10, 2025

## Project Vision

To create an interactive application that allows friends to play the Prisoner's Dilemma game, learn about game theory, and analyze strategies and outcomes.

## Development Approach

This project follows **Test Driven Development (TDD)** principles with a **Red-Green-Refactor** workflow:

1. **Red**: Write a failing test that defines the expected behavior *(CURRENT PHASE)*
2. **Green**: Write the minimal code needed to make the test pass
3. **Refactor**: Improve the code quality while maintaining passing tests

We've established a **hybrid testing strategy** with:
- **Vitest**: For fast unit testing of logic functions
- **@web/test-runner**: For browser-based component testing

We will commit to making the smallest possible code changes with each iteration, ensuring:
- Every feature is thoroughly tested before implementation
- Code remains clean, maintainable, and follows best practices
- Technical debt is minimized through continuous refactoring
- Development progress is measurable through test coverage

## User Stories

We've created a comprehensive set of [user stories](/prisoners-dilemma-docs/docs/technical/user-stories) to guide our development process. Key user stories include:

- Player registration and local storage functionality
- Connection management between players
- Game initiation and statistics tracking

These user stories are prioritized to ensure we focus on the most important features first. See the full [user stories document](/prisoners-dilemma-docs/docs/technical/user-stories) for details.

## Core App Functionality

### 1. Game Mechanics
- ✓ Simple initial version showing "The Game" screen
- ✓ Display of player names
- ✓ Counter showing how many times each player has opened the game
- ⬜ *(Future)* Full Prisoner's Dilemma rules and scoring

### 2. Social Features
- ✓ Connect with friends via shareable links (using enhanced connection service)
- 🟡 Maintain a connections list with status indicators *(Red Phase)*
- 🟡 Custom naming of connections *(Red Phase)*
- 🟡 Connection request management (creation and deletion) *(Red Phase)*

### 3. Data Management
- ✓ Local storage for all game data (player registration & stats complete)
- ✓ Type-safe error handling with Result pattern
- ✓ Complete adoption of Result pattern across codebase (no backward compatibility)
- ✓ Immutable data patterns for service implementations
- ⬜ Track game state and history with each connection
- ⬜ Store player preferences
- ⬜ Downloadable backup and restore functionality

### 4. Minimal Viable Product
- ✓ Players can register with a name
- ✓ Players can connect via shareable links 
- ✓ Basic game screen shows player names
- ✓ Count and display number of game opens per player
- ✓ Local data persistence

## Development Roadmap

### Phase 1: Planning & Setup ✓
- ✓ Create project documentation structure
- ✓ Define user stories and requirements
- ✓ Define architecture and technology stack
- ✓ Set up development environment with TDD tooling
- ✓ Configure dual testing frameworks (Vitest and @web/test-runner)
- ✓ Integrate Tailwind CSS with Vite
- ✓ Create .gitignore for project
- ⬜ Create initial wireframes/mockups

### Phase 2: Core Implementation
- ✓ **Player Registration Component**: Implement following TDD approach (User Stories #1-2)
  - ✓ Create failing tests for player registration (Red phase)
  - ✓ Implement minimal player registration to pass tests (Green phase)
  - ✓ Refactor player registration component (Refactor phase)
- ✓ Set up local storage functionality with tests (User Stories #2-4)
  - ✓ Create PlayerStorageService with comprehensive tests
  - ✓ Implement localStorage persistence for player data
  - ✓ Track app usage statistics (open count)
- ✓ Integrate player storage with game app
  - ✓ Display player info and open count in game screen
  - ✓ Handle player registration flow
  - ✓ Track app usage
- ✓ Resolve component integration issues
  - ✓ Fix player registration to game screen transition
  - ✓ Ensure proper component initialization and connection
- ⬅️ Create connection mechanism (User Stories #5-9)
  - ✓ Design ConnectionService interface and test structure (Red phase)
  - ✓ Implement ConnectionService to pass tests (Green phase)
  - ✓ Refactor ConnectionService with type-safe Result pattern (Refactor phase)
  - ✓ Remove backward compatibility layer and update all consumers to use Result pattern
  - ✓ Refactor ConnectionService to use immutable data patterns
  - 🟡 **Design connection UI components and tests (Red Phase - CURRENT)**
  - ⬜ Implement connection UI components
  - ⬜ Integrate connection management with game app
- ⬜ Implement basic game mechanics (User Stories #11-12)

### Phase 3: Testing & Refinement
- ⬜ Set up CI/CD pipeline for automated testing
- ⬜ Test with small user group
- ⬜ Refine UI based on feedback
- ⬜ Optimize performance
- ⬜ Fix identified bugs

### Phase 4: Advanced Features
- ⬜ Implement full Prisoner's Dilemma rules
- ⬜ Add scoring system
- ⬜ Create visualization of game history
- ⬜ Add strategy suggestions

## Key Decisions & Notes

<details>
<summary>Click to expand/collapse</summary>
<div markdown="1">

| Date | Decision | Rationale |
|------|----------|-----------|
| May 10, 2025 | Started Red Phase for Connection UI Components | Beginning to write failing tests that define the expected behavior of our connection UI components |
| May 8, 2025 | Decided against PlayerStorageService [increased immutability](https://claude.ai/share/aa0dd37e-1b27-4999-b1b9-a91b94e10842) | to move forward more quickly, focus on functionality in this implementation |
| May 7, 2025 | Documented storage options beyond localStorage | To address potential storage limitations as game data grows and to prepare for advanced features requiring more storage |
| May 7, 2025 | Documented comprehensive code improvement recommendations | To provide a roadmap for enhancing code quality, architecture, and performance |
| May 7, 2025 | Refactored PlayerStorageService for immutability | Enhanced predictability and eliminated side effects while maintaining test compatibility |
| May 7, 2025 | Refactored ConnectionService for immutability | Improved predictability, maintainability, and data integrity by eliminating state mutations |
| May 7, 2025 | Used spread operator for object copying | Provides concise syntax for creating copies, suitable for simple data structures, and consistent with modern JavaScript practices |
| May 6, 2025 | Removed all backward compatibility layers | Clean break eliminates technical debt and forces update of all consumers to the Result pattern |
| May 6, 2025 | Created ResultUtils utility library | Centralizes common Result operations and promotes consistent handling across components |
| May 6, 2025 | Implemented Result pattern for error handling | Provides explicit, type-safe error handling without relying on exceptions or null checks |
| May 6, 2025 | Created shared UuidUtils class | Centralizes UUID generation and validation for better maintainability and consistency |
| May 5, 2025 | Implemented ConnectionService methods using Array methods | Standard JS Array methods provide clear, readable code suitable for the expected data volume |
| May 5, 2025 | Structured ConnectionService implementation by dependency order | Creates a natural progression of complexity and enables testing of dependent functionality |
| May 4, 2025 | Created connection service test structure | Following TDD approach for implementing connection mechanism |
| May 4, 2025 | Established debugging checklist for web component issues | To provide a systematic approach to troubleshooting component integration problems |
| May 3, 2025 | Added dedicated test helper methods to components | To maintain encapsulation while enabling proper component testing |
| May 3, 2025 | Used class extension for service mocking | To ensure type compatibility and selective method overriding |
| May 3, 2025 | Implemented PlayerStorageService with localStorage | To provide persistent storage for player data across sessions |
| May 2, 2025 | Adapted tests to trim whitespace before assertions | To maintain readable component templates while ensuring reliable tests |
| May 2, 2025 | Created dedicated CommonJS script for Tailwind generation | To resolve TypeScript compatibility issues with ES modules |
| May 1, 2025 | Updated to Tailwind CSS v4 with dedicated PostCSS plugin | To fix build configuration and leverage the latest Tailwind capabilities |
| May 1, 2025 | Used .cjs extension for configuration files | To explicitly mark CommonJS modules in an ES module project |
| April 30, 2025 | Added explicit Shadow DOM configuration | To ensure consistent component testing with proper element access |
| April 30, 2025 | Used explicit Mocha imports in test files | To provide proper TypeScript type checking for test functions |
| April 30, 2025 | Established wait patterns for component tests | To ensure proper handling of LitElement rendering cycle in tests |
| April 28, 2025 | Fixed npm script configuration | To support dual testing strategy and enable proper TDD workflow |
| April 28, 2025 | Created .gitignore file | To ensure proper version control with appropriate exclusions |
| April 28, 2025 | Started with player registration tests | To establish TDD workflow and implement fundamental component first |
| April 27, 2025 | Adopted dual testing strategy with Vitest and @web/test-runner | To leverage Vitest's speed for unit tests and @web/test-runner's browser environment for component tests |
| April 27, 2025 | Switched to Vite build system with Tailwind CSS v4.0 | To improve developer experience with faster builds and simplified Tailwind integration |
| April 27, 2025 | Maintained strict TypeScript configuration | To ensure type safety throughout the project and prevent bugs |
| April 27, 2025 | Adopted Test Driven Development (TDD) with Red-Green-Refactor | To ensure quality code, prevent regressions, and provide clear development path |
| April 26, 2025 | Selected Lit + Open WC + Tailwind CSS | For optimal web component development with modern styling approach |
| April 26, 2025 | Created dedicated project plan page | To maintain a central record of project status and plans |
| April 26, 2025 | Defined initial user stories | To establish clear requirements and development priorities |
| April 26, 2025 | Selected friends-connect API | To leverage an existing solution for player connections rather than building from scratch |

</div>
</details>

## Current Focus: Connection UI Components - Red Phase

We are currently in the **Red Phase** of TDD for connection UI components. This means we are:

1. Writing failing tests that define the expected behavior of our connection components
2. Designing component interfaces and public methods
3. Establishing test patterns for event handling and Result pattern integration
4. Creating comprehensive test suites for each planned component

### Components Being Tested:

1. **Connection Creation Component**:
   - Tests for generating shareable links
   - Tests for error handling using Result pattern
   - Tests for UI state changes based on operation outcomes
   - Tests for accessibility and user interaction

2. **Connection List Component**:
   - Tests for displaying connection lists
   - Tests for empty states and error conditions
   - Tests for custom event dispatching
   - Tests for connection state indicators

3. **Connection Detail Component**:
   - Tests for displaying connection information
   - Tests for status-based UI rendering
   - Tests for operation controls (accept, delete, etc.)
   - Tests for error state handling

4. **Connection Request Component**:
   - Tests for request acceptance and rejection
   - Tests for validation error handling
   - Tests for UI messaging based on request status
   - Tests for Result pattern integration

Each test suite is being designed to:
- Use the Result pattern for operation outcomes
- Leverage Lit's reactive properties for state management
- Include both happy path and error scenarios
- Test component communication via custom events
- Ensure accessibility requirements are met

Once all tests are written and failing properly (Red phase complete), we'll move to the Green phase to implement the minimal code to make these tests pass.

## Technology Choices

### Build System
- **Build Tool**: Vite
- **CSS Framework**: Tailwind CSS v4.0 with `@tailwindcss/postcss` plugin
- **Language**: TypeScript with strict type checking
- **Build Scripts**: CommonJS (.cjs) for build-time utilities

### Testing Strategy
- **Unit Testing**: Vitest (for pure logic functions)
- **Component Testing**: @web/test-runner (for browser-based component testing)
- **Test Helpers**: @open-wc/testing for component fixtures and assertions
- **Text Content Testing**: Use `.trim()` for consistent assertions
- **Service Mocking**: Result-based mocks replacing direct values
- **Component State Testing**: Dedicated public test helper methods

### Frontend Technologies
- **Web Components**: Lit with Shadow DOM
- **Best Practices**: Open WC principles (adapted for Vite)
- **Styling**: Tailwind CSS with dedicated build script
- **Data Persistence**: localStorage with service abstraction
- **Error Handling**: Result pattern for type-safe error handling
- **State Management**: Lit's reactive properties with Result pattern
- **Component Communication**: Custom events with strongly typed detail
- **UI Error Handling**: Error type-based conditional rendering

### TypeScript Event Handling Pattern

We've established a comprehensive TypeScript pattern for handling custom events in our connection UI components:

#### Implementation Details:

1. **Strongly Typed Custom Events**
   - Interfaces for event detail objects (e.g., `ConnectionEventDetail`)
   - Extended `HTMLElementEventMap` globally to type custom events
   - Defined events: `game-requested`, `connection-created`, `confirm-delete-connection`, `refresh-connections`, `play-with-connection`

2. **Type-Safe Event Handlers**
   - Handlers declared as `EventListener` interface for standard compatibility
   - Implementation uses type assertions for proper type checking
   - Bound handlers stored as class properties for clean lifecycle management

3. **Typed Event Dispatching**
   - Custom events with proper type parameters
   - Events configured with `bubbles: true` and `composed: true` for Shadow DOM compatibility
   - All events include strongly typed detail objects

4. **Event Helper Methods**
   - Generic `fireEvent<T>` method for consistent event dispatching
   - Simplifies event creation while maintaining type safety
   - Reduces boilerplate code across components

```typescript
// Example pattern:
interface ConnectionEventDetail {
  connectionId: string;
  connectionName: string;
}

declare global {
  interface HTMLElementEventMap {
    'game-requested': CustomEvent<ConnectionEventDetail>;
  }
}

protected fireEvent<T>(eventName: string, detail: T): void {
  this.dispatchEvent(new CustomEvent(eventName, {
    detail,
    bubbles: true,
    composed: true
  }));
}
```

### API Selection
We will use the **friends-connect** API for handling player connections. This is a Rust library specifically designed for connecting players in game applications.

**Source**: [github.com/randallard/friends-connect](https://github.com/randallard/friends-connect)

**Description**: A Rust library for connecting with friends to play games, providing the server-side functionality we need for managing connection requests and active connections.

## Open Questions

- What is our target test coverage percentage?
- How will we handle cross-browser compatibility testing?
- Should we automate accessibility testing?
- How will we handle edge cases like connection failures?
- Should we create a centralized error reporting service?
- What metrics should we collect about error frequency and types?
- How do we best balance immutability with performance for complex data structures?
- Should we adopt a dedicated immutability library for more complex state management?

## Next Steps

1. Complete Red Phase for all connection UI component tests
2. Move to Green Phase - implement minimal code to pass tests
3. Enter Refactor Phase - optimize implementation
4. Integrate connection management with game app
5. Begin implementing game mechanics