# Claude Marketplace - .NET Development Tools

A Claude Code marketplace providing plugins for .NET development, with a focus on improving test coverage through mutation testing with Stryker.NET.

## What's Included

This marketplace contains the **stryker-dotnet** plugin, which provides:

- **Skills**: Automatic assistance with Stryker.NET mutation testing and test coverage analysis
- **Slash Commands**: Quick access to test coverage improvement workflows
- **Sub-agents**: Specialized agents for analyzing coverage gaps and planning test improvements

## Installation

### Prerequisites

- [Claude Code](https://claude.ai/code) installed and configured
- .NET SDK installed (for running Stryker.NET)
- `jq` installed (for analyzing mutation reports)

### Add the Marketplace

Add this marketplace to Claude Code using the GitHub repository:

```bash
/plugin marketplace add kbjorklid/claude-marketplace-dotnet
```

Or using the full repository URL:

```bash
/plugin marketplace add https://github.com/kbjorklid/claude-marketplace-dotnet
```

### Install the Plugin

Once the marketplace is added, install the stryker-dotnet plugin:

```bash
/plugin install stryker-dotnet@claude-dotnet-marketpalace
```

Verify the installation:

```bash
/plugin list
```

## Usage

### Skills

The **stryker-dotnet** skill is automatically invoked by Claude when you:

- Mention "mutation testing" or "Stryker.NET"
- Ask about "test coverage" or "coverage gaps"
- Reference terms like "NoCoverage" or "survived mutations"
- Request help improving test quality

**Example interactions:**

```
How can I improve test coverage in my project?
```

```
Can you help me analyze my Stryker.NET report?
```

```
What mutations survived in my latest test run?
```

Claude will automatically use the stryker-dotnet skill to provide expert guidance, including:
- How to install and run Stryker.NET
- How to generate and analyze mutation reports
- Practical recipes for finding coverage gaps
- Recommendations for improving test quality

### Slash Commands

#### `/improve-coverage`

Analyzes your project and provides a comprehensive plan for improving test coverage.

**Usage:**

```
/improve-coverage Analyze the PaymentProcessor class and improve its test coverage
```

**What it does:**

1. Confirms the scope of analysis with you
2. Launches a specialized test coverage improvement agent
3. Runs Stryker.NET mutation testing
4. Analyzes the results and generates recommendations
5. Helps you implement the recommended test cases
6. Verifies all tests pass

### Sub-agents

#### test-coverage-improvement-planner

A specialized agent that automatically runs when using `/improve-coverage`. It:

- Runs Stryker.NET mutation testing
- Locates and analyzes mutation reports
- Identifies coverage gaps and weak tests
- Generates a prioritized list of test recommendations
- Provides specific test plans for each recommendation

The agent prioritizes recommendations by impact:
1. Happy path logic
2. Exceptional path logic
3. Error cases and exceptions
4. Logging and observability

## Typical Workflow

1. **Add and install the marketplace** (one-time setup)
   ```
   /plugin marketplace add kbjorklid/claude-marketplace-dotnet
   /plugin install stryker-dotnet@claude-dotnet-marketpalace
   ```

2. **Ask Claude to analyze your test coverage**
   ```
   Can you help me improve test coverage for my PaymentProcessor class?
   ```

3. **Use the improve-coverage command for comprehensive analysis**
   ```
   /improve-coverage Focus on the core business logic in the Services folder
   ```

4. **Claude will automatically:**
   - Install Stryker.NET if needed
   - Run mutation testing
   - Analyze results using jq
   - Provide specific recommendations
   - Help implement new tests
   - Verify everything works

## Stryker.NET Integration

This plugin includes comprehensive documentation for Stryker.NET, covering:

- Installation and configuration
- Running mutation tests
- Analyzing JSON reports with jq
- Understanding mutation types
- CI/CD pipeline integration
- F# support

See the [stryker-dotnet skill documentation](plugins/stryker-dotnet/skills/stryker-dotnet/SKILL.md) for detailed recipes and examples.

## Team Configuration

Organizations can automate marketplace setup by adding this to `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "dotnet-tools": {
      "source": {
        "source": "github",
        "repo": "kbjorklid/claude-marketplace-dotnet"
      }
    }
  },
  "enabledPlugins": {
    "stryker-dotnet@claude-dotnet-marketpalace": {}
  }
}
```

When team members trust the repository, Claude Code will automatically install the marketplace and plugins.

## Requirements

- Claude Code
- .NET SDK (any supported version)
- `jq` (for JSON report analysis)
- Git (for repository access)

## Contributing

Contributions are welcome! This marketplace is designed to help .NET developers improve test quality through mutation testing.

## Licensing

This project uses a mixed licensing approach:

**Main Project**: MIT License - See [LICENSE](LICENSE)

**Third-Party Content**: The Stryker.NET documentation included in `plugins/stryker-dotnet/skills/stryker-dotnet/official-documentation/` is licensed under Apache License 2.0 - See [LICENSE-APACHE](LICENSE-APACHE) and [NOTICE](NOTICE) for details.

## Support

- **Documentation**: https://code.claude.com/docs
- **Issues**: https://github.com/kbjorklid/claude-marketplace-dotnet/issues
- **Stryker.NET**: https://stryker-mutator.io/docs/stryker-net/introduction/