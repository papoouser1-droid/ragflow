# File Documentation: web/src/locales/en.ts

## File Metadata

- **Path**: `web/src/locales/en.ts`
- **Extension**: `.ts`
- **Lines**: 2,082
- **Characters**: 98,667
- **Size**: 98,712 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
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
      uploadDescription:
        'Supports single or batch file upload. For a locally deployed RAGFlow: the total file size limit per upload is 1GB, with a batch upload limit of 32 files. There is no cap on the total number of files per account. For demo.ragflow.io, the total file size limit per upload is 10MB, with each file not exceeding 10MB and a maximum of 128 files per account.',
      chunk: 'Chunk',
      bulk: 'Bulk',
      cancel: 'Cancel',
      close: 'Close',
      rerankModel: 'Rerank model',
      rerankPlaceholder: 'Please select',
      rerankTip: `Optional. If left empty, RAGFlow will use a combination of weighted keyword similarity and weighted vector cosine similarity; if a rerank model is selected, a weighted reranking score will replace the weighted vector cosine similarity. Please be aware that using a rerank model will significantly increase the system's response time. If you wish to use a rerank model, ensure you use a SaaS reranker; if you prefer a locally deployed rerank model, ensure you start RAGFlow with docker-compose-gpu.yml.`,
      topK: 'Top-K',
      topKTip: `Used together with the Rerank model, this setting defines the number of text chunks to be sent to the specified reranking model.`,
      delimiter: `Delimiter for text`,
      delimiterTip:
        'A delimiter or separator can consist of one or multiple special characters. If it is multiple characters, ensure they are enclosed in backticks( ``). For example, if you configure your delimiters like this: \\n`##`;, then your texts will be separated at line breaks, double hash symbols (##), and semicolons.',
      html4excel: 'Excel to HTML',
      html4excelTip: `Use with the General chunking method. When disabled, spreadsheets (XLSX or XLS(Excel 97-2003)) in the knowledge base will be parsed into key-value pairs. When enabled, they will be parsed into HTML tables, splitting every 12 rows if the original table has more than 12 rows. See https://ragflow.io/docs/dev/enable_excel2html for details.`,
      autoKeywords: 'Auto-keyword',
      autoKeywordsTip: `Automatically extract N keywords for each chunk to increase their ranking for queries containing those keywords. Be aware that extra tokens will be consumed by the chat model specified in 'System model settings'. You can check or update the added keywords for a chunk from the chunk list. For details, see https://ragflow.io/docs/dev/autokeyword_autoquestion.`,
      autoQuestions: 'Auto-question',
      autoQuestionsTip: `Automatically extract N questions for each chunk to increase their ranking for queries containing those questions. You can check or update the added questions for a chunk from the chunk list. This feature will not disrupt the chunking process if an error occurs, except that it may add an empty result to the original chunk. Be aware that extra tokens will be consumed by the LLM specified in 'System model settings'. For details, see https://ragflow.io/docs/dev/autokeyword_autoquestion.`,
      redo: 'Do you want to clear the existing {{chunkNum}} chunks?',
      setMetaData: 'Set Meta Data',
      pleaseInputJson: 'Please enter JSON',
      documentMetaTips: `<p>The meta data is in Json format(it's not searchable). It will be added into prompt for LLM if any chunks of this document are included in the prompt.</p>
<p>Examples:</p>
<b>The meta data is:</b><br>
<code>
  {
      "Author": "Alex Dowson",
      "Date": "2024-11-12"
  }
</code><br>
<b>The prompt will be:</b><br>
<p>Document: the_name_of_document</p>
<p>Author: Alex Dowson</p>
<p>Date: 2024-11-12</p>
<p>Relevant fragments as following:</p>
<ul>
<li>  Here is the chunk content....</li>
<li>  Here is the chunk content....</li>
</ul>
`,
      metaData: 'Meta data',
      deleteDocumentConfirmContent:
        'The document is associated with the knowledge graph. After deletion, the related node and relationship information will be deleted, but the graph will not be updated immediately. The update graph action is performed during the process of parsing the new document that carries the knowledge graph extraction task.',
      plainText: 'Naive',
      reRankModelWaring: 'Re-rank model is very time consuming.',
    },
    knowledgeConfiguration: {
      generationScopeTip:
        'Determines whether RAPTOR is generated for the entire dataset or for a single file.',
      scopeDataset: 'Dataset',
      generationScope: 'Generation Scope',
      scopeSingleFile: 'Single File',
      autoParse: 'Auto Parse',
      rebuildTip:
        'Re-downloads files from the linked data source and parses them again.',
      baseInfo: 'Basic Info',
      globalIndex: 'Global Index',
      dataSource: 'Data Source',
      linkSourceSetTip: 'Manage data source linkage with this dataset',
      linkDataSource: 'Link Data Source',
      tocExtraction: 'TOC Enhance',
      tocExtractionTip:
        " For existing chunks, generate a hierarchical table of contents (one directory per file). During queries, when Directory Enhancement is activated, the system will use a large model to determine which directory items are relevant to the user's question, thereby identifying the relevant chunks.",
      deleteGenerateModalContent: `
        <p>Deleting the generated <strong class='text-text-primary'>{{type}}</strong>  results
        will remove all derived entities and relationships from this dataset.
        Your original files will remain intact.<p>
        <br/>
        Do you want to continue?
      `,
      extractRaptor: 'Extract Raptor',
      extractKnowledgeGraph: 'Extract Knowledge Graph',
      filterPlaceholder: 'please input filter',
      fileFilterTip: '',
      fileFilter: 'File Filter',
      setDefaultTip: '',
      setDefault: 'Set as Default',
      eidtLinkDataPipeline: 'Edit Ingestion pipeline',
      linkPipelineSetTip: 'Manage Ingestion pipeline linkage with this dataset',
      default: 'Default',
      dataPipeline: 'Ingestion pipeline',
      linkDataPipeline: 'Link Ingestion pipeline',
      enableAutoGenerate: 'Enable Auto Generate',
      teamPlaceholder: 'Please select a team.',
      dataFlowPlaceholder: 'Please select a pipeline.',
      buildItFromScratch: 'Build it from scratch',
      dataFlow: 'Pipeline',
      parseType: 'Parse Type',
      manualSetup: 'Choose pipeline',
      builtIn: 'Built-in',
      titleDescription:
        'Update your knowledge base configuration here, particularly the chunking method.',
      name: 'Knowledge base name',
      photo: 'Knowledge base photo',
      photoTip: 'You can upload a file with 4 MB',
      description: 'Description',
      language: 'Document language',
      languageMessage: 'Please input your language!',
      languagePlaceholder: 'Please input your language!',
      permissions: 'Permissions',
      embeddingModel: 'Embedding model',
      chunkTokenNumber: 'Recommended chunk size',
      chunkTokenNumberMessage: 'Chunk token number for text is required',
      embeddingModelTip:
        'The default embedding model for the knowledge base. It cannot be changed once the knowledge base has chunks. To switch to a different default embedding model, you must delete all existing chunks in the knowledge base.',
      permissionsTip:
        "If it is set to 'Team', all your team members will be able to manage the knowledge base.",
      chunkTokenNumberTip:
        'It kind of sets the token threshold for a creating a chunk. A segment with fewer tokens than this threshold will be combined with the following segments until the token count exceeds the threshold, at which point a chunk is created. No new chunk is created unless a delimiter is encountered, even if the threshold is exceeded.',
      chunkMethod: 'Chunking method',
      chunkMethodTip: 'View the tips on the right.',
      upload: 'Upload',
      english: 'English',
      chinese: 'Chinese',
      portugueseBr: 'Portuguese (Brazil)',
      embeddingModelPlaceholder: 'Please select a embedding model.',
      chunkMethodPlaceholder: 'Please select a chunking method.',
      save: 'Save',
      me: 'Only me',
      team: 'Team',
      cancel: 'Cancel',
      methodTitle: 'Chunking method description',
      methodExamples: 'Examples',
      methodExamplesDescription:
        'The following screenshots are provided for clarification.',
      dialogueExamplesTitle: 'view',
      methodEmpty:
        'This will display a visual explanation of the knowledge base categories',
      book: `<p>Supported file formats are <b>DOCX</b>, <b>PDF</b>, <b>TXT</b>.</p><p>
      For each book in PDF, please set the <i>page ranges</i> to remove unwanted information and reduce analysis time.</p>`,
      laws: `<p>Supported file formats are <b>DOCX</b>, <b>PDF</b>, <b>TXT</b>.</p><p>
      Legal documents typically follow a rigorous writing format. We use text feature to identify split point.
      </p><p>
      The chunk has a granularity consistent with 'ARTICLE', ensuring all upper level text is included in the chunk.
      </p>`,
      manual: `<p>Only <b>PDF</b> is supported.</p><p>
      We assume that the manual has a hierarchical section structure, using the lowest section titles as basic unit for chunking documents. Therefore, figures and tables in the same section will not be separated, which may result in larger chunk sizes.
      </p>`,
      naive: `<p>Supported file formats are <b>MD, MDX, DOCX, XLSX, XLS (Excel 97-2003), PPTX, PDF, TXT, JPEG, JPG, PNG, TIF, GIF, CSV, JSON, EML, HTML</b>.</p>
      <p>This method chunks files using a 'naive' method: </p>
      <p>
      <li>Use vision detection model to split the texts into smaller segments.</li>
      <li>Then, combine adjacent segments until the token count exceeds the threshold specified by 'Chunk token number for text', at which point a chunk is created.</li></p>`,
      paper: `<p>Only <b>PDF</b> file is supported.</p><p>
      Papers will be split by section, such as <i>abstract, 1.1, 1.2</i>. </p><p>
      This approach enables the LLM to summarize the paper more effectively and to provide more comprehensive, understandable responses.
      However, it also increases the context for AI conversations and adds to the computational cost for the LLM. So during a conversation, consider reducing the value of ‘<b>topN</b>’.</p>`,
      presentation: `<p>Supported file formats are <b>PDF</b>, <b>PPTX</b>.</p><p>
      Every page in the slides is treated as a chunk, with its thumbnail image stored.</p><p>
      <i>This chunking method is automatically applied to all uploaded PPT files, so you do not need to specify it manually.</i></p>`,
      qa: `
      <p>
      This chunking method supports <b>XLSX</b> and <b>CSV/TXT</b> file formats.
    </p>
    <li>
      If a file is in <b>XLSX</b> or <b>XLS (Excel 97-2003)</b> format, it should contain two columns without headers: one for questions and the other for answers, with the question column preceding the answer column. Multiple sheets are
      acceptable, provided the columns are properly structured.
    </li>
    <li>
      If a file is in <b>CSV/TXT</b> format, it must be UTF-8 encoded with TAB as the delimiter to separate questions and answers.
    </li>
    <p>
      <i>
        Lines of texts that fail to follow the above rules will be ignored, and
        each Q&A pair will be considered a distinct chunk.
      </i>
    </p>
      `,
      resume: `<p>Supported file formats are <b>DOCX</b>, <b>PDF</b>, <b>TXT</b>.
      </p><p>
      Résumés of various forms are parsed and organized into structured data to facilitate candidate search for recruiters.
      </p>
      `,
      table: `<p>Supported file formats are <b>XLSX</b> and <b>CSV/TXT</b>.</p><p>
      Here are some prerequisites and tips:
      <ul>
    <li>For CSV or TXT file, the delimiter between columns must be <em><b>TAB</b></em>.</li>
    <li>The first row must be column headers.</li>
    <li>Column headers must be meaningful terms to aid your LLM's understanding.
    It is good practice to juxtapose synonyms separated by a slash <i>'/'</i> and to enumerate values using brackets, for example: <i>'Gender/Sex (male, female)'</i>.<p>
    Here are some examples of headers:<ol>
        <li>supplier/vendor<b>'TAB'</b>Color (Yellow, Blue, Brown)<b>'TAB'</b>Sex/Gender (male, female)<b>'TAB'</b>size (M, L, XL, XXL)</li>
        </ol>
        </p>
    </li>
    <li>Every row in table will be treated as a chunk.</li>
    </ul>`,
      picture: `
    <p>Image files are supported, with video support coming soon.</p><p>
    This method employs an OCR model to extract texts from images.
    </p><p>
    If the text extracted by the OCR model is deemed insufficient, a specified visual LLM will be used to provide a description of the image.
    </p>`,
      one: `
    <p>Supported file formats are <b>DOCX, XLSX, XLS (Excel 97-2003), PDF, TXT</b>.
    </p><p>
    This method treats each document in its entirety as a chunk.
    </p><p>
    Applicable when you require the LLM to summarize the entire document, provided it can handle that amount of context length.
    </p>`,
      knowledgeGraph: `<p>Supported file formats are <b>DOCX, EXCEL, PPT, IMAGE, PDF, TXT, MD, JSON, EML</b>

<p>This approach chunks files using the 'naive'/'General' method. It splits a document into segments and then combines adjacent segments until the token count exceeds the threshold specified by 'Chunk token number for text', at which point a chunk is created.</p>
<p>The chunks are then fed to the LLM to extract entities and relationships for a knowledge graph and a mind map.</p>
<p>Ensure that you set the <b>Entity types</b>.</p>`,
      tag: `<p>A knowledge base using the 'Tag' chunking method functions as a tag set. Other knowledge bases use it to tag their chunks, and queries to these knowledge bases are also tagged using this tag set.</p>
<p>A tag set will <b>NOT</b> be directly involved in a Retrieval-Augmented Generation (RAG) process.</p>
<p>Each chunk in this knowledge base is an independent description-tag pair.</p>
<p>Supported file formats include <b>XLSX</b> and <b>CSV/TXT</b>:</p>
<p>If a file is in <b>XLSX</b> format, it should contain two columns without headers: one for tag descriptions and the other for tag names, with the Description column preceding the Tag column. Multiple sheets are acceptable, provided the columns are properly structured.</p>
<p>If a file is in <b>CSV/TXT</b> format, it must be UTF-8 encoded with TAB as the delimiter to separate descriptions and tags.</p>
<p>In a Tag column, <b>comma</b> is used to separate tags.</p>
<i>Lines of texts that fail to follow the above rules will be ignored.</i>
`,
      useRaptor: 'RAPTOR',
      useRaptorTip:
        'RAPTOR can be used for multi-hop question-answering tasks. Navigate to the Files page, click Generate > RAPTOR to enable it. See https://ragflow.io/docs/dev/enable_raptor for details.',
      prompt: 'Prompt',
      promptTip:
        'Use the system prompt to describe the task for the LLM, specify how it should respond, and outline other miscellaneous requirements. The system prompt is often used in conjunction with keys (variables), which serve as various data inputs for the LLM. Use a forward slash `/` or the (x) button to show the keys to use.',
      promptMessage: 'Prompt is required',
      promptText: `Please summarize the following paragraphs. Be careful with the numbers, do not make things up. Paragraphs as following:
      {cluster_content}
The above is the content you need to summarize.`,
      maxToken: 'Max token',
      maxTokenTip: 'The maximum number of tokens per generated summary chunk.',
      maxTokenMessage: 'Max token is required',
      threshold: 'Threshold',
      thresholdTip:
        'In RAPTOR, chunks are clustered by their semantic similarity. The Threshold parameter sets the minimum similarity required for chunks to be grouped together. A higher Threshold means fewer chunks in each cluster, while a lower one means more.',
      thresholdMessage: 'Threshold is required',
      maxCluster: 'Max cluster',
      maxClusterTip: 'The maximum number of clusters to create.',
      maxClusterMessage: 'Max cluster is required',
      randomSeed: 'Random seed',
      randomSeedMessage: 'Random seed is required',
      entityTypes: 'Entity types',
      vietnamese: 'Vietnamese',
      pageRank: 'Page rank',
      pageRankTip: `You can assign a higher PageRank score to specific knowledge bases during retrieval. The corresponding score is added to the hybrid similarity scores of retrieved chunks from these knowledge bases, increasing their ranking. See https://ragflow.io/docs/dev/set_page_rank for details.`,
      tagName: 'Tag',
      frequency: 'Frequency',
      searchTags: 'Search tags',
      tagCloud: 'Cloud',
      tagTable: 'Table',
      tagSet: 'Tag sets',
      tagSetTip: `
     <p> Select one or multiple tag knowledge bases to auto-tag chunks in your knowledge base. See https://ragflow.io/docs/dev/use_tag_sets for details.</p>
<p>The user query will also be auto-tagged.</p>
This auto-tagging feature enhances retrieval by adding another layer of domain-specific knowledge to the existing dataset.
<p>Difference between auto-tag and auto-keyword:</p>
<ul>
  <li>A tag knowledge base is a user-defined close set, whereas keywords extracted by the LLM can be regarded as an open set.</li>
  <li>You must upload tag sets in specified formats before running the auto-tag feature.</li>
  <li>The auto-keyword feature is dependent on the LLM and consumes a significant number of tokens.</li>
</ul>
      `,
      topnTags: 'Top-N Tags',
      tags: 'Tags',
      addTag: 'Add tag',
      useGraphRag: 'Knowledge graph',
      useGraphRagTip:
        'Construct a knowledge graph over file chunks of the current knowledge base to enhance multi-hop question-answering involving nested logic. See https://ragflow.io/docs/dev/construct_knowledge_graph for details.',
      graphRagMethod: 'Method',
      graphRagMethodTip: `
      Light: (Default) Use prompts provided by github.com/HKUDS/LightRAG to extract entities and relationships. This option consumes fewer tokens, less memory, and fewer computational resources.</br>
      General: Use prompts provided by github.com/microsoft/graphrag to extract entities and relationships`,
      resolution: 'Entity resolution',
      resolutionTip: `An entity deduplication switch. When enabled, the LLM will combine similar entities - e.g., '2025' and 'the year of 2025', or 'IT' and 'Information Technology' - to construct a more accurate graph`,
      community: 'Community reports',
      communityTip:
        'In a knowledge graph, a community is a cluster of entities linked by relationships. You can have the LLM generate an abstract for each community, known as a community report. See here for more information: https://www.microsoft.com/en-us/research/blog/graphrag-improving-global-search-via-dynamic-community-selection/',
      theDocumentBeingParsedCannotBeDeleted:
        'The document being parsed cannot be deleted',
    },
    chunk: {
      chunk: 'Chunk',
      bulk: 'Bulk',
      selectAll: 'Select All',
      enabledSelected: 'Enable selected',
      disabledSelected: 'Disable selected',
      deleteSelected: 'Delete selected',
      search: 'Search',
      all: 'All',
      enabled: 'Enabled',
      disabled: 'Disabled',
      keyword: 'Keyword',
      function: 'Function',
      chunkMessage: 'Please input value!',
      full: 'Full text',
      ellipse: 'Ellipse',
      graph: 'Knowledge graph',
      mind: 'Mind map',
      question: 'Question',
      questionTip: `If there are given questions, the embedding of the chunk will be based on them.`,
      chunkResult: 'Chunk Result',
      chunkResultTip: `View the chunked segments used for embedding and retrieval.`,
      enable: 'Enable',
      disable: 'Disable',
      delete: 'Delete',
    },
    chat: {
      messagePlaceholder: 'Type your message here...',
      exit: 'Exit',
      multipleModels: 'Multiple Models',
      applyModelConfigs: 'Apply model configs',
      conversations: 'Conversations',
      chatApps: 'Chat Apps',
      newConversation: 'New conversation',
      createAssistant: 'Create an Assistant',
      assistantSetting: 'Assistant settings',
      promptEngine: 'Prompt engine',
      modelSetting: 'Model settings',
      chat: 'Chat',
      newChat: 'New chat',
      send: 'Send',
      sendPlaceholder: 'Message the assistant...',
      chatConfiguration: 'Chat Configuration',
      chatConfigurationDescription:
        ' Set up a chat assistant for your selected datasets (knowledge bases) here! 💕',
      assistantName: 'Assistant name',
      assistantNameMessage: 'Assistant name is required',
      namePlaceholder: 'e.g. Resume Jarvis',
      assistantAvatar: 'Assistant avatar',
      language: 'Language',
      emptyResponse: 'Empty response',
      emptyResponseTip: `Set this as a response if no results are retrieved from the knowledge bases for your query, or leave this field blank to allow the LLM to improvise when nothing is found.`,
      emptyResponseMessage: `Empty response will be triggered when nothing relevant is retrieved from knowledge bases. You must clear the 'Empty response' field if no knowledge base is selected.`,
      setAnOpener: 'Opening greeting',
      setAnOpenerInitial: `Hi! I'm your assistant. What can I do for you?`,
      setAnOpenerTip: 'Set an opening greeting for users.',
      knowledgeBases: 'Datasets',
      knowledgeBasesMessage: 'Please select',
      knowledgeBasesTip:
        'Select the datasets to associate with this chat assistant. An empty knowledge base will not appear in the dropdown list.',
      system: 'System prompt',
      systemInitialValue: `You are an intelligent assistant. Please summarize the content of the knowledge base to answer the question. Please list the data in the knowledge base and answer in detail. When all knowledge base content is irrelevant to the question, your answer must include the sentence "The answer you are looking for is not found in the knowledge base!" Answers need to consider chat history.
      Here is the knowledge base:
      {knowledge}
      The above is the knowledge base.`,
      systemMessage: 'Please input!',
      systemTip:
        'Your prompts or instructions for the LLM, including but not limited to its role, the desired length, tone, and language of its answers. If your model has native support for reasoning, you can add //no_thinking add the prompt to stop reasoning.',
      topN: 'Top N',
      topNTip: `Not all chunks with similarity score above the 'similarity threshold' will be sent to the LLM. This selects 'Top N' chunks from the retrieved ones.`,
      variable: 'Variable',
      variableTip: `Used together with RAGFlow's chat assistant management APIs, variables can help develop more flexible system prompt strategies. The defined variables will be used by 'System prompt' as part of the prompts for the LLM. {knowledge} is a reserved special variable representing chunks retrieved from specified knowledge base(s), and all variables should be enclosed in curly braces {} in the 'System prompt'. See https://ragflow.io/docs/dev/set_chat_variables for details.`,
      add: 'Add',
      key: 'Key',
      optional: 'Optional',
      operation: 'Operation',
      model: 'Model',
      modelTip: 'Large language chat model',
      modelMessage: 'Please select!',
      modelEnabledTools: 'Enabled tools',
      modelEnabledToolsTip:
        'Please select one or more tools for the chat model to use. It takes no effect for models not supporting tool call.',
      freedom: 'Creativity',
      improvise: 'Improvise',
      precise: 'Precise',
      balance: 'Balance',
      custom: 'Custom',
      freedomTip: `A shortcut to 'Temperature', 'Top P', 'Presence penalty', and 'Frequency penalty' settings, indicating the freedom level of the model. This parameter has three options: Select 'Improvise' to produce more creative responses; select 'Precise' (default) to produce more conservative responses; 'Balance' is a middle ground between 'Improvise' and 'Precise'.`,
      temperature: 'Temperature',
      temperatureMessage: 'Temperature is required',
      temperatureTip: `This parameter controls the randomness of the model's predictions. A lower temperature results in more conservative responses, while a higher temperature yields more creative and diverse responses.`,
      topP: 'Top P',
      topPMessage: 'Top P is required',
      topPTip:
        'Also known as "nucleus sampling", this parameter sets a threshold for selecting a smaller set of the most likely words to sample from, cutting off the less probable ones.',
      presencePenalty: 'Presence penalty',
      presencePenaltyMessage: 'Presence penalty is required',
      presencePenaltyTip:
        'This discourages the model from repeating the same information by penalizing words that have already appeared in the conversation.',
      frequencyPenalty: 'Frequency penalty',
      frequencyPenaltyMessage: 'Frequency penalty is required',
      frequencyPenaltyTip:
        'Similar to the presence penalty, this reduces the model’s tendency to repeat the same words frequently.',
      maxTokens: 'Max tokens',
      maxTokensMessage: 'Max tokens is required',
      maxTokensTip: `This sets the maximum length of the model's output, measured in the number of tokens (words or pieces of words). Defaults to 512. If disabled, you lift the maximum token limit, allowing the model to determine the number of tokens in its responses.`,
      maxTokensInvalidMessage: 'Please enter a valid number for Max Tokens.',
      maxTokensMinMessage: 'Max Tokens cannot be less than 0.',
      quote: 'Show quote',
      quoteTip: 'Whether to display the original text as a reference.',
      selfRag: 'Self-RAG',
      selfRagTip: 'Please refer to: https://huggingface.co/papers/2310.11511',
      overview: 'Chat ID',
      pv: 'Number of messages',
      uv: 'Active user number',
      speed: 'Token output speed',
      tokens: 'Consume the token number',
      round: 'Session Interaction Number',
      thumbUp: 'customer satisfaction',
      preview: 'Preview',
      embedded: 'Embedded',
      serviceApiEndpoint: 'Service API Endpoint',
      apiKey: 'API KEY',
      apiReference: 'API Documents',
      dateRange: 'Date Range:',
      backendServiceApi: 'API Server',
      createNewKey: 'Create new key',
      created: 'Created',
      action: 'Action',
      embedModalTitle: 'Embed into webpage',
      comingSoon: 'Coming soon',
      fullScreenTitle: 'Full Embed',
      fullScreenDescription:
        'Embed the following iframe into your website at the desired location',
      partialTitle: 'Partial Embed',
      extensionTitle: 'Chrome Extension',
      tokenError: 'Please create API key first.',
      betaError:
        'Please acquire a RAGFlow API key from the System Settings page first.',
      searching: 'Searching...',
      parsing: 'Parsing',
      uploading: 'Uploading',
      uploadFailed: 'Upload failed',
      regenerate: 'Regenerate',
      read: 'Read content',
      tts: 'Text to speech',
      ttsTip:
        'Ensure you select a TTS model on the Settings page before enabling this toggle to play text as audio.',
      relatedQuestion: 'Related question',
      answerTitle: 'R',
      multiTurn: 'Multi-turn optimization',
      multiTurnTip:
        'This optimizes user queries using context in a multi-round conversation. When enabled, it will consume additional LLM tokens.',
      howUseId: 'How to use chat ID?',
      description: 'Description of assistant',
      descriptionPlaceholder: 'e.g. A chat assistant for resume.',
      useKnowledgeGraph: 'Use knowledge graph',
      useKnowledgeGraphTip:
        'Whether to use knowledge graph(s) in the specified knowledge base(s) during retrieval for multi-hop question answering. When enabled, this would involve iterative searches across entity, relationship, and community report chunks, greatly increasing retrieval time.',
      keyword: 'Keyword analysis',
      keywordTip: `Use LLM to analyze user's questions, extract keywords which will be emphasize during the relevance computation. Works well with lengthy queries but will increase response time.`,
      languageTip:
        'Allows sentence rewriting with the specified language or defaults to the latest question if not selected.',
      avatarHidden: 'Hide avatar',
      locale: 'Locale',
      selectLanguage: 'Select a language',
      reasoning: 'Reasoning',
      reasoningTip: `Whether to enable a reasoning workflow during question answering, as seen in models like Deepseek-R1 or OpenAI o1. When enabled, this allows the model to access external knowledge and tackle complex questions in a step-by-step manner, leveraging techniques like chain-of-thought reasoning. This approach enhances the model's ability to provide accurate responses by breaking down problems into manageable steps, improving performance on tasks that require logical reasoning and multi-step thinking.`,
      tavilyApiKeyTip:
        'If an API key is correctly set here, Tavily-based web searches will be used to supplement knowledge base retrieval.',
      tavilyApiKeyMessage: 'Please enter your Tavily API Key',
      tavilyApiKeyHelp: 'How to get it?',
      crossLanguage: 'Cross-language search',
      crossLanguageTip: `Select one or more languages for cross‑language search. If no language is selected, the system searches with the original query.`,
      createChat: 'Create chat',
      metadata: 'Meta Data',
      metadataTip:
        'Metadata filtering is the process of using metadata attributes (such as tags, categories, or access permissions) to refine and control the retrieval of relevant information within a system.',
      conditions: 'Conditions',
      addCondition: 'Add Condition',
      meta: {
        disabled: 'Disabled',
        auto: 'Automatic',
        manual: 'Manual',
      },
      cancel: 'Cancel',
      chatSetting: 'Chat setting',
      tocEnhance: 'TOC enhance',
      tocEnhanceTip: ` During the parsing of the document, table of contents information was generated (see the 'Enable Table of Contents Extraction' option in the General method). This allows the large model to return table of contents items relevant to the user's query, thereby using these items to retrieve related chunks and apply weighting to these chunks during the sorting process. This approach is derived from mimicking the behavioral logic of how humans search for knowledge in books.`,
    },
    setting: {
      cropTip:
        'Drag the selection area to choose the cropping position of the image, and scroll to zoom in/out',
      cropImage: 'Crop image',
      selectModelPlaceholder: 'Select model',
      configureModelTitle: 'Configure model',
      confluenceIsCloudTip:
        'Check if this is a Confluence Cloud instance, uncheck for Confluence Server/Data Center',
      confluenceWikiBaseUrlTip:
        'The base URL of your Confluence instance (e.g., https://your-domain.atlassian.net/wiki)',
      s3PrefixTip: `Specify the folder path within your S3 bucket to fetch files from. 
Example: general/v2/`,
      addDataSourceModalTital: 'Create your {{name}} connector',
      deleteSourceModalTitle: 'Delete data source',
      deleteSourceModalContent: `
      <p>Are you sure you want to delete this data source link?</p>`,
      deleteSourceModalConfirmText: 'Comfirm',
      errorMsg: 'Error message',
      newDocs: 'New Docs',
      timeStarted: 'Time started',
      log: 'Log',
      confluenceDescription:
        'Integrate your Confluence workspace to search documentation.',
      s3Description:
        'Connect to your AWS S3 bucket to import and sync stored files.',
      discordDescription:
        'Link your Discord server to access and analyze chat data.',
      notionDescription:
        'Sync pages and databases from Notion for knowledge retrieval.',
      google_driveDescription:
        'Connect your Google Drive via OAuth and sync specific folders or drives.',
      google_driveTokenTip:
        'Upload the OAuth token JSON generated from the OAuth helper or Google Cloud Console. You may also upload a client_secret JSON from an "installed" or "web" application. If this is your first sync, a browser window will open to complete the OAuth consent. If the JSON already contains a refresh token, it will be reused automatically.',
      google_drivePrimaryAdminTip:
        'Email address that has access to the Drive content being synced.',
      google_driveMyDriveEmailsTip:
        'Comma-separated emails whose “My Drive” contents should be indexed (include the primary admin).',
      google_driveSharedFoldersTip:
        'Comma-separated Google Drive folder links to crawl.',
      availableSourcesDescription: 'Select a data source to add',
      availableSources: 'Available sources',
      datasourceDescription: 'Manage your data source and connections',
      save: 'Save',
      search: 'Search',
      availableModels: 'Available models',
      profile: 'Profile',
      avatar: 'Avatar',
      avatarTip: 'This will be displayed on your profile.',
      profileDescription: 'Update your photo and personal details here.',
      maxTokens: 'Max Tokens',
      maxTokensMessage: 'Max Tokens is required',
      maxTokensTip: `This sets the maximum length of the model's output, measured in the number of tokens (words or pieces of words). Defaults to 512. If disabled, you lift the maximum token limit, allowing the model to determine the number of tokens in its responses.`,
      maxTokensInvalidMessage: 'Please enter a valid number for Max Tokens.',
      maxTokensMinMessage: 'Max Tokens cannot be less than 0.',
      password: 'Password',
      passwordDescription:
        'Please enter your current password to change your password.',
      model: 'Model providers',
      systemModelDescription: 'Please complete these settings before beginning',
      dataSources: 'Data sources',
      team: 'Team',
      system: 'System',
      logout: 'Log out',
      api: 'API',
      username: 'Name',
      usernameMessage: 'Please input your username!',
      photo: 'Your photo',
      photoDescription: 'This will be displayed on your profile.',
      colorSchema: 'Color schema',
      colorSchemaMessage: 'Please select your color schema!',
      colorSchemaPlaceholder: 'select your color schema',
      bright: 'Bright',
      dark: 'Dark',
      timezone: 'Time zone',
      timezoneMessage: 'Please input your timezone!',
      timezonePlaceholder: 'select your timezone',
      email: 'Email',
      emailDescription: 'Once registered, E-mail cannot be changed.',
      currentPassword: 'Current password',
      currentPasswordMessage: 'Please input your password!',
      newPassword: 'New password',
      changePassword: 'Change Password',
      newPasswordMessage: 'Please input your password!',
      newPasswordDescription:
        'Your new password must be more than 8 characters.',
      confirmPassword: 'Confirm new password',
      confirmPasswordMessage: 'Please confirm your password!',
      confirmPasswordNonMatchMessage:
        'The new password that you entered do not match!',
      cancel: 'Cancel',
      addedModels: 'Added models',
      modelsToBeAdded: 'Models to be added',
      addTheModel: 'Add',
      apiKey: 'API-Key',
      apiKeyMessage:
        'Please enter the API key (for locally deployed model,ignore this).',
      apiKeyTip:
        'The API key can be obtained by registering the corresponding LLM supplier.',
      showMoreModels: 'View models',
      hideModels: 'Hide models',
      baseUrl: 'Base-Url',
      baseUrlTip:
        'If your API key is from OpenAI, just ignore it. Any other intermediate providers will give this base url with the API key.',
      tongyiBaseUrlTip:
        'For Chinese users, no need to fill in or use https://dashscope.aliyuncs.com/compatible-mode/v1. For international users, use https://dashscope-intl.aliyuncs.com/compatible-mode/v1',
      tongyiBaseUrlPlaceholder: '(International users only, please see tip)',
      modify: 'Modify',
      systemModelSettings: 'Set default models',
      chatModel: 'LLM',
      chatModelTip: 'The default LLM for each newly created knowledge base.',
      embeddingModel: 'Embedding',
      embeddingModelTip:
        'The default embedding model for each newly created knowledge base. If you cannot find an embedding model from the dropdown, check if you are using RAGFlow slim edition (which does not include embedding models) or check https://ragflow.io/docs/dev/supported_models to see if your model provider supports this model.',
      img2txtModel: 'VLM',
      img2txtModelTip:
        'The default VLM for each newly created knowledge base. It describes a picture or video. If you cannot find a model from the dropdown, check https://ragflow.io/docs/dev/supported_models to see if your model provider supports this model.',
      sequence2txtModel: 'ASR',
      sequence2txtModelTip:
        'The default ASR model for each newly created knowledgebase. Use this model to translate voices to corresponding text.',
      rerankModel: 'Rerank',
      rerankModelTip: `The default rerank model for reranking chunks. If you cannot find a model from the dropdown, check https://ragflow.io/docs/dev/supported_models to see if your model provider supports this model.`,
      ttsModel: 'TTS',
      ttsModelTip:
        'The default text-to-speech model. If you cannot find a model from the dropdown, check https://ragflow.io/docs/dev/supported_models to see if your model provider supports this model.',
      workspace: 'workspace',
      upgrade: 'Upgrade',
      addLlmTitle: 'Add LLM',
      editLlmTitle: 'Edit {{name}} Model',
      editModel: 'Edit Model',
      modelName: 'Model name',
      modelID: 'Model ID',
      modelUid: 'Model UID',
      modelNameMessage: 'Please input your model name!',
      modelType: 'Model type',
      modelTypeMessage: 'Please input your model type!',
      addLlmBaseUrl: 'Base url',
      baseUrlNameMessage: 'Please input your base url!',
      vision: 'Does it support Vision?',
      ollamaLink: 'How to integrate {{name}}',
      FishAudioLink: 'How to use FishAudio',
      TencentCloudLink: 'How to use TencentCloud ASR',
      volcModelNameMessage: 'Please input your model name!',
      addEndpointID: 'EndpointID of the model',
      endpointIDMessage: 'Please input your EndpointID of the model',
      addArkApiKey: 'VOLC ARK_API_KEY',
      ArkApiKeyMessage: 'Please input your ARK_API_KEY',
      bedrockModelNameMessage: 'Please input your model name!',
      addBedrockEngineAK: 'ACCESS KEY',
      bedrockAKMessage: 'Please input your ACCESS KEY',
      addBedrockSK: 'SECRET KEY',
      bedrockSKMessage: 'Please input your SECRET KEY',
      bedrockRegion: 'AWS Region',
      bedrockRegionMessage: 'Please select!',
      'us-east-2': 'US East (Ohio)',
      'us-east-1': 'US East (N. Virginia)',
      'us-west-1': 'US West (N. California)',
      'us-west-2': 'US West (Oregon)',
      'af-south-1': 'Africa (Cape Town)',
      'ap-east-1': 'Asia Pacific (Hong Kong)',
      'ap-south-2': 'Asia Pacific (Hyderabad)',
      'ap-southeast-3': 'Asia Pacific (Jakarta)',
      'ap-southeast-5': 'Asia Pacific (Malaysia)',
      'ap-southeast-4': 'Asia Pacific (Melbourne)',
      'ap-south-1': 'Asia Pacific (Mumbai)',
      'ap-northeast-3': 'Asia Pacific (Osaka)',
      'ap-northeast-2': 'Asia Pacific (Seoul)',
      'ap-southeast-1': 'Asia Pacific (Singapore)',
      'ap-southeast-2': 'Asia Pacific (Sydney)',
      'ap-east-2': 'Asia Pacific (Taipei)',
      'ap-southeast-7': 'Asia Pacific (Thailand)',
      'ap-northeast-1': 'Asia Pacific (Tokyo)',
      'ca-central-1': 'Canada (Central)',
      'ca-west-1': 'Canada West (Calgary)',
      'eu-central-1': 'Europe (Frankfurt)',
      'eu-west-1': 'Europe (Ireland)'

[... Content truncated for brevity ...]
```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/locales/en.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 2082 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (1)

- `across()`: Function definition

## Code Structure Analysis

- Total lines: 2082
- Blank lines: 29 (1.4%)
- Comment lines: ~2 (0.1%)
- Code lines: ~2051


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/locales`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 97 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **SQL Operations**: Ensure parameterized queries to prevent SQL injection
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

- Other files in `web/src/locales/` directory
- Potential test file: `test_en.ts`

## Keywords

ACCESS, AISummary, AIs, AND, API, APIKey, APIPassword, APISecret, APIs, APP, ARK_API_KEY, ARTICLE, ASR, AWS, Academic, Accept, Account, Accuracy, Act, Action, Actions, Active, Add, Added, Admin, Advanced, Aerospace, Africa, After, Agent, Agents, Air, AkShare, Alex, Alive, All, Allows, Already, Also, America, Anonymous, Answer, Answers, Any, Anyway, App, Append, Applicable, Application, Apply...

---
*Generated by RAGFlow Repository Documentation Generator*
