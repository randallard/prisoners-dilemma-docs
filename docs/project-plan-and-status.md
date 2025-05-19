---
layout: page
title: Project Plan & Status
nav_order: 2
permalink: /docs/project-plan-and-status/
---

# Prisoner's Dilemma App: Project Plan & Status

This document serves as a living record of our project plan, current status, and progress towards completing the Prisoner's Dilemma application.

## Current Status

**Phase**: Core Implementation (Connection UI Components - Green Phase)  
**Current TDD Phase**: GREEN - Implementing code to pass tests for Connection API Service  
**Last Updated**: May 19, 2025

## Project Vision

To create an interactive application that allows friends to play the Prisoner's Dilemma game, learn about game theory, and analyze strategies and outcomes.

## Development Approach

This project follows **Test Driven Development (TDD)** principles with a **Red-Green-Refactor** workflow:

1. **Red**: Write a failing test that defines the expected behavior
2. **Green**: Write the minimal code needed to make the test pass *(CURRENT PHASE)*
3. **Refactor**: Improve the code quality while maintaining passing tests

We've established a **hybrid testing strategy** with:
- **Vitest**: For fast unit testing of logic functions
- **@web/test-runner**: For browser-based component testing

We will commit to making the smallest possible code changes with each iteration, ensuring:
- Every feature is thoroughly tested before implementation
- Code remains clean, maintainable, and follows best practices
- Technical debt is minimized through continuous refactoring
- Development progress is measurable through test coverage

### Focused AI Pairing Methodology

We've adopted a structured approach called "Focused AI Pairing" for complex debugging and implementation tasks. This approach has proven particularly effective for the WebSocket implementation:

1. **Single Issue Focus**: Address one problem at a time instead of the entire codebase
2. **Staged Problem Analysis**: Analyze issues in stages before implementing solutions
3. **Wait-For-Approval Workflow**: Request analysis, wait for approval, then proceed to implementation
4. **Documented Decision Thinking**: Create visual diagrams of the solution approach for team reference

This methodology allows us to leverage AI assistance without sacrificing architectural consistency or code quality. For details on our WebSocket testing implementation using this approach, see the [WebSocket Testing Flow](/prisoners-dilemma-docs/docs/technical/websocket-testing-flow/) diagram.

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
- 🟢 WebSocket service integration for real-time connection status *(Green Phase)*
- 🟡 Maintain a connections list with status indicators *(Testing Phase)*
- 🟡 Custom naming of connections *(Testing Phase)*
- 🟡 Connection request management (creation and deletion) *(Testing Phase)*

### 3. Data Management
- ✓ Local storage for all game data (player registration & stats complete)
- ✓ Type-safe error handling with Result pattern
- ✓ Complete adoption of Result pattern across codebase (no backward compatibility)
- ✓ Immutable data patterns for service implementations
- 🟢 WebSocket message handling with type safety *(Green Phase)*
- ⬜ Track game state and history with each connection
- ⬜ Store player preferences
- ⬜ Downloadable backup and restore functionality

### 4. User Experience
- ✓ Dark mode support with system preference detection
- ✓ Smooth theme transitions with toggle button
- ✓ Persistent theme preference storage
- ✓ Responsive design with Tailwind CSS
- ✓ CSS Variables for theme-consistent Shadow DOM components
- 🟡 Complete implementation of semantic CSS variables across all components

### 5. Minimal Viable Product
- ✓ Players can register with a name
- ✓ Players can connect via shareable links 
- ✓ Basic game screen shows player names
- ✓ Count and display number of game opens per player
- ✓ Local data persistence
- ✓ Dark mode support

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
- ✓ **Dark Mode Implementation**
  - ✓ Add system preference detection
  - ✓ Update all components with dark mode classes
  - ✓ Write comprehensive tests for dark mode functionality
  - ✓ Resolve test runner conflicts by separating unit/component tests
  - ✓ Implement media query-based dark mode strategy
  - ✓ Update CSS to use prefers-color-scheme media querieS
  - ✓ Remove theme toggle and localStorage persistence 
