# Example Task: Documentation Generator

This directory contains an example task sequence that demonstrates how to use the `cursor-agent` library to create automated documentation workflows.

## Overview

The `docs-generator.ts` file showcases a complete task sequence that:
1. Lists source files in the `src` directory
2. Reads the main index file
3. Searches for export statements
4. Updates documentation across multiple files in parallel
5. Searches for and documents React components

## Task Structure

```typescript
import type { TaskSequence } from "../src";

export const task102: TaskSequence = {
  tasks: [
    // Task 1: Explore source files
    // Task 2: Update documentation in parallel
    // Task 3: Document React components
  ]
};
```

## How to Use This Example

### 1. With Cursor AI Composer

1. Open Cursor IDE in this project
2. Open the Composer (Cmd/Ctrl + Shift + P)
3. Switch to "Agent" mode
4. Attach this file to the conversation
5. Use a prompt like:

```
Please execute this documentation task @docs-generator.ts
```

### 2. Modify for Your Project

You can customize this example for your own needs:

```typescript
import type { TaskSequence } from "cursor-agent";

export const myCustomTask: TaskSequence = {
  tasks: [
    {
      name: "list_dir",
      parameters: {
        relative_workspace_path: "YOUR_DIRECTORY",
        explanation: "YOUR_EXPLANATION"
      },
      subtasks: [
        // Add your subtasks here
      ]
    }
  ]
};
```

## Task Breakdown

### Task 1: List and Explore Source Files

```typescript
{
  name: "list_dir",
  parameters: {
    relative_workspace_path: "src",
    explanation: "List source files"
  },
  subtasks: [
    // Read the main index file
    {
      name: "read_file",
      parameters: {
        relative_workspace_path: "src/index.ts",
        should_read_entire_file: true,
        start_line_one_indexed: 1,
        end_line_one_indexed_inclusive: 100,
        explanation: "Read index file"
      }
    },
    // Search for all export statements
    {
      name: "grep_search",
      parameters: {
        query: "export",
        include_pattern: "*.ts",
        explanation: "Find exports"
      }
    }
  ]
}
```

**Purpose**: Discovers the project structure by listing the source directory, reading the main index file, and finding all export statements.

### Task 2: Parallel Documentation Updates

```typescript
{
  name: "parallel_apply",
  parameters: {
    edit_plan: "Update exports documentation",
    edit_regions: [
      {
        relative_workspace_path: "src/index.ts",
        start_line: 1,
        end_line: 100
      },
      {
        relative_workspace_path: "docs/api.md",
        start_line: 1,
        end_line: 50
      }
    ]
  }
}
```

**Purpose**: Applies documentation updates to multiple files simultaneously for efficiency.

### Task 3: Component Documentation

```typescript
{
  name: "codebase_search",
  parameters: {
    query: "Find React components",
    explanation: "Looking for React components in the codebase",
    target_directories: ["src/components"]
  },
  subtasks: [
    {
      name: "edit_file",
      parameters: {
        target_file: "docs/components.md",
        instructions: "Document the components",
        code_edit: "# Components Documentation\n\n// ... existing code ...",
        blocking: true
      }
    }
  ]
}
```

**Purpose**: Searches for React components and generates documentation based on the findings.

## Key Concepts Demonstrated

### 1. **Task Nesting**
Tasks can have subtasks, allowing you to create complex workflows where each task triggers follow-up actions.

### 2. **Sequential Execution**
Tasks in the main array execute in sequence, ensuring proper order of operations.

### 3. **Parallel Processing**
The `parallel_apply` task shows how to update multiple files simultaneously.

### 4. **Semantic Search**
The `codebase_search` task demonstrates semantic code searching, which is more powerful than simple text matching.

### 5. **Blocking Operations**
The `blocking: true` parameter ensures critical edits complete before moving forward.

## Common Patterns

### Pattern 1: Explore → Read → Edit
```typescript
{
  name: "list_dir",        // 1. Discover files
  subtasks: [
    {
      name: "read_file",   // 2. Read specific files
      subtasks: [
        {
          name: "edit_file" // 3. Make changes
        }
      ]
    }
  ]
}
```

### Pattern 2: Search → Filter → Apply
```typescript
{
  name: "codebase_search",  // 1. Find relevant code
  subtasks: [
    {
      name: "grep_search",  // 2. Filter results
      subtasks: [
        {
          name: "parallel_apply" // 3. Apply changes
        }
      ]
    }
  ]
}
```

### Pattern 3: Read → Analyze → Document
```typescript
{
  name: "read_file",       // 1. Read source
  subtasks: [
    {
      name: "codebase_search", // 2. Find related code
      subtasks: [
        {
          name: "edit_file"    // 3. Create docs
        }
      ]
    }
  ]
}
```

## Tips for Creating Your Own Tasks

1. **Start Simple**: Begin with a single task and gradually add complexity
2. **Use Explanations**: Always include the `explanation` field to help the AI understand your intent
3. **Test Incrementally**: Test each task level before adding more subtasks
4. **Be Specific**: Provide clear, specific instructions for file edits
5. **Use Blocking Wisely**: Set `blocking: true` for critical operations that must complete first

## Troubleshooting

### Task Not Executing
- Ensure you're in "Agent" mode in Cursor Composer
- Verify the file is properly attached (you should see @docs-generator.ts)
- Check that your prompt mentions the task file

### Unexpected Results
- Review the `explanation` fields to ensure clarity
- Check file paths are relative to workspace root
- Verify line numbers are within file bounds

### Type Errors
- Make sure to import `TaskSequence` type
- Ensure all required parameters are provided
- Check parameter types match the tool interfaces

## Next Steps

1. **Copy this example** and modify it for your use case
2. **Explore the main README** for more task creators
3. **Check the source code** in `../src/index.ts` for all available tools
4. **Create your own examples** and share them with the community

## Related Resources

- [Main README](../README.md) - Full library documentation
- [Source Code](../src/index.ts) - All available task creators
- [Tool Definitions](../agents/tools.ts) - TypeScript interfaces for all tools

## Contributing

If you create useful example tasks, consider contributing them back to the project! Open a PR with your examples in this directory.
