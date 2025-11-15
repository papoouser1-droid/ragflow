# Documentation: web/src/locales/zh.ts

## File Metadata

- **Path**: `web/src/locales/zh.ts`
- **Size**: 86423 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/locales/zh.ts`.

## Original Source Code

```ts
export default {
  translation: {
    common: {
      back: '返回',
      noResults: '无结果。',
      selectPlaceholder: '请选择',
      selectAll: '全选',
      delete: '删除',
      deleteModalTitle: '确定删除吗?',
      ok: '确认',
      cancel: '取消',
      yes: '是',
      no: '否',
      total: '总共',
      rename: '重命名',
      name: '名称',
      save: '保存',
      namePlaceholder: '请输入名称',
      next: '下一步',
      create: '创建',
      edit: '编辑',
      upload: '上传',
      english: '英文',
      portugueseBr: '葡萄牙语 (巴西)',
      chinese: '简体中文',
      traditionalChinese: '繁体中文',
      language: '语言',
      languageMessage: '请输入语言',
      languagePlaceholder: '请选择语言',
      copy: '复制',
      copied: '复制成功',
      comingSoon: '即将推出',
      download: '下载',
      close: '关闭',
      preview: '预览',
      move: '移动',
      warn: '提醒',
      action: '操作',
      s: '秒',
      pleaseSelect: '请选择',
      pleaseInput: '请输入',
      submit: '提交',
      clear: '清空',
      embedIntoSite: '嵌入网站',
      previousPage: '上一页',
      nextPage: '下一页',
      add: '添加',
      remove: '移除',
      search: '搜索',
      noDataFound: '没有找到数据。',
      noData: '暂无数据',
      promptPlaceholder: '请输入或使用 / 快速插入变量。',
    },
    login: {
      loginTitle: '登录账户',
      signUpTitle: '创建账户',
      login: '登录',
      signUp: '注册',
      loginDescription: '很高兴再次见到您！',
      registerDescription: '很高兴您加入！',
      emailLabel: '邮箱',
      emailPlaceholder: '请输入邮箱地址',
      passwordLabel: '密码',
      passwordPlaceholder: '请输入密码',
      rememberMe: '记住我',
      signInTip: '没有帐户？',
      signUpTip: '已经有帐户？',
      nicknameLabel: '名称',
      nicknamePlaceholder: '请输入名称',
      register: '创建账户',
      continue: '继续',
      title: 'A leading RAG engine for LLM context',
      start: '立即开始',
      description:
        '免费注册以探索顶级 RAG 技术。 创建知识库和人工智能来增强您的业务',
      review: '来自 500 多条评论',
    },
    header: {
      knowledgeBase: '知识库',
      chat: '聊天',
      register: '注册',
      signin: '登录',
      home: '首页',
      setting: '用户设置',
      logout: '登出',
      fileManager: '文件管理',
      flow: '智能体',
      search: '搜索',
      welcome: '欢迎来到',
      dataset: '知识库',
    },
    knowledgeList: {
      welcome: '欢迎回来',
      description: '今天我们要使用哪个知识库？',
      createKnowledgeBase: '创建知识库',
      name: '名称',
      namePlaceholder: '请输入名称',
      doc: '文档',
      searchKnowledgePlaceholder: '搜索',
      noMoreData: '没有更多数据了',
    },
    knowledgeDetails: {
      localUpload: '本地上传',
      fileSize: '文件大小',
      fileType: '文件类型',
      uploadedBy: '创建者',
      notGenerated: '未生成',
      generatedOn: '生成于',
      subbarFiles: '文件列表',
      generate: '生成',
      raptor: 'RAPTOR',
      processingType: '处理类型',
      dataPipeline: '数据管道',
      operations: '操作',
      taskId: '任务ID',
      duration: '耗时',
      details: '详情',
      status: '状态',
      task: '任务',
      startDate: '开始时间',
      source: '来源',
      fileName: '文件名',
      datasetLogs: '数据集',
      fileLogs: '文件',
      overview: '日志',
      success: '成功',
      failed: '失败',
      completed: '已完成',
      datasetLog: '知识库日志',
      created: '创建于',
      learnMore: '内置pipeline简介',
      general: '通用',
      chunkMethodTab: '切片方法',
      testResults: '测试结果',
      testSetting: '测试设置',
      retrievalTesting: '知识检索测试',
      retrievalTestingDescription:
        '进行检索测试，检查 RAGFlow 是否能够为大语言模型（LLM）恢复预期的内容。',
      Parse: '解析',
      dataset: '知识库',
      testing: '检索测试',
      configuration: '配置',
      knowledgeGraph: '知识图谱',
      files: '个文件',
      name: '名称',
      namePlaceholder: '请输入名称',
      doc: '文档',
      datasetDescription: '解析成功后才能问答哦。',
      addFile: '新增文件',
      searchFiles: '搜索文件',
      localFiles: '本地文件',
      emptyFiles: '新建空文件',
      webCrawl: '网页抓取',
      chunkNumber: '分块数',
      uploadDate: '上传日期',
      chunkMethod: '切片方法',
      enabled: '启用',
      disabled: '禁用',
      action: '动作',
      parsingStatus: '解析状态',
      parsingStatusTip:
        '文本解析的时间取决于诸多因素。如果开启了知识图谱、RAPTOR、自动问题提取、自动关键词提取等功能，时间会更长。如果解析进度条长时间不更新，也可以参考这两条 FAQ：https://ragflow.io/docs/dev/faq#why-does-my-document-parsing-stall-at-under-one-percent。',
      processBeginAt: '开始于',
      processDuration: '持续时间',
      progressMsg: '进度',
      noTestResultsForRuned: '未找到相关结果，请尝试调整查询语句或参数',
      noTestResultsForNotRuned: '尚未运行测试，结果会显示在这里',
      testingDescription:
        '请完成召回测试：确保你的配置可以从数据库召回正确的文本块。如果你调整了这里的默认设置，比如关键词相似度权重，请注意这里的改动不会被自动保存。请务必在聊天助手设置或者召回算子设置处同步更新相关设置。',
      similarityThreshold: '相似度阈值',
      similarityThresholdTip:
        '我们使用混合相似度得分来评估两行文本之间的距离。 它是加权关键词相似度和向量余弦相似度。 如果查询和块之间的相似度小于此阈值，则该块将被过滤掉。默认设置为 0.2，也就是说文本块的混合相似度得分至少 20 才会被召回。',
      vectorSimilarityWeight: '向量相似度权重',
      vectorSimilarityWeightTip:
        '我们使用混合相似性评分来评估两行文本之间的距离。它是加权关键字相似性和矢量余弦相似性或rerank得分（0〜1）。两个权重的总和为1.0。',
      keywordSimilarityWeight: '关键词相似度权重',
      keywordSimilarityWeightTip:
        '我们使用混合相似性评分来评估两行文本之间的距离。它是加权关键字相似性和矢量余弦相似性或rerank得分（0〜1）。两个权重的总和为1.0。',
      testText: '测试文本',
      testTextPlaceholder: '请输入您的问题！',
      testingLabel: '运行',
      similarity: '混合相似度',
      termSimilarity: '关键词相似度',
      vectorSimilarity: '向量相似度',
      hits: '命中数',
      view: '看法',
      filesSelected: '选定的文件',
      upload: '上传',
      run: '解析',
      runningStatus0: '未解析',
      runningStatus1: '解析中',
      runningStatus2: '取消',
      runningStatus3: '成功',
      runningStatus4: '失败',
      pageRanges: '页码范围',
      pageRangesTip:
        '页码范围：定义需要解析的页面范围。 不包含在这些范围内的页面将被忽略。',
      fromPlaceholder: '从',
      fromMessage: '缺少起始页码',
      toPlaceholder: '到',
      toMessage: '缺少结束页码（不包含）',
      layoutRecognize: 'PDF解析器',
      layoutRecognizeTip:
        '使用视觉模型进行 PDF 布局分析，以更好地识别文档结构，找到标题、文本块、图像和表格的位置。 如果选择 Naive 选项，则只能获取 PDF 的纯文本。请注意该功能只适用于 PDF 文档，对其他文档不生效。欲了解更多信息，请参阅 https://ragflow.io/docs/dev/select_pdf_parser。',
      taskPageSize: '任务页面大小',
      taskPageSizeMessage: '请输入您的任务页面大小！',
      taskPageSizeTip: `如果使用布局识别，PDF 文件将被分成连续的组。 布局分析将在组之间并行执行，以提高处理速度。 “任务页面大小”决定组的大小。 页面大小越大，将页面之间的连续文本分割成不同块的机会就越低。`,
      addPage: '新增页面',
      greaterThan: '当前值必须大于起始值！',
      greaterThanPrevious: '当前值必须大于之前的值！',
      selectFiles: '选择文件',
      changeSpecificCategory: '更改特定类别',
      uploadTitle: '点击或拖拽文件至此区域即可上传',
      uploadDescription:
        '支持单次或批量上传。本地部署的单次上传文件总大小上限为 1GB，单次批量上传文件数不超过 32，单个账户不限文件数量。对于 demo.ragflow.io：每次上传的总文件大小限制为 10MB，每个文件不得超过 10MB，每个账户最多可上传 128 个文件。严禁上传违禁文件。',
      chunk: '解析块',
      bulk: '批量',
      cancel: '取消',
      close: '关闭',
      rerankModel: 'Rerank模型',
      rerankPlaceholder: '请选择',
      rerankTip: `非必选项：若不选择 rerank 模型，系统将默认采用关键词相似度与向量余弦相似度相结合的混合查询方式；如果设置了 rerank 模型，则混合查询中的向量相似度部分将被 rerank 打分替代。请注意：采用 rerank 模型会非常耗时。如需选用 rerank 模型，建议使用 SaaS 的 rerank 模型服务；如果你倾向使用本地部署的 rerank 模型，请务必确保你使用 docker-compose-gpu.yml 启动 RAGFlow。`,
      topK: 'Top-K',
      topKTip: `与 Rerank 模型配合使用，用于设置传给 Rerank 模型的文本块数量。`,
      delimiter: `文本分段标识符`,
      delimiterTip:
        '支持多字符作为分隔符，多字符用两个反引号 \\`\\` 分隔符包裹。若配置成：\\n`##`; 系统将首先使用换行符、两个#号以及分号先对文本进行分割，随后再对分得的小文本块按照「建议文本块大小」设定的大小进行拼装。在设置文本分段标识符前请确保理解上述文本分段切片机制。',
      html4excel: '表格转HTML',
      html4excelTip: `与 General 切片方法配合使用。未开启状态下，表格文件（XLSX、XLS（Excel 97-2003））会按行解析为键值对。开启后，表格文件会被解析为 HTML 表格。若原始表格超过 12 行，系统会自动按每 12 行拆分为多个 HTML 表格。欲了解更多详情，请参阅 https://ragflow.io/docs/dev/enable_excel2html。`,
      autoKeywords: '自动关键词提取',
      autoKeywordsTip: `自动为每个文本块中提取 N 个关键词，用以提升查询精度。请注意：该功能采用“系统模型设置”中设置的默认聊天模型提取关键词，因此也会产生更多 Token 消耗。另外，你也可以手动更新生成的关键词。详情请见 https://ragflow.io/docs/dev/autokeyword_autoquestion。`,
      autoQuestions: '自动问题提取',
      autoQuestionsTip: `利用“系统模型设置”中设置的 chat model 对知识库的每个文本块提取 N 个问题以提高其排名得分。请注意，开启后将消耗额外的 token。您可以在块列表中查看、编辑结果。如果自动问题提取发生错误，不会妨碍整个分块过程，只会将空结果添加到原始文本块。详情请见 https://ragflow.io/docs/dev/autokeyword_autoquestion。`,
      redo: '是否清空已有 {{chunkNum}}个 chunk？',
      setMetaData: '设置元数据',
      pleaseInputJson: '请输入JSON',
      documentMetaTips: `<p>元数据为 Json 格式（不可搜索）。如果提示中包含此文档的任何块，它将被添加到 LLM 的提示中。</p>
<p>示例：</p>
<b>元数据为：</b><br>
<code>
{
    "作者": "Alex Dowson",
    "日期": "2024-11-12"
}
</code><br>
<b>提示将为：</b><br>
<p>文档：the_name_of_document</p>
<p>作者：Alex Dowson</p>
<p>日期：2024-11-12</p>
<p>相关片段如下：</p>
<ul>
<li> 这是块内容....</li>
<li> 这是块内容....</li>
</ul>
`,
      metaData: '元数据',
      deleteDocumentConfirmContent:
        '该文档与知识图谱相关联。删除后，相关节点和关系信息将被删除，但图不会立即更新。更新图动作是在解析承载知识图谱提取任务的新文档的过程中执行的。',
      plainText: 'Naive',
      reRankModelWaring: '重排序模型非常耗时。',
      theDocumentBeingParsedCannotBeDeleted: '正在解析的文档不能被删除',
    },
    knowledgeConfiguration: {
      generationScopeTip: '选择 RAPTOR 的生成范围：整个知识库或单个文件。',
      generationScope: '生成范围',
      scopeSingleFile: '单文件',
      scopeDataset: '整库',

      autoParse: '自动解析',
      rebuildTip: '从所有已关联的数据源重新下载文件并再次解析。',
      baseInfo: '基础信息',
      globalIndex: '全局索引',
      dataSource: '数据源',
      linkSourceSetTip: '管理与此数据集的数据源链接',
      linkDataSource: '链接数据源',
      tocExtractionTip:
        '对于已有的chunk生成层级结构的目录信息（每个文件一个目录）。在查询时，激活`目录增强`后，系统会用大模型去判断用户问题和哪些目录项相关，从而找到相关的chunk。',
      deleteGenerateModalContent: `
        <p>删除生成的 <strong class='text-text-primary'>{{type}}</strong> 结果
          将从此数据集中移除所有派生实体和关系。
          您的原始文件将保持不变。<p>
          <br/>
          是否要继续？
      `,
      extractRaptor: '从文档中提取RAPTOR',
      extractKnowledgeGraph: '从文档中提取知识图谱',
      filterPlaceholder: '请输入',
      fileFilterTip: '',
      fileFilter: '正则匹配表达式',
      setDefaultTip: '',
      setDefault: '设置默认',
      eidtLinkDataPipeline: '编辑pipeline',
      linkPipelineSetTip: '管理与此数据集的数据管道链接',
      default: '默认',
      dataPipeline: 'Ingestion pipeline',
      linkDataPipeline: '关联pipeline',
      enableAutoGenerate: '是否启用自动生成',
      teamPlaceholder: '请选择团队',
      dataFlowPlaceholder: '请选择pipeline',
      buildItFromScratch: '去Scratch构建',
      dataFlow: 'pipeline',
      parseType: '解析方法',
      manualSetup: '选择pipeline',
      builtIn: '内置',
      titleDescription: '在这里更新您的知识库详细信息，尤其是切片方法。',
      name: '知识库名称',
      photo: '知识库图片',
      photoTip: '你可以上传4MB的文件',
      description: '描述',
      language: '文档语言',
      languag

... [Content truncated - file is 57222 bytes] ...

_email: 抄送邮箱(可选)',
      subjectTip: 'subject: 邮件主题(可选)',
      contentTip: 'content: 邮件内容(可选)',
      jsonUploadTypeErrorMessage: '请上传json文件',
      jsonUploadContentErrorMessage: 'json 文件错误',
      iteration: '循环',
      iterationDescription: `该组件负责迭代生成新的内容，对列表对象执行多次步骤直至输出所有结果。`,
      delimiterTip: `该分隔符用于将输入文本分割成几个文本片段，每个文本片段的回显将作为每次迭代的输入项。`,
      delimiterOptions: {
        comma: '逗号',
        lineBreak: '换行',
        tab: '制表符',
        underline: '下划线',
        diagonal: '斜线',
        minus: '连字符',
        semicolon: '分号',
      },
      addCategory: '新增分类',
      categoryName: '分类名称',
      nextStep: '下一步',
      insertVariableTip: `输入 / 插入变量`,
      setting: '设置',
      settings: {
        agentSetting: 'Agent设置',
        title: '标题',
        description: '描述',
        upload: '上传',
        photo: '照片',
        permissions: '权限',
        permissionsTip: '你可以在这里设置团队访问权限。',
        me: '仅限自己',
        team: '团队',
      },
      systemPrompt: '系统提示词',
      userPrompt: '用户提示词',
      prompt: '提示词',
      promptMessage: '提示词是必填项',
      promptTip:
        '系统提示为大模型提供任务描述、规定回复方式，以及设置其他各种要求。系统提示通常与 key （变量）合用，通过变量设置大模型的输入数据。你可以通过斜杠或者 (x) 按钮显示可用的 key。',
      knowledgeBasesTip: '选择关联的知识库，或者在下方选择包含知识库ID的变量。',
      knowledgeBaseVars: '知识库变量',
      code: '代码',
      codeDescription: '它允许开发人员编写自定义 Python 逻辑。',
      dataOperations: '数据操作',
      dataOperationsDescription: '对数据对象执行各种操作。',
      listOperations: '列表操作',
      listOperationsDescription: '对列表对象执行各种操作。',
      variableAssigner: '变量赋值器',
      variableAssignerDescription:
        '此组件对数据对象执行操作，包括提取、筛选和编辑数据中的键和值。',
      variableAggregator: '变量聚合',
      variableAggregatorDescription: `将多路分支的变量聚合为一个变量，以实现下游节点统一配置。
变量聚合节点（原变量赋值节点）是工作流程中的一个关键节点，它负责整合不同分支的输出结果，确保无论哪个分支被执行，其结果都能通过一个统一的变量来引用和访问。这在多分支的情况下非常有用，可将不同分支下相同作用的变量映射为一个输出变量，避免下游节点重复定义。`,
      inputVariables: '输入变量',
      addVariable: '新增变量',
      runningHintText: '正在运行中...🕞',
      openingSwitch: '开场白开关',
      openingCopy: '开场白文案',
      openingSwitchTip: '您的用户将在开始时看到此欢迎消息。',
      modeTip: '模式定义了工作流的启动方式。',
      mode: '模式',
      conversational: '对话式',
      task: '任务',
      beginInputTip: '通过定义输入参数，此内容可以被后续流程中的其他组件访问。',
      query: '查询变量',
      queryTip: '选择您想要使用的变量',
      agent: '智能体',
      addAgent: '添加智能体',
      agentDescription: '构建具备推理、工具调用和多智能体协同的智能体组件。',
      maxRecords: '最大记录数',
      createAgent: '智能体流程',
      stringTransform: '文本处理',
      userFillUp: '等待输入',
      userFillUpDescription: `此组件会暂停当前的流程并等待用户发送消息，接收到消息之后再进行之后的流程。`,

      codeExec: '代码',
      tavilySearch: 'Tavily 搜索',
      tavilySearchDescription: '通过 Tavily 服务搜索结果',
      tavilyExtract: 'Tavily 提取',
      tavilyExtractDescription: 'Tavily 提取',
      log: '日志',
      management: '管理',
      import: '导入',
      export: '导出',
      subject: '主题',
      logTimeline: {
        begin: '准备开始',
        userFillUp: '等你输入',
        agent: '智能体正在思考',
        retrieval: '查找知识',
        message: '回复',
        awaitResponse: '等你输入',
        switch: '选择最佳路线',
        iteration: '批量处理',
        categorize: '信息归类',
        code: '运行小段代码',
        textProcessing: '整理文字',
        tavilySearch: '正在网上搜索',
        tavilyExtract: '读取网页内容',
        exeSQL: '查询数据库',
        google: '正在网上搜索',
        wikipedia: '搜索维基百科',
        googleScholar: '学术检索',
        gitHub: '搜索',
        email: '发送邮件',
        httpRequest: '请求接口',
        wenCai: '查询财务数据',
      },
      sqlStatement: 'SQL 语句',
      sqlStatementTip:
        '在此处编写您的 SQL 查询。您可以使用变量、原始 SQL，或使用变量语法混合使用两者。',
      frameworkPrompts: '框架',
      release: '发布',
      createFromBlank: '从空白创建',
      createFromTemplate: '从模板创建',
      importJsonFile: '导入 JSON 文件',
      chooseAgentType: '选择智能体类型',
      parser: '解析器',
      parserDescription: '从文件中提取原始文本和结构以供下游处理。',
      tokenizer: '分词器',
      tokenizerRequired: '请先添加Tokenizer节点',
      tokenizerDescription:
        '根据所选的搜索方法，将文本转换为所需的数据结构（例如，用于嵌入搜索的向量嵌入）。',
      splitter: '按字符分割',
      splitterDescription:
        '根据分词器长度将文本拆分成块，并带有可选的分隔符和重叠。',
      hierarchicalMergerDescription:
        '使用正则表达式规则按标题层次结构将文档拆分成多个部分，以实现更精细的控制。',
      hierarchicalMerger: '按标题分割',
      extractor: '提取器',
      extractorDescription:
        '使用 LLM 从文档块（例如摘要、分类等）中提取结构化见解。',
      outputFormat: '输出格式',
      fileFormats: '文件类型',
      fields: '字段',
      addParser: '增加解析器',
      hierarchy: '层次结构',
      regularExpressions: '正则表达式',
      overlappedPercent: '重叠百分比（%）',
      searchMethod: '搜索方法',
      searchMethodTip: `决定该数据集启用的搜索方式，可选择全文、向量，或两者兼有。
Tokenizer 会根据所选方式将内容存储为对应的数据结构。`,
      filenameEmbdWeight: '文件名嵌入权重',
      parserMethod: '解析方法',
      systemPromptPlaceholder:
        '请输入用于图像分析的系统提示词，若为空则使用系统缺省值',
      exportJson: '导出 JSON',
      viewResult: '查看结果',
      running: '运行中',
      summary: '增强上下文',
      keywords: '关键词',
      questions: '问题',
      metadata: '元数据',
      fieldName: '结果目的地',
      prompts: {
        system: {
          keywords: `角色
你是一名文本分析员。

任务
从给定的文本内容中提取最重要的关键词/短语。

要求
- 总结文本内容，并给出最重要的5个关键词/短语。
- 关键词必须与给定的文本内容使用相同的语言。
- 关键词之间用英文逗号分隔。
- 仅输出关键词。`,
          questions: `角色
你是一名文本分析员。

任务
针对给定的文本内容提出3个问题。

要求
- 理解并总结文本内容，并提出最重要的3个问题。
- 问题的含义不应重叠。
- 问题应尽可能涵盖文本的主要内容。
- 问题必须与给定的文本内容使用相同的语言。
- 每行一个问题。
- 仅输出问题。`,
          summary: `扮演一个精准的摘要者。你的任务是为提供的内容创建一个简洁且忠实于原文的摘要。

关键说明：
1. 准确性：摘要必须严格基于所提供的信息。请勿引入任何未明确说明的新事实、结论或解释。
2. 语言：摘要必须使用与原文相同的语言。
3. 客观性：不带偏见地呈现要点，保留内容的原始意图和语气。请勿进行编辑。
4. 简洁性：专注于最重要的思想，省略细节和多余的内容。`,
          metadata: `从给定内容中提取重要的结构化信息。仅输出有效的 JSON 字符串，不包含任何附加文本。如果未找到重要的结构化信息，则输出一个空的 JSON 对象：{}。

重要的结构化信息可能包括：姓名、日期、地点、事件、关键事实、数字数据或其他可提取实体。`,
        },
        user: {
          keywords: `文本内容
[在此处插入文本]`,
          questions: `文本内容
[在此处插入文本]`,
          summary: `要总结的文本：
[在此处插入文本]`,
          metadata: `内容：[在此处插入内容]`,
        },
      },
      cancel: '取消',
      filenameEmbeddingWeight: '文件名嵌入权重',
      switchPromptMessage: '提示词将发生变化，请确认是否放弃已有提示词？',
      structuredOutput: {
        configuration: '配置',
        structuredOutput: '结构化输出',
      },
      operations: '操作',
      operationsOptions: {
        selectKeys: '选择键',
        literalEval: '字面值求值',
        combine: '合并',
        filterValues: '筛选值',
        appendOrUpdate: '追加或更新',
        removeKeys: '删除键',
        renameKeys: '重命名键',
      },
      ListOperationsOptions: {
        topN: '取前N项',
        head: '取前第N项',
        tail: '取后第N项',
        sort: '排序',
        filter: '筛选',
        dropDuplicates: '去重',
      },
      sortMethod: '排序方式',
      SortMethodOptions: {
        asc: '升序',
        desc: '降序',
      },
    },
    footer: {
      profile: 'All rights reserved @ React',
    },
    layout: {
      file: 'file',
      knowledge: 'knowledge',
      chat: 'chat',
    },
    llmTools: {
      bad_calculator: {
        name: '计算器',
        description: '用于计算两个数的和的工具（会给出错误答案）',
        params: {
          a: '第一个数',
          b: '第二个数',
        },
      },
    },
    modal: {
      okText: '确认',
      cancelText: '取消',
    },
    mcp: {
      export: '导出',
      import: '导入',
      url: 'URL',
      serverType: '服务器类型',
      addMCP: '添加 MCP',
      editMCP: '编辑 MCP',
      toolsAvailable: '可用的工具',
      mcpServers: 'MCP 服务器',
      customizeTheListOfMcpServers: '自定义 MCP 服务器列表',
      cachedTools: '缓存工具',
      selected: '已选择',
      bulkManage: '批量管理',
      exitBulkManage: '退出批量管理',
    },
    search: {
      searchApps: '搜索',
      createSearch: '创建查询',
      searchGreeting: '今天我能为你做些什么？',
      profile: '隐藏个人资料',
      locale: '语言',
      embedCode: '嵌入代码',
      id: 'ID',
      copySuccess: '复制成功',
      welcomeBack: '欢迎回来',
      searchSettings: '搜索设置',
      name: '姓名',
      avatar: '头像',
      description: '描述',
      datasets: '知识库',
      rerankModel: 'rerank 模型',
      AISummary: 'AI 总结',
      enableWebSearch: '启用网页搜索',
      enableRelatedSearch: '启用相关搜索',
      showQueryMindmap: '显示查询思维导图',
      embedApp: '嵌入网站',
      relatedSearch: '相关搜索',
      descriptionValue: '你是一位智能助手。',
      okText: '保存',
      cancelText: '返回',
      chooseDataset: '请先选择知识库',
    },
    language: {
      english: '英语',
      chinese: '中文',
      spanish: '西班牙语',
      french: '法语',
      german: '德语',
      japanese: '日语',
      korean: '韩语',
      vietnamese: '越南语',
    },
    pagination: {
      total: '总共 {{total}} 条',
      page: '{{page}}条/页',
    },
    dataflowParser: {
      result: '结果',
      parseSummary: '解析摘要',
      parseSummaryTip: '解析器: deepdoc',
      parserMethod: '解析方法',
      outputFormat: '输出格式',
      rerunFromCurrentStep: '从当前步骤重新运行',
      rerunFromCurrentStepTip: '已修改，点击重新运行。',
      confirmRerun: '确认重新运行流程',
      confirmRerunModalContent: `
      <p class="text-sm text-text-disabled font-medium mb-2">
        您即将从 <span class="text-text-secondary">{{step}}</span> 步骤开始重新运行该过程
      </p>
      <p class="text-sm mb-3 text-text-disabled">这将:</p>
      <ul class="list-disc list-inside space-y-1 text-sm text-text-secondary">
        <li>• 从当前步骤开始覆盖现有结果</li>
        <li>• 创建新的日志条目进行跟踪</li>
        <li>• 之前的步骤将保持不变</li>
      </ul>`,
      changeStepModalTitle: '切换步骤警告',
      changeStepModalContent: `
      <p>您目前正在编辑此阶段的结果。</p>
      <p>如果您切换到后续阶段，您的更改将会丢失。</p>
      <p>要保留这些更改，请点击“重新运行”以重新运行当前阶段。</p> `,
      changeStepModalConfirmText: '继续切换',
      changeStepModalCancelText: '取消',
      unlinkPipelineModalTitle: '解绑pipeline',
      unlinkPipelineModalContent: `
      <p>一旦取消链接，该数据集将不再连接到当前数据管道。</p>
      <p>正在解析的文件将继续解析，直到完成。</p>
      <p>尚未解析的文件将不再被处理。</p> <br/>
      <p>你确定要继续吗?</p> `,
      unlinkPipelineModalConfirmText: '解绑',
      unlinkSourceModalTitle: '取消链接数据源',
      unlinkSourceModalContent: `
      <p>您确定要取消链接此数据源吗？</p>`,
      unlinkSourceModalConfirmText: '取消链接',
    },
    datasetOverview: {
      downloadTip: '正在从数据源下载文件。',
      processingTip: '正在由pipeline处理文件。',
      totalFiles: '文件总数',
      downloading: '正在下载',
      processing: '正在处理',
      downloadSuccessTip: '下载成功总数',
      downloadFailedTip: '下载失败总数',
      processingSuccessTip: '处理成功的文件总数',
      processingFailedTip: '处理失败的文件总数',
    },
  },
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/locales/zh.ts` is located in the `web/src/locales` directory.

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
- [en.ts](en.ts_docs.md)
- [es.ts](es.ts_docs.md)
- [fr.ts](fr.ts_docs.md)
- [id.ts](id.ts_docs.md)
- [ja.ts](ja.ts_docs.md)
- [pt-br.ts](pt-br.ts_docs.md)
- [ru.ts](ru.ts_docs.md)
- [until.ts](until.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