- ⬅️ Create connection mechanism (User Stories #5-9)
  - ✓ Design ConnectionService interface and test structure (Red phase)
  - ✓ Implement ConnectionService to pass tests (Green phase)
  - ✓ Refactor ConnectionService with type-safe Result pattern (Refactor phase)
  - ✓ Remove backward compatibility layer and update all consumers to use Result pattern
  - ✓ Refactor ConnectionService to use immutable data patterns
  - ✓ Design WebSocket API service tests (Red Phase)
  - 🟢 **Implement WebSocket API service to pass tests (Green Phase - CURRENT)**
    - ✓ Create [WebSocket Testing Flow](/prisoners-dilemma-docs/docs/technical/websocket-testing-flow/) for improved test reliability
    - ✓ Implement connection state management
    - 🟢 Implement message processing system with type safety
    - 🟢 Integrate with Result pattern for error handling
  - ⬜ Refactor WebSocket implementation (Refactor Phase)
  - 🟡 Design connection UI components and tests (Red Phase)
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
| May 19, 2025 | Adopted Focused AI Pairing methodology | Created a structured approach for AI assistance that maintains architectural consistency while accelerating development. This approach focuses on single issues, stages problem analysis, uses a wait-for-approval workflow, and documents decision thinking with visual diagrams. [Journal Entry #25](/prisoners-dilemma-docs/docs/journal/entry-025) contains more details. |
| May 19, 2025 | Created WebSocket Testing Flow documentation | Visualized the WebSocket test flow to improve understanding and ensure reliable test implementation. This approach helped identify and fix key issues in the testing process, such as socket instance tracking and message handling. |
| May 18, 2025 | Decided against full AI code generation | After experimentation with an IDE agent (documented in [Journal Entry #24](/prisoners-dilemma-docs/docs/journal/entry-024)), we found that maintaining architectural consistency and code quality was more challenging with full code generation. Switched to a collaborative approach where human developers maintain control of architecture. |
| May 14, 2025 | Switched from class-based dark mode to media query strategy | Simplified theming implementation by relying on OS preferences via prefers-color-scheme media queries, removing the need for JavaScript toggles and state management. This reduces codebase complexity while maintaining responsive theme handling. |
| May 10, 2025 | Adopted separated unit/component test directories | Following testing tool conventions, unit tests (Vitest) are now in `test/unit/` mirroring source structure, while component tests (Web Test Runner) are in `test/components/`. This separation prevents test runner conflicts and follows each tool's best practices. |
| May 10, 2025 | Chose happy-dom over jsdom for testing environment | Selected happy-dom for its superior Web Components and Shadow DOM support, 2-10x performance improvement for TDD workflow, better Vite/Vitest integration, and more accurate browser behavior emulation. These advantages outweigh the trade-off of a smaller community compared to jsdom. [Detailed analysis and recommendations](https://claude.ai/share/07e3d8f6-0e80-4bf1-8c79-90e01f4a900a) |
| May 10, 2025 | Implemented dark mode support with toggle | Added comprehensive dark mode functionality with system preference detection, persistent storage, and smooth transitions to improve user experience |
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

## Current Focus: WebSocket API Service - Green Phase

We are currently in the **Green Phase** of TDD for the WebSocket API service implementation. This involves:

1. Implementing the minimal code needed to make our tests pass
2. Ensuring proper WebSocket connection management
3. Creating a type-safe message processing system
4. Integrating with our Result pattern for error handling

### Key Components Being Implemented:

1. **WebSocket Connection Management**:
   - Robust connection establishment and maintenance
   - Proper handling of connection states and events
   - Error detection and reporting through ApiError types

2. **Message Processing System**:
   - Type-safe message handling with TypeScript interfaces
   - Request-response mapping with unique request IDs
   - Timeout handling for incomplete requests

3. **Event-Based Communication**:
   - Callback registration for different message types
   - Event propagation to UI components
   - Error event handling for network issues

4. **Result Pattern Integration**:
   - Consistent error handling across asynchronous operations
   - Type-safe Result returns for all API methods
   - Clear error type mapping for network and API errors

### Testing Methodology

For the WebSocket implementation, we've created a detailed [WebSocket Testing Flow](/prisoners-dilemma-docs/docs/technical/websocket-testing-flow/) diagram to ensure our tests reliably verify component behavior. This approach has significantly improved our test reliability by:

1. Properly tracking WebSocket instances throughout the test lifecycle
2. Reliably capturing and verifying sent messages
3. Simulating server responses in a controlled manner
4. Managing asynchronous timing issues that previously caused flaky tests

Our "Focused AI Pairing" methodology has been particularly effective for this implementation, allowing us to address complex testing issues in a structured, incremental way.

Once the WebSocket API service implementation is complete and passing tests, we'll move to the Refactor phase to optimize the implementation while maintaining test coverage.

## Technology Choices

### Build System
- **Build Tool**: Vite
- **CSS Framework**: Tailwind CSS v4.0 with `@tailwindcss/postcss` plugin
- **Language**: TypeScript with strict type checking
- **Build Scripts**: CommonJS (.cjs) for build-time utilities

### Testing Strategy
- **Unit Testing**: Vitest (for pure logic functions)
- **Component Testing**: @web/test-runner (for browser-based component testing)
- **Testing Environment**: happy-dom (chosen for superior Web Components support and performance) - [See detailed analysis](https://claude.ai/share/07e3d8f6-0e80-4bf1-8c79-90e01f4a900a)
- **Test Helpers**: @open-wc/testing for component fixtures and assertions
- **Text Content Testing**: Use `.trim()` for consistent assertions
- **Service Mocking**: Result-based mocks replacing direct values
- **Component State Testing**: Dedicated public test helper methods
- **Test Organization**: Separated directories - `test/unit/` for unit tests, `test/components/` for component tests
- **WebSocket Testing**: [Structured testing flow](/prisoners-dilemma-docs/docs/technical/websocket-testing-flow/) with reliable mock implementations

### Frontend Technologies
- **Web Components**: Lit with Shadow DOM
- **Best Practices**: Open WC principles (adapted for Vite)
- **Styling**: Tailwind CSS with dedicated build script
- **Data Persistence**: localStorage with service abstraction
- **Error Handling**: Result pattern for type-safe error handling
- **State Management**: Lit's reactive properties with Result pattern
- **Component Communication**: Custom events with strongly typed detail
- **UI Error Handling**: Error type-based conditional rendering
- **Dark Mode**: Class-based dark mode with system preference detection
- **Real-time Communication**: WebSocket with type-safe message handling

### Dark Mode Implementation

We've implemented a simplified dark mode system with:

1. **Theme Application**
   - Media query-based dark mode (@media (prefers-color-scheme: dark)) instead of class-based
   - Smooth color transitions throughout the app
   - All components updated with dark mode classes
   - Prevents FOUC (Flash of Unstyled Content)

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

1. Complete Green Phase for WebSocket API Service
2. Move to Refactor Phase for WebSocket implementation
3. Resume Red Phase for connection UI component tests
4. Implement connection UI components
5. Integrate connection management with game app
6. Begin implementing game mechanics