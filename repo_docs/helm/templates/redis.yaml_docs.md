# File Documentation: helm/templates/redis.yaml

## File Metadata

- **Path**: `helm/templates/redis.yaml`
- **Extension**: `.yaml`
- **Lines**: 134
- **Characters**: 3,909
- **Size**: 3,909 bytes
- **Purpose**: Configuration - YAML configuration file

## Original Source

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: {{ include "ragflow.fullname" . }}-redis
  annotations:
    "helm.sh/resource-policy": keep
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: redis
spec:
  ports:
    - port: 6379
      name: redis
      protocol: TCP
  clusterIP: None  # Headless service for StatefulSet
  selector:
    {{- include "ragflow.selectorLabels" . | nindent 4 }}
    app.kubernetes.io/component: redis
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: {{ include "ragflow.fullname" . }}-redis
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: redis
spec:
  serviceName: {{ include "ragflow.fullname" . }}-redis
  replicas: 1
  selector:
    matchLabels:
      {{- include "ragflow.selectorLabels" . | nindent 6 }}
      app.kubernetes.io/component: redis
  template:
    metadata:
      labels:
        {{- include "ragflow.labels" . | nindent 8 }}
        app.kubernetes.io/component: redis
      annotations:
        checksum/config-env: {{ include (print $.Template.BasePath "/env.yaml") . | sha256sum }}
    spec:
      {{- if or .Values.imagePullSecrets .Values.redis.image.pullSecrets }}
      imagePullSecrets:
        {{- with .Values.imagePullSecrets }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
        {{- with .Values.redis.image.pullSecrets }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
      {{- end }}
      terminationGracePeriodSeconds: 60
      containers:
        - name: redis
          image: {{ .Values.redis.image.repository }}:{{ .Values.redis.image.tag }}
          {{- with .Values.redis.image.pullPolicy }}
          imagePullPolicy: {{ . }}
          {{- end }}
          command:
            - "sh"
            - "-c"
            - "exec redis-server --requirepass ${REDIS_PASSWORD} --maxmemory 128mb --maxmemory-policy allkeys-lru"
          envFrom:
            - secretRef:
                name: {{ include "ragflow.fullname" . }}-env-config
          ports:
            - containerPort: 6379
              name: redis
          {{- if .Values.redis.persistence.enabled }}
          volumeMounts:
            - name: redis-data
              mountPath: /data
          {{- end }}
          {{- with .Values.redis.deployment.resources }}
          resources:
            {{- . | toYaml | nindent 12 }}
          {{- end }}
  {{- if .Values.redis.persistence.enabled }}
  {{- with .Values.redis.persistence.retentionPolicy }}
  persistentVolumeClaimRetentionPolicy:
    {{- with .whenDeleted }}
    whenDeleted: {{ . }}
    {{- end }}
    {{- with .whenScaled }}
    whenScaled: {{ . }}
    {{- end }}
  {{- end }}
  volumeClaimTemplates:
    - metadata:
        name: redis-data
        labels:
          {{- include "ragflow.selectorLabels" . | nindent 10 }}
          app.kubernetes.io/component: redis
      spec:
        accessModes:
          - ReadWriteOnce
        {{- with .Values.redis.storage.className }}
        storageClassName: {{ . }}
        {{- end }}
        resources:
          requests:
            storage: {{ .Values.redis.storage.capacity }}
  {{- end }}
---
apiVersion: v1
kind: Service
metadata:
  name: {{ include "ragflow.fullname" . }}-redis-svc
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: redis
spec:
  ports:
    - port: 6379
      targetPort: redis
      protocol: TCP
  selector:
    {{- include "ragflow.selectorLabels" . | nindent 4 }}
    app.kubernetes.io/component: redis
---
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: {{ include "ragflow.fullname" . }}-redis-pdb
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: redis
spec:
  minAvailable: 1
  selector:
    matchLabels:
      {{- include "ragflow.selectorLabels" . | nindent 6 }}
      app.kubernetes.io/component: redis

```

## High-Level Overview

This file is part of the RAGFlow repository located at `helm/templates/redis.yaml`.

Based on the file structure and naming, it appears to be a configuration - yaml configuration file.

The file contains approximately 134 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 134
- Blank lines: 1 (0.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~133


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `helm` directory, specifically within `helm/templates`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `helm/templates/` directory
- Potential test file: `test_redis.yaml`

## Keywords

BasePath, Headless, None, PodDisruptionBudget, REDIS_PASSWORD, ReadWriteOnce, Service, StatefulSet, TCP, Template, Values

---
*Generated by RAGFlow Repository Documentation Generator*
