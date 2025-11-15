# Documentation: helm/templates/ragflow.yaml

## File Metadata

- **Path**: `helm/templates/ragflow.yaml`
- **Size**: 3683 bytes
- **Type**: .yaml
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `helm/templates/ragflow.yaml`.

## Original Source Code

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "ragflow.fullname" . }}
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: ragflow
spec:
  replicas: 1
  selector:
    matchLabels:
      {{- include "ragflow.selectorLabels" . | nindent 6 }}
      app.kubernetes.io/component: ragflow
  {{- with .Values.ragflow.deployment.strategy }}
  strategy:
    {{- . | toYaml | nindent 4 }}
  {{- end }}
  template:
    metadata:
      labels:
        {{- include "ragflow.labels" . | nindent 8 }}
        app.kubernetes.io/component: ragflow
      annotations:
        checksum/config-env: {{ include (print $.Template.BasePath "/env.yaml") . | sha256sum }}
        checksum/config-ragflow: {{ include (print $.Template.BasePath "/ragflow_config.yaml") . | sha256sum }}
    spec:
      {{- if or .Values.imagePullSecrets .Values.ragflow.image.pullSecrets }}
      imagePullSecrets:
        {{- with .Values.imagePullSecrets }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
        {{- with .Values.ragflow.image.pullSecrets }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
      {{- end }}
      containers:
      - name: ragflow
        image: {{ .Values.ragflow.image.repository }}:{{ .Values.ragflow.image.tag }}
        {{- with .Values.ragflow.image.pullPolicy }}
        imagePullPolicy: {{ . }}
        {{- end }}
        ports:
          - containerPort: 80
            name: http
          - containerPort: 9380
            name: http-api
        volumeMounts:
          - mountPath: /etc/nginx/conf.d/ragflow.conf
            subPath: ragflow.conf
            name: nginx-config-volume
          - mountPath: /etc/nginx/proxy.conf
            subPath: proxy.conf
            name: nginx-config-volume
          - mountPath: /etc/nginx/nginx.conf
            subPath: nginx.conf
            name: nginx-config-volume
          {{- with .Values.ragflow.service_conf }}
          - mountPath: /ragflow/conf/local.service_conf.yaml
            subPath: local.service_conf.yaml
            name: service-conf-volume
          {{- end }}
          {{- with .Values.ragflow.llm_factories }}
          - mountPath: /ragflow/conf/llm_factories.json
            subPath: llm_factories.json
            name: service-conf-volume
          {{- end }}
        envFrom:
          - secretRef:
              name: {{ include "ragflow.fullname" . }}-env-config
        {{- with .Values.ragflow.deployment.resources }}
        resources:
          {{- . | toYaml | nindent 10 }}
        {{- end }}
      volumes:
        - name: nginx-config-volume
          configMap:
            name: nginx-config
        - name: service-conf-volume
          configMap:
            name: ragflow-service-config
---
apiVersion: v1
kind: Service
metadata:
  name: {{ include "ragflow.fullname" . }}
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: ragflow
spec:
  selector:
    {{- include "ragflow.selectorLabels" . | nindent 4 }}
    app.kubernetes.io/component: ragflow
  ports:
    - protocol: TCP
      port: 80
      targetPort: http
      name: http
  type: {{ .Values.ragflow.service.type }}
---
{{- if .Values.ragflow.api.service.enabled }}
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-api
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: ragflow
spec:
  selector:
    {{- include "ragflow.selectorLabels" . | nindent 4 }}
    app.kubernetes.io/component: ragflow
  ports:
    - protocol: TCP
      port: 80
      targetPort: http-api
      name: http-api
  type: {{ .Values.ragflow.api.service.type }}
{{- end }}

```

## Detailed Analysis

### File Role in Repository

The file `helm/templates/ragflow.yaml` is located in the `helm/templates` directory.

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
