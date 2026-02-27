# Agent Orchestration & Multi-Agent Workflows

This guide explains how to coordinate multiple AI agents with MCP servers for complex OpenClaw development tasks.

## Overview

The OpenClaw repository provides:
- **13 specialized agents** with distinct expertise
- **8 MCP servers** providing enhanced capabilities
- **Seamless integration** between agents and MCP servers
- **Optimized workflows** for messaging gateway development

## Agent Orchestration Principles

### 1. Task Decomposition

```
Complex Task: "Build a new voice-to-text channel extension"

Decomposition:
├── @agent:deep-research
│   └── Research voice-to-text APIs and WebRTC integration patterns
├── @agent:architect
│   └── Design extension structure: plugin-sdk interface, audio pipeline, routing hooks
├── @agent:api-developer
│   └── Design Channel interface: connect(), send(), receive(), disconnect()
├── @agent:rapid-implementer
│   └── Implement extension, audio pipeline, routing integration, channel config
├── @agent:testing-stability-expert
│   └── Create tests (unit + integration + mock channel tests)
├── @agent:code-reviewer
│   └── Security review: credential handling, audio data privacy, input validation
├── @agent:performance-optimizer
│   └── Optimize: audio buffering, stream processing, reconnection logic
└── @agent:docs-master
    └── Document new extension: setup guide, config reference, usage examples
```

### 2. Sequential vs Parallel Execution

**Sequential** (tasks depend on each other):
```
Step 1: @agent:architect - Design channel architecture
  ↓ (wait for completion)
Step 2: @agent:rapid-implementer - Implement the design
  ↓ (wait for completion)
Step 3: @agent:testing-stability-expert - Create tests
  ↓ (wait for completion)
Step 4: @agent:docs-master - Document the feature
```

**Parallel** (independent tasks):
```
@agent:docs-master - Update channel documentation
+
@agent:performance-optimizer - Profile message routing throughput
+
@agent:testing-stability-expert - Add integration tests for existing channels
(All can run simultaneously)
```

## OpenClaw Workflow Patterns

### Pattern 1: New Channel Extension Development

```bash
# Phase 1: Design
@agent:architect "Design a Matrix protocol channel extension following the plugin-sdk interface. Include connection lifecycle, message mapping, room→channel routing, and auth token handling."

# Phase 2: API Design
@agent:api-developer "Design the Channel interface implementation for Matrix: connect, send, receive, status, and config schema."

# Phase 3: Implementation
@agent:rapid-implementer "Implement the Matrix channel extension in extensions/matrix/ with full plugin-sdk compliance, error handling, and reconnection logic."

# Phase 4: Testing
@agent:testing-stability-expert "Create comprehensive tests for the Matrix channel extension, including mock Matrix homeserver responses and reconnection scenarios."

# Phase 5: Review
@agent:code-reviewer "Security review of Matrix channel extension — check credential storage, room access controls, and message data handling."

# Phase 6: Docs
@agent:docs-master "Document the Matrix channel extension: setup guide, config reference, and troubleshooting."
```

### Pattern 2: Routing Bug Fix

```bash
# Step 1: Investigation
@agent:debug-detective "Investigate messages being dropped in the routing layer when two channels send simultaneously — analyze src/routing/ for race conditions."

# Step 2: Fix
@agent:rapid-implementer "Fix the routing race condition based on debug-detective's findings."

# Step 3: Regression Prevention
@agent:testing-stability-expert "Add tests to prevent routing race conditions from recurring."
```

### Pattern 3: Performance Sprint

```bash
# Step 1: Profile
@agent:performance-optimizer "Profile the message routing pipeline — identify slow paths, missing concurrency, and unnecessary serialization."

# Step 2: Implement
@agent:rapid-implementer "Implement performance fixes based on profiling results."

# Step 3: Verify
@agent:testing-stability-expert "Verify performance optimizations maintain correctness."
```

### Pattern 4: Repository Health Check

```bash
# Weekly health check workflow
@agent:repo-optimizer "Audit dependencies for outdated packages and security vulnerabilities"
@agent:testing-stability-expert "Run full test suite and report coverage gaps"
@agent:code-reviewer "Scan for new security issues in recent commits"
@agent:performance-optimizer "Profile critical routing paths for regressions"
@agent:docs-master "Verify docs match current implementation"
```

## Troubleshooting

### Agent Not Meeting Expectations
1. Be more specific in the prompt
2. Break task into smaller subtasks
3. Try different agent better suited to task

### Multiple Agents Conflicting
1. Use sequential workflow for dependent tasks
2. Review each agent's output before next step
3. Use git branches to isolate agent work

---

**Related Documentation**:
- [Agent README](.github/agents/README.md)
- [MCP Config](.github/copilot/mcp.json)
- [Coding Workflow](.github/agents/CODING_WORKFLOW.md)
