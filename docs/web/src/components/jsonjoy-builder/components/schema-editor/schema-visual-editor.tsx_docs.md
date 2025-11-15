# Documentation: web/src/components/jsonjoy-builder/components/schema-editor/schema-visual-editor.tsx

## File Metadata

- **Path**: `web/src/components/jsonjoy-builder/components/schema-editor/schema-visual-editor.tsx`
- **Size**: 4011 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/jsonjoy-builder/components/schema-editor/schema-visual-editor.tsx`.

## Original Source Code

```tsx
import type { FC } from 'react';
import { useTranslation } from '../../hooks/use-translation';
import {
  createFieldSchema,
  updateObjectProperty,
  updatePropertyRequired,
} from '../../lib/schema-editor';
import type { JSONSchema, NewField } from '../../types/json-schema';
import { asObjectSchema, isBooleanSchema } from '../../types/json-schema';
import AddFieldButton from './add-field-button';
import SchemaFieldList from './schema-field-list';

/** @public */
export interface SchemaVisualEditorProps {
  schema: JSONSchema;
  onChange: (schema: JSONSchema) => void;
}

/** @public */
const SchemaVisualEditor: FC<SchemaVisualEditorProps> = ({
  schema,
  onChange,
}) => {
  const t = useTranslation();
  // Handle adding a top-level field
  const handleAddField = (newField: NewField) => {
    // Create a field schema based on the new field data
    const fieldSchema = createFieldSchema(newField);

    // Add the field to the schema
    let newSchema = updateObjectProperty(
      asObjectSchema(schema),
      newField.name,
      fieldSchema,
    );

    // Update required status if needed
    if (newField.required) {
      newSchema = updatePropertyRequired(newSchema, newField.name, true);
    }

    // Update the schema
    onChange(newSchema);
  };

  // Handle editing a top-level field
  const handleEditField = (name: string, updatedField: NewField) => {
    // Create a field schema based on the updated field data
    const fieldSchema = createFieldSchema(updatedField);

    // Update the field in the schema
    let newSchema = updateObjectProperty(
      asObjectSchema(schema),
      updatedField.name,
      fieldSchema,
    );

    // Update required status
    newSchema = updatePropertyRequired(
      newSchema,
      updatedField.name,
      updatedField.required || false,
    );

    // If name changed, we need to remove the old field
    if (name !== updatedField.name) {
      const { properties, ...rest } = newSchema;
      const { [name]: _, ...remainingProps } = properties || {};

      newSchema = {
        ...rest,
        properties: remainingProps,
      };

      // Re-add the field with the new name
      newSchema = updateObjectProperty(
        newSchema,
        updatedField.name,
        fieldSchema,
      );

      // Re-update required status if needed
      if (updatedField.required) {
        newSchema = updatePropertyRequired(newSchema, updatedField.name, true);
      }
    }

    // Update the schema
    onChange(newSchema);
  };

  // Handle deleting a top-level field
  const handleDeleteField = (name: string) => {
    // Check if the schema is valid first
    if (isBooleanSchema(schema) || !schema.properties) {
      return;
    }

    // Create a new schema without the field
    const { [name]: _, ...remainingProps } = schema.properties;

    const newSchema = {
      ...schema,
      properties: remainingProps,
    };

    // Remove from required array if present
    if (newSchema.required) {
      newSchema.required = newSchema.required.filter((field) => field !== name);
    }

    // Update the schema
    onChange(newSchema);
  };

  const hasFields =
    !isBooleanSchema(schema) &&
    schema.properties &&
    Object.keys(schema.properties).length > 0;

  return (
    <div className="p-4 h-full flex flex-col overflow-auto jsonjoy">
      <div className="mb-6 shrink-0">
        <AddFieldButton onAddField={handleAddField} />
      </div>

      <div className="grow overflow-auto">
        {!hasFields ? (
          <div className="text-center py-10 text-muted-foreground">
            <p className="mb-3">{t.visualEditorNoFieldsHint1}</p>
            <p className="text-sm">{t.visualEditorNoFieldsHint2}</p>
          </div>
        ) : (
          <SchemaFieldList
            schema={schema}
            onAddField={handleAddField}
            onEditField={handleEditField}
            onDeleteField={handleDeleteField}
          />
        )}
      </div>
    </div>
  );
};

export default SchemaVisualEditor;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/jsonjoy-builder/components/schema-editor/schema-visual-editor.tsx` is located in the `web/src/components/jsonjoy-builder/components/schema-editor` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to schema-editor.

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

- [add-field-button.tsx](add-field-button.tsx_docs.md)
- [json-schema-editor.tsx](json-schema-editor.tsx_docs.md)
- [json-schema-visualizer.tsx](json-schema-visualizer.tsx_docs.md)
- [schema-field-list.tsx](schema-field-list.tsx_docs.md)
- [schema-field.tsx](schema-field.tsx_docs.md)
- [schema-property-editor.tsx](schema-property-editor.tsx_docs.md)
- [schema-type-selector.tsx](schema-type-selector.tsx_docs.md)
- [type-dropdown.tsx](type-dropdown.tsx_docs.md)
- [type-editor.tsx](type-editor.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
