---
name: stryker-dotnet
description: Analyze Stryker.NET mutation testing reports to identify code coverage gaps and areas needing additional tests. Focus on interpreting JSON reports to improve test effectiveness.
allowed-tools: [Read, Grep, Glob]
---

# Stryker.NET Coverage Analysis

This skill helps you analyze Stryker.NET mutation testing reports to identify gaps in test coverage and areas where additional tests are needed. Use this skill when you have mutation test results and want to improve your test suite's effectiveness.

## Primary Use Case

The primary use case is to run the Stryker.net mutation testing tool, and then analyze the results for gaps in test coverage.

## Runnig Stryker.NET to Generate Mutation Report
```bash
# Install Stryker.NET if not already installed
dotnet tool install -g dotnet-stryker

# Recommended: Run with multiple reporters for comprehensive analysis
dotnet stryker --reporter "json" --reporter "progress"
```

## Locate the Generated Report
```bash
# Find the most recent mutation reportp
find . -name "mutation-report.json" -type f | head -5

# Typical location: StrykerOutput/{timestamp}/reports/mutation-report.json
```

## Analyze the report

Use the comprehensive **[JSON Reporter Guide](JSON_REPORTER_GUIDE.md)** for detailed schema information and interpretation guidelines.

### Report Generation Configuration example
```json
{
  "stryker-config": {
    "reporters": ["json", "progress", "html"],
    "report-file-name": "my-mutation-report.json",
    "output": "./MutationReports",
    "thresholds": {
      "high": 80,
      "low": 60,
      "break": 0
    }
  }
}

## Official Documentation Table of Contents

### Getting Started
- [Introduction](official-documentation/docs/introduction.md) - Overview of Stryker.NET and mutation testing
- [Getting Started](official-documentation/docs/getting-started.md) - Quick start guide and first steps
- [Configuration](official-documentation/docs/configuration.md) - Comprehensive configuration options
- [Updating](official-documentation/docs/updating.md) - How to update Stryker.NET

### Core Features
- [Mutations](official-documentation/docs/mutations.md) - Understanding mutation types
- [Operating Modes](official-documentation/docs/operating-modes.md) - Different testing modes available 
- [Reporters](official-documentation/docs/reporters.md) - Output report formats and options

### Advanced Features
- [Ignore Mutations](official-documentation/docs/ignore-mutations.md) - Excluding specific mutations
- [Regex Mutations](official-documentation/docs/regex-mutations.md) - Regular expression mutation patterns
- [Stryker in Pipeline](official-documentation/docs/stryker-in-pipeline.md) - CI/CD integration

### Technical Reference
- [Technical Reference Introduction](official-documentation/docs/technical-reference/introduction.md) - Technical overview
- [Project Components](official-documentation/docs/project-components.md) - Architecture and components
- [Mutation Schemata](official-documentation/docs/mutant-schemata.md) - Mutation schema definitions
- [Testing Framework](official-documentation/docs/testing-framework.md) - Testing framework integration
- [Research](official-documentation/docs/research.md) - Research and findings

#### Technical Reference - Advanced
- [Mutation Orchestration Design](official-documentation/docs/technical-reference/Mutation%20Orchestration%20Design.md) - Mutation orchestration architecture
- [VsTest Internal](official-documentation/docs/technical-reference/VsTest%20internal.md) - VsTest integration details

### F# Support
- [F# Current State](official-documentation/docs/technical-reference/fsharp/current-state.md) - F# language support status
- [F# Classes](official-documentation/docs/technical-reference/fsharp/classes.md) - F# class mutations
- [F# Packages](official-documentation/docs/technical-reference/fsharp/packages.md) - F# package handling

### Migration and Guides
- [Migration Guide](official-documentation/docs/migration-guide.md) - Migrating between versions

### Additional Resources
- [Diagrams](official-documentation/docs/diagrams) - Visual diagrams and documentation
- [Images](official-documentation/docs/images) - Reference images and screenshots
