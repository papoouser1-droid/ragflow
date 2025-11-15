# Documentation: web/src/pages/dataset/dataset-setting/form-schema.ts

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/form-schema.ts`
- **Size**: 3424 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/dataset-setting/form-schema.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/dataset-setting/form-schema.ts` is located in the `web/src/pages/dataset/dataset-setting` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to dataset-setting.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Watch for XSS vulnerabilities
- Ensure proper input sanitization
- Validate all API calls

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [category-panel.tsx](category-panel.tsx_docs.md)
- [chunk-method-form.tsx](chunk-method-form.tsx_docs.md)
- [chunk-method-learn-more.tsx](chunk-method-learn-more.tsx_docs.md)
- [configuration-form-container.tsx](configuration-form-container.tsx_docs.md)
- [general-form.tsx](general-form.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.tsx](index.tsx_docs.md)
- [permission-form-field.tsx](permission-form-field.tsx_docs.md)
- [saving-button.tsx](saving-button.tsx_docs.md)
- [tag-tabs.tsx](tag-tabs.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
