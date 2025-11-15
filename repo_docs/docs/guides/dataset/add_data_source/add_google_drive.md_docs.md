# File Documentation: docs/guides/dataset/add_data_source/add_google_drive.md

## File Metadata

- **Path**: `docs/guides/dataset/add_data_source/add_google_drive.md`
- **Extension**: `.md`
- **Lines**: 138
- **Characters**: 6,696
- **Size**: 6,702 bytes
- **Purpose**: Documentation - Markdown documentation file

## Original Source

```markdown
---
sidebar_position: 3
slug: /add_google_drive
---

# Add Google Drive

## 1. Create a Google Cloud Project

You can either create a dedicated project for RAGFlow or use an existing
Google Cloud external project.

**Steps:** 
1. Open the project creation page\
`https://console.cloud.google.com/projectcreate` 
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image1.jpeg?raw=true)
2. Select **External** as the Audience
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image2.png?raw=true)
3. Click **Create**
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image3.jpeg?raw=true)

------------------------------------------------------------------------

## 2. Configure OAuth Consent Screen

1.  Go to **APIs & Services → OAuth consent screen**
2.  Ensure **User Type = External**
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image4.jpeg?raw=true)
3.  Add your test users under **Test Users** by entering email addresses
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image5.jpeg?raw=true)
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image6.jpeg?raw=true)

------------------------------------------------------------------------

## 3. Create OAuth Client Credentials

1.  Navigate to:\
    `https://console.cloud.google.com/auth/clients`
2.  Create a **Web Application**
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image7.png?raw=true)
3.  Enter a name for the client
4.  Add the following **Authorized Redirect URIs**:

```
http://localhost:9380/v1/connector/google-drive/oauth/web/callback
```

### If using Docker deployment:

**Authorized JavaScript origin:**
```
http://localhost:80
```

![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image8.png?raw=true)
### If running from source:
**Authorized JavaScript origin:**
```
http://localhost:9222
```

![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image9.png?raw=true)
5.  After saving, click **Download JSON**. This file will later be
    uploaded into RAGFlow.

![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image10.png?raw=true)

------------------------------------------------------------------------

## 4. Add Scopes

1.  Open **Data Access → Add or remove scopes**

2.  Paste and add the following entries:

```
https://www.googleapis.com/auth/drive.readonly
https://www.googleapis.com/auth/drive.metadata.readonly
https://www.googleapis.com/auth/admin.directory.group.readonly
https://www.googleapis.com/auth/admin.directory.user.readonly
```

![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image11.jpeg?raw=true)
3.  Update and Save changes

![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image12.jpeg?raw=true)
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image13.jpeg?raw=true)

------------------------------------------------------------------------

## 5. Enable Required APIs
Navigate to the Google API Library:\
`https://console.cloud.google.com/apis/library`
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image14.png?raw=true)

Enable the following APIs:

- Google Drive API 
- Admin SDK API 
- Google Sheets API 
- Google Docs API
  

![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image15.png?raw=true)

![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image16.png?raw=true)

![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image17.png?raw=true)

![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image18.png?raw=true)

![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image19.png?raw=true)

![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image21.png?raw=true)

------------------------------------------------------------------------

## 6. Add Google Drive As a Data Source in RAGFlow

1.  Go to **Data Sources** inside RAGFlow
2.  Select **Google Drive**
3.  Upload the previously downloaded JSON credentials
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image22.jpeg?raw=true)
4.  Enter the shared Google Drive folder link (https://drive.google.com/drive), such as:
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image23.png?raw=true)

5.  Click **Authorize with Google**
A browser window will appear. 
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image25.jpeg?raw=true)
Click: - **Continue** - **Select All → Continue** - Authorization should
succeed - Select **OK** to add the data source
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image26.jpeg?raw=true)
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image27.jpeg?raw=true)
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image28.png?raw=true)
![placeholder-image](https://github.com/infiniflow/ragflow-docs/blob/040e4acd4c1eac6dc73dc44e934a6518de78d097/images/google_drive/image29.png?raw=true)



```

## High-Level Overview

# Add Google Drive

## Detailed Walkthrough

This is a documentation file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 138
- Blank lines: 41 (29.7%)
- Comment lines: ~12 (8.7%)
- Code lines: ~85


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `docs` directory, specifically within `docs/guides/dataset/add_data_source`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity

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

- Other files in `docs/guides/dataset/add_data_source/` directory
- Potential test file: `test_add_google_drive.md`

## Keywords

API, APIs, Access, Add, Admin, After, All, Application, Audience, Authorization, Authorize, Authorized, Click, Client, Cloud, Configure, Consent, Continue, Create, Credentials, Data, Docker, Docs, Documentation, Download, Drive, Enable, Ensure, Enter, External, Google, JSON, JavaScript, Library, Navigate, OAuth, Open, Paste, Project, RAGFlow, Redirect, Required, SDK, Save, Scopes, Screen, Select, Services, Sheets, Source...

---
*Generated by RAGFlow Repository Documentation Generator*
