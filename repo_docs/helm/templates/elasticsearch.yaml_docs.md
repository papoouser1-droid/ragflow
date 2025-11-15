# File Documentation: helm/templates/elasticsearch.yaml

## File Metadata

- **Path**: `helm/templates/elasticsearch.yaml`
- **Extension**: `.yaml`
- **Lines**: 132
- **Characters**: 4,366
- **Size**: 4,366 bytes
- **Purpose**: Configuration - YAML configuration file

## Original Source

```yaml
{{- if eq .Values.env.DOC_ENGINE "elasticsearch" -}}
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: {{ include "ragflow.fullname" . }}-es-data
  annotations:
    "helm.sh/resource-policy": keep
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: elasticsearch
spec:
  {{- with .Values.elasticsearch.storage.className }}
  storageClassName: {{ . }}
  {{- end }}
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: {{ .Values.elasticsearch.storage.capacity }}
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: {{ include "ragflow.fullname" . }}-es
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: elasticsearch
spec:
  replicas: 1
  selector:
    matchLabels:
      {{- include "ragflow.selectorLabels" . | nindent 6 }}
      app.kubernetes.io/component: elasticsearch
  {{- with .Values.elasticsearch.deployment.strategy }}
  strategy:
    {{- . | toYaml | nindent 4 }}
  {{- end }}
  template:
    metadata:
      labels:
      {{- include "ragflow.labels" . | nindent 8 }}
        app.kubernetes.io/component: elasticsearch
      annotations:
        checksum/config-es: {{ include (print $.Template.BasePath "/elasticsearch-config.yaml") . | sha256sum }}
        checksum/config-env: {{ include (print $.Template.BasePath "/env.yaml") . | sha256sum }}
    spec:
      {{- if or .Values.imagePullSecrets .Values.elasticsearch.image.pullSecrets }}
      imagePullSecrets:
        {{- with .Values.imagePullSecrets }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
        {{- with .Values.elasticsearch.image.pullSecrets }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
      {{- end }}
      initContainers:
      - name: fix-data-volume-permissions
        image: {{ .Values.elasticsearch.initContainers.alpine.repository }}:{{ .Values.elasticsearch.initContainers.alpine.tag }}
        {{- with .Values.elasticsearch.initContainers.alpine.pullPolicy }}
        imagePullPolicy: {{ . }}
        {{- end }}
        command:
        - sh
        - -c
        - "chown -R 1000:0 /usr/share/elasticsearch/data"
        volumeMounts:
          - mountPath: /usr/share/elasticsearch/data
            name: es-data
      - name: sysctl
        image: {{ .Values.elasticsearch.initContainers.busybox.repository }}:{{ .Values.elasticsearch.initContainers.busybox.tag }}
        {{- with .Values.elasticsearch.initContainers.busybox.pullPolicy }}
        imagePullPolicy: {{ . }}
        {{- end }}
        securityContext:
          privileged: true
          runAsUser: 0
        command: ["sysctl", "-w", "vm.max_map_count=262144"]
      containers:
      - name: elasticsearch
        image: {{ .Values.elasticsearch.image.repository }}:{{ .Values.elasticsearch.image.tag }}
        {{- with .Values.elasticsearch.image.pullPolicy }}
        imagePullPolicy: {{ . }}
        {{- end }}
        envFrom:
          - secretRef:
              name: {{ include "ragflow.fullname" . }}-env-config
          - configMapRef:
              name: {{ include "ragflow.fullname" . }}-es-config
        ports:
          - containerPort: 9200
            name: http
          - containerPort: 9300
            name: transport
        volumeMounts:
          - mountPath: /usr/share/elasticsearch/data
            name: es-data
        {{- with .Values.elasticsearch.deployment.resources }}
        resources:
          {{- . | toYaml | nindent 10 }}
        {{- end }}
        securityContext:
          capabilities:
            add:
              - "IPC_LOCK"
          runAsUser: 1000
          # NOTE: fsGroup doesn't seem to
          # work so use init container instead
          # fsGroup: 1000
          allowPrivilegeEscalation: false
      volumes:
        - name: es-data
          persistentVolumeClaim:
            claimName: {{ include "ragflow.fullname" . }}-es-data
---
apiVersion: v1
kind: Service
metadata:
  name: {{ include "ragflow.fullname" . }}-es
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: elasticsearch
spec:
  selector:
    {{- include "ragflow.selectorLabels" . | nindent 4 }}
    app.kubernetes.io/component: elasticsearch
  ports:
    - protocol: TCP
      port: 9200
      targetPort: http
  type: {{ .Values.elasticsearch.service.type }}
{{- end -}}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `helm/templates/elasticsearch.yaml`.

Based on the file structure and naming, it appears to be a configuration - yaml configuration file.

The file contains approximately 132 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 132
- Blank lines: 1 (0.8%)
- Comment lines: ~3 (2.3%)
- Code lines: ~128


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
- Potential test file: `test_elasticsearch.yaml`

## Keywords

BasePath, DOC_ENGINE, IPC_LOCK, NOTE, PersistentVolumeClaim, ReadWriteOnce, Service, StatefulSet, TCP, Template, Values

---
*Generated by RAGFlow Repository Documentation Generator*
