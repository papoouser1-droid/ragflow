# Documentation: helm/templates/redis.yaml

## File Metadata

- **Path**: `helm/templates/redis.yaml`
- **Size**: 3909 bytes
- **Type**: .yaml
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `helm/templates/redis.yaml`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `helm/templates/redis.yaml` is located in the `helm/templates` directory.

### Architecture Context

Files in this location typically handle concerns related to templates.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [_helpers.tpl](_helpers.tpl_docs.md)
- [elasticsearch-config.yaml](elasticsearch-config.yaml_docs.md)
- [elasticsearch.yaml](elasticsearch.yaml_docs.md)
- [env.yaml](env.yaml_docs.md)
- [infinity.yaml](infinity.yaml_docs.md)
- [ingress.yaml](ingress.yaml_docs.md)
- [minio.yaml](minio.yaml_docs.md)
- [mysql-config.yaml](mysql-config.yaml_docs.md)
- [mysql.yaml](mysql.yaml_docs.md)
- [opensearch-config.yaml](opensearch-config.yaml_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
