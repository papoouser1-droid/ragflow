# Documentation: web/src/locales/ru.ts

## File Metadata

- **Path**: `web/src/locales/ru.ts`
- **Size**: 91018 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/locales/ru.ts`.

## Original Source Code

```ts
export default {
  translation: {
    common: {
      noResults: 'Нет результатов.',
      selectPlaceholder: 'выберите значение',
      selectAll: 'Выбрать все',
      delete: 'Удалить',
      deleteModalTitle: 'Вы уверены, что хотите удалить этот элемент?',
      ok: 'Да',
      cancel: 'Нет',
      no: 'Нет',
      total: 'Всего',
      rename: 'Переименовать',
      name: 'Название',
      save: 'Сохранить',
      namePlaceholder: 'Введите название',
      next: 'Далее',
      create: 'Создать',
      edit: 'Редактировать',
      upload: 'Загрузить',
      english: 'Английский',
      portugueseBr: 'Португальский (Бразилия)',
      chinese: 'Упрощенный китайский',
      traditionalChinese: 'Традиционный китайский',
      language: 'Язык',
      languageMessage: 'Пожалуйста, укажите язык!',
      languagePlaceholder: 'Выберите язык',
      copy: 'Копировать',
      copied: 'Скопировано',
      comingSoon: 'Скоро будет',
      download: 'Скачать',
      close: 'Закрыть',
      preview: 'Просмотр',
      move: 'Переместить',
      warn: 'Предупреждение',
      action: 'Действие',
      s: 'С',
      pleaseSelect: 'Выберите',
      pleaseInput: 'Введите',
      submit: 'Отправить',
      clear: 'Очистить',
      embedIntoSite: 'Встроить на веб-страницу',
      previousPage: 'Назад',
      nextPage: 'Вперед',
      add: 'Добавить',
      remove: 'Удалить',
      search: 'Поиск',
      noDataFound: 'Данные не найдены.',
      noData: 'Нет данных',
      promptPlaceholder: `Введите текст или используйте / для быстрой вставки переменных.`,
      mcp: {
        namePlaceholder: 'Мой MCP сервер',
        nameRequired:
          'Должно быть 1-64 символов и содержать только буквы, цифры, дефисы и подчеркивания.',
        urlPlaceholder: 'https://api.example.com/v1/mcp',
        tokenPlaceholder: 'например, eyJhbGciOiJIUzI1Ni...',
      },
    },
    login: {
      login: 'Войти',
      signUp: 'Регистрация',
      loginDescription: 'Рады снова видеть вас!',
      registerDescription: 'Рады приветствовать вас на борту!',
      emailLabel: 'Email',
      emailPlaceholder: 'Введите email',
      passwordLabel: 'Пароль',
      passwordPlaceholder: 'Введите пароль',
      rememberMe: 'Запомнить меня',
      signInTip: 'Нет аккаунта?',
      signUpTip: 'Уже есть аккаунт?',
      nicknameLabel: 'Никнейм',
      nicknamePlaceholder: 'Введите никнейм',
      register: 'Создать аккаунт',
      continue: 'Продолжить',
      title: 'Начните создавать умных помощников.',
      description:
        'Зарегистрируйтесь бесплатно, чтобы изучить передовые RAG-технологии. Создавайте базы знаний и ИИ для развития вашего бизнеса.',
      review: 'на основе 500+ отзывов',
    },
    header: {
      knowledgeBase: 'База знаний',
      chat: 'Чат',
      register: 'Регистрация',
      signin: 'Вход',
      home: 'Главная',
      setting: 'Настройки пользователя',
      logout: 'Выйти',
      fileManager: 'Управление файлами',
      flow: 'Агент',
      search: 'Поиск',
      welcome: 'Добро пожаловать в',
      dataset: 'Набор данных',
    },
    knowledgeList: {
      welcome: 'С возвращением',
      description: 'Какие базы знаний вы будете использовать сегодня?',
      createKnowledgeBase: 'Создать базу знаний',
      name: 'Название',
      namePlaceholder: 'Введите название!',
      doc: 'Документы',
      searchKnowledgePlaceholder: 'Поиск',
      noMoreData: `Это всё. Больше ничего нет.`,
    },
    knowledgeDetails: {
      generateKnowledgeGraph:
        'Это извлечет сущности и связи из всех ваших документов в этом наборе данных. Процесс может занять некоторое время.',
      generateRaptor:
        'Это извлечет сущности и связи из всех ваших документов в этом наборе данных. Процесс может занять некоторое время.',
      generate: 'Сгенерировать',
      raptor: 'RAPTOR',
      knowledgeGraph: 'Граф знаний',
      processingType: 'Тип обработки',
      dataPipeline: 'Пайплайн данных',
      operations: 'Операции',
      status: 'Статус',
      task: 'Задача',
      startDate: 'Дата начала',
      source: 'Источник',
      fileName: 'Имя файла',
      datasetLogs: 'Логи набора данных',
      fileLogs: 'Логи файлов',
      overview: 'Обзор',
      success: 'Успешно',
      failed: 'Ошибка',
      completed: 'Завершено',
      processLog: 'Лог процесса',
      created: 'Создано',
      learnMore: 'Узнать больше',
      general: 'Общие',
      chunkMethodTab: 'Метод фрагментации',
      testResults: 'Результаты тестирования',
      testSetting: 'Настройки тестирования',
      retrievalTesting: 'Тестирование поиска',
      retrievalTestingDescription:
        'Проведите тест поиска, чтобы проверить, может ли RAGFlow находить нужный контент для LLM.',
      Parse: 'Обработать',
      dataset: 'Набор данных',
      testing: 'Тестирование поиска',
      files: 'файлы',
      configuration: 'Конфигурация',
      knowledgeGraph: 'Граф знаний',
      name: 'Название',
      namePlaceholder: 'Введите название!',
      doc: 'Документы',
      datasetDescription:
        '😉 Пожалуйста, дождитесь завершения обработки файлов перед началом чата с ИИ.',
      addFile: 'Добавить файл',
      searchFiles: 'Поиск файлов',
      localFiles: 'Локальные файлы',
      emptyFiles: 'Создать пустой файл',
      webCrawl: 'Веб-сканирование',
      chunkNumber: 'Количество фрагментов',
      uploadDate: 'Дата загрузки',
      chunkMethod: 'Метод фрагментации',
      enabled: 'Включено',
      disabled: 'Отключено',
      action: 'Действие',
      parsingStatus: 'Статус обработки',
      parsingStatusTip:
        'Время обработки документа зависит от нескольких факторов. Включение таких функций, как Граф знаний, RAPTOR, Автоизвлечение вопросов или Автоизвлечение ключевых слов, значительно увеличит время обработки. Если индикатор выполнения завис, обратитесь к FAQ: https://ragflow.io/docs/dev/faq#why-does-my-document-parsing-stall-at-under-one-percent.',
      processBeginAt: 'Начато в',
      processDuration: 'Длительность',
      progressMsg: 'Прогресс',
      noTestResultsForRuned:
        'Релевантные результаты не найдены. Попробуйте изменить запрос или параметры.',
      noTestResultsForNotRuned:
        'Тест еще не проводился. Результаты появятся здесь.',
      testingDescription:
        'Проведите тест поиска, чтобы проверить, может ли RAGFlow находить нужный контент для LLM. Если вы изменили настройки по умолчанию (например, вес сходства ключевых слов или порог сходства), имейте в виду, что эти изменения не сохранятся автоматически. Вы должны применить их в настройках чат-ассистента или компонента поиска.',
      similarityThreshold: 'Порог сходства',
      similarityThresholdTip:
        'RAGFlow использует взвешенное сходство ключевых слов в комбинации с косинусным сходством векторов или реранкингом. Этот параметр устанавливает порог сходства между запросом пользователя и фрагментами. Фрагменты с оценкой ниже порога будут исключены из результатов. По умолчанию порог установлен на 0.2.',
      vectorSimilarityWeight: 'Вес сходства ключевых слов',
      vectorSimilarityWeightTip:
        'Устанавливает вес сходства ключевых слов в общей оценке сходства. Сумма весов должна быть равна 1.0.',
      keywordSimilarityWeight: 'Вес сходства ключевых слов',
      keywordSimilarityWeightTip:
        'Устанавливает вес сходства ключевых слов в общей оценке сходства. Сумма весов должна быть равна 1.0.',
      testText: 'Тестовый текст',
      testTextPlaceholder: 'Введите ваш вопрос здесь!',
      testingLabel: 'Тестирование',
      similarity: 'Комбинированное сходство',
      termSimilarity: 'Сходство терминов',
      vectorSimilarity: 'Векторное сходство',
      hits: 'Найденные результаты',
      view: 'Просмотр',
      filesSelected: 'Выбрано файлов',
      upload: 'Загрузить',
      run: 'Обработать',
      runningStatus0: 'ОЖИДАЕТ',
      runningStatus1: 'ОБРАБАТЫВАЕТ',
      runningStatus2: 'ОТМЕНЕНО',
      runningStatus3: 'УСПЕШНО',
      runningStatus4: 'ОШИБКА',
      pageRanges: 'Диапазон страниц',
      pageRangesTip:
        'Диапазон страниц для обработки; страницы вне диапазона обрабатываться не будут.',
      fromPlaceholder: 'от',
      fromMessage: 'Не указан номер начальной страницы',
      toPlaceholder: 'до',
      toMessage: 'Не указан номер конечной страницы (не включительно)',
      layoutRecognize: 'Анализатор PDF',
      layoutRecognizeTip:
        'Используйте визуальную модель для анализа макета PDF для эффективного определения заголовков, текстовых блоков, изображений и таблиц. При выборе опции "Простой" извлекается только простой текст из PDF.',
      taskPageSize: 'Размер задачи',
      taskPageSizeMessage: 'Укажите размер задачи!',
      taskPageSizeTip: `При распознавании макета PDF-файл разбивается на части для параллельной обработки. Этот параметр задает размер каждой части.`,
      addPage: 'Добавить страницу',
      greaterThan: 'Текущее значение должно быть больше предыдущего!',
      greaterThanPrevious:
        'Текущее значение должно быть больше предыдущего "до"!',
      selectFiles: 'Выбрать файлы',
      changeSpecificCategory: 'Изменить категорию',
      uploadTitle: 'Перетащите файлы для загрузки',
      uploadDescription:
        'Поддерживает одиночную или пакетную загрузку. Для локального RAGFlow: максимальный размер загрузки 1GB, до 32 файлов. Для demo.ragflow.io: максимальный размер загрузки 10MB, до 128 файлов.',
      chunk: 'Фрагмент',
      bulk: 'Пакетно',
      cancel: 'Отмена',
      close: 'Закрыть',
      rerankModel: 'Модель реранкинга',
      rerankPlaceholder: 'Выберите',
      rerankTip: `Опционально. Если оставить пустым, RAGFlow будет использовать комбинацию сходства ключевых слов и векторов. Выбор модели реранкинга заменит векторное сходство на оценку реранкинга.`,
      topK: 'Топ-K',
      topKTip: `Определяет количество текстовых фрагментов, отправляемых в модель реранкинга.`,
      delimiter: `Разделитель текста`,
      delimiterTip:
        'Разделитель может состоять из одного или нескольк

... [Content truncated - file is 65263 bytes] ...

константы как входные данные.`,
      url: 'Url',
      method: 'Метод',
      timeout: 'Таймаут',
      headers: 'Заголовки',
      cleanHtml: 'Очистить HTML',
      cleanHtmlTip:
        'Включите, если нужен только основной контент из HTML-ответа.',
      invalidUrl:
        'Должен быть действительный URL или URL с заполнителями переменных в формате {имя_переменной} или {компонент@переменная}',
      reference: 'Ссылка',
      input: 'Вход',
      output: 'Выход',
      parameter: 'Параметр',
      howUseId: 'Как использовать ID агента?',
      content: 'Содержимое',
      operationResults: 'Результаты операций',
      autosaved: 'Автосохранено',
      optional: 'Опционально',
      pasteFileLink: 'Вставить ссылку на файл',
      testRun: 'Тестовый запуск',
      template: 'Шаблон',
      templateDescription:
        'Форматирует вывод других компонентов. Поддерживает Jinja2 и замену строк {параметр}.',
      emailComponent: 'Email',
      emailDescription: 'Отправляет email на указанный адрес.',
      smtpServer: 'SMTP Сервер',
      smtpPort: 'SMTP Порт',
      senderEmail: 'Email отправителя',
      authCode: 'Код авторизации',
      senderName: 'Имя отправителя',
      toEmail: 'Email получателя',
      ccEmail: 'Копия (CC)',
      emailSubject: 'Тема',
      emailContent: 'Содержимое',
      smtpServerRequired: 'Введите адрес SMTP сервера',
      senderEmailRequired: 'Введите email отправителя',
      authCodeRequired: 'Введите код авторизации',
      toEmailRequired: 'Введите email получателя',
      emailContentRequired: 'Введите содержимое письма',
      emailSentSuccess: 'Email успешно отправлен',
      emailSentFailed: 'Ошибка отправки email',
      dynamicParameters: 'Динамические параметры',
      jsonFormatTip:
        'Вышестоящий компонент должен предоставить JSON-строку в формате:',
      toEmailTip: 'to_email: Email получателя (Обязательно)',
      ccEmailTip: 'cc_email: Копия (CC) (Опционально)',
      subjectTip: 'subject: Тема письма (Опционально)',
      contentTip: 'content: Содержимое письма (Опционально)',
      jsonUploadTypeErrorMessage: 'Загрузите json файл',
      jsonUploadContentErrorMessage: 'Ошибка json файла',
      iteration: 'Итерация',
      iterationDescription: `Циклический компонент, выполняющий логику для каждого элемента входного массива.`,
      delimiterTip: `
Разделитель используется для разбиения входного текста на части для каждой итерации.`,
      delimiterOptions: {
        comma: 'Запятая',
        lineBreak: 'Перенос строки',
        tab: 'Табуляция',
        underline: 'Подчеркивание',
        diagonal: 'Косая черта',
        minus: 'Дефис',
        semicolon: 'Точка с запятой',
      },
      addVariable: 'Добавить переменную',
      variableSettings: 'Настройки переменных',
      systemPrompt: 'Системный промпт',
      userPrompt: 'Пользовательский промпт',
      addCategory: 'Добавить категорию',
      categoryName: 'Название категории',
      nextStep: 'Следующий шаг',
      variableExtractDescription:
        'Извлекает информацию пользователя в глобальную переменную в течение диалога',
      variableExtract: 'Переменные',
      variables: 'Переменные',
      variablesTip: `Задайте четкие json-ключи с пустыми значениями. Например:
      {
        "UserCode":"",
        "NumberPhone":""
      }`,
      datatype: 'MINE тип HTTP запроса',
      insertVariableTip: `Введите / Вставьте переменные`,
      historyversion: 'История версий',
      filename: 'Имя файла',
      version: {
        created: 'Создано',
        details: 'Детали версии',
        dsl: 'DSL',
        download: 'Скачать',
        version: 'Версия',
        select: 'Версия не выбрана',
      },
      setting: 'Настройки',
      settings: {
        agentSetting: 'Настройки агента',
        title: 'название',
        description: 'описание',
        upload: 'Загрузить',
        photo: 'Фото',
        permissions: 'Права доступа',
        permissionsTip: 'Установите права для участников команды.',
        me: 'я',
        team: 'Команда',
      },
      noMoreData: 'Нет больше данных',
      searchAgentPlaceholder: 'Поиск агента',
      footer: {
        profile: 'Все права защищены @ React',
      },
      layout: {
        file: 'файл',
        knowledge: 'знания',
        chat: 'чат',
      },
      prompt: 'Промпт',
      promptTip:
        'Опишите задачу для LLM, укажите формат ответа и требования. Используйте / для вставки переменных.',
      promptMessage: 'Требуется промпт',
      infor: 'Информационный запуск',
      knowledgeBasesTip:
        'Выберите базы знаний для ассистента или переменные с ID баз знаний.',
      knowledgeBaseVars: 'Переменные базы знаний',
      code: 'Код',
      codeDescription:
        'Позволяет разработчикам писать пользовательскую логику на Python.',
      inputVariables: 'Входные переменные',
      runningHintText: 'выполняется...🕞',
      openingSwitch: 'Приветствие',
      openingCopy: 'Приветственное сообщение',
      openingSwitchTip: 'Пользователи увидят это приветствие в начале.',
      modeTip: 'Режим определяет, как запускается рабочий процесс.',
      mode: 'Режим',
      conversational: 'диалоговый',
      task: 'задача',
      beginInputTip:
        'Определите входные параметры для доступа в последующих процессах.',
      query: 'Переменные запроса',
      queryTip: 'Выберите переменную, которую хотите использовать',
      agent: 'Агент',
      addAgent: 'Добавить агента',
      agentDescription:
        'Создает агентов с рассуждениями, использованием инструментов и многопользовательским взаимодействием.',
      maxRecords: 'Макс. записей',
      createAgent: 'Создать Агента',
      stringTransform: 'Обработка текста',
      userFillUp: 'Ожидание ответа',
      userFillUpDescription: `Приостанавливает рабочий процесс и ожидает сообщения пользователя.`,
      codeExec: 'Код',
      tavilySearch: 'Tavily Search',
      tavilySearchDescription: 'Поиск через сервис Tavily.',
      tavilyExtract: 'Tavily Extract',
      tavilyExtractDescription: 'Извлечение через Tavily',
      log: 'Журнал',
      management: 'Управление',
      import: 'Импорт',
      export: 'Экспорт',
      seconds: 'Секунды',
      subject: 'Тема',
      tag: 'Тег',
      tagPlaceholder: 'Введите тег',
      descriptionPlaceholder: 'Введите описание',
      line: 'Однострочный текст',
      paragraph: 'Текст абзаца',
      options: 'Выпадающие опции',
      file: 'Загрузка файла',
      integer: 'Число',
      boolean: 'Булево',

      logTimeline: {
        begin: 'Готов к началу',
        agent: 'Агент думает',
        userFillUp: 'Ожидает вас',
        retrieval: 'Ищет знания',
        message: 'Агент говорит',
        awaitResponse: 'Ожидает вас',
        switch: 'Выбирает путь',
        iteration: 'Пакетная обработка',
        categorize: 'Классификация информации',
        code: 'Запускает скрипт',
        textProcessing: 'Организует текст',
        tavilySearch: 'Ищет в интернете',
        tavilyExtract: 'Читает страницу',
        exeSQL: 'Запрос к БД',
        google: 'Поиск в Google',
        wikipedia: 'Поиск в Wikipedia',
        googleScholar: 'Академический поиск',
        gitHub: 'Поиск в GitHub',
        email: 'Отправка email',
        httpRequest: 'Вызов API',
        wenCai: 'Запрос финансовых данных',
      },
      goto: 'Ветка неудачи',
      comment: 'Значение по умолчанию',
      sqlStatement: 'SQL запрос',
      sqlStatementTip:
        'Напишите ваш SQL запрос здесь. Вы можете использовать переменные, чистый SQL или комбинировать оба метода с использованием синтаксиса переменных.',
      frameworkPrompts: 'Фреймворк',
    },
    llmTools: {
      bad_calculator: {
        name: 'Калькулятор',
        description:
          'Инструмент для вычисления суммы двух чисел (дает неверный ответ)',
        params: {
          a: 'Первое число',
          b: 'Второе число',
        },
      },
    },
    modal: {
      okText: 'Подтвердить',
      cancelText: 'Отмена',
    },
    mcp: {
      export: 'Экспорт',
      import: 'Импорт',
      url: 'URL',
      serverType: 'Тип сервера',
      addMCP: 'Добавить MCP',
      editMCP: 'Редактировать MCP',
    },
    search: {
      searchApps: 'Поисковые приложения',
      createSearch: 'Создать поиск',
      searchGreeting: 'Чем я могу помочь вам сегодня?',
      profile: 'Скрыть профиль',
      locale: 'Локаль',
      embedCode: 'Код для вставки',
      id: 'ID',
      copySuccess: 'Успешно скопировано',
      welcomeBack: 'С возвращением',
      searchSettings: 'Настройки поиска',
      name: 'Название',
      avatar: 'Аватар',
      description: 'Описание',
      datasets: 'Наборы данных',
      rerankModel: 'Модель реранкинга',
      AISummary: 'AI-резюме',
      enableWebSearch: 'Включить веб-поиск',
      enableRelatedSearch: 'Включить связанный поиск',
      showQueryMindmap: 'Показать ментальную карту запроса',
      embedApp: 'Встроить приложение',
      relatedSearch: 'Связанный поиск',
      descriptionValue: 'Вы умный ассистент.',
      okText: 'Сохранить',
      cancelText: 'Отмена',
      chooseDataset: 'Сначала выберите набор данных',
    },
    language: {
      english: 'Английский',
      chinese: 'Китайский',
      spanish: 'Испанский',
      french: 'Французский',
      german: 'Немецкий',
      japanese: 'Японский',
      korean: 'Корейский',
      vietnamese: 'Вьетнамский',
      russian: 'Русский',
    },
    pagination: {
      total: 'Всего {{total}}',
      page: '{{page}} /Страница',
    },
    dataflowParser: {
      parseSummary: 'Резюме обработки',
      parseSummaryTip: 'Обработчик: deepdoc',
      rerunFromCurrentStep: 'Перезапустить с текущего шага',
      rerunFromCurrentStepTip: 'Обнаружены изменения. Нажмите для перезапуска.',
    },
    dataflow: {
      parser: 'Обработчик',
      parserDescription: 'Обработчик',
      chunker: 'Фрагментатор',
      chunkerDescription: 'Фрагментатор',
      tokenizer: 'Токенизатор',
      tokenizerDescription: 'Токенизатор',
    },
  },
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/locales/ru.ts` is located in the `web/src/locales` directory.

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
- [until.ts](until.ts_docs.md)
- [vi.ts](vi.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
