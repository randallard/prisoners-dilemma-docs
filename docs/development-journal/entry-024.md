---
layout: default
title: "Journal Entry #24"
parent: Development Journal
nav_order: [REVERSE_CHRONOLOGICAL_ORDER]
date: 2025-05-18
---

# Journal Entry #24 - IDE Agent Development Experience
{: .no_toc }

**Date:** May 18, 2025
{: .fs-5 }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## IDE Agent Development Experiment

Today I experimented with using an AI-based IDE agent for implementing the WebSocket functionality we planned on May 14th. The short version: it didn't work out as well as I'd hoped, and I'm returning to my previous workflow of using Claude chat alongside Visual Studio Code.

### What I Tried

I attempted to use an AI IDE agent to help implement the WebSocket service based on our detailed plan. I thought it might accelerate development since the agent would have direct access to the codebase. I provided both our implementation plan and project plan documents to give context.

### Issues Encountered

Several problems emerged that ultimately made this approach less efficient:

1. **File Inclusion Friction**: Adding relevant files to the context was surprisingly klunky, breaking my flow.

2. **Token Limitations**: I quickly ran out of tokens with Anthropic. Switching to the 4o model helped somewhat but still felt constraining.

3. **Naming Convention Inconsistency**: The agent ignored our established file naming conventions, which meant I spent significant time going back and fixing these inconsistencies.

4. **Conflicting Implementations**: The agent created new implementations of our Result type pattern that conflicted with existing code, creating redundancy and confusion.

5. **Context Disconnection**: Despite providing the implementation plan and project documents, the agent seemed to miss important context about our architecture and patterns.

### Time Cost Assessment

The experiment ultimately cost more time than it saved:
- Time spent providing context and uploading files
- Debugging naming inconsistencies across generated files
- Resolving conflicts between new and existing implementations
- Explaining established patterns repeatedly

### Decision: Reset and Return to Previous Workflow

After spending too much time trying to straighten out these issues, I've decided to do a hard reset and return to my previous workflow:
- Using Claude chat alongside Visual Studio Code
- Writing code myself with AI consultation rather than generation
- Following our established patterns without deviation

This approach gives me the benefits of AI assistance while maintaining full control over implementation details and consistency.

## Next Steps for WebSocket Implementation

Moving forward with our original plan, I'll:
1. Create the `WebSocketService` interface following our naming conventions
2. Implement connection state management with proper typing
3. Build the message processing system with type safety
4. Integrate with our existing Result pattern correctly

I expect this to go more smoothly now that I'm back to a workflow where I maintain architectural consistency.

## Lessons Learned

This experiment offered valuable insights about AI code generation:

1. **Context is Expensive**: The token cost of providing enough context for consistent code generation is high.

2. **Architectural Consistency Matters**: Even with planning documents, maintaining consistency across an existing codebase is challenging for AI agents.

3. **Review Cost**: The time needed to review and correct generated code can outweigh the time saved in generation.

4. **Best Use Case**: AI seems most valuable as a collaborative consultant rather than primary code generator for established projects with specific patterns.

I might reconsider this approach for more isolated features or greenfield projects, but for now, our hybrid approach of human coding with AI consultation seems most efficient.

---

**Hours Logged:** 4.5

**Tags:** #ai-development #ide-agents #workflow-optimization #lessons-learned #websocket #implementation-approach