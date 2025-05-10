---
layout: page
title: Event Handling
nav_order: 3
parent: Technical Documentation
permalink: /docs/technical/event-handling/
---

derived from [this chat](https://claude.ai/share/a63469f9-547a-4b01-81ba-2fac15f469f1)

# Event Handling in Lit Components

This document describes our approach to event handling in Lit components with TypeScript, ensuring type safety while maintaining the flexibility of web component event systems.

## Overview

Our application uses custom events to enable communication between components. While Lit components support standard DOM events, we often need custom events with specific data payloads. TypeScript introduces additional complexity by requiring proper type definitions for event handlers and event details.

## Core Principles

1. **Type Safety**: All custom events should have properly typed detail objects
2. **Consistency**: Use consistent patterns across all components
3. **Maintainability**: Centralize event definitions for easier updates
4. **Memory Management**: Proper cleanup of event listeners to prevent memory leaks
5. **Documentation**: Clear interfaces for event communication between components

## Event Type Architecture

### Centralized Event Definitions

We maintain all custom event types in a centralized file: `src/events/custom-events.ts`

```typescript
// src/events/custom-events.ts

// Define event detail interfaces
export interface ConnectionEventDetail {
  connectionId: string;
  connectionName: string;
}

export interface PlayerRegisterEventDetail {
  name: string;
}

// Augment the global HTMLElementEventMap
declare global {
  interface HTMLElementEventMap {
    // Connection events
    'game-requested': CustomEvent<ConnectionEventDetail>;
    'connection-created': CustomEvent<void>;
    'confirm-delete-connection': CustomEvent<ConnectionEventDetail>;
    'refresh-connections': CustomEvent<void>;
    'play-with-connection': CustomEvent<ConnectionEventDetail>;
    
    // Player events
    'register': CustomEvent<PlayerRegisterEventDetail>;
  }
}
```

### Advantages of This Approach

1. **Single Source of Truth**: All event definitions in one location
2. **Type Checking**: TypeScript validates event names and detail structures
3. **IDE Support**: Autocompletion and inline documentation
4. **Refactoring**: Easier to update event structures across the application

## Event Handler Patterns

### Bound Event Handlers

For event handlers that need to be added and removed dynamically (like in `connectedCallback` and `disconnectedCallback`), we use bound handlers:

```typescript
@customElement('my-component')
export class MyComponent extends LitElement {
  // Define handler with EventListener type
  private boundHandleGameRequested: EventListener;
  
  constructor() {
    super();
    // Bind in constructor for consistent reference
    this.boundHandleGameRequested = this.handleGameRequested.bind(this);
  }
  
  connectedCallback() {
    super.connectedCallback();
    // Add event listener with bound reference
    this.addEventListener('game-requested', this.boundHandleGameRequested);
  }
  
  disconnectedCallback() {
    super.disconnectedCallback();
    // Remove using the same bound reference
    this.removeEventListener('game-requested', this.boundHandleGameRequested);
  }
  
  // Handler implementation with type casting
  private handleGameRequested(e: Event) {
    const customEvent = e as CustomEvent<ConnectionEventDetail>;
    const { connectionId, connectionName } = customEvent.detail;
    // Handle the event...
  }
}
```

### Why This Pattern?

1. **Memory Management**: Ensures proper listener cleanup
2. **Type Compatibility**: Works with TypeScript's EventListener interface
3. **Consistent References**: Same function reference for add/remove operations

## Dispatching Custom Events

### Helper Function

We provide a typed helper function for dispatching custom events:

```typescript
// src/events/custom-events.ts
export function fireEvent<T>(
  element: HTMLElement, 
  eventName: keyof HTMLElementEventMap, 
  detail?: T,
  options: { bubbles?: boolean; composed?: boolean } = {}
): boolean {
  const { bubbles = true, composed = true } = options;
  
  return element.dispatchEvent(
    new CustomEvent(eventName, {
      detail,
      bubbles,
      composed
    })
  );
}
```

### Component Implementation

```typescript
// In components, create a convenience method
protected fireEvent<T>(eventName: keyof HTMLElementEventMap, detail?: T): boolean {
  return fireEvent(this, eventName, detail);
}

// Usage
this.fireEvent('game-requested', { connectionId, connectionName });
```

### Inline Event Handlers

For component-internal events (like click handlers), use typed inline handlers:

```typescript
render() {
  return html`
    <button @click=${(e: MouseEvent) => this.handleClick(e)}>
      Click me
    </button>
  `;
}

private handleClick(e: MouseEvent) {
  // Handle the click event
}
```

## Common Event Categories

### Connection Events

Events related to player connections:

| Event Name | Detail Type | Description |
|------------|-------------|-------------|
| `connection-created` | `void` | New connection was successfully created |
| `confirm-delete-connection` | `ConnectionEventDetail` | Request to confirm connection deletion |
| `refresh-connections` | `void` | Signal to refresh connection list |
| `play-with-connection` | `ConnectionEventDetail` | User wants to play with a connection |
| `game-requested` | `ConnectionEventDetail` | Request to start a game with connection |

### Player Events

Events related to player management:

| Event Name | Detail Type | Description |
|------------|-------------|-------------|
| `register` | `PlayerRegisterEventDetail` | New player registration |

## Implementation Guidelines

### When Creating New Events

1. **Add Type Definition**: Update `custom-events.ts` with new interfaces and event mappings
2. **Follow Naming Convention**: Use kebab-case for event names
3. **Document Purpose**: Add JSDoc comments explaining the event's purpose
4. **Consider Scope**: Use appropriate bubbling and composition settings

### Example: Adding a New Event

```typescript
// 1. Define the detail interface
export interface GameResultEventDetail {
  winnerId: string;
  score: number;
  rounds: number;
}

// 2. Add to HTMLElementEventMap
declare global {
  interface HTMLElementEventMap {
    'game-completed': CustomEvent<GameResultEventDetail>;
  }
}

// 3. Use in component
this.fireEvent('game-completed', {
  winnerId: playerId,
  score: finalScore,
  rounds: totalRounds
});
```

### Best Practices

1. **Minimize Event Payload**: Only include necessary data in event details
2. **Use Result Pattern**: For operations that might fail, use our Result pattern in handlers
3. **Handle Errors**: Gracefully handle errors in event handlers
4. **Test Events**: Write tests for both event dispatch and handling

## Testing Custom Events

### Testing Event Dispatch

```typescript
it('should dispatch game-requested event', async () => {
  const component = await fixture(html`<connection-manager></connection-manager>`);
  
  let capturedEvent: CustomEvent<ConnectionEventDetail> | null = null;
  component.addEventListener('game-requested', ((e: Event) => {
    capturedEvent = e as CustomEvent<ConnectionEventDetail>;
  }) as EventListener);
  
  // Trigger the action
  await component.handlePlayWithConnection(mockConnectionEvent);
  
  // Verify event was dispatched
  expect(capturedEvent).to.not.be.null;
  expect(capturedEvent!.detail.connectionId).to.equal('test-id');
  expect(capturedEvent!.detail.connectionName).to.equal('Test Friend');
});
```

### Testing Event Handling

```typescript
it('should handle connection-created event', async () => {
  const component = await fixture(html`<connection-manager></connection-manager>`);
  
  // Dispatch the event
  component.dispatchEvent(new CustomEvent('connection-created', {
    bubbles: true,
    composed: true
  }));
  
  await component.updateComplete;
  
  // Verify the handler's effects
  expect(component.activeTab).to.equal('connections-list');
});
```

## Migration Guide

### Updating Existing Components

When updating components to use our event handling patterns:

1. **Update Handler Types**: Change from `(e: CustomEvent) => void` to `EventListener`
2. **Add Type Casting**: Cast events to proper types in handler implementations
3. **Update Dispatch Calls**: Use the typed `fireEvent` helper
4. **Add Event Definitions**: Define events in `custom-events.ts`

### Example Migration

Before:
```typescript
private handleGameRequested(e: CustomEvent) {
  const { connectionId } = e.detail;
  // ...
}
```

After:
```typescript
private boundHandleGameRequested: EventListener;

constructor() {
  super();
  this.boundHandleGameRequested = this.handleGameRequested.bind(this);
}

private handleGameRequested(e: Event) {
  const customEvent = e as CustomEvent<ConnectionEventDetail>;
  const { connectionId, connectionName } = customEvent.detail;
  // ...
}
```

## Troubleshooting

### Common Issues

1. **TypeScript Errors on addEventListener**
   - **Cause**: Handler expects `CustomEvent` but `addEventListener` requires `EventListener`
   - **Solution**: Use `EventListener` type and cast in implementation

2. **Memory Leaks**
   - **Cause**: Event listeners not properly removed
   - **Solution**: Use bound handlers and remove in `disconnectedCallback`

3. **Event Not Bubbling**
   - **Cause**: Missing `bubbles: true` in event creation
   - **Solution**: Use helper function with proper defaults

4. **Type Errors on Event Details**
   - **Cause**: Incorrect or missing type definitions
   - **Solution**: Update `custom-events.ts` with proper interfaces

## Future Considerations

### Potential Improvements

1. **Event Bus**: Consider implementing a centralized event bus for application-wide events
2. **Event Validation**: Add runtime validation for event details in development
3. **Event Logging**: Implement optional event logging for debugging
4. **Performance Monitoring**: Track event frequency and performance impact

### Scalability

As the application grows:

1. **Namespace Events**: Consider prefixing events by feature area
2. **Event Categories**: Group related events for better organization
3. **Documentation Generation**: Auto-generate event documentation from types
4. **Event Visualization**: Create tools to visualize event flow in the application

## References

- [Lit Custom Events Documentation](https://lit.dev/docs/components/events/#custom-events)
- [TypeScript Event Handling](https://www.typescriptlang.org/docs/handbook/dom-manipulation.html#event-listeners)
- [Web Components Best Practices](https://web.dev/custom-elements-best-practices/)
- [MDN CustomEvent Documentation](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent)