# Stryker.NET JSON Reporter Guide for LLM Agents

This guide provides LLM agents with comprehensive information on using and interpreting Stryker.NET's JSON reporter output for automated mutation testing analysis.

## Overview

The JSON reporter is the primary machine-readable output format for Stryker.NET mutation testing. It provides structured data about mutants, their status, location, and test coverage - ideal for programmatic analysis by LLM agents.

## Configuration

### Enable JSON Reporter

```bash
# Command line
dotnet stryker --reporter "json"

# Multiple reporters
dotnet stryker --reporter "json" --reporter "progress"

# Configuration file
{
  "stryker-config": {
    "reporters": ["json", "progress"]
  }
}
```

### Output Location

- **Default**: `StrykerOutput/{date-time}/reports/mutation-report.json`
- **Custom filename**: Use `report-file-name` configuration option
- **Custom output directory**: Use `output` configuration option

## JSON Schema Structure

### Root Object Structure

```json
{
  "schemaVersion": "2",
  "projectRoot": "/path/to/project",
  "thresholds": {
    "high": 80,
    "low": 60
  },
  "files": {
    "path/to/file.cs": { /* SourceFile object */ }
  },
  "testFiles": {
    "path/to/test.cs": { /* TestFile object */ }
  }
}
```

### Core Properties

| Property | Type | Description |
|----------|------|-------------|
| `schemaVersion` | string | JSON schema version (currently "2") |
| `projectRoot` | string | Absolute path to project root directory |
| `thresholds` | object | Mutation score thresholds configured |
| `files` | object | Dictionary of source files with mutations |
| `testFiles` | object | Dictionary of test files (optional, may be null) |

## SourceFile Object

Each mutated source file contains:

