# File Documentation: helm/templates/infinity.yaml

## File Metadata

- **Path**: `helm/templates/infinity.yaml`
- **Extension**: `.yaml`
- **Lines**: 123
- **Characters**: 3,486
- **Size**: 3,486 bytes
- **Purpose**: Configuration - YAML configuration file

## Original Source

```yaml
{{- if eq .Values.env.DOC_ENGINE "infinity" -}}
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: {{ include "ragflow.fullname" . }}-infinity
  annotations:
    "helm.sh/resource-policy": keep
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: infinity
spec:
  {{- with .Values.infinity.storage.className }}
  storageClassName: {{ . }}
  {{- end }}
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: {{ .Values.infinity.storage.capacity }}
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: {{ include "ragflow.fullname" . }}-infinity
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: infinity
spec:
  replicas: 1
  selector:
    matchLabels:
      {{- include "ragflow.selectorLabels" . | nindent 6 }}
      app.kubernetes.io/component: infinity
  {{- with .Values.infinity.deployment.strategy }}
  strategy:
    {{- . | toYaml | nindent 4 }}
  {{- end }}
  template:
    metadata:
      labels:
        {{- include "ragflow.labels" . | nindent 8 }}
        app.kubernetes.io/component: infinity
      annotations:
        checksum/config: {{ include (print $.Template.BasePath "/env.yaml") . | sha256sum }}
    spec:
      {{- if or .Values.imagePullSecrets .Values.infinity.image.pullSecrets }}
      imagePullSecrets:
        {{- with .Values.imagePullSecrets }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
        {{- with .Values.infinity.image.pullSecrets }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
      {{- end }}
      containers:
      - name: infinity
        image: {{ .Values.infinity.image.repository }}:{{ .Values.infinity.image.tag }}
        {{- with .Values.infinity.image.pullPolicy }}
        imagePullPolicy: {{ . }}
        {{- end }}
        envFrom:
          - secretRef:
              name: {{ include "ragflow.fullname" . }}-env-config
        ports:
          - containerPort: 23817
            name: thrift
          - containerPort: 23820
            name: http
          - containerPort: 5432
            name: psql
        volumeMounts:
          - mountPath: /var/infinity
            name: infinity-data
        {{- with .Values.infinity.deployment.resources }}
        resources:
          {{- . | toYaml | nindent 10 }}
        {{- end }}
        securityContext:
          capabilities:
            add:
              - "NET_BIND_SERVICE"
          seccompProfile:
            type: RuntimeDefault
        livenessProbe:
          httpGet:
            path: /admin/node/current
            port: 23820
          initialDelaySeconds: 60
          periodSeconds: 10
          timeoutSeconds: 10
          failureThreshold: 120
      volumes:
        - name: infinity-data
          persistentVolumeClaim:
            claimName: {{ include "ragflow.fullname" . }}-infinity
---
apiVersion: v1
kind: Service
metadata:
  name: {{ include "ragflow.fullname" . }}-infinity
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: infinity
spec:
  selector:
      {{- include "ragflow.selectorLabels" . | nindent 6 }}
      app.kubernetes.io/component: infinity
  ports:
    - protocol: TCP
      port: 23817
      targetPort: thrift
      name: thrift
    - protocol: TCP
      port: 23820
      targetPort: http
      name: http
    - protocol: TCP
      port: 5432
      targetPort: psql
      name: psql
  type: {{ .Values.infinity.service.type }}
{{- end -}}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `helm/templates/infinity.yaml`.

Based on the file structure and naming, it appears to be a configuration - yaml configuration file.

The file contains approximately 123 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 123
- Blank lines: 1 (0.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~122


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `helm` directory, specifically within `helm/templates`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `helm/templates/` directory
- Potential test file: `test_infinity.yaml`

## Keywords

BasePath, DOC_ENGINE, NET_BIND_SERVICE, PersistentVolumeClaim, ReadWriteOnce, RuntimeDefault, Service, StatefulSet, TCP, Template, Values

---
*Generated by RAGFlow Repository Documentation Generator*
