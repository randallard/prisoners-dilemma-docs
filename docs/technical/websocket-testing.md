---
layout: page
title: WebSocket Testing
nav_order: 5
permalink: /docs/technical/websocket-testing/
---

# WebSocket Testing Flow Diagram
{: .no_toc }

This diagram illustrates the flow of WebSocket communication in our test implementation, showing how the different components interact during test execution.

## Diagram

```mermaid
graph TD
    subgraph "Test Setup"
        A[Start Test] --> B[Create ConnectionApiService]
        B --> C[Mock WebSocket Global]
        C --> D[Capture Socket Instance]
    end
    
    subgraph "Connection Flow"
        D --> E[Call service.connect]
        E --> F[Create WebSocket Instance]
        F --> G[Wait for Socket Ready State]
        G --> H[Socket onopen Triggered]
        H --> I[Connection Result Resolved]
    end
    
    subgraph "Request-Response Cycle"
        I --> J[Call Service Method\ne.g., createConnection]
        J --> K[Generate Request ID]
        K --> L[Create Promise for Response]
        L --> M[Send WebSocket Message]
        M --> N[Store Request in Map]
        N --> O[Wait for Server Response]
        O --> P[Get Response via mockServerResponse]
        P --> Q[Find Request in Map]
        Q --> R[Resolve Promise with Result]
    end
    
    subgraph "Event Handling"
        S[Register Event Callback] --> T[Server Message Received]
        T --> U[Parse Message]
        U --> V{Message Type?}
        V -->|"response"| W[Find Request in Map]
        W --> X[Resolve Promise]
        V -->|"event"| Y[Call Appropriate Callbacks]
        V -->|"error"| Z[Create ApiError]
    end
    
    subgraph "Test Verification"
        R --> AA[Await Method Promise]
        AA --> AB[Assert Result Success]
        AB --> AC[Verify Result Data]
        AC --> AD[Restore Original Methods if Needed]
    end
```

## Key Components

1. **Test Setup**: Creating the service and mocking WebSocket, capturing the actual socket instance.

2. **Connection Flow**: Establishing the WebSocket connection and handling the connection events.

3. **Request-Response Cycle**: Managing the request ID generation, promise creation, and message sending/receiving.

4. **Event Handling**: Processing different types of incoming WebSocket messages and triggering appropriate callbacks.

5. **Test Verification**: Asserting that operations completed successfully with the expected results.

## Implementation Notes

This flow diagram helped us identify key issues in our test implementation:

- Socket instance capturing was unreliable, causing spies to miss method calls
- Asynchronous timing required careful management to ensure operations completed
- Message tracking needed improvement for more reliable test verification
- Proper error handling and debugging were necessary throughout the flow

By visualizing this flow, we were able to address these issues systematically and create a more robust test implementation.