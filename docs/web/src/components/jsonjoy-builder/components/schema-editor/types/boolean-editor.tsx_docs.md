# Documentation: web/src/components/jsonjoy-builder/components/schema-editor/types/boolean-editor.tsx

## File Metadata

- **Path**: `web/src/components/jsonjoy-builder/components/schema-editor/types/boolean-editor.tsx`
- **Size**: 3440 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/jsonjoy-builder/components/schema-editor/types/boolean-editor.tsx`.

## Original Source Code

```tsx
import { Label } from '@/components/ui/label';
import { Switch } from '@/components/ui/switch';
import { useId } from 'react';
import { useTranslation } from '../../../hooks/use-translation';
import type { ObjectJSONSchema } from '../../../types/json-schema';
import { withObjectSchema } from '../../../types/json-schema';
import type { TypeEditorProps } from '../type-editor';

const BooleanEditor: React.FC<TypeEditorProps> = ({ schema, onChange }) => {
  const t = useTranslation();
  const allowTrueId = useId();
  const allowFalseId = useId();

  // Extract boolean-specific validation
  const enumValues = withObjectSchema(
    schema,
    (s) => s.enum as boolean[] | undefined,
    null,
  );

  // Determine if we have enum restrictions
  const hasRestrictions = Array.isArray(enumValues);
  const allowsTrue = !hasRestrictions || enumValues?.includes(true) || false;
  const allowsFalse = !hasRestrictions || enumValues?.includes(false) || false;

  // Handle changing the allowed values
  const handleAllowedChange = (value: boolean, allowed: boolean) => {
    let newEnum: boolean[] | undefined;

    if (allowed) {
      // If allowing this value
      if (!hasRestrictions) {
        // No current restrictions, nothing to do
        return;
      }

      if (enumValues?.includes(value)) {
        // Already allowed, nothing to do
        return;
      }

      // Add this value to enum
      newEnum = enumValues ? [...enumValues, value] : [value];

      // If both are now allowed, we can remove the enum constraint
      if (newEnum.includes(true) && newEnum.includes(false)) {
        newEnum = undefined;
      }
    } else {
      // If disallowing this value
      if (hasRestrictions && !enumValues?.includes(value)) {
        // Already disallowed, nothing to do
        return;
      }

      // Create a new enum with just the opposite value
      newEnum = [!value];
    }

    // Create a new validation object with just the type and enum
    const updatedValidation: ObjectJSONSchema = {
      type: 'boolean',
    };

    if (newEnum) {
      updatedValidation.enum = newEnum;
    } else {
      // Remove enum property if no restrictions
      onChange({ type: 'boolean' });
      return;
    }

    onChange(updatedValidation);
  };

  return (
    <div className="space-y-4">
      <div className="space-y-2 pt-2">
        <Label>Allowed Values</Label>

        <div className="space-y-3">
          <div className="flex items-center space-x-2">
            <Switch
              id={allowTrueId}
              checked={allowsTrue}
              onCheckedChange={(checked) => handleAllowedChange(true, checked)}
            />
            <Label htmlFor={allowTrueId} className="cursor-pointer">
              {t.booleanAllowTrueLabel}
            </Label>
          </div>

          <div className="flex items-center space-x-2">
            <Switch
              id={allowFalseId}
              checked={allowsFalse}
              onCheckedChange={(checked) => handleAllowedChange(false, checked)}
            />
            <Label htmlFor={allowFalseId} className="cursor-pointer">
              {t.booleanAllowFalseLabel}
            </Label>
          </div>
        </div>

        {!allowsTrue && !allowsFalse && (
          <p className="text-xs text-amber-600 mt-2">
            {t.booleanNeitherWarning}
          </p>
        )}
      </div>
    </div>
  );
};

export default BooleanEditor;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/jsonjoy-builder/components/schema-editor/types/boolean-editor.tsx` is located in the `web/src/components/jsonjoy-builder/components/schema-editor/types` directory.

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
- [number-editor.tsx](number-editor.tsx_docs.md)
- [object-editor.tsx](object-editor.tsx_docs.md)
- [string-editor.tsx](string-editor.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
