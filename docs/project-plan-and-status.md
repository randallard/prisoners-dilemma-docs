---
layout: page
title: Project Plan & Status
nav_order: 2
permalink: /docs/project-plan-and-status/
---

# Prisoner's Dilemma App: Project Plan & Status

This document serves as a living record of our project plan, current status, and progress towards completing the Prisoner's Dilemma application.

## Current Status

**Phase**: Core Implementation (Player Registration Component - Green Phase Complete)  
**Last Updated**: May 2, 2025

## Project Vision

To create an interactive application that allows friends to play the Prisoner's Dilemma game, learn about game theory, and analyze strategies and outcomes.

## Development Approach

This project follows **Test Driven Development (TDD)** principles with a **Red-Green-Refactor** workflow:

1. **Red**: Write a failing test that defines the expected behavior
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

We've created a comprehensive set of [user stories](/docs/technical/user-stories) to guide our development process. Key user stories include:

- Player registration and local storage functionality
- Connection management between players
- Game initiation and statistics tracking

These user stories are prioritized to ensure we focus on the most important features first. See the full [user stories document](/docs/technical/user-stories) for details.

## Core App Functionality

### 1. Game Mechanics
- ⬜ Simple initial version showing "The Game" screen
- ⬜ Display of player names
- ⬜ Counter showing how many times each player has opened the game
- ⬜ *(Future)* Full Prisoner's Dilemma rules and scoring

### 2. Social Features
- ⬜ Connect with friends via shareable links (using friends-connect API)
- ⬜ Maintain a connections list with status indicators
- ⬜ Custom naming of connections
- ⬜ Connection request management (creation and deletion)

### 3. Data Management
- 🔄 Local storage for all game data (in progress - player registration)
- ⬜ Track game state and history with each connection
- ⬜ Store player preferences
- ⬜ Downloadable backup and restore functionality

### 4. Minimal Viable Product
- ✓ Players can register with a name (Green phase completed)
- ⬜ Players can connect via shareable links
- ⬜ Basic game screen shows player names
- ⬜ Count and display number of game opens per player
- ⬜ Local data persistence

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
- 🔄 **Player Registration Component**: Implement following TDD approach (User Stories #1-2)
  - ✓ Create failing tests for player registration (Red phase)
  - ✓ Implement minimal player registration to pass tests (Green phase)
  - ⬜ Refactor player registration component (Refactor phase)
- ⬜ Set up local storage functionality with tests (User Stories #2-4)
- ⬜ Create connection mechanism (User Stories #5-9)
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

| Date | Decision | Rationale |
|------|----------|-----------|
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

</details>

## Next Steps

1. Complete the Refactor phase for player registration component
2. Set up local storage service with appropriate tests
3. Integrate player registration with local storage
4. Begin connection mechanism implementation
5. Create basic game screen layout

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

### Frontend Technologies
- **Web Components**: Lit
- **Best Practices**: Open WC principles (adapted for Vite)
- **Styling**: Tailwind CSS with dedicated build script

### API Selection
We will use the **friends-connect** API for handling player connections. This is a Rust library specifically designed for connecting players in game applications.

**Source**: [github.com/randallard/friends-connect](https://github.com/randallard/friends-connect)

**Description**: A Rust library for connecting with friends to play games, providing the server-side functionality we need for managing connection requests and active connections.

## Open Questions

- What is our target test coverage percentage?
- How will we handle cross-browser compatibility testing?
- Should we automate accessibility testing?
- How will we handle edge cases like connection failures?