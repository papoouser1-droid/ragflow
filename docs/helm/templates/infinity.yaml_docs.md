# Documentation: helm/templates/infinity.yaml

## File Metadata

- **Path**: `helm/templates/infinity.yaml`
- **Size**: 3486 bytes
- **Type**: .yaml
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `helm/templates/infinity.yaml`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `helm/templates/infinity.yaml` is located in the `helm/templates` directory.

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
- [ingress.yaml](ingress.yaml_docs.md)
- [minio.yaml](minio.yaml_docs.md)
- [mysql-config.yaml](mysql-config.yaml_docs.md)
- [mysql.yaml](mysql.yaml_docs.md)
- [opensearch-config.yaml](opensearch-config.yaml_docs.md)
- [opensearch.yaml](opensearch.yaml_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
