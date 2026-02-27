# Optimized Coding Workflow for OpenClaw

This guide provides optimized development workflows leveraging custom agents and MCP servers for maximum productivity and code quality.

## Quick Reference: Development Scenarios

### 🔌 New Channel/Extension
```
Time: 30-60 minutes | Quality: Production-ready
Agents: architect → api-developer → rapid-implementer → testing-stability-expert → code-reviewer → docs-master
```

### 🐛 Bug Fix (Routing/Channel)
```
Time: 15-30 minutes | Quality: Regression-proof
Agents: debug-detective → rapid-implementer → testing-stability-expert
```

### ⚡ Performance Optimization
```
Time: 20-40 minutes | Quality: Benchmarked improvement
Agents: performance-optimizer → rapid-implementer → testing-stability-expert
```

### 📚 Documentation Update
```
Time: 10-20 minutes | Quality: Verified accuracy
Agents: docs-master → code-reviewer
```

### 🔒 Security Audit
```
Time: 30-45 minutes | Quality: Production-secure
Agents: code-reviewer → testing-stability-expert → docs-master
```

## Workflow 1: New Channel Extension (Full Cycle)

### Stage 1: Architecture (5-10 min)
```bash
@agent:architect "Analyze the plugin-sdk interface and design an extension for [ChannelName]. Include:
- Extension directory structure (extensions/<name>/)
- Channel interface implementation plan
- Message type mapping (platform types → OpenClaw types)
- Auth/credential handling pattern
- Config schema design
- Error handling and reconnection strategy"
```

### Stage 2: API Design (5 min)
```bash
@agent:api-developer "Design the Channel interface implementation for [ChannelName]:
- connect() / disconnect() lifecycle
- send(message: Message) method
- receive() / event stream
- status() health check
- Config schema with required/optional fields"
```

### Stage 3: Implementation (20-30 min)
```bash
@agent:rapid-implementer "Implement the [ChannelName] channel extension:
- extensions/<name>/src/channel.ts (Channel interface)
- extensions/<name>/src/index.ts (plugin entry)
- extensions/<name>/package.json (correct deps in dependencies, not devDependencies)
Include reconnection logic, error handling, TypeScript strict types"
```

### Stage 4: Testing (10-15 min)
```bash
@agent:testing-stability-expert "Create comprehensive tests for [ChannelName] extension:
- extensions/<name>/src/channel.test.ts
Cover: connect/disconnect lifecycle, send/receive, error handling, reconnection, config validation"
```

### Stage 5: Security Review (5-10 min)
```bash
@agent:code-reviewer "Security review of [ChannelName] extension:
- Check credential/token handling (never log secrets)
- Validate all external input
- Verify no prototype mutation
- Check no secrets committed"
```

### Stage 6: Documentation (5-10 min)
```bash
@agent:docs-master "Document [ChannelName] extension:
- Add docs/channels/<name>.md
- Add to docs/channels/ index
- Include setup guide, config reference, troubleshooting"
```

### Total Time: ~60-90 minutes for production-ready extension

## Workflow 2: Routing/Channel Bug Fix

### Step 1: Investigation (10 min)
```bash
@agent:debug-detective "Investigate [bug description]:
- Analyze src/routing/ for related logic
- Check src/channels/ base types
- Look for race conditions or edge cases
- Review recent commits for regressions"
```

### Step 2: Fix (5-10 min)
```bash
@agent:rapid-implementer "Fix [issue] based on debug-detective's analysis:
[paste debug-detective findings]"
```

### Step 3: Prevention (5 min)
```bash
@agent:testing-stability-expert "Add regression tests to prevent [issue] from recurring"
```

## Workflow 3: Performance Optimization Sprint

### Phase 1: Profiling (10 min)
```bash
@agent:performance-optimizer "Profile [component] performance:
- Identify hot paths in message routing
- Find missing parallelism (sequential where parallel is safe)
- Check for unnecessary serialization/deserialization
- Measure throughput bottlenecks"
```

### Phase 2: Implementation (15-20 min)
```bash
@agent:rapid-implementer "Implement performance fixes:
[paste performance-optimizer findings]"
```

### Phase 3: Benchmarking (5 min)
```bash
@agent:testing-stability-expert "Benchmark performance improvements:
- Measure before/after throughput
- Confirm no correctness regression"
```

## Commands Quick Reference

```bash
# Install dependencies
pnpm install

# Type-check (run first)
pnpm tsgo

# Lint + format check
pnpm check

# Format fix
pnpm format:fix

# Run tests
pnpm test

# Run tests with coverage
pnpm test:coverage

# Build
pnpm build

# Dev mode
pnpm dev
```

## Measuring Success

### Code Quality Metrics
- **Security**: Zero critical vulnerabilities
- **Testing**: ≥70% coverage (lines/branches/functions/statements)
- **TypeScript**: Zero type errors (`pnpm tsgo`)
- **Lint**: Zero lint errors (`pnpm check`)
- **Documentation**: All channels documented

### Expected Time Savings

| Task | Manual | With Agents | Savings |
|------|--------|-------------|---------|
| New channel extension | 180 min | 60-90 min | 50-67% |
| Bug fix | 45 min | 20-30 min | 33-56% |
| Code review | 30 min | 10-15 min | 50-67% |
| Documentation | 60 min | 25-30 min | 50-58% |
| Security audit | 90 min | 50-60 min | 33-44% |

## Best Practices Summary

1. **Start with Architecture**: Use architect to analyze and plan before coding
2. **Test Early**: Don't wait until the end to add tests
3. **Review Security**: Always run security review before merge
4. **Document as You Go**: Update docs alongside code
5. **Type-check First**: Run `pnpm tsgo` before lint/test
6. **Iterate**: Refine agent requests based on results
7. **Check All Channels**: When modifying shared logic, verify all channels still work

---

**Pro Tip**: For channel-specific tasks, always check the existing channel implementations (e.g., `src/telegram/`, `src/discord/`) for patterns before implementing a new one. Consistency with existing patterns is critical.

**Related**: [Agent README](.github/agents/README.md) | [Agent Orchestration](.github/agents/AGENT_ORCHESTRATION.md)
