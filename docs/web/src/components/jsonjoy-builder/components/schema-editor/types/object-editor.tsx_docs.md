# Documentation: web/src/components/jsonjoy-builder/components/schema-editor/types/object-editor.tsx

## File Metadata

- **Path**: `web/src/components/jsonjoy-builder/components/schema-editor/types/object-editor.tsx`
- **Size**: 4302 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/jsonjoy-builder/components/schema-editor/types/object-editor.tsx`.

## Original Source Code

```tsx
import { useTranslation } from '../../../hooks/use-translation';
import {
  getSchemaProperties,
  removeObjectProperty,
  updateObjectProperty,
  updatePropertyRequired,
} from '../../../lib/schema-editor';
import type { NewField, ObjectJSONSchema } from '../../../types/json-schema';
import { asObjectSchema, isBooleanSchema } from '../../../types/json-schema';
import AddFieldButton from '../add-field-button';
import SchemaPropertyEditor from '../schema-property-editor';
import type { TypeEditorProps } from '../type-editor';

const ObjectEditor: React.FC<TypeEditorProps> = ({
  schema,
  validationNode,
  onChange,
  depth = 0,
}) => {
  const t = useTranslation();

  // Get object properties
  const properties = getSchemaProperties(schema);

  // Create a normalized schema object
  const normalizedSchema: ObjectJSONSchema = isBooleanSchema(schema)
    ? { type: 'object', properties: {} }
    : { ...schema, type: 'object', properties: schema.properties || {} };

  // Handle adding a new property
  const handleAddProperty = (newField: NewField) => {
    // Create field schema from the new field data
    const fieldSchema = {
      type: newField.type,
      description: newField.description || undefined,
      ...(newField.validation || {}),
    } as ObjectJSONSchema;

    // Add the property to the schema
    let newSchema = updateObjectProperty(
      normalizedSchema,
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

  // Handle deleting a property
  const handleDeleteProperty = (propertyName: string) => {
    const newSchema = removeObjectProperty(normalizedSchema, propertyName);
    onChange(newSchema);
  };

  // Handle property name change
  const handlePropertyNameChange = (oldName: string, newName: string) => {
    if (oldName === newName) return;

    const property = properties.find((p) => p.name === oldName);
    if (!property) return;

    const propertySchemaObj = asObjectSchema(property.schema);

    // Add property with new name
    let newSchema = updateObjectProperty(
      normalizedSchema,
      newName,
      propertySchemaObj,
    );

    if (property.required) {
      newSchema = updatePropertyRequired(newSchema, newName, true);
    }

    newSchema = removeObjectProperty(newSchema, oldName);

    onChange(newSchema);
  };

  // Handle property required status change
  const handlePropertyRequiredChange = (
    propertyName: string,
    required: boolean,
  ) => {
    const newSchema = updatePropertyRequired(
      normalizedSchema,
      propertyName,
      required,
    );
    onChange(newSchema);
  };

  const handlePropertySchemaChange = (
    propertyName: string,
    propertySchema: ObjectJSONSchema,
  ) => {
    const newSchema = updateObjectProperty(
      normalizedSchema,
      propertyName,
      propertySchema,
    );
    onChange(newSchema);
  };

  return (
    <div className="space-y-4">
      {properties.length > 0 ? (
        <div className="space-y-2">
          {properties.map((property) => (
            <SchemaPropertyEditor
              key={property.name}
              name={property.name}
              schema={property.schema}
              required={property.required}
              validationNode={validationNode?.children[property.name]}
              onDelete={() => handleDeleteProperty(property.name)}
              onNameChange={(newName) =>
                handlePropertyNameChange(property.name, newName)
              }
              onRequiredChange={(required) =>
                handlePropertyRequiredChange(property.name, required)
              }
              onSchemaChange={(schema) =>
                handlePropertySchemaChange(property.name, schema)
              }
              depth={depth}
            />
          ))}
        </div>
      ) : (
        <div className="text-sm text-muted-foreground italic p-2 text-center border rounded-md">
          {t.objectPropertiesNone}
        </div>
      )}

      <div className="mt-4">
        <AddFieldButton onAddField={handleAddProperty} variant="secondary" />
      </div>
    </div>
  );
};

export default ObjectEditor;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/jsonjoy-builder/components/schema-editor/types/object-editor.tsx` is located in the `web/src/components/jsonjoy-builder/components/schema-editor/types` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to types.

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

- [array-editor.tsx](array-editor.tsx_docs.md)
- [boolean-editor.tsx](boolean-editor.tsx_docs.md)
- [number-editor.tsx](number-editor.tsx_docs.md)
- [string-editor.tsx](string-editor.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
