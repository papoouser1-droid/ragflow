# File Documentation: web/src/pages/dataset/dataset-setting/form-schema.ts

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/form-schema.ts`
- **Extension**: `.ts`
- **Lines**: 117
- **Characters**: 3,424
- **Size**: 3,424 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { t } from 'i18next';
import { z } from 'zod';

export const formSchema = z
  .object({
    parseType: z.number(),
    name: z.string().min(1, {
      message: 'Username must be at least 2 characters.',
    }),
    description: z.string().min(2, {
      message: 'Username must be at least 2 characters.',
    }),
    // avatar: z.instanceof(File),
    avatar: z.any().nullish(),
    permission: z.string().optional(),
    parser_id: z.string(),
    pipeline_id: z.string().optional(),
    pipeline_name: z.string().optional(),
    pipeline_avatar: z.string().optional(),
    embd_id: z.string(),
    parser_config: z
      .object({
        layout_recognize: z.string(),
        chunk_token_num: z.number(),
        delimiter: z.string(),
        auto_keywords: z.number().optional(),
        auto_questions: z.number().optional(),
        html4excel: z.boolean(),
        tag_kb_ids: z.array(z.string()).nullish(),
        topn_tags: z.number().optional(),
        toc_extraction: z.boolean().optional(),
        raptor: z
          .object({
            use_raptor: z.boolean().optional(),
            prompt: z.string().optional(),
            max_token: z.number().optional(),
            threshold: z.number().optional(),
            max_cluster: z.number().optional(),
            random_seed: z.number().optional(),
            scope: z.string().optional(),
          })
          .refine(
            (data) => {
              if (data.use_raptor && !data.prompt) {
                return false;
              }
              return true;
            },
            {
              message: 'Prompt is required',
              path: ['prompt'],
            },
          ),
        graphrag: z
          .object({
            use_graphrag: z.boolean().optional(),
            entity_types: z.array(z.string()).optional(),
            method: z.string().optional(),
            resolution: z.boolean().optional(),
            community: z.boolean().optional(),
          })
          .refine(
            (data) => {
              if (
                data.use_graphrag &&
                (!data.entity_types || data.entity_types.length === 0)
              ) {
                return false;
              }
              return true;
            },
            {
              message: 'Please enter Entity types',
              path: ['entity_types'],
            },
          ),
      })
      .optional(),
    pagerank: z.number(),
    connectors: z
      .array(
        z.object({
          id: z.string().optional(),
          name: z.string().optional(),
          source: z.string().optional(),
          ststus: z.string().optional(),
          auto_parse: z.string().optional(),
        }),
      )
      .optional(),
    // icon: z.array(z.instanceof(File)),
  })
  .superRefine((data, ctx) => {
    if (data.parseType === 2 && !data.pipeline_id) {
      ctx.addIssue({
        path: ['pipeline_id'],
        message: t('common.pleaseSelect'),
        code: 'custom',
      });
    }
  });

export const pipelineFormSchema = z.object({
  pipeline_id: z.string().optional(),
  set_default: z.boolean().optional(),
  file_filter: z.string().optional(),
});

// export const linkPiplineFormSchema = pipelineFormSchema.pick({
//   pipeline_id: true,
//   file_filter: true,
// });
// export const editPiplineFormSchema = pipelineFormSchema.pick({
//   set_default: true,
//   file_filter: true,
// });

```

## High-Level Overview

    // avatar: z.instanceof(File),

## Detailed Walkthrough

### Exports (2)

- `formSchema`: Exported entity
- `pipelineFormSchema`: Exported entity

### Functions (1)

- `formSchema()`: Function definition

### Imports (2)

- `import { t } from 'i18next';`
- `import { z } from 'zod';`

## Code Structure Analysis

- Total lines: 117
- Blank lines: 4 (3.4%)
- Comment lines: ~10 (8.5%)
- Code lines: ~103


## Dependencies and Imports

- `i18next`
- `zod`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset-setting`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/dataset/dataset-setting/` directory
- Potential test file: `test_form-schema.ts`

## Keywords

Entity, File, Please, Prompt, TypeScript, Username, editPiplineFormSchema, formSchema, i18next, linkPiplineFormSchema, pipelineFormSchema, zod

---
*Generated by RAGFlow Repository Documentation Generator*