```json
{
  "language": "cs",
  "source": "complete source code content",
  "mutants": [
    { /* Mutant object */ }
  ]
}
```

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `language` | string | Programming language ("cs" for C#) |
| `source` | string | Complete source code of the file |
| `mutants` | array | Array of mutants generated for this file |

## Mutant Object

Each mutant provides comprehensive mutation information:

```json
{
  "id": "1",
  "mutatorName": "BinaryOperator",
  "description": "- was replaced with +",
  "replacement": "-",
  "location": {
    "start": { "line": 10, "column": 15 },
    "end": { "line": 10, "column": 16 }
  },
  "status": "Killed",
  "statusReason": "Test method TestAddition failed",
  "static": false,
  "coveredBy": ["TestMethod1", "TestMethod2"],
  "killedBy": ["TestMethod1"],
  "testsCompleted": 5,
  "duration": 150
}
```

### Mutant Properties

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier for the mutant |
| `mutatorName` | string | Type of mutation applied |
| `description` | string | Human-readable description of mutation |
| `replacement` | string | The replacement code that was inserted |
| `location` | object | Position information in source code |
| `status` | string | Result status of the mutant |
| `statusReason` | string | Detailed reason for the status |
| `static` | boolean? | Whether mutant affects static values |
| `coveredBy` | array | List of tests that cover this mutant |
| `killedBy` | array | List of tests that killed this mutant |
| `testsCompleted` | number? | Number of tests that ran against this mutant |
| `duration` | number? | Execution time in milliseconds |

## Location Object

```json
{
  "start": { "line": 10, "column": 15 },
  "end": { "line": 10, "column": 16 }
}
```

### Position Properties

| Property | Type | Description |
|----------|------|-------------|
| `line` | number | Line number (1-based indexing) |
| `column` | number | Column number (1-based indexing) |

## Mutant Status Values

| Status | Description | Action Required |
|--------|-------------|-----------------|
| `Killed` | Mutant was detected and killed by tests | ✅ Good - tests caught the mutation |
| `Survived` | Mutant survived all tests | ⚠️ Concerning - needs investigation |
| `NoCoverage` | No tests cover the mutated code | ⚠️ Add tests to cover this code |
| `Timeout` | Tests timed out when running mutant | ⚠️ May indicate performance issues |
| `CompileError` | Mutation caused compilation errors | ⚠️ Review mutation logic |
| `Ignored` | Mutant was explicitly ignored | ℹ️ Intentionally excluded |
| `Pending` | Mutant is waiting to be tested | ℹ️ In-progress status |

## Common Mutator Names

| Mutator Name | Description |
|--------------|-------------|
| `BinaryOperator` | Changes binary operators (+, -, *, /, %, etc.) |
| `EqualityOperator` | Changes equality operators (==, !=, ===, !==) |
| `LogicalOperator` | Changes logical operators (&&, ||, !) |
| `AssignmentStatement` | Changes assignment operators (=, +=, -=, etc.) |
| `StringLiteral` | Changes string values and concatenations |
| `BooleanLiteral` | Changes boolean values (true/false) |
| `UnaryOperator` | Changes unary operators (+, -, ++, --, !) |
| `ArithmeticOperator` | Changes arithmetic operations |
| `BitwiseOperator` | Changes bitwise operations (&, |, ^, ~, <<, >>) |
| `LinqMethod` | Changes LINQ method calls |
| `MathMethod` | Changes mathematical method calls |
| `StringMethod` | Changes string method calls |

## Agent Interpretation Guidelines

### Analyzing Mutation Score

```javascript
// Calculate mutation score
function calculateMutationScore(files) {
  let totalMutants = 0;
  let killedMutants = 0;

  Object.values(files).forEach(file => {
    file.mutants.forEach(mutant => {
      totalMutants++;
      if (mutant.status === 'Killed') {
        killedMutants++;
      }
    });
  });

  return totalMutants > 0 ? (killedMutants / totalMutants) * 100 : 0;
}
```

### Priority Areas for Agent Action

1. **NoCoverage Mutants**: Highest priority - code is completely untested and could contain undetectable bugs
2. **Survived Mutants**: Medium priority - tests reach the code but aren't comprehensive enough
3. **Timeout Mutants**: Investigate potential infinite loops or performance issues
4. **CompileError Mutants**: Review if mutations are breaking code structure

### Code Quality Assessment

- **Good mutation score (>80%)**: Strong test coverage
- **Acceptable score (60-80%)**: Adequate coverage, room for improvement
- **Poor score (<60%)**: Significant gaps in test coverage

### Location-Based Analysis

Use the `location` object to:
- Navigate to exact positions in source code
- Identify patterns in surviving mutants
- Suggest targeted test improvements

### Test Coverage Analysis

Examine `coveredBy` and `killedBy` arrays to:
- Identify which tests are most effective at mutation killing
- Find redundant or ineffective tests
- Suggest test refactoring opportunities

## Integration Examples

### Example Agent Analysis Workflow

1. **Parse JSON report**: Load and validate the JSON structure
2. **Calculate metrics**: Compute mutation score, coverage percentages
3. **Identify issues**: Find survived, no-coverage, and timeout mutants
4. **Prioritize actions**: Rank mutants by severity and impact
5. **Generate suggestions**: Recommend specific test additions or modifications
6. **Track trends**: Compare with previous reports if available

### Sample Analysis Code

```javascript
function analyzeMutationReport(jsonReport) {
  const analysis = {
    totalMutants: 0,
    killedMutants: 0,
    survivedMutants: 0,
    noCoverageMutants: 0,
    timeoutMutants: 0,
    filesWithIssues: [],
    suggestedActions: []
  };

  Object.entries(jsonReport.files).forEach(([filePath, file]) => {
    file.mutants.forEach(mutant => {
      analysis.totalMutants++;

      switch(mutant.status) {
        case 'Killed':
          analysis.killedMutants++;
          break;
        case 'Survived':
          analysis.survivedMutants++;
          analysis.suggestedActions.push({
            type: 'add_test',
            file: filePath,
            location: mutant.location,
            description: `Add test to kill ${mutant.mutatorName} mutant`
          });
          break;
        case 'NoCoverage':
          analysis.noCoverageMutants++;
          analysis.suggestedActions.push({
            type: 'add_coverage',
            file: filePath,
            location: mutant.location,
            description: `Add test coverage for this code path`
          });
          break;
      }
    });
  });

  analysis.mutationScore = (analysis.killedMutants / analysis.totalMutants) * 100;
  return analysis;
}
```

## Best Practices for Agents

1. **Always validate** the JSON structure before processing
2. **Handle null/optional fields** like `testFiles` gracefully
3. **Use location information** for precise code navigation
4. **Consider mutation context** when suggesting improvements
5. **Prioritize critical mutants** that affect core functionality
6. **Track changes over time** to monitor improvement trends

## Error Handling

- Malformed JSON: Report parsing errors and suggest regeneration
- Missing files: Handle missing source files gracefully
- Invalid locations: Validate line/column ranges before navigation
- Null test information: Work with available mutation data even if test details are missing

This JSON reporter provides LLM agents with comprehensive, structured data for intelligent mutation testing analysis and automated code quality improvement suggestions.