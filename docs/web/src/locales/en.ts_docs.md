# Documentation: web/src/locales/en.ts

## File Metadata

- **Path**: `web/src/locales/en.ts`
- **Size**: 98712 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/locales/en.ts`.

## Original Source Code

```ts
export default {
  translation: {
    common: {
      back: 'Back',
      noResults: 'No results.',
      selectPlaceholder: 'select value',
      selectAll: 'Select all',
      delete: 'Delete',
      deleteModalTitle: 'Are you sure to delete this item?',
      ok: 'Ok',
      cancel: 'Cancel',
      yes: 'Yes',
      no: 'No',
      total: 'Total',
      rename: 'Rename',
      name: 'Name',
      save: 'Save',
      namePlaceholder: 'Please input name',
      next: 'Next',
      create: 'Create',
      edit: 'Edit',
      upload: 'Upload',
      english: 'English',
      portugueseBr: 'Portuguese (Brazil)',
      chinese: 'Simplified Chinese',
      traditionalChinese: 'Traditional Chinese',
      language: 'Language',
      languageMessage: 'Please input your language!',
      languagePlaceholder: 'select your language',
      copy: 'Copy',
      copied: 'Copied',
      comingSoon: 'Coming soon',
      download: 'Download',
      close: 'Close',
      preview: 'Preview',
      move: 'Move',
      warn: 'Warn',
      action: 'Action',
      s: 'S',
      pleaseSelect: 'Please select',
      pleaseInput: 'Please input',
      submit: 'Submit',
      clear: 'Clear',
      embedIntoSite: 'Embed into webpage',
      previousPage: 'Previous',
      nextPage: 'Next',
      add: 'Add',
      remove: 'Remove',
      search: 'Search',
      noDataFound: 'No data found.',
      noData: 'No data',
      promptPlaceholder: `Please input or use / to quickly insert variables.`,
      mcp: {
        namePlaceholder: 'My MCP Server',
        nameRequired:
          'It must be 1–64 characters long and can only contain letters, numbers, hyphens, and underscores.',
        urlPlaceholder: 'https://api.example.com/v1/mcp',
        tokenPlaceholder: 'e.g. eyJhbGciOiJIUzI1Ni...',
      },
    },
    login: {
      loginTitle: 'Sign in to Your Account',
      signUpTitle: 'Create an Account',
      login: 'Sign in',
      signUp: 'Sign up',
      loginDescription: 'We’re so excited to see you again!',
      registerDescription: 'Glad to have you on board!',
      emailLabel: 'Email',
      emailPlaceholder: 'Please input email',
      passwordLabel: 'Password',
      passwordPlaceholder: 'Please input password',
      rememberMe: 'Remember me',
      signInTip: 'Don’t have an account?',
      signUpTip: 'Already have an account?',
      nicknameLabel: 'Nickname',
      nicknamePlaceholder: 'Please input nickname',
      register: 'Create an account',
      continue: 'Continue',
      title: 'A leading RAG engine for LLM context',
      start: "Let's get started",
      description:
        'Sign up for free to explore top RAG technology. Create knowledge bases and AIs to empower your business.',
      review: 'from 500+ reviews',
    },
    header: {
      knowledgeBase: 'Dataset',
      chat: 'Chat',
      register: 'Register',
      signin: 'Sign in',
      home: 'Home',
      setting: 'User settings',
      logout: 'Log out',
      fileManager: 'File Management',
      flow: 'Agent',
      search: 'Search',
      welcome: 'Welcome to',
      dataset: 'Dataset',
    },
    knowledgeList: {
      welcome: 'Welcome back',
      description: 'Which knowledge bases will you use today?',
      createKnowledgeBase: 'Create Dataset',
      name: 'Name',
      namePlaceholder: 'Please input name.',
      doc: 'Docs',
      searchKnowledgePlaceholder: 'Search',
      noMoreData: `That's all. Nothing more.`,
    },
    knowledgeDetails: {
      localUpload: 'Local Upload',
      fileSize: 'File Size',
      fileType: 'File Type',
      uploadedBy: 'Uploaded by',
      notGenerated: 'Not generated',
      generatedOn: 'Generated on ',
      subbarFiles: 'Files',
      generateKnowledgeGraph:
        'This will extract entities and relationships from all your documents in this dataset. The process may take a while to complete.',
      generateRaptor:
        'Performs recursive clustering and summarization of document chunks to build a hierarchical tree structure, enabling more context-aware retrieval across lengthy documents.',
      generate: 'Generate',
      raptor: 'RAPTOR',
      processingType: 'Processing Type',
      dataPipeline: 'Ingestion pipeline',
      operations: 'Operations',
      taskId: 'Task ID',
      duration: 'Duration',
      details: 'Details',
      status: 'Status',
      task: 'Task',
      startDate: 'Start Date',
      source: 'Source',
      fileName: 'File Name',
      datasetLogs: 'Dataset',
      fileLogs: 'File',
      overview: 'Logs',
      success: 'Success',
      failed: 'Failed',
      completed: 'Completed',
      datasetLog: 'Dataset Log',
      created: 'Created',
      learnMore: 'Built-in pipeline introduction',
      general: 'General',
      chunkMethodTab: 'Chunk Method',
      testResults: 'Test Results',
      testSetting: 'Test Setting',
      retrievalTesting: 'Retrieval Testing',
      retrievalTestingDescription:
        'Conduct a retrieval test to check if RAGFlow can recover the intended content for the LLM.',
      Parse: 'Parse',
      dataset: 'Dataset',
      testing: 'Retrieval testing',
      files: 'files',
      configuration: 'Configuration',
      knowledgeGraph: 'Knowledge Graph',
      name: 'Name',
      namePlaceholder: 'Please input name!',
      doc: 'Docs',
      datasetDescription:
        'Please wait for your files to finish parsing before starting an AI-powered chat.',
      addFile: 'Add file',
      searchFiles: 'Search your files',
      localFiles: 'Local files',
      emptyFiles: 'Create empty file',
      webCrawl: 'Web Crawl',
      chunkNumber: 'Chunk Number',
      uploadDate: 'Upload Date',
      chunkMethod: 'Chunking method',
      enabled: 'Enable',
      disabled: 'Disable',
      action: 'Action',
      parsingStatus: 'Parsing Status',
      parsingStatusTip:
        'Document parsing time varies based on several factors. Enabling features like Knowledge Graph, RAPTOR, Auto Question Extraction, or Auto Keyword Extraction will significantly increase processing time. If the progress bar stalls, please consult these two FAQs: https://ragflow.io/docs/dev/faq#why-does-my-document-parsing-stall-at-under-one-percent.',
      processBeginAt: 'Begin at',
      processDuration: 'Duration',
      progressMsg: 'Progress',
      noTestResultsForRuned:
        'No relevant results found. Try adjusting your query or parameters.',
      noTestResultsForNotRuned:
        'No test has been run yet. Results will appear here.',
      testingDescription:
        'Conduct a retrieval test to check if RAGFlow can recover the intended content for the LLM. If you have adjusted the default settings, such as keyword similarity weight or similarity threshold, to achieve the optimal results, be aware that these changes will not be automatically saved. You must apply them to your chat assistant settings or the Retrieval agent component settings.',
      similarityThreshold: 'Similarity threshold',
      similarityThresholdTip:
        'RAGFlow employs either a combination of weighted keyword similarity and weighted vector cosine similarity, or a combination of weighted keyword similarity and weighted reranking score during retrieval. This parameter sets the threshold for similarities between the user query and chunks. Any chunk with a similarity score below this threshold will be excluded from the results. By default, the threshold is set to 0.2. This means that only chunks with hybrid similarity score of 20 or higher will be retrieved.',
      vectorSimilarityWeight: 'Vector similarity weight',
      vectorSimilarityWeightTip:
        'This sets the weight of keyword similarity in the combined similarity score, either used with vector cosine similarity or with reranking score. The total of the two weights must equal 1.0.',
      keywordSimilarityWeight: 'Keyword similarity weight',
      keywordSimilarityWeightTip:
        'This sets the weight of keyword similarity in the combined similarity score, either used with vector cosine similarity or with reranking score. The total of the two weights must equal 1.0.',
      testText: 'Test text',
      testTextPlaceholder: 'Input your question here!',
      testingLabel: 'Run',
      similarity: 'Hybrid similarity',
      termSimilarity: 'Term similarity',
      vectorSimilarity: 'Vector similarity',
      hits: 'Hits',
      view: 'View',
      filesSelected: 'Files selected',
      upload: 'Upload',
      run: 'Parse',
      runningStatus0: 'PENDING',
      runningStatus1: 'PARSING',
      runningStatus2: 'CANCELED',
      runningStatus3: 'SUCCESS',
      runningStatus4: 'FAIL',
      pageRanges: 'Page Ranges',
      pageRangesTip:
        'Range of pages to be parsed; pages outside this range will not be processed.',
      fromPlaceholder: 'from',
      fromMessage: 'Missing start page number',
      toPlaceholder: 'to',
      toMessage: 'Missing end page number (excluded)',
      layoutRecognize: 'PDF parser',
      layoutRecognizeTip:
        'Use a visual model for PDF layout analysis to effectively locate document titles, text blocks, images, and tables. If the naive option is chosen, only the plain text in the PDF will be retrieved. Please note that this option currently works ONLY for PDF documents.',
      taskPageSize: 'Task page size',
      taskPageSizeMessage: 'Please input your task page size!',
      taskPageSizeTip: `During layout recognition, a PDF file is split into chunks and processed in parallel to increase processing speed. This parameter sets the size of each chunk. A larger chunk size reduces the likelihood of splitting continuous text between pages.`,
      addPage: 'Add page',
      greaterThan: 'The current value must be greater than to!',
      greaterThanPrevious:
        'The current value must be greater than the previous to!',
      selectFiles: 'Select files',
      changeSpecificCategory: 'Change specific category',
      uploadTitle: 'Drag and drop your file here to upload',
      uploadDescrip

... [Content truncated - file is 98667 bytes] ...

s.`,
        },
        user: {
          keywords: `Text Content
[Insert text here]`,
          questions: `Text Content
[Insert text here]`,
          summary: `Text to Summarize:
[Insert text here]`,
          metadata: `Content: [INSERT CONTENT HERE]`,
        },
      },
      cancel: 'Cancel',
      swicthPromptMessage:
        'The prompt word will change. Please confirm whether to abandon the existing prompt word?',
      tokenizerSearchMethodOptions: {
        full_text: 'Full-text',
        embedding: 'Embedding',
      },
      filenameEmbeddingWeight: 'Filename embedding weight',
      tokenizerFieldsOptions: {
        text: 'Processed Text',
        keywords: 'Keywords',
        questions: 'Questions',
        summary: 'Augmented Context',
      },
      imageParseMethodOptions: {
        ocr: 'OCR',
      },
      structuredOutput: {
        configuration: 'Configuration',
        structuredOutput: 'Structured output',
      },
      operations: 'Operations',
      operationsOptions: {
        selectKeys: 'Select keys',
        literalEval: 'Literal eval',
        combine: 'Combine',
        filterValues: 'Filter values',
        appendOrUpdate: 'Append or update',
        removeKeys: 'Remove keys',
        renameKeys: 'Rename keys',
      },
      ListOperationsOptions: {
        topN: 'Top N',
        head: 'Head',
        tail: 'Tail',
        sort: 'Sort',
        filter: 'Filter',
        dropDuplicates: 'Drop duplicates',
      },
      sortMethod: 'Sort method',
      SortMethodOptions: {
        asc: 'Ascending',
        desc: 'Descending',
      },
    },
    llmTools: {
      bad_calculator: {
        name: 'Calculator',
        description:
          'A tool to calculate the sum of two numbers (will give wrong answer)',
        params: {
          a: 'The first number',
          b: 'The second number',
        },
      },
    },
    modal: {
      okText: 'Confirm',
      cancelText: 'Cancel',
    },
    mcp: {
      export: 'Export',
      import: 'Import',
      url: 'URL',
      serverType: 'Server Type',
      addMCP: 'Add MCP',
      editMCP: 'Edit MCP',
      toolsAvailable: 'tools available',
      mcpServers: 'MCP servers',
      customizeTheListOfMcpServers: 'Customize the list of MCP servers',
      cachedTools: 'cached tools',
      bulkManage: 'Bulk manage',
      exitBulkManage: 'Exit bulk manage',
      selected: 'Selected',
    },
    search: {
      searchApps: 'Search Apps',
      createSearch: 'Create Search',
      searchGreeting: 'How can I help you today ？',
      profile: 'Hide Profile',
      locale: 'Locale',
      embedCode: 'Embed code',
      id: 'ID',
      copySuccess: 'Copy Success',
      welcomeBack: 'Welcome back',
      searchSettings: 'Search Settings',
      name: 'Name',
      avatar: 'Avatar',
      description: 'Description',
      datasets: 'Datasets',
      rerankModel: 'Rerank Model',
      AISummary: 'AI Summary',
      enableWebSearch: 'Enable Web Search',
      enableRelatedSearch: 'Enable Related Search',
      showQueryMindmap: 'Show Query Mindmap',
      embedApp: 'Embed App',
      relatedSearch: 'Related Search',
      descriptionValue: 'You are an intelligent assistant.',
      okText: 'Save',
      cancelText: 'Cancel',
      chooseDataset: 'Please select a dataset first',
    },
    language: {
      english: 'English',
      chinese: 'Chinese',
      spanish: 'Spanish',
      french: 'French',
      german: 'German',
      japanese: 'Japanese',
      korean: 'Korean',
      vietnamese: 'Vietnamese',
    },
    pagination: {
      total: 'Total {{total}}',
      page: '{{page}} /Page',
    },
    dataflowParser: {
      result: 'Result',
      parseSummary: 'Parse Summary',
      parseSummaryTip: 'Parser：deepdoc',
      parserMethod: 'Parser Method',
      outputFormat: 'Output Format',
      rerunFromCurrentStep: 'Rerun From Current Step',
      rerunFromCurrentStepTip: 'Changes detected. Click to re-run.',
      confirmRerun: 'Confirm Rerun Process',
      confirmRerunModalContent: `
      <p class="text-sm text-text-disabled font-medium mb-2">
        You are about to rerun the process starting from the <span class="text-text-secondary">{{step}}</span> step.
      </p>
      <p class="text-sm mb-3 text-text-disabled">This will:</p><br />
      <ul class="list-disc list-inside space-y-1 text-sm text-text-secondary">
        <li>• Overwrite existing results from the current step onwards</li>
        <li>• Create a new log entry for tracking</li>
        <li>• Previous steps will remain unchanged</li>
      </ul>`,
      changeStepModalTitle: 'Step Switch Warning',
      changeStepModalContent: `
      <p>You are currently editing the results of this stage.</p>
      <p>If you switch to a later stage, your changes will be lost. </p>
      <p>To keep them, please click Rerun to re-run the current stage.</p> `,
      changeStepModalConfirmText: 'Switch Anyway',
      changeStepModalCancelText: 'Cancel',
      unlinkPipelineModalTitle: 'Unlink Ingestion pipeline',
      unlinkPipelineModalConfirmText: 'Unlink',
      unlinkPipelineModalContent: `
      <p>Once unlinked, this Dataset will no longer be connected to the current Ingestion pipeline.</p>
      <p>Files that are already being parsed  will continue until completion</p>
      <p>Files that are not yet parsed will no longer be processed</p> <br/>
      <p>Are you sure you want to proceed?</p> `,
      unlinkSourceModalTitle: 'Unlink data source',
      unlinkSourceModalContent: `
      <p>Are you sure to unlink this data source ？</p>`,
      unlinkSourceModalConfirmText: 'Unlink',
    },
    datasetOverview: {
      downloadTip: 'Files being downloaded from data sources. ',
      processingTip: 'Files being processed by Ingestion pipeline.',
      totalFiles: 'Total Files',
      downloading: 'Downloading',
      downloadSuccessTip: 'Total successful downloads',
      downloadFailedTip: 'Total failed downloads',
      processingSuccessTip: 'Total successfully processed files',
      processingFailedTip: 'Total failed processes',
      processing: 'Processing',
    },
    admin: {
      loginTitle: 'Admin Console',
      title: 'RAGFlow',
      confirm: 'Confirm',
      close: 'Close',
      yes: 'Yes',
      no: 'No',
      delete: 'Delete',
      cancel: 'Cancel',
      reset: 'Reset',
      import: 'Import',
      description: 'Description',
      noDescription: 'No description',

      resourceType: {
        dataset: 'Dataset',
        chat: 'Chat',
        agent: 'Agent',
        search: 'Search',
        file: 'File',
        team: 'Team',
        memory: 'Memory',
      },

      permissionType: {
        enable: 'Enable',
        read: 'Read',
        write: 'Write',
        share: 'Share',
      },

      serviceStatus: 'Service status',
      userManagement: 'User management',
      registrationWhitelist: 'Registration whitelist',
      roles: 'Roles',
      monitoring: 'Monitoring',

      back: 'Back',
      active: 'Active',
      inactive: 'Inactive',
      enable: 'Enable',
      disable: 'Disable',
      all: 'All',
      actions: 'Actions',
      newUser: 'New user',
      email: 'Email',
      name: 'Name',
      nickname: 'Nickname',
      status: 'Status',
      id: 'ID',
      serviceType: 'Service type',
      host: 'Host',
      port: 'Port',

      role: 'Role',
      user: 'User',
      superuser: 'Superuser',

      createTime: 'Create time',
      lastLoginTime: 'Last login time',
      lastUpdateTime: 'Last update time',

      isAnonymous: 'Is Anonymous',
      isSuperuser: 'Is Superuser',

      deleteUser: 'Delete user',
      deleteUserConfirmation: 'Are you sure you want to delete this user?',

      createNewUser: 'Create new user',
      changePassword: 'Change password',
      newPassword: 'New password',
      confirmNewPassword: 'Confirm new password',
      password: 'Password',
      confirmPassword: 'Confirm password',

      invalidEmail: 'Please input a valid email address!',
      passwordRequired: 'Please input your password!',
      passwordMinLength: 'Password must be more than 8 characters.',
      confirmPasswordRequired: 'Please confirm your password!',
      confirmPasswordDoNotMatch: 'The password that you entered do not match!',

      read: 'Read',
      write: 'Write',
      share: 'Share',
      create: 'Create',

      extraInfo: 'Extra information',
      serviceDetail: `Service {{name}} detail`,
      taskExecutorDetail: 'Task executor detail',

      whitelistManagement: 'Whitelist management',
      exportAsExcel: 'Export Excel',
      importFromExcel: 'Import Excel',
      createEmail: 'Create email',
      deleteEmail: 'Delete email',
      editEmail: 'Edit email',
      deleteWhitelistEmailConfirmation:
        'Are you sure you want to delete this email from whitelist? This action cannot be undone.',

      importWhitelist: 'Import whitelist (Excel)',
      importSelectExcelFile: 'Excel file (.xlsx)',
      importOverwriteExistingEmails: 'Overwrite existing emails',
      importInvalidExcelFile: 'Please select a valid Excel file',
      importFileRequired: 'Please select a file to import',
      importFileTips:
        'File must contain a single header column named <code>email</code>.',

      chunkNum: 'Chunks',
      docNum: 'Documents',
      tokenNum: 'Tokens used',
      language: 'Language',
      createDate: 'Create date',
      updateDate: 'Update date',
      permission: 'Permission',

      agentTitle: 'Agent title',
      canvasCategory: 'Canvas category',

      newRole: 'New Role',
      addNewRole: 'Add new role',
      roleName: 'Role name',
      roleNameRequired: 'Role name is required',
      resources: 'Resources',

      editRoleDescription: 'Edit role description',
      deleteRole: 'Delete role',
      deleteRoleConfirmation:
        'Are you sure you want to delete this role? This action cannot be undone.',

      alive: 'Alive',
      timeout: 'Timeout',
      fail: 'Fail',
    },
  },
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/locales/en.ts` is located in the `web/src/locales` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to locales.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Watch for XSS vulnerabilities
- Ensure proper input sanitization
- Validate all API calls

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [config.ts](config.ts_docs.md)
- [de.ts](de.ts_docs.md)
- [es.ts](es.ts_docs.md)
- [fr.ts](fr.ts_docs.md)
- [id.ts](id.ts_docs.md)
- [ja.ts](ja.ts_docs.md)
- [pt-br.ts](pt-br.ts_docs.md)
- [ru.ts](ru.ts_docs.md)
- [until.ts](until.ts_docs.md)
- [vi.ts](vi.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
