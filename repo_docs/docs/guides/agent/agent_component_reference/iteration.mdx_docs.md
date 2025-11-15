# File Documentation: docs/guides/agent/agent_component_reference/iteration.mdx

## File Metadata

- **Path**: `docs/guides/agent/agent_component_reference/iteration.mdx`
- **Extension**: `.mdx`
- **Lines**: 65
- **Characters**: 3,309
- **Size**: 3,309 bytes
- **Purpose**: General file in the repository

## Original Source

```
---
sidebar_position: 7
slug: /iteration_component
---

# Iteration component

A component that splits text input into text segments and iterates a predefined workflow for each one.

---

An **Interaction** component can divide text input into text segments and apply its built-in component workflow to each segment. 


## Scenario

An **Iteration** component is essential when a workflow loop is required and the loop count is *not* fixed but depends on number of segments created from the output of specific agent components. 

- If, for instance, you plan to feed several paragraphs into an LLM for content generation, each with its own focus, and feeding them to the LLM all at once could create confusion or contradictions, then you can use an **Iteration** component, which encapsulates a **Generate** component, to repeat the content generation process for each paragraph.
- Another example: If you wish to use the LLM to translate a lengthy paper into a target language without exceeding its token limit, consider using an **Iteration** component, which encapsulates a **Generate** component, to break the paper into smaller pieces and repeat the translation process for each one.

## Internal components

### IterationItem

Each **Iteration** component includes an internal **IterationItem** component. The **IterationItem** component serves as both the starting point and input node of the workflow within the **Iteration** component. It manages the loop of the workflow for all text segments created from the input.

:::tip NOTE
The **IterationItem** component is visible *only* to the components encapsulated by the current **Iteration** components.
:::

### Build an internal workflow 

You are allowed to pull other components into the **Iteration** component to build an internal workflow, and these "added internal components" are no longer visible to components outside of the current **Iteration** component.

:::danger IMPORTANT
To reference the created text segments from an added internal component, simply add a **Reference** variable that equals **IterationItem** within the **Input** section of that internal component. There is no need to reference the corresponding external component, as the **IterationItem** component manages the loop of the workflow for all created text segments. 
:::

:::tip NOTE
An added internal component can reference an external component when necessary.
:::

## Configurations

### Input

The **Iteration** component uses input variables to specify its data inputs, namely the texts to be segmented. You are allowed to specify multiple input sources for the **Iteration** component. Click **+ Add variable** in the **Input** section to include the desired input variables. There are two types of input variables: **Reference** and **Text**.

- **Reference**: Uses a component's output or a user input as the data source. You are required to select from the dropdown menu:
  - A component ID under **Component Output**, or 
  - A global variable under **Begin input**, which is defined in the **Begin** component.
- **Text**: Uses fixed text as the query. You are required to enter static text.

### Delimiter

The delimiter to use to split the text input into segments:

- Comma (Default)
- Line break
- Tab
- Underline
- Forward slash
- Dash
- Semicolon
```

## High-Level Overview

# Iteration component

## Detailed Walkthrough

This file's structure is not automatically analyzed. See the 'Original Source' section for content.

## Code Structure Analysis

- Total lines: 65
- Blank lines: 23 (35.4%)
- Comment lines: ~8 (12.3%)
- Code lines: ~34


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `docs` directory, specifically within `docs/guides/agent/agent_component_reference`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 8 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `docs/guides/agent/agent_component_reference/` directory
- Potential test file: `test_iteration.mdx`

## Keywords

Add, Another, Begin, Build, Click, Comma, Component, Configurations, Dash, Default, Delimiter, Each, Forward, Generate, IMPORTANT, Input, Interaction, Internal, Iteration, IterationItem, LLM, Line, NOTE, Output, Reference, Scenario, Semicolon, Tab, Text, The, There, Underline, Uses, You

---
*Generated by RAGFlow Repository Documentation Generator*
