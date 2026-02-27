# GitHub Copilot Custom Agents - OpenClaw Quick Reference

This repository includes **13 specialized AI agents** optimized for OpenClaw development workflows with **8 MCP servers** for enhanced capabilities.

## 🤖 Available Custom Agents

| Agent | Purpose | When to Use |
|-------|---------|-------------|
| **jules** ⭐ | Code quality, collaboration, refactoring | Code review, quality improvement, agent coordination |
| **rapid-implementer** | Fast, autonomous code implementation | Speed-focused feature development, end-to-end implementation |
| **architect** | System architecture and design | Designing channel integrations, routing systems, architectural decisions |
| **debug-detective** | Advanced debugging and root cause analysis | Investigating bugs (channel auth, routing errors, plugin issues) |
| **deep-research** | Comprehensive research and analysis | Repository analysis, messaging API research, problem investigation |
| **repo-optimizer** | Repository setup, tooling, improvements | Setting up configs, improving tools, enhancing DX |
| **testing-stability-expert** | Comprehensive testing, stability validation | Writing tests, ensuring reliability, quality assurance |
| **docs-master** | Documentation creation and verification | Writing docs, verifying accuracy, updating guides |
| **code-reviewer** | Code quality and security review | Reviewing PRs, checking security, ensuring best practices |
| **performance-optimizer** | Performance tuning and optimization | Fixing slow routing, message processing bottlenecks |
| **full-stack-developer** | Complete feature development | Building full features (CLI + channel + app) |
| **devops-infrastructure** | Docker, CI/CD, deployment | Infrastructure setup, deployment, DevOps tasks |
| **api-developer** | API/channel design and implementation | Creating channel integrations, protocol implementations |

## ⚡ Quick Start Examples

```bash
# In GitHub Copilot Chat (VS Code, GitHub.com, etc.)

# Fast channel feature implementation
@agent:rapid-implementer Implement Slack thread reply support in the Slack channel handler

# System design for routing
@agent:architect Design a scalable message routing engine with priority queues

# Debug a channel issue
@agent:debug-detective Investigate why Discord channel drops messages under high load

# Deep research on messaging protocols
@agent:deep-research Analyze Matrix protocol spec and recommend implementation approach

# Testing the routing service
@agent:testing-stability-expert Create comprehensive tests for MessageRouter

# Documentation
@agent:docs-master Update channel docs for the new Matrix extension

# Code review for security
@agent:code-reviewer Review extensions/msteams/src/channel.ts for security issues

# Performance optimization
@agent:performance-optimizer Identify message processing bottlenecks in the routing layer

# Full-stack feature development
@agent:full-stack-developer Build end-to-end group message broadcast feature

# DevOps
@agent:devops-infrastructure Setup GitHub Actions to test all extension packages on PR

# API/Channel development
@agent:api-developer Design and implement REST API for webhook-based channel messages
```

## 🔧 Agent + MCP Server Combinations

### Rapid Implementer
```
📁 filesystem - Batch read/write operations
🔧 git - Version control
🐙 github - Search patterns
🧠 memory - Remember implementation patterns
🧮 sequential-thinking - Plan complex features
```

### Debug Detective
```
📁 filesystem - Read code and logs
🔧 git - Analyze code history
🐙 github - Find similar issues
🧠 memory - Remember bug patterns
🧮 sequential-thinking - Systematic debugging
```

### Performance Optimizer
```
📁 filesystem - Read/write optimized code
🐙 github - Search optimization patterns
🧠 memory - Track performance metrics
🧮 sequential-thinking - Plan complex optimizations
```

## 🤝 Agent Collaboration Mode

```bash
# Complex task: Build a new messaging channel extension
# Step 1: Research
@agent:deep-research Research the target messaging protocol/API capabilities and limitations

# Step 2: Architecture
@agent:architect Design the channel extension structure following the plugin-sdk interface

# Step 3: Implementation
@agent:rapid-implementer Implement the channel extension based on architect's design

# Step 4: Testing
@agent:testing-stability-expert Create comprehensive tests for the new channel

# Step 5: Security review
@agent:code-reviewer Review channel implementation for security vulnerabilities

# Step 6: Documentation
@agent:docs-master Document the new channel, its setup, and usage guide
```

## 🌐 Environment Variables for MCP

```bash
# Required for GitHub integration
export GITHUB_TOKEN="ghp_your_token_here"

# Optional MCP servers (see .github/copilot/OPTIONAL_SERVERS.md)
export COPILOT_MCP_BRAVE_API_KEY="your_brave_api_key"
```

## 📁 Agent Documentation

- [`rapid-implementer.agent.md`](.github/agents/rapid-implementer.agent.md) — Fast implementation patterns for OpenClaw
- [`architect.agent.md`](.github/agents/architect.agent.md) — Architecture design and decisions
- [`debug-detective.agent.md`](.github/agents/debug-detective.agent.md) — Debugging channel/routing/plugin issues
- [`deep-research.agent.md`](.github/agents/deep-research.agent.md) — Research and analysis
- [`testing-stability-expert.agent.md`](.github/agents/testing-stability-expert.agent.md) — Testing patterns
- [`docs-master.agent.md`](.github/agents/docs-master.agent.md) — Documentation best practices
- [`code-reviewer.agent.md`](.github/agents/code-reviewer.agent.md) — Code review standards
- [`performance-optimizer.agent.md`](.github/agents/performance-optimizer.agent.md) — Performance patterns
- [`full-stack-developer.agent.md`](.github/agents/full-stack-developer.agent.md) — Full-stack development
- [`devops-infrastructure.agent.md`](.github/agents/devops-infrastructure.agent.md) — DevOps and infrastructure
- [`api-developer.agent.md`](.github/agents/api-developer.agent.md) — Channel API design and implementation
- [`repo-optimizer.agent.md`](.github/agents/repo-optimizer.agent.md) — Repository optimization
- [`jules.agent.md`](.github/agents/jules.agent.md) — Collaborative coding and review

### For MCP Servers
- [`.github/copilot/mcp.json`](.github/copilot/mcp.json) — GitHub Copilot MCP config
- [`.github/copilot/OPTIONAL_SERVERS.md`](.github/copilot/OPTIONAL_SERVERS.md) — Optional MCP servers guide

### Workflow Guides
- [`AGENT_ORCHESTRATION.md`](.github/agents/AGENT_ORCHESTRATION.md) — Multi-agent coordination
- [`CODING_WORKFLOW.md`](.github/agents/CODING_WORKFLOW.md) — Optimized development workflows

## 💡 Tips for Best Results

1. **Be Specific**: Clear instructions get better results
   - ❌ "Fix the code"
   - ✅ "Fix the Telegram channel dropping outbound messages when token expires"

2. **Use Right Agent**: Match task to agent expertise
   - Channel/API issues → `api-developer` or `debug-detective`
   - Performance → `performance-optimizer`
   - New features → `rapid-implementer` or `full-stack-developer`
   - Deployment → `devops-infrastructure`

3. **Leverage MCP**: Agents use MCP servers automatically
   - No need to manually specify tools
   - They choose optimal tools for the task

4. **Iterate**: Agents can work incrementally
   - Start with one agent, chain to others

5. **Verify**: Always review agent outputs
   - Agents are powerful but not perfect

---

**Version**: 1.0.0
**Last Updated**: 2026-02-27
**Repository**: OpenClaw
