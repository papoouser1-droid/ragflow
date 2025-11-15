# File Documentation: helm/templates/mysql.yaml

## File Metadata

- **Path**: `helm/templates/mysql.yaml`
- **Extension**: `.yaml`
- **Lines**: 111
- **Characters**: 3,361
- **Size**: 3,361 bytes
- **Purpose**: Configuration - YAML configuration file

## Original Source

```yaml
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: {{ include "ragflow.fullname" . }}-mysql
  annotations:
    "helm.sh/resource-policy": keep
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: mysql
spec:
  {{- with .Values.mysql.storage.className }}
  storageClassName: {{ . }}
  {{- end }}
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: {{ .Values.mysql.storage.capacity }}
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: {{ include "ragflow.fullname" . }}-mysql
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      {{- include "ragflow.selectorLabels" . | nindent 6 }}
      app.kubernetes.io/component: mysql
  {{- with .Values.mysql.deployment.strategy }}
  strategy:
    {{- . | toYaml | nindent 4 }}
  {{- end }}
  template:
    metadata:
      labels:
        {{- include "ragflow.labels" . | nindent 8 }}
        app.kubernetes.io/component: mysql
      annotations:
        checksum/config-mysql: {{ include (print $.Template.BasePath "/mysql-config.yaml") . | sha256sum }}
        checksum/config-env: {{ include (print $.Template.BasePath "/env.yaml") . | sha256sum }}
    spec:
      {{- if or .Values.imagePullSecrets .Values.mysql.image.pullSecrets }}
      imagePullSecrets:
        {{- with .Values.imagePullSecrets }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
        {{- with .Values.mysql.image.pullSecrets }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
      {{- end }}
      containers:
      - name: mysql
        image: {{ .Values.mysql.image.repository }}:{{ .Values.mysql.image.tag }}
        {{- with .Values.mysql.image.pullPolicy }}
        imagePullPolicy: {{ . }}
        {{- end }}
        envFrom:
          - secretRef:
              name: {{ include "ragflow.fullname" . }}-env-config
        args:
          - --max_connections=1000
          - --character-set-server=utf8mb4
          - --collation-server=utf8mb4_general_ci
          - --default-authentication-plugin=mysql_native_password
          - --tls_version=TLSv1.2,TLSv1.3
          - --init-file=/data/application/init.sql
          - --disable-log-bin
        ports:
          - containerPort: 3306
            name: mysql
        {{- with .Values.mysql.deployment.resources }}
        resources:
          {{- . | toYaml | nindent 10 }}
        {{- end }}
        volumeMounts:
          - mountPath: /var/lib/mysql
            name: mysql-data
          - mountPath: /data/application/init.sql
            subPath: init.sql
            readOnly: true
            name: init-script-volume
      volumes:
        - name: mysql-data
          persistentVolumeClaim:
            claimName: {{ include "ragflow.fullname" . }}-mysql
        - name: init-script-volume
          configMap:
            name: mysql-init-script
---
apiVersion: v1
kind: Service
metadata:
  name: {{ include "ragflow.fullname" . }}-mysql
  labels:
    {{- include "ragflow.labels" . | nindent 4 }}
    app.kubernetes.io/component: mysql
spec:
  selector:
    {{- include "ragflow.selectorLabels" . | nindent 4 }}
    app.kubernetes.io/component: mysql
  ports:
    - protocol: TCP
      port: 3306
      targetPort: mysql
  type: {{ .Values.mysql.service.type }}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `helm/templates/mysql.yaml`.

Based on the file structure and naming, it appears to be a configuration - yaml configuration file.

The file contains approximately 111 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 111
- Blank lines: 1 (0.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~110


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
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `helm/templates/` directory
- Potential test file: `test_mysql.yaml`

## Keywords

BasePath, PersistentVolumeClaim, ReadWriteOnce, Service, StatefulSet, TCP, TLSv1, Template, Values

---
*Generated by RAGFlow Repository Documentation Generator*
