# Documentation: web/src/components/jsonjoy-builder/lib/schema-editor.ts

## File Metadata

- **Path**: `web/src/components/jsonjoy-builder/lib/schema-editor.ts`
- **Size**: 4130 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/jsonjoy-builder/lib/schema-editor.ts`.

## Original Source Code

```ts
import type {
  JSONSchema,
  NewField,
  ObjectJSONSchema,
} from '../types/json-schema';
import { isBooleanSchema, isObjectSchema } from '../types/json-schema';

export type Property = {
  name: string;
  schema: JSONSchema;
  required: boolean;
};

export function copySchema<T extends JSONSchema>(schema: T): T {
  if (typeof structuredClone === 'function') return structuredClone(schema);
  return JSON.parse(JSON.stringify(schema));
}

/**
 * Updates a property in an object schema
 */
export function updateObjectProperty(
  schema: ObjectJSONSchema,
  propertyName: string,
  propertySchema: JSONSchema,
): ObjectJSONSchema {
  if (!isObjectSchema(schema)) return schema;

  const newSchema = copySchema(schema);
  if (!newSchema.properties) {
    newSchema.properties = {};
  }

  newSchema.properties[propertyName] = propertySchema;
  return newSchema;
}

/**
 * Removes a property from an object schema
 */
export function removeObjectProperty(
  schema: ObjectJSONSchema,
  propertyName: string,
): ObjectJSONSchema {
  if (!isObjectSchema(schema) || !schema.properties) return schema;

  const newSchema = copySchema(schema);
  const { [propertyName]: _, ...remainingProps } = newSchema.properties;
  newSchema.properties = remainingProps;

  // Also remove from required array if present
  if (newSchema.required) {
    newSchema.required = newSchema.required.filter(
      (name) => name !== propertyName,
    );
  }

  return newSchema;
}

/**
 * Updates the 'required' status of a property
 */
export function updatePropertyRequired(
  schema: ObjectJSONSchema,
  propertyName: string,
  required: boolean,
): ObjectJSONSchema {
  if (!isObjectSchema(schema)) return schema;

  const newSchema = copySchema(schema);
  if (!newSchema.required) {
    newSchema.required = [];
  }

  if (required) {
    // Add to required array if not already there
    if (!newSchema.required.includes(propertyName)) {
      newSchema.required.push(propertyName);
    }
  } else {
    // Remove from required array
    newSchema.required = newSchema.required.filter(
      (name) => name !== propertyName,
    );
  }

  return newSchema;
}

/**
 * Updates an array schema's items
 */
export function updateArrayItems(
  schema: JSONSchema,
  itemsSchema: JSONSchema,
): JSONSchema {
  if (isObjectSchema(schema) && schema.type === 'array') {
    return {
      ...schema,
      items: itemsSchema,
    };
  }
  return schema;
}

/**
 * Creates a schema for a new field
 */
export function createFieldSchema(field: NewField): JSONSchema {
  const { type, description, validation } = field;
  if (isObjectSchema(validation)) {
    return {
      type,
      description,
      ...validation,
    };
  }
  return validation;
}

/**
 * Validates a field name
 */
export function validateFieldName(name: string): boolean {
  if (!name || name.trim() === '') {
    return false;
  }

  // Check that the name doesn't contain invalid characters for property names
  const validNamePattern = /^[a-zA-Z_$][a-zA-Z0-9_$]*$/;
  return validNamePattern.test(name);
}

/**
 * Gets properties from an object schema
 */
export function getSchemaProperties(schema: JSONSchema): Property[] {
  if (!isObjectSchema(schema) || !schema.properties) return [];

  const required = schema.required || [];

  return Object.entries(schema.properties).map(([name, propSchema]) => ({
    name,
    schema: propSchema,
    required: required.includes(name),
  }));
}

/**
 * Gets the items schema from an array schema
 */
export function getArrayItemsSchema(schema: JSONSchema): JSONSchema | null {
  if (isBooleanSchema(schema)) return null;
  if (schema.type !== 'array') return null;

  return schema.items || null;
}

/**
 * Checks if a schema has children
 */
export function hasChildren(schema: JSONSchema): boolean {
  if (!isObjectSchema(schema)) return false;

  if (schema.type === 'object' && schema.properties) {
    return Object.keys(schema.properties).length > 0;
  }

  if (schema.type === 'array' && schema.items && isObjectSchema(schema.items)) {
    return schema.items.type === 'object' && !!schema.items.properties;
  }

  return false;
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/jsonjoy-builder/lib/schema-editor.ts` is located in the `web/src/components/jsonjoy-builder/lib` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to lib.

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

- [schema-inference.ts](schema-inference.ts_docs.md)
- [utils.ts](utils.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
