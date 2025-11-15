# File Documentation: helm/templates/opensearch.yaml

## File Metadata

- **Path**: `helm/templates/opensearch.yaml`
- **Extension**: `.yaml`
- **Lines**: 136
- **Characters**: 4,435
- **Size**: 4,435 bytes
- **Purpose**: Configuration - YAML configuration file

## Original Source

```yaml
{{- if eq .Values.env.DOC_ENGINE "opensearch" -}}
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: {{ include "ragflow.fullname" . }}-opensearch-data
  annotations:
    "helm.sh/resource-policy": keep
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: opensearch
spec:
  {{- with .Values.opensearch.storage.className }}
  storageClassName: {{ . }}
  {{- end }}
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: {{ .Values.opensearch.storage.capacity }}
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: {{ include "ragflow.fullname" . }}-opensearch
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: opensearch
spec:
  replicas: 1
  selector:
    matchLabels:
      {{- include "ragflow.selectorLabels" . | nindent 6 }}
      app.kubernetes.io/component: opensearch
  {{- with .Values.opensearch.deployment.strategy }}
  strategy:
    {{- . | toYaml | nindent 4 }}
  {{- end }}
  template:
    metadata:
      labels:
      {{- include "ragflow.labels" . | nindent 8 }}
        app.kubernetes.io/component: opensearch
      annotations:
        checksum/config-opensearch: {{ include (print $.Template.BasePath "/opensearch-config.yaml") . | sha256sum }}
        checksum/config-env: {{ include (print $.Template.BasePath "/env.yaml") . | sha256sum }}
    spec:
      {{- if or .Values.imagePullSecrets .Values.opensearch.image.pullSecrets }}
      imagePullSecrets:
        {{- with .Values.imagePullSecrets }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
        {{- with .Values.opensearch.image.pullSecrets }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
      {{- end }}
      initContainers:
      - name: fix-data-volume-permissions
        image: {{ .Values.opensearch.initContainers.alpine.repository }}:{{ .Values.opensearch.initContainers.alpine.tag }}
        {{- with .Values.opensearch.initContainers.alpine.pullPolicy }}
        imagePullPolicy: {{ . }}
        {{- end }}
        command:
        - sh
        - -c
        - "chown -R 1000:0 /usr/share/opensearch/data"
        volumeMounts:
          - mountPath: /usr/share/opensearch/data
            name: opensearch-data
      - name: sysctl
        image: {{ .Values.opensearch.initContainers.busybox.repository }}:{{ .Values.opensearch.initContainers.busybox.tag }}
        {{- with .Values.opensearch.initContainers.busybox.pullPolicy }}
        imagePullPolicy: {{ . }}
        {{- end }}
        securityContext:
          privileged: true
          runAsUser: 0
        command: ["sysctl", "-w", "vm.max_map_count=262144"]
      containers:
      - name: opensearch
        image: {{ .Values.opensearch.image.repository }}:{{ .Values.opensearch.image.tag }}
        {{- with .Values.opensearch.image.pullPolicy }}
        imagePullPolicy: {{ . }}
        {{- end }}
        envFrom:
          - secretRef:
              name: {{ include "ragflow.fullname" . }}-env-config
          - configMapRef:
              name: {{ include "ragflow.fullname" . }}-opensearch-config
        ports:
          - containerPort: 9201
            name: http
        volumeMounts:
          - mountPath: /usr/share/opensearch/data
            name: opensearch-data
        {{- with .Values.opensearch.deployment.resources }}
        resources:
          {{- . | toYaml | nindent 10 }}
        {{- end }}
        securityContext:
          capabilities:
            add:
              - "IPC_LOCK"
          runAsUser: 1000
          allowPrivilegeEscalation: false
        livenessProbe:
          exec:
            command:
              - sh
              - -c
              - curl -u admin:$OPENSEARCH_PASSWORD localhost:9201
          initialDelaySeconds: 30
          periodSeconds: 10
          failureThreshold: 6
      volumes:
        - name: opensearch-data
          persistentVolumeClaim:
            claimName: {{ include "ragflow.fullname" . }}-opensearch-data
---
apiVersion: v1
kind: Service
metadata:
  name: {{ include "ragflow.fullname" . }}-opensearch
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: opensearch
spec:
  selector:
    {{- include "ragflow.selectorLabels" . | nindent 4 }}
    app.kubernetes.io/component: opensearch
  ports:
    - protocol: TCP
      port: 9201
      targetPort: http
  type: {{ .Values.opensearch.service.type }}
{{- end -}}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `helm/templates/opensearch.yaml`.

Based on the file structure and naming, it appears to be a configuration - yaml configuration file.

The file contains approximately 136 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 136
- Blank lines: 1 (0.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~135


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `helm` directory, specifically within `helm/templates`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- No specific performance concerns identified through static analysis

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
- Potential test file: `test_opensearch.yaml`

## Keywords

BasePath, DOC_ENGINE, IPC_LOCK, OPENSEARCH_PASSWORD, PersistentVolumeClaim, ReadWriteOnce, Service, StatefulSet, TCP, Template, Values

---
*Generated by RAGFlow Repository Documentation Generator*
