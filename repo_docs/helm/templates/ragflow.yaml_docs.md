# File Documentation: helm/templates/ragflow.yaml

## File Metadata

- **Path**: `helm/templates/ragflow.yaml`
- **Extension**: `.yaml`
- **Lines**: 120
- **Characters**: 3,683
- **Size**: 3,683 bytes
- **Purpose**: Configuration - YAML configuration file

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `helm/templates/ragflow.yaml`.

Based on the file structure and naming, it appears to be a configuration - yaml configuration file.

The file contains approximately 120 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 120
- Blank lines: 1 (0.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~119


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `helm` directory, specifically within `helm/templates`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `helm/templates/` directory
- Potential test file: `test_ragflow.yaml`

## Keywords

BasePath, Deployment, Name, Release, Service, TCP, Template, Values

---
*Generated by RAGFlow Repository Documentation Generator*
