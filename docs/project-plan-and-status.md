---
layout: page
title: Project Plan & Status
nav_order: 2
permalink: /docs/project-plan-and-status/
---

# Prisoner's Dilemma App: Project Plan & Status

This document serves as a living record of our project plan, current status, and progress towards completing the Prisoner's Dilemma application.

## Current Status

**Phase**: Initial Planning → Setup & Development Environment  
**Last Updated**: April 27, 2025

## Project Vision

To create an interactive application that allows friends to play the Prisoner's Dilemma game, learn about game theory, and analyze strategies and outcomes.

## Development Approach

This project follows **Test Driven Development (TDD)** principles with a **Red-Green-Refactor** workflow:

1. **Red**: Write a failing test that defines the expected behavior
2. **Green**: Write the minimal code needed to make the test pass
3. **Refactor**: Improve the code quality while maintaining passing tests

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
- ⬜ Local storage for all game data
- ⬜ Track game state and history with each connection
- ⬜ Store player preferences
- ⬜ Downloadable backup and restore functionality

### 4. Minimal Viable Product
- ⬜ Players can connect via shareable links
- ⬜ Basic game screen shows player names
- ⬜ Count and display number of game opens per player
- ⬜ Local data persistence

## Development Roadmap

### Phase 1: Planning & Setup ✅
- ✅ Create project documentation structure
- ✅ Define user stories and requirements
- ✅ Define architecture and technology stack
- ⬜ Create initial wireframes/mockups
- ⬜ Set up development environment with TDD tooling

### Phase 2: Core Implementation
- ⬜ **Test suite setup**: Configure testing framework and write first tests
- ⬜ Implement player registration (User Stories #1-2)
- ⬜ Set up local storage functionality (User Stories #2-4)
- ⬜ Create connection mechanism (User Stories #5-9)
- ⬜ Implement basic game mechanics (User Stories #11-12)

### Phase 3: Testing & Refinement
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

| Date | Decision | Rationale |
|------|----------|-----------|
| April 27, 2025 | Adopted Test Driven Development (TDD) with Red-Green-Refactor | To ensure quality code, prevent regressions, and provide clear development path |
| April 26, 2025 | Selected Lit + Open WC + Tailwind CSS | For optimal web component development with modern styling approach |
| April 26, 2025 | Created dedicated project plan page | To maintain a central record of project status and plans |
| April 26, 2025 | Defined initial user stories | To establish clear requirements and development priorities |
| April 26, 2025 | Selected friends-connect API | To leverage an existing solution for player connections rather than building from scratch |

## Next Steps

1. Set up project scaffold using Open WC
2. Configure testing environment with Jest and Web Test Runner
3. Write first failing tests for player registration component
4. Implement minimal code to pass tests
5. Refactor and iterate

## Technology Choices

### Development Approach
We will follow **Test Driven Development** using the testing tools provided by Open WC:
- Web Test Runner for in-browser component testing
- Jest for unit testing
- Storybook for component development and visual testing

### Frontend Technologies
- **Web Components**: Lit
- **Best Practices**: Open WC
- **Styling**: Tailwind CSS

### API Selection
We will use the **friends-connect** API for handling player connections. This is a Rust library specifically designed for connecting players in game applications.

**Source**: [github.com/randallard/friends-connect](https://github.com/randallard/friends-connect)

**Description**: A Rust library for connecting with friends to play games, providing the server-side functionality we need for managing connection requests and active connections.

## Open Questions

- How will we measure and maintain test coverage?
- Should we prioritize web or mobile development?
- How will we handle offline gameplay?
- What specific features from the friends-connect API will we leverage?