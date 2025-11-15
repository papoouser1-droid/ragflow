# File Documentation: web/src/pages/user-setting/data-source/component/google-drive-token-field.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/data-source/component/google-drive-token-field.tsx`
- **Extension**: `.tsx`
- **Lines**: 386
- **Characters**: 12,090
- **Size**: 12,090 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useCallback, useEffect, useMemo, useRef, useState } from 'react';

import { FileUploader } from '@/components/file-uploader';
import { Button } from '@/components/ui/button';
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
} from '@/components/ui/dialog';
import message from '@/components/ui/message';
import { FileMimeType } from '@/constants/common';
import {
  pollGoogleDriveWebAuthResult,
  startGoogleDriveWebAuth,
} from '@/services/data-source-service';
import { Loader2 } from 'lucide-react';

type GoogleDriveTokenFieldProps = {
  value?: string;
  onChange: (value: any) => void;
};

const credentialHasRefreshToken = (content: string) => {
  try {
    const parsed = JSON.parse(content);
    return Boolean(parsed?.refresh_token);
  } catch {
    return false;
  }
};

const describeCredentials = (content?: string) => {
  if (!content) return '';
  try {
    const parsed = JSON.parse(content);
    if (parsed?.refresh_token) {
      return 'Uploaded OAuth tokens with a refresh token.';
    }
    if (parsed?.installed || parsed?.web) {
      return 'Client credentials detected. Complete verification to mint long-lived tokens.';
    }
    return 'Stored Google credential JSON.';
  } catch {
    return '';
  }
};

const GoogleDriveTokenField = ({
  value,
  onChange,
}: GoogleDriveTokenFieldProps) => {
  const [files, setFiles] = useState<File[]>([]);
  const [pendingCredentials, setPendingCredentials] = useState<string>('');
  const [dialogOpen, setDialogOpen] = useState(false);
  const [webAuthLoading, setWebAuthLoading] = useState(false);
  const [webFlowId, setWebFlowId] = useState<string | null>(null);
  const [webStatus, setWebStatus] = useState<
    'idle' | 'waiting' | 'success' | 'error'
  >('idle');
  const [webStatusMessage, setWebStatusMessage] = useState('');
  const webFlowIdRef = useRef<string | null>(null);
  const webPollTimerRef = useRef<ReturnType<typeof setTimeout> | null>(null);

  const clearWebState = useCallback(() => {
    if (webPollTimerRef.current) {
      clearTimeout(webPollTimerRef.current);
      webPollTimerRef.current = null;
    }
    webFlowIdRef.current = null;
    setWebFlowId(null);
    setWebStatus('idle');
    setWebStatusMessage('');
  }, []);

  useEffect(() => {
    return () => {
      if (webPollTimerRef.current) {
        clearTimeout(webPollTimerRef.current);
      }
    };
  }, []);

  useEffect(() => {
    webFlowIdRef.current = webFlowId;
  }, [webFlowId]);

  const credentialSummary = useMemo(() => describeCredentials(value), [value]);
  const hasVerifiedTokens = useMemo(
    () => Boolean(value && credentialHasRefreshToken(value)),
    [value],
  );
  const hasUploadedButUnverified = useMemo(
    () => Boolean(value && !hasVerifiedTokens),
    [hasVerifiedTokens, value],
  );

  const resetDialog = useCallback(
    (shouldResetState: boolean) => {
      setDialogOpen(false);
      clearWebState();
      if (shouldResetState) {
        setPendingCredentials('');
        setFiles([]);
      }
    },
    [clearWebState],
  );

  const fetchWebResult = useCallback(
    async (flowId: string) => {
      try {
        const { data } = await pollGoogleDriveWebAuthResult({
          flow_id: flowId,
        });
        if (data.code === 0 && data.data?.credentials) {
          onChange(data.data.credentials);
          setPendingCredentials('');
          message.success('Google Drive credentials verified.');
          resetDialog(false);
          return;
        }
        if (data.code === 106) {
          setWebStatus('waiting');
          setWebStatusMessage('Authorization confirmed. Finalizing tokens...');
          if (webPollTimerRef.current) {
            clearTimeout(webPollTimerRef.current);
          }
          webPollTimerRef.current = setTimeout(
            () => fetchWebResult(flowId),
            1500,
          );
          return;
        }
        message.error(data.message || 'Authorization failed.');
        clearWebState();
      } catch (err) {
        message.error('Unable to retrieve authorization result.');
        clearWebState();
      }
    },
    [clearWebState, onChange, resetDialog],
  );

  useEffect(() => {
    const handler = (event: MessageEvent) => {
      const payload = event.data;
      if (!payload || payload.type !== 'ragflow-google-drive-oauth') {
        return;
      }
      if (!payload.flowId) {
        return;
      }
      if (webFlowIdRef.current && webFlowIdRef.current !== payload.flowId) {
        return;
      }

      if (payload.status === 'success') {
        setWebStatus('waiting');
        setWebStatusMessage('Authorization confirmed. Finalizing tokens...');
        fetchWebResult(payload.flowId);
      } else {
        message.error(
          payload.message || 'Authorization window reported an error.',
        );
        clearWebState();
      }
    };

    window.addEventListener('message', handler);
    return () => window.removeEventListener('message', handler);
  }, [clearWebState, fetchWebResult]);

  const handleValueChange = useCallback(
    (nextFiles: File[]) => {
      if (!nextFiles.length) {
        setFiles([]);
        onChange('');
        setPendingCredentials('');
        clearWebState();
        return;
      }
      const file = nextFiles[nextFiles.length - 1];
      file
        .text()
        .then((text) => {
          try {
            JSON.parse(text);
          } catch {
            message.error('Invalid JSON file.');
            setFiles([]);
            clearWebState();
            return;
          }
          setFiles([file]);
          clearWebState();
          if (credentialHasRefreshToken(text)) {
            onChange(text);
            setPendingCredentials('');
            message.success('OAuth credentials uploaded.');
            return;
          }
          setPendingCredentials(text);
          setDialogOpen(true);
          message.info(
            'Client configuration uploaded. Verification is required to finish setup.',
          );
        })
        .catch(() => {
          message.error('Unable to read the uploaded file.');
          setFiles([]);
        });
    },
    [clearWebState, onChange],
  );

  const handleStartWebAuthorization = useCallback(async () => {
    if (!pendingCredentials) {
      message.error('No Google credential file detected.');
      return;
    }
    setWebAuthLoading(true);
    clearWebState();
    try {
      const { data } = await startGoogleDriveWebAuth({
        credentials: pendingCredentials,
      });
      if (data.code === 0 && data.data?.authorization_url) {
        const flowId = data.data.flow_id;
        const popup = window.open(
          data.data.authorization_url,
          'ragflow-google-drive-oauth',
          'width=600,height=720',
        );
        if (!popup) {
          message.error(
            'Popup was blocked. Please allow popups for this site.',
          );
          return;
        }
        popup.focus();
        webFlowIdRef.current = flowId;
        setWebFlowId(flowId);
        setWebStatus('waiting');
        setWebStatusMessage('Complete the Google consent in the popup window.');
      } else {
        message.error(data.message || 'Failed to start browser authorization.');
      }
    } catch (err) {
      message.error('Failed to start browser authorization.');
    } finally {
      setWebAuthLoading(false);
    }
  }, [clearWebState, pendingCredentials]);

  const handleManualWebCheck = useCallback(() => {
    if (!webFlowId) {
      message.info('Start browser authorization first.');
      return;
    }
    setWebStatus('waiting');
    setWebStatusMessage('Checking authorization status...');
    fetchWebResult(webFlowId);
  }, [fetchWebResult, webFlowId]);

  const handleCancel = useCallback(() => {
    message.warning(
      'Verification canceled. Upload the credential again to restart.',
    );
    resetDialog(true);
  }, [resetDialog]);

  return (
    <div className="flex flex-col gap-3">
      {(credentialSummary ||
        hasVerifiedTokens ||
        hasUploadedButUnverified ||
        pendingCredentials) && (
        <div className="flex flex-wrap items-center gap-3 rounded-md border border-dashed border-muted-foreground/40 bg-muted/20 px-3 py-2 text-xs text-muted-foreground">
          <div className="flex flex-wrap items-center gap-2">
            {hasVerifiedTokens ? (
              <span className="rounded-full bg-emerald-100 px-2 py-0.5 text-[11px] font-semibold uppercase tracking-wide text-emerald-700">
                Verified
              </span>
            ) : null}
            {hasUploadedButUnverified ? (
              <span className="rounded-full bg-amber-100 px-2 py-0.5 text-[11px] font-semibold uppercase tracking-wide text-amber-700">
                Needs authorization
              </span>
            ) : null}
            {pendingCredentials && !hasVerifiedTokens ? (
              <span className="rounded-full bg-blue-100 px-2 py-0.5 text-[11px] font-semibold uppercase tracking-wide text-blue-700">
                Uploaded (pending)
              </span>
            ) : null}
          </div>
          {credentialSummary ? (
            <p className="m-0">{credentialSummary}</p>
          ) : null}
        </div>
      )}
      <FileUploader
        className="py-4 border-[0.5px] bg-bg-card text-text-secondary"
        value={files}
        onValueChange={handleValueChange}
        accept={{ '*.json': [FileMimeType.Json] }}
        maxFileCount={1}
        description="Upload your Google OAuth JSON file."
      />

      <Dialog
        open={dialogOpen}
        onOpenChange={(open) => {
          if (!open) {
            handleCancel();
          }
        }}
      >
        <DialogContent>
          <DialogHeader>
            <DialogTitle>Complete Google verification</DialogTitle>
            <DialogDescription>
              The uploaded client credentials do not contain a refresh token.
              Run the verification flow once to mint reusable tokens.
            </DialogDescription>
          </DialogHeader>

          <div className="space-y-4">
            <div className="rounded-md border border-dashed border-muted-foreground/40 bg-muted/10 px-4 py-4 text-sm text-muted-foreground">
              <div className="text-sm font-semibold text-foreground">
                Authorize in browser
              </div>
              <p className="mt-2">
                We will open Google&apos;s consent page in a new window. Sign in
                with the admin account, grant access, and return here. Your
                credentials will update automatically.
              </p>
              {webStatus !== 'idle' && (
                <p
                  className={`mt-2 text-xs ${
                    webStatus === 'error'
                      ? 'text-destructive'
                      : 'text-muted-foreground'
                  }`}
                >
                  {webStatusMessage}
                </p>
              )}
              <div className="mt-3 flex flex-wrap gap-2">
                <Button
                  onClick={handleStartWebAuthorization}
                  disabled={webAuthLoading}
                >
                  {webAuthLoading && (
                    <Loader2 className="mr-2 size-4 animate-spin" />
                  )}
                  Authorize with Google
                </Button>
                {webFlowId ? (
                  <Button
                    variant="outline"
                    onClick={handleManualWebCheck}
                    disabled={webStatus === 'success'}
                  >
                    Refresh status
                  </Button>
                ) : null}
              </div>
            </div>
          </div>

          <DialogFooter className="pt-2">
            <Button variant="ghost" onClick={handleCancel}>
              Cancel
            </Button>
          </DialogFooter>
        </DialogContent>
      </Dialog>
    </div>
  );
};

export default GoogleDriveTokenField;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/data-source/component/google-drive-token-field.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 386 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (15)

- `credentialHasRefreshToken()`: Function definition
- `describeCredentials()`: Function definition
- `GoogleDriveTokenField()`: Function definition
- `clearWebState()`: Function definition
- `credentialSummary()`: Function definition
- `hasVerifiedTokens()`: Function definition
- `hasUploadedButUnverified()`: Function definition
- `resetDialog()`: Function definition
- `fetchWebResult()`: Function definition
- `handler()`: Function definition
- `handleValueChange()`: Function definition
- `file()`: Function definition
- `handleStartWebAuthorization()`: Function definition
- `handleManualWebCheck()`: Function definition
- `handleCancel()`: Function definition

### Imports (8)

- `import { useCallback, useEffect, useMemo, useRef, useState } from 'react';`
- `import { FileUploader } from '@/components/file-uploader';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import message from '@/components/ui/message';`
- `import { FileMimeType } from '@/constants/common';`
- `import {`
- `import { Loader2 } from 'lucide-react';`

## Code Structure Analysis

- Total lines: 386
- Blank lines: 24 (6.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~362


## Dependencies and Imports

- `react`
- `@/components/file-uploader`
- `@/components/ui/button`
- `@/components/ui/message`
- `@/constants/common`
- `lucide-react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/data-source/component`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/user-setting/data-source/component/` directory
- Potential test file: `test_google-drive-token-field.tsx`

## Keywords

@/components/file-uploader, @/components/ui/button, @/components/ui/message, @/constants/common, Authorization, Authorize, Boolean, Button, Cancel, Checking, Client, Complete, Dialog, DialogContent, DialogDescription, DialogFooter, DialogHeader, DialogTitle, Drive, Failed, File, FileMimeType, FileUploader, Finalizing, Google, GoogleDriveTokenField, GoogleDriveTokenFieldProps, Invalid, JSON, Json, Loader2, MessageEvent, Needs, OAuth, Please, Popup, Refresh, ReturnType, Run, Sign, Start, Stored, The, TypeScript, Unable, Upload, Uploaded, Verification, Verified, Your...

---
*Generated by RAGFlow Repository Documentation Generator*
