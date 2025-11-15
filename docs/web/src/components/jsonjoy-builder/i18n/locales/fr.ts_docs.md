# Documentation: web/src/components/jsonjoy-builder/i18n/locales/fr.ts

## File Metadata

- **Path**: `web/src/components/jsonjoy-builder/i18n/locales/fr.ts`
- **Size**: 6863 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/jsonjoy-builder/i18n/locales/fr.ts`.

## Original Source Code

```ts
import type { Translation } from '../translation-keys.ts';

export const fr: Translation = {
  collapse: 'Réduire',
  expand: 'Étendre',

  fieldDescriptionPlaceholder: 'Décrivez le but de ce champ',
  fieldDelete: 'Supprimer le champ',
  fieldDescription: 'Description',
  fieldDescriptionTooltip: 'Ajoutez du contexte sur ce que ce champ représente',
  fieldNameLabel: 'Nom du champ',
  fieldNamePlaceholder: 'ex. prenom, age, estActif',
  fieldNameTooltip:
    'Utilisez camelCase pour une meilleure lisibilité (ex. prenom)',
  fieldRequiredLabel: 'Champ obligatoire',
  fieldType: 'Type de champ',
  fieldTypeExample: 'Exemple:',
  fieldTypeTooltipString: 'chaîne: Texte',
  fieldTypeTooltipNumber: 'nombre: Numérique',
  fieldTypeTooltipBoolean: 'booléen: Vrai/faux',
  fieldTypeTooltipObject: 'objet: JSON imbriqué',
  fieldTypeTooltipArray: 'tableau: Listes de valeurs',
  fieldAddNewButton: 'Ajouter un champ',
  fieldAddNewBadge: 'Constructeur de schéma',
  fieldAddNewCancel: 'Annuler',
  fieldAddNewConfirm: 'Ajouter un champ',
  fieldAddNewDescription: 'Créez un nouveau champ pour votre schéma JSON',
  fieldAddNewLabel: 'Ajouter un nouveau champ',

  fieldTypeTextLabel: 'Texte',
  fieldTypeTextDescription:
    'Pour les valeurs textuelles comme les noms, descriptions, etc.',
  fieldTypeNumberLabel: 'Nombre',
  fieldTypeNumberDescription: 'Pour les nombres décimaux ou entiers',
  fieldTypeBooleanLabel: 'Oui/Non',
  fieldTypeBooleanDescription: 'Pour les valeurs vrai/faux',
  fieldTypeObjectLabel: 'Groupe',
  fieldTypeObjectDescription: 'Pour regrouper des champs connexes',
  fieldTypeArrayLabel: 'Liste',
  fieldTypeArrayDescription: "Pour les collections d'éléments",

  propertyDescriptionPlaceholder: 'Ajouter une description...',
  propertyDescriptionButton: 'Ajouter une description...',
  propertyRequired: 'Obligatoire',
  propertyOptional: 'Facultatif',
  propertyDelete: 'Supprimer le champ',

  schemaEditorTitle: 'Éditeur de schéma JSON',
  schemaEditorToggleFullscreen: 'Basculer en plein écran',
  schemaEditorEditModeVisual: 'Visuel',
  schemaEditorEditModeJson: 'JSON',

  arrayMinimumLabel: 'Éléments minimum',
  arrayMinimumPlaceholder: 'Pas de minimum',
  arrayMaximumLabel: 'Éléments maximum',
  arrayMaximumPlaceholder: 'Pas de maximum',
  arrayForceUniqueItemsLabel: 'Forcer les éléments uniques',
  arrayItemTypeLabel: "Type d'élément",
  arrayValidationErrorMinMax:
    "'minItems' ne peut pas être supérieur à 'maxItems'.",
  arrayValidationErrorContainsMinMax:
    "'minContains' ne peut pas être supérieur à 'maxContains'.",

  booleanAllowFalseLabel: 'Autoriser la valeur faux',
  booleanAllowTrueLabel: 'Autoriser la valeur vrai',
  booleanNeitherWarning:
    'Avertissement: Vous devez autoriser au moins une valeur.',

  numberMinimumLabel: 'Valeur minimale',
  numberMinimumPlaceholder: 'Pas de minimum',
  numberMaximumLabel: 'Valeur maximale',
  numberMaximumPlaceholder: 'Pas de maximum',
  numberExclusiveMinimumLabel: 'Minimum exclusif',
  numberExclusiveMinimumPlaceholder: 'Pas de min exclusif',
  numberExclusiveMaximumLabel: 'Maximum exclusif',
  numberExclusiveMaximumPlaceholder: 'Pas de max exclusif',
  numberMultipleOfLabel: 'Multiple de',
  numberMultipleOfPlaceholder: 'Quelconque',
  numberAllowedValuesEnumLabel: 'Valeurs autorisées (enum)',
  numberAllowedValuesEnumNone: 'Aucune valeur restreinte définie',
  numberAllowedValuesEnumAddLabel: 'Ajouter',
  numberAllowedValuesEnumAddPlaceholder: 'Ajouter une valeur autorisée...',
  numberValidationErrorMinMax: 'Minimum et maximum doivent être cohérents.',
  numberValidationErrorBothExclusiveAndInclusiveMin:
    "Les champs 'exclusiveMinimum' et 'minimum' ne peuvent pas être définis en même temps.",
  numberValidationErrorBothExclusiveAndInclusiveMax:
    "Les champs 'exclusiveMaximum' et 'maximum' ne peuvent pas être définis en même temps.",
  numberValidationErrorEnumOutOfRange:
    "Les valeurs d'énumération doivent être dans la plage définie.",

  objectPropertiesNone: 'Aucune propriété définie',
  objectValidationErrorMinMax:
    "'minProperties' ne peut pas être supérieur à 'maxProperties'.",

  stringMinimumLengthLabel: 'Longueur minimale',
  stringMinimumLengthPlaceholder: 'Pas de minimum',
  stringMaximumLengthLabel: 'Longueur maximale',
  stringMaximumLengthPlaceholder: 'Pas de maximum',
  stringPatternLabel: 'Motif (regex)',
  stringPatternPlaceholder: '^[a-zA-Z]+$',
  stringFormatLabel: 'Format',
  stringFormatNone: 'Aucun',
  stringFormatDateTime: 'Date-Heure',
  stringFormatDate: 'Date',
  stringFormatTime: 'Heure',
  stringFormatEmail: 'Email',
  stringFormatUri: 'URI',
  stringFormatUuid: 'UUID',
  stringFormatHostname: "Nom d'hôte",
  stringFormatIpv4: 'Adresse IPv4',
  stringFormatIpv6: 'Adresse IPv6',
  stringAllowedValuesEnumLabel: 'Valeurs autorisées (enum)',
  stringAllowedValuesEnumNone: 'Aucune valeur restreinte définie',
  stringAllowedValuesEnumAddPlaceholder: 'Ajouter une valeur autorisée...',
  stringValidationErrorLengthRange:
    "'Longueur minimale' ne peut pas être supérieure à 'Longueur maximale'.",

  schemaTypeArray: 'Liste',
  schemaTypeBoolean: 'Oui/Non',
  schemaTypeNumber: 'Nombre',
  schemaTypeObject: 'Objet',
  schemaTypeString: 'Texte',
  schemaTypeNull: 'Vide',

  inferrerTitle: 'Déduire le schéma JSON',
  inferrerDescription:
    'Collez votre document JSON ci-dessous pour en générer un schéma.',
  inferrerCancel: 'Annuler',
  inferrerGenerate: 'Générer le schéma',
  inferrerErrorInvalidJson:
    'Format JSON invalide. Veuillez vérifier votre saisie.',

  validatorTitle: 'Valider le JSON',
  validatorDescription:
    'Collez votre document JSON pour le valider par rapport au schéma actuel. La validation se produit automatiquement pendant que vous tapez.',
  validatorCurrentSchema: 'Schéma actuel:',
  validatorContent: 'Votre JSON:',
  validatorValid: 'Le JSON est valide selon le schéma!',
  validatorErrorInvalidSyntax: 'Syntaxe JSON invalide',
  validatorErrorSchemaValidation: 'Erreur de validation du schéma',
  validatorErrorCount: '{count} erreurs de validation détectées',
  validatorErrorPathRoot: 'Élément racine',
  validatorErrorLocationLineAndColumn: 'Ligne {line}, Col {column}',
  validatorErrorLocationLineOnly: 'Ligne {line}',

  visualizerDownloadTitle: 'Télécharger le schéma',
  visualizerDownloadFileName: 'schema.json',
  visualizerSource: 'Source du schéma JSON',

  visualEditorNoFieldsHint1: 'Aucun champ défini pour le moment',
  visualEditorNoFieldsHint2: 'Ajoutez votre premier champ pour commencer',

  typeValidationErrorNegativeLength:
    'Les valeurs de longueur ne peuvent pas être négatives.',
  typeValidationErrorIntValue: 'La valeur doit être un nombre entier.',
  typeValidationErrorPositive: 'La valeur doit être positive.',
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/jsonjoy-builder/i18n/locales/fr.ts` is located in the `web/src/components/jsonjoy-builder/i18n/locales` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to locales.

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

- [de.ts](de.ts_docs.md)
- [en.ts](en.ts_docs.md)
- [ru.ts](ru.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
