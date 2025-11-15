# File Documentation: web/src/locales/ru.ts

## File Metadata

- **Path**: `web/src/locales/ru.ts`
- **Extension**: `.ts`
- **Lines**: 1,601
- **Characters**: 65,263
- **Size**: 91,018 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
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
        'Разделитель может состоять из одного или нескольких спецсимволов. Для нескольких символов укажите их в обратных кавычках (``).',
      html4excel: 'Excel в HTML',
      html4excelTip: `При включении электронные таблицы будут преобразованы в HTML-таблицы.`,
      autoKeywords: 'Авто-ключевые слова',
      autoKeywordsTip: `Автоматически извлекает N ключевых слов для каждого фрагмента.`,
      autoQuestions: 'Авто-вопросы',
      autoQuestionsTip: `Автоматически извлекает N вопросов для каждого фрагмента.`,
      redo: 'Очистить существующие {{chunkNum}} фрагментов?',
      setMetaData: 'Установить метаданные',
      pleaseInputJson: 'Введите JSON',
      documentMetaTips: `<p>Метаданные в формате JSON (не индексируются). Добавляются в промпт для LLM, если фрагменты документа включены в промпт.</p>
<p>Примеры:</p>
<b>Метаданные:</b><br>
<code>
  {
      "Автор": "Алекс Доусон",
      "Дата": "2024-11-12"
  }
</code><br>
<b>Промпт будет:</b><br>
<p>Документ: название_документа</p>
<p>Автор: Алекс Доусон</p>
<p>Дата: 2024-11-12</p>
<p>Релевантные фрагменты:</p>
<ul>
<li>Содержание фрагмента...</li>
<li>Содержание фрагмента...</li>
</ul>
`,
      metaData: 'Метаданные',
      deleteDocumentConfirmContent:
        'Документ связан с графом знаний. После удаления связанная информация о узлах и связях будет удалена, но граф не обновится немедленно.',
      plainText: 'Простой',
      reRankModelWaring: 'Модель реранкинга требует много времени.',
    },
    knowledgeConfiguration: {
      enableAutoGenerate: 'Включить авто-генерацию',
      teamPlaceholder: 'Выберите команду.',
      dataFlowPlaceholder: 'Выберите поток данных.',
      buildItFromScratch: 'Создать с нуля',
      useRAPTORToEnhanceRetrieval: 'Использовать RAPTOR для улучшения поиска',
      extractKnowledgeGraph: 'Извлечь граф знаний',
      dataFlow: 'Поток данных',
      parseType: 'Тип обработки',
      manualSetup: 'Ручная настройка',
      builtIn: 'Встроенный',
      titleDescription:
        'Обновите конфигурацию базы знаний, особенно метод фрагментации.',
      name: 'Название базы знаний',
      photo: 'Изображение базы знаний',
      photoTip: 'Максимальный размер файла 4 МБ',
      description: 'Описание',
      language: 'Язык документов',
      languageMessage: 'Укажите язык!',
      languagePlaceholder: 'Укажите язык!',
      permissions: 'Права доступа',
      embeddingModel: 'Модель эмбеддинга',
      chunkTokenNumber: 'Рекомендуемый размер фрагмента',
      chunkTokenNumberMessage: 'Укажите количество токенов для текста',
      embeddingModelTip:
        'Модель эмбеддинга по умолчанию. Не может быть изменена после создания фрагментов.',
      permissionsTip:
        "При установке 'Команда' все участники смогут управлять базой знаний.",
      chunkTokenNumberTip:
        'Устанавливает порог токенов для создания фрагмента. Сегменты с меньшим количеством токенов объединяются до превышения порога.',
      chunkMethod: 'Метод фрагментации',
      chunkMethodTip: 'См. подсказки справа.',
      upload: 'Загрузить',
      english: 'Английский',
      chinese: 'Китайский',
      portugueseBr: 'Португальский (Бразилия)',
      embeddingModelPlaceholder: 'Выберите модель эмбеддинга.',
      chunkMethodPlaceholder: 'Выберите метод фрагментации.',
      save: 'Сохранить',
      me: 'Только я',
      team: 'Команда',
      cancel: 'Отмена',
      methodTitle: 'Описание метода фрагментации',
      methodExamples: 'Примеры',
      methodExamplesDescription: 'Скриншоты для пояснения:',
      dialogueExamplesTitle: 'просмотр',
      methodEmpty: 'Здесь будет визуальное объяснение категорий баз знаний',
      book: `<p>Поддерживаемые форматы: <b>DOCX, PDF, TXT</b>.</p><p>
      Для PDF укажите <i>диапазон страниц</i>.</p>`,
      laws: `<p>Поддерживаемые форматы: <b>DOCX, PDF, TXT</b>.</p><p>
      Юридические документы обрабатываются с учетом их структуры. 
      </p><p>
      Фрагменты соответствуют уровню 'СТАТЬИ'.
      </p>`,
      manual: `<p>Только <b>PDF</b>.</p><p>
      Использует заголовки разделов как базовые единицы фрагментации.
      </p>`,
      naive: `<p>Поддерживаемые форматы: <b>MD, MDX, DOCX, XLSX, XLS, PPT, PDF, TXT, JPEG, JPG, PNG, TIF, GIF, CSV, JSON, EML, HTML</b>.</p>
      <p>'Простой' метод фрагментации: </p>
      <p>
      <li>Использует модель для разделения текста на сегменты.</li>
      <li>Объединяет соседние сегменты до превышения порога токенов.</li></p>`,
      paper: `<p>Только <b>PDF</b>.</p><p>
      Статьи разделяются по разделам (аннотация, 1.1, 1.2 и т.д.). </p><p>
      Увеличивает контекст для ИИ-диалогов и вычислительные затраты.
      </p>`,
      presentation: `<p>Поддерживаемые форматы: <b>PDF, PPTX</b>.</p><p>
      Каждый слайд обрабатывается как отдельный фрагмент.</p>`,
      qa: `
      <p>
      Форматы: <b>XLSX, CSV/TXT</b>.
    </p>
    <li>
      <b>XLSX/XLS</b>: два столбца без заголовков (вопросы и ответы).
    </li>
    <li>
      <b>CSV/TXT</b>: UTF-8 с разделителем TAB.
    </li>
      `,
      resume: `<p>Форматы: <b>DOCX, PDF, TXT</b>.
      </p><p>
      Резюме структурируются для удобства поиска.
      </p>
      `,
      table: `<p>Форматы: <b>XLSX, CSV/TXT</b>.</p><p>
      Требования:
      <ul>
    <li>Для CSV/TXT используйте <em><b>TAB</b></em> как разделитель.</li>
    <li>Первая строка - заголовки столбцов.</li>
    <li>Заголовки должны быть осмысленными.</li>
    <li>Каждая строка - отдельный фрагмент.</li>
    </ul>`,
      picture: `
    <p>Поддерживаются изображения (видео - скоро).</p><p>
    Использует OCR для извлечения текста и визуальную LLM для описаний.
    </p>`,
      one: `
    <p>Форматы: <b>DOCX, XLSX, XLS, PDF, TXT</b>.
    </p><p>
    Весь документ обрабатывается как один фрагмент.
    </p>`,
      knowledgeGraph: `<p>Форматы: <b>DOCX, EXCEL, PPT, IMAGE, PDF, TXT, MD, JSON, EML</b>

<p>Использует 'Простой'/'Общий' метод фрагментации.</p>
<p>Фрагменты передаются в LLM для извлечения сущностей и связей.</p>
<p>Установите <b>Типы сущностей</b>.</p>`,
      tag: `<p>База знаний 'Тег' служит набором тегов. Другие базы используют её для тегирования фрагментов.</p>
<p>Набор тегов <b>НЕ</b> участвует непосредственно в RAG-процессе.</p>
<p>Каждый фрагмент - пара описание-тег.</p>
<p>Форматы: <b>XLSX, CSV/TXT</b>.</p>
<p><b>XLSX</b>: два столбца без заголовков (описание и тег).</p>
<p><b>CSV/TXT</b>: UTF-8 с разделителем TAB.</p>
<p>В столбце тегов используйте <b>запятую</b> для разделения тегов.</p>
`,
      useRaptor: 'Использовать RAPTOR',
      useRaptorTip: 'Включите RAPTOR для многошаговых вопросно-ответных задач.',
      prompt: 'Промпт',
      promptTip: 'Опишите задачу для LLM, укажите формат ответа и требования.',
      promptMessage: 'Требуется промпт',
      promptText: `Пожалуйста, обобщите следующие абзацы. Будьте внимательны с числами, не выдумывайте. Абзацы:
      {cluster_content}
Выше представлен контент для обобщения.`,
      maxToken: 'Макс. токенов',
      maxTokenTip:
        'Максимальное количество токенов на суммаризирующий фрагмент.',
      maxTokenMessage: 'Требуется макс. токенов',
      threshold: 'Порог',
      thresholdTip: 'Минимальное сходство для группировки фрагментов в RAPTOR.',
      thresholdMessage: 'Требуется порог',
      maxCluster: 'Макс. кластеров',
      maxClusterTip: 'Максимальное количество кластеров.',
      maxClusterMessage: 'Требуется макс. кластеров',
      randomSeed: 'Случайное зерно',
      randomSeedMessage: 'Требуется случайное зерно',
      entityTypes: 'Типы сущностей',
      vietnamese: 'Вьетнамский',
      pageRank: 'PageRank',
      pageRankTip: `Назначьте более высокий PageRank определенным базам знаний для повышения рейтинга их фрагментов.`,
      tagName: 'Тег',
      frequency: 'Частота',
      searchTags: 'Поиск тегов',
      tagCloud: 'Облако',
      tagTable: 'Таблица',
      tagSet: 'Наборы тегов',
      tagSetTip: `
     <p> Выберите одну или несколько баз знаний тегов для автоматического тегирования фрагментов.</p>
<p>Запрос пользователя также будет автоматически тегирован.</p>
<p>Отличие авто-тегов от авто-ключевых слов:</p>
<ul>
  <li>Теги - закрытое множество, ключевые слова - открытое.</li>
  <li>Авто-ключевые слова требуют значительных ресурсов LLM.</li>
</ul>
      `,
      topnTags: 'Топ-N Тегов',
      tags: 'Теги',
      addTag: 'Добавить тег',
      useGraphRag: 'Извлечь граф знаний',
      useGraphRagTip:
        'Постройте граф знаний для улучшения многошаговых вопросно-ответных задач.',
      graphRagMethod: 'Метод',
      graphRagMethodTip: `Легкий: (По умолчанию) Использует промпты от github.com/HKUDS/LightRAG. Меньше токенов и ресурсов.</br>
        Общий: Использует промпты от github.com/microsoft/graphrag`,
      resolution: 'Разрешение сущностей',
      resolutionTip: `Включите для объединения схожих сущностей (например, '2025' и 'год 2025').`,
      community: 'Генерация отчетов сообществ',
      communityTip:
        'Генерирует сводки для кластеров связанных сущностей в графе знаний.',
      theDocumentBeingParsedCannotBeDeleted:
        'Обрабатываемый документ нельзя удалить',
    },
    chunk: {
      chunk: 'Фрагмент',
      bulk: 'Пакетно',
      selectAll: 'Выбрать все',
      enabledSelected: 'Включить выбранные',
      disabledSelected: 'Отключить выбранные',
      deleteSelected: 'Удалить выбранные',
      search: 'Поиск',
      all: 'Все',
      enabled: 'Включен',
      disabled: 'Отключен',
      keyword: 'Ключевое слово',
      function: 'Функция',
      chunkMessage: 'Введите значение!',
      full: 'Полный текст',
      ellipse: 'Сокращенный',
      graph: 'Граф знаний',
      mind: 'Ментальная карта',
      question: 'Вопрос',
      questionTip: `Если есть заданные вопросы, эмбеддинг фрагмента будет основан на них.`,
      chunkResult: 'Результат фрагментации',
      chunkResultTip: `Просмотр сегментов, используемых для эмбеддинга и поиска.`,
      enable: 'Включить',
      disable: 'Отключить',
      delete: 'Удалить',
    },
    chat: {
      messagePlaceholder: 'Введите ваше сообщение здесь...',
      exit: 'Выйти',
      multipleModels: 'Несколько моделей',
      applyModelConfigs: 'Применить настройки моделей',
      conversations: 'Диалоги',
      chatApps: 'Чат-приложения',
      newConversation: 'Новый диалог',
      createAssistant: 'Создать ассистента',
      assistantSetting: 'Настройки ассистента',
      promptEngine: 'Промпт-движок',
      modelSetting: 'Настройки модели',
      chat: 'Чат',
      newChat: 'Новый чат',
      send: 'Отправить',
      sendPlaceholder: 'Сообщение ассистенту...',
      chatConfiguration: 'Конфигурация чата',
      chatConfigurationDescription:
        'Настройте чат-ассистента для ваших наборов данных (баз знаний)! 💕',
      assistantName: 'Имя ассистента',
      assistantNameMessage: 'Требуется имя ассистента',
      namePlaceholder: 'например, Резюме Jarvis',
      assistantAvatar: 'Аватар ассистента',
      language: 'Язык',
      emptyResponse: 'Пустой ответ',
      emptyResponseTip: `Ответ, если в базах знаний не найдено релевантной информации.`,
      emptyResponseMessage: `Срабатывает, если ничего не найдено. Очистите поле, если не выбраны базы знаний.`,
      setAnOpener: 'Приветственное сообщение',
      setAnOpenerInitial: `Привет! Я ваш ассистент, чем могу помочь?`,
      setAnOpenerTip: 'Установите приветствие для пользователей.',
      knowledgeBases: 'Базы знаний',
      knowledgeBasesMessage: 'Выберите',
      knowledgeBasesTip:
        'Выберите базы знаний для ассистента. Пустые базы не отображаются.',
      system: 'Системный промпт',
      systemInitialValue: `Вы умный ассистент. Обобщите контент базы знаний, чтобы ответить на вопрос. Подробно перечислите данные из базы знаний. Если контент не релевантен, включите фразу "Ответ не найден в базе знаний!". Учитывайте историю диалога.
      База знаний:
      {knowledge}
      Выше представлена база знаний.`,
      systemMessage: 'Введите текст!',
      systemTip:
        'Инструкции для LLM: роль, длина, тон и язык ответов. Используйте //no_thinking для отключения рассуждений.',
      topN: 'Топ N',
      topNTip: `Выбирает 'Топ N' фрагментов из найденных.`,
      variable: 'Переменная',
      variableTip: `Переменные делают системные промпты более гибкими. {knowledge} - зарезервированная переменная для найденных фрагментов.`,
      add: 'Добавить',
      key: 'Ключ',
      optional: 'Опционально',
      operation: 'Действие',
      model: 'Модель',
      modelTip: 'Модель чата',
      modelMessage: 'Выберите!',
      modelEnabledTools: 'Включенные инструменты',
      modelEnabledToolsTip:
        'Выберите инструменты для модели чата. Эффективно только для моделей, поддерживающих вызов инструментов.',
      freedom: 'Свобода',
      improvise: 'Импровизация',
      precise: 'Точность',
      balance: 'Баланс',
      custom: 'Пользовательский',
      freedomTip: `Сокращенная настройка 'Температуры', 'Top P', 'Штрафа за присутствие' и 'Штрафа за частоту'.`,
      temperature: 'Температура',
      temperatureMessage: 'Требуется температура',
      temperatureTip: `Контролирует случайность предсказаний модели.`,
      topP: 'Top P',
      topPMessage: 'Требуется Top P',
      topPTip:
        'Устанавливает порог для выбора наиболее вероятных слов (ядерная выборка).',
      presencePenalty: 'Штраф за присутствие',
      presencePenaltyMessage: 'Требуется штраф за присутствие',
      presencePenaltyTip: 'Штрафует слова, уже появившиеся в диалоге.',
      frequencyPenalty: 'Штраф за частоту',
      frequencyPenaltyMessage: 'Требуется штраф за частоту',
      frequencyPenaltyTip:
        'Уменьшает тенденцию модели повторять одни и те же слова.',
      maxTokens: 'Макс. токенов',
      maxTokensMessage: 'Требуется макс. токенов',
      maxTokensTip: `Максимальная длина вывода модели в токенах. По умолчанию 512.`,
      maxTokensInvalidMessage: 'Введите корректное число для Макс. токенов.',
      maxTokensMinMessage: 'Макс. токенов не может быть меньше 0.',
      quote: 'Показать источник',
      quoteTip: 'Отображать исходный текст как ссылку.',
      selfRag: 'Self-RAG',
      selfRagTip: 'См.: https://huggingface.co/papers/2310.11511',
      overview: 'ID чата',
      pv: 'Количество сообщений',
      uv: 'Количество активных пользователей',
      speed: 'Скорость вывода токенов',
      tokens: 'Потрачено токенов',
      round: 'Количество взаимодействий',
      thumbUp: 'Удовлетворенность клиентов',
      preview: 'Предпросмотр',
      embedded: 'Встроенный',
      serviceApiEndpoint: 'Конечная точка API сервиса',
      apiKey: 'API КЛЮЧ',
      apiReference: 'Документация API',
      dateRange: 'Диапазон дат:',
      backendServiceApi: 'API Сервер',
      createNewKey: 'Создать новый ключ',
      created: 'Создан',
      action: 'Действие',
      embedModalTitle: 'Встроить на веб-страницу',
      comingSoon: 'Скоро будет',
      fullScreenTitle: 'Полное встраивание',
      fullScreenDescription:
        'Встройте следующий iframe в нужное место вашего сайта',
      partialTitle: 'Частичное встраивание',
      extensionTitle: 'Расширение Chrome',
      tokenError: 'Сначала создайте API ключ.',
      betaError:
        'Сначала получите API ключ RAGFlow на странице системных настроек.',
      searching: 'Поиск...',
      parsing: 'Обработка',
      uploading: 'Загрузка',
      uploadFailed: 'Ошибка загрузки',
      regenerate: 'Повторить',
      read: 'Читать содержимое',
      tts: 'Текст в речь',
      ttsTip:
        'Выберите модель TTS на странице настроек перед включением этой опции.',
      relatedQuestion: 'Связанный вопрос',
      answerTitle: 'О',
      multiTurn: 'Многоходовая оптимизация',
      multiTurnTip:
        'Оптимизирует пользовательские запросы с использованием контекста многоходового диалога. Требует дополнительных токенов LLM.',
      howUseId: 'Как использовать ID чата?',
      description: 'Описание ассистента',
      descriptionPlaceholder: 'например, Чат-ассистент для резюме.',
      useKnowledgeGraph: 'Использовать граф знаний',
      useKnowledgeGraphTip:
        'Использовать графы знаний для многоходовых вопросов. Увеличивает время поиска.',
      keyword: 'Анализ ключевых слов',
      keywordTip: `Использовать LLM для анализа вопросов пользователя и извлечения ключевых слов. Увеличивает время ответа.`,
      languageTip:
        'Переписывает предложения на выбранном языке или использует язык последнего вопроса.',
      avatarHidden: 'Скрыть аватар',
      locale: 'Локаль',
      selectLanguage: 'Выберите язык',
      reasoning: 'Рассуждение',
      reasoningTip: `Включите рабочий процесс рассуждений для сложных вопросов, как в Deepseek-R1 или OpenAI o1.`,
      tavilyApiKeyTip:
        'Если API ключ установлен, будут использоваться веб-поиски на основе Tavily.',
      tavilyApiKeyMessage: 'Введите ваш Tavily API Key',
      tavilyApiKeyHelp: 'Как получить?',
      crossLanguage: 'Межъязыковый поиск',
      crossLanguageTip: `Выберите один или несколько языков для межъязыкового поиска.`,
      createChat: 'Создать чат',
      metadata: 'Метаданные',
      metadataTip:
        'Фильтрация метаданных - это процесс использования атрибутов метаданных для уточнения и контроля поиска релевантной информации.',
      conditions: 'Условия',
      addCondition: 'Добавить условие',
      meta: {
        disabled: 'Отключено',
        auto: 'Автоматически',
        manual: 'Вручную',
      },
      cancel: 'Отмена',
      chatSetting: 'Настройки чата',
    },
    setting: {
      profile: 'Профиль',
      avatar: 'Аватар',
      avatarTip: 'Отображается в вашем профиле.',
      profileDescription: 'Обновите фото и личные данные.',
      maxTokens: 'Макс. токенов',
      maxTokensMessage: 'Требуется макс. токенов',
      maxTokensTip: `Максимальная длина вывода модели в токенах. По умолчанию 512.`,
      maxTokensInvalidMessage: 'Введите корректное число для Макс. токенов.',
      maxTokensMinMessage: 'Макс. токенов не может быть меньше 0.',
      password: 'Пароль',
      passwordDescription: 'Введите текущий пароль для изменения пароля.',
      model: 'Провайдеры моделей',
      modelDescription: 'Настройте параметры моделей и API KEY.',
      team: 'Команда',
      system: 'Система',
      logout: 'Выйти',
      api: 'API',
      username: 'Имя пользователя',
      usernameMessage: 'Введите имя пользователя!',
      photo: 'Ваше фото',
      photoDescription: 'Отображается в вашем профиле.',
      colorSchema: 'Цветовая схема',
      colorSchemaMessage: 'Выберите цветовую схему!',
      colorSchemaPlaceholder: 'выберите цветовую схему',
      bright: 'Светлая',
      dark: 'Тёмная',
      timezone: 'Часовой пояс',
      timezoneMessage: 'Укажите часовой пояс!',
      timezonePlaceholder: 'выберите часовой пояс',
      email: 'Email адрес',
      emailDescription: 'После регистрации email нельзя изменить.',
      currentPassword: 'Текущий пароль',
      currentPasswordMessage: 'Введите пароль!',
      newPassword: 'Новый пароль',
      changePassword: 'Изменить пароль',
      newPasswordMessage: 'Введите пароль!',
      newPasswordDescription:
        'Ваш новый пароль должен быть длиннее 8 символов.',
      confirmPassword: 'Подтвердите новый пароль',
      confirmPasswordMessage: 'Подтвердите пароль!',
      confirmPasswordNonMatchMessage: 'Новые пароли не совпадают!',
      cancel: 'Отмена',
      addedModels: 'Добавленные модели',
      modelsToBeAdded: 'Модели для добавления',
      addTheModel: 'Добавить модель',
      apiKey: 'API-Ключ',
      apiKeyMessage: 'Введите API ключ (для локальных моделей игнорируйте).',
      apiKeyTip: 'API ключ можно получить у поставщика LLM.',
      showMoreModels: 'Показать модели',
      hideModels: 'Скрыть модели',
      baseUrl: 'Базовый URL',
      baseUrlTip:
        'Если ваш API ключ от OpenAI, оставьте пустым. Другие провайдеры предоставляют базовый URL с API ключом.',
      tongyiBaseUrlTip:
        'Для китайских пользователей не нужно заполнять, используйте https://dashscope.aliyuncs.com/compatible-mode/v1. Для международных пользователей используйте https://dashscope-intl.aliyuncs.com/compatible-mode/v1',
      tongyiBaseUrlPlaceholder:
        '(Только для международных пользователей, см. подсказку)',
      modify: 'Изменить',
      systemModelSettings: 'Установить модели по умолчанию',
      chatModel: 'Модель чата',
      chatModelTip: 'Модель чата по умолчанию для новых баз знаний.',
      embeddingModel: 'Модель эмбеддинга',
      embeddingModelTip: 'Модель эмбеддинга по умолчанию для новых баз знаний.',
      img2txtModel: 'Модель Img2txt',
      img2txtModelTip: 'Модель описания изображений/видео по умолчанию.',
      sequence2txtModel: 'Модель Speech2txt',
      sequence2txtModelTip:
        'Модель ASR по умолчанию для преобразования речи в текст.',
      rerankModel: 'Модель реранкинга',
      rerankModelTip: `Модель реранкинга фрагментов по умолчанию.`,
      ttsModel: 'Модель TTS',
      ttsModelTip: 'Модель преобразования текста в речь по умолчанию.',
      workspace: 'Рабочее пространство',
      upgrade: 'Обновить',
      addLlmTitle: 'Добавить LLM',
      editLlmTitle: 'Редактировать модель {{name}}',
      editModel: 'Редактировать модель',
      modelName: 'Название модели',
      modelID: 'ID модели',
      modelUid: 'UID модели',
      modelNameMessage: 'Введите название модели!',
      modelType: 'Тип модели',
      modelTypeMessage: 'Введите тип модели!',
      addLlmBaseUrl: 'Базовый URL',
      baseUrlNameMessage: 'Введите базовый URL!',
      vision: 'Поддерживает Vision?',
      ollamaLink: 'Как интегрировать {{name}}',
      FishAudioLink: 'Как использовать FishAudio',
      TencentCloudLink: 'Как использовать TencentCloud ASR',
      volcModelNameMessage: 'Введите название модели!',
      addEndpointID: 'EndpointID модели',
      endpointIDMessage: 'Введите EndpointID модели',
      addArkApiKey: 'VOLC ARK_API_KEY',
      ArkApiKeyMessage: 'Введите ваш ARK_API_KEY',
      bedrockModelNameMessage: 'Введите название модели!',
      addBedrockEngineAK: 'ACCESS KEY',
      bedrockAKMessage: 'Введите ваш ACCESS KEY',
      addBedrockSK: 'SECRET KEY',
      bedrockSKMessage: 'Введите ваш SECRET KEY',
      bedrockRegion: 'Регион AWS',
      bedrockRegionMessage: 'Выберите!',
      'us-east-2': 'US East (Огайо)',
      'us-east-1': 'US East (Северная Вирджиния)',
      'us-west-1': 'US West (Северная Калифорния)',
      'us-west-2': 'US West (Орегон)',
      'af-south-1': 'Африка (Кейптаун)',
      'ap-east-1': 'Азиатско-Тихоокеанский регион (Гонконг)',
      'ap-south-2': 'Азиатско-Тихоокеанский регион (Хайдарабад)',
      'ap-southeast-3': 'Азиатско-Тихоокеанский регион (Джакарта)',
      'ap-southeast-5': 'Азиатско-Тихоокеанский регион (Малайзия)',
      'ap-southeast-4': 'Азиатско-Тихоокеанский регион (Мельбурн)',
      'ap-south-1': 'Азиатско-Тихоокеанский регион (Мумбаи)',
      'ap-northeast-3': 'Азиатско-Тихоокеанский регион (Осака)',
      'ap-northeast-2': 'Азиатско-Тихоокеанский регион (Сеул)',
      'ap-southeast-1': 'Азиатско-Тихоокеанский регион (Сингапур)',
      'ap-southeast-2': 'Азиатско-Тихоокеанский регион (Сидней)',
      'ap-east-2': 'Азиатско-Тихоокеанский регион (Тайбэй)',
      'ap-southeast-7': 'Азиатско-Тихоокеанский регион (Таиланд)',
      'ap-northeast-1': 'Азиатско-Тихоокеанский регион (Токио)',
      'ca-central-1': 'Канада (Центральный)',
      'ca-west-1': 'Канада Запад (Калгари)',
      'eu-central-1': 'Европа (Франкфурт)',
      'eu-west-1': 'Европа (Ирландия)',
      'eu-west-2': 'Европа (Лондон)',
      'eu-south-1': 'Европа (Милан)',
      'eu-west-3': 'Европа (Париж)',
      'eu-south-2': 'Европа (Испания)',
      'eu-north-1': 'Европа (Стокгольм)',
      'eu-central-2': 'Европа (Цюрих)',
      'il-central-1': 'Израиль (Тель-Авив)',
      'mx-central-1': 'Мексика (Центральный)',
      'me-south-1': 'Ближний Восток (Бахрейн)',
      'me-central-1': 'Ближний Восток (ОАЭ)',
      'sa-east-1': 'Южная Америка (Сан-Паулу)',
      'us-gov-east-1': 'AWS GovCloud (US-East)',
      'us-gov-west-1': 'AWS GovCloud (US-West)',
      addHunyuanSID: 'Hunyuan Secret ID',
      HunyuanSIDMessage: 'Введите ваш Secret ID',
      addHunyuanSK: 'Hunyuan Secret Key',
      HunyuanSKMessage: 'Введите ваш Secret Key',
      addTencentCloudSID: 'TencentCloud Secret ID',
      TencentCloudSIDMessage: 'Введите ваш Secret ID',
      addTencentCloudSK: 'TencentCloud Secret Key',
      TencentCloudSKMessage: 'Введите ваш Secret Key',
      SparkModelNameMessage: 'Выберите модель Spark',
      addSparkAPIPassword: 'Spark APIPassword',
      SparkAPIPasswordMessage: 'введите ваш APIPassword',
      addSparkAPPID: 'Spark APP ID',
      SparkAPPIDMessage: 'введите ваш APP ID',
      addSparkAPISecret: 'Spark APISecret',
      SparkAPISecretMessage: 'введите ваш APISecret',
      addSparkAPIKey: 'Spark APIKey',
      SparkAPIKeyMessage: 'введите ваш APIKey',
      yiyanModelNameMessage: 'Введите название модели',
      addyiyanAK: 'yiyan API KEY',
      yiyanAKMessage: 'Введите ваш API KEY',
      addyiyanSK: 'yiyan Secret KEY',
      yiyanSKMessage: 'Введите ваш Secret KEY',
      FishAudioModelNameMessage: 'Дайте имя вашей модели синтеза речи',
      addFishAudioAK: 'Fish Audio API KEY',
      addFishAudioAKMessage: 'Введите ваш API KEY',
      addFishAudioRefID: 'FishAudio Reference ID',
      addFishAudioRefIDMessage:
        'Введите Reference ID (оставьте пустым для модели по умолчанию).',
      GoogleModelIDMessage: 'Введите ID модели!',
      addGoogleProjectID: 'Project ID',
      GoogleProjectIDMessage: 'Введите Project ID',
      addGoogleServiceAccountKey:
        'Service Account Key (Оставьте пустым для Application Default Credentials)',
      GoogleServiceAccountKeyMessage:
        'Введите Google Cloud Service Account Key в формате base64',
      addGoogleRegion: 'Регион Google Cloud',
      GoogleRegionMessage: 'Введите регион Google Cloud',
      modelProvidersWarn: `Сначала добавьте модели эмбеддинга и LLM в <b>Настройки > Провайдеры моделей</b>. Затем установите их в 'Модели по умолчанию'.`,
      apiVersion: 'Версия API',
      apiVersionMessage: 'Введите версию API',
      add: 'Добавить',
      updateDate: 'Дата обновления',
      role: 'Роль',
      invite: 'Пригласить',
      agree: 'Принять',
      refuse: 'Отклонить',
      teamMembers: 'Участники команды',
      joinedTeams: 'Присоединенные команды',
      sureDelete: 'Удалить этого участника?',
      quit: 'Выйти',
      sureQuit: 'Покинуть команду?',
      secretKey: 'Секретный ключ',
      publicKey: 'Публичный ключ',
      secretKeyMessage: 'Введите секретный ключ',
      publicKeyMessage: 'Введите публичный ключ',
      hostMessage: 'Введите хост',
      configuration: 'Конфигурация',
      langfuseDescription:
        'Трассировка, оценка, управление промптами и метрики для отладки и улучшения вашего LLM-приложения.',
      viewLangfuseSDocumentation: 'Документация Langfuse',
      view: 'Просмотр',
      modelsToBeAddedTooltip:
        'Если ваш провайдер не указан, но заявляет о "совместимости с OpenAI API", выберите соответствующую карточку.',
      mcp: 'MCP',
    },
    message: {
      registered: 'Зарегистрирован!',
      logout: 'выход',
      logged: 'вошел!',
      pleaseSelectChunk: 'Выберите фрагмент!',
      registerDisabled: 'Регистрация пользователей отключена',
      modified: 'Изменено',
      created: 'Создано',
      deleted: 'Удалено',
      renamed: 'Переименовано',
      operated: 'Выполнено',
      updated: 'Обновлено',
      uploaded: 'Загружено',
      200: 'Сервер успешно вернул запрошенные данные.',
      201: 'Данные успешно созданы или изменены.',
      202: 'Запрос поставлен в очередь (асинхронная задача).',
      204: 'Данные успешно удалены.',
      400: 'Ошибка в запросе, сервер не создал/не изменил данные.',
      401: 'Пожалуйста, войдите снова.',
      403: 'Пользователь авторизован, но доступ запрещен.',
      404: 'Запрошенная запись не существует.',
      406: 'Запрошенный формат недоступен.',
      410: 'Ресурс удален и больше не доступен.',
      413: 'Слишком большой общий размер загружаемых файлов.',
      422: 'Ошибка валидации при создании объекта.',
      500: 'Ошибка сервера, проверьте сервер.',
      502: 'Ошибка шлюза.',
      503: 'Сервис недоступен, перегружен или на обслуживании.',
      504: 'Таймаут шлюза.',
      requestError: 'Ошибка запроса',
      networkAnomalyDescription:
        'Проблемы с сетью, невозможно подключиться к серверу.',
      networkAnomaly: 'Сетевая аномалия',
      hint: 'Подсказка',
    },
    fileManager: {
      files: 'Файлы',
      name: 'Название',
      uploadDate: 'Дата загрузки',
      knowledgeBase: 'База знаний',
      size: 'Размер',
      action: 'Действие',
      addToKnowledge: 'Связать с Базой Знаний',
      pleaseSelect: 'Выберите',
      newFolder: 'Новая папка',
      file: 'Файл',
      uploadFile: 'Загрузить файл',
      parseOnCreation: 'Обработать при создании',
      directory: 'Директория',
      uploadTitle: 'Перетащите файлы для загрузки',
      uploadDescription:
        'Поддерживает одиночную или пакетную загрузку. Ограничения: локальный RAGFlow - 1GB/32 файла; demo.ragflow.io - 10MB/128 файлов.',
      local: 'Локальные загрузки',
      s3: 'S3 загрузки',
      preview: 'Просмотр',
      fileError: 'Ошибка файла',
      uploadLimit: 'Каждый файл ≤10MB, всего файлов ≤128.',
      destinationFolder: 'Целевая папка',
      pleaseUploadAtLeastOneFile: 'Пожалуйста, загрузите хотя бы один файл',
    },
    flow: {
      recommended: 'Рекомендуемые',
      customerSupport: 'Поддержка клиентов',
      marketing: 'Маркетинг',
      consumerApp: 'Потребительские приложения',
      other: 'Другое',
      agents: 'Агенты',
      days: 'Дни',
      beginInput: 'Входные параметры',
      ref: 'Переменная',
      stockCode: 'Код акции',
      apiKeyPlaceholder:
        'YOUR_API_KEY (получить с https://serpapi.com/manage-api-key)',
      flowStart: 'Начать',
      flowNum: 'Номер',
      test: 'Тест',
      extractDepth: 'Глубина извлечения',
      format: 'Формат',
      basic: 'базовый',
      advanced: 'продвинутый',
      general: 'общий',
      searchDepth: 'Глубина поиска',
      tavilyTopic: 'Тема Tavily',
      maxResults: 'Макс. результатов',
      includeAnswer: 'Включать ответ',
      includeRawContent: 'Включать исходный контент',
      includeImages: 'Включать изображения',
      includeImageDescriptions: 'Включать описания изображений',
      includeDomains: 'Включать домены',
      ExcludeDomains: 'Исключать домены',
      Days: 'Дни',
      comma: 'Запятая',
      semicolon: 'Точка с запятой',
      period: 'Точка',
      lineBreak: 'Перенос строки',
      tab: 'Табуляция',
      space: 'Пробел',
      delimiters: 'Разделители',
      merge: 'Объединить',
      split: 'Разделить',
      script: 'Скрипт',
      iterationItemDescription:
        'Представляет текущий элемент в итерации, который можно использовать в последующих шагах.',
      guidingQuestion: 'Направляющий вопрос',
      onFailure: 'При ошибке',
      userPromptDefaultValue: 'Это заказ, который нужно отправить агенту.',
      search: 'Поиск',
      communication: 'Коммуникация',
      developer: 'Разработчик',
      typeCommandOrsearch: 'Введите команду или поиск...',
      builtIn: 'Встроенный',
      ExceptionDefaultValue: 'Значение по умолчанию при исключении',
      exceptionMethod: 'Метод обработки исключений',
      maxRounds: 'Макс. раундов рефлексии',
      delayEfterError: 'Задержка после ошибки',
      maxRetries: 'Макс. попыток',
      advancedSettings: 'Расширенные настройки',
      addTools: 'Добавить инструменты',
      sysPromptDefaultValue: `
      <role>
        Вы полезный помощник, ИИ-ассистент, специализирующийся на решении проблем пользователя.
        Если указана конкретная область, адаптируйте вашу экспертизу к этой области; в противном случае действуйте как универсальный специалист.
      </role>
      <instructions>
        1. Поймите запрос пользователя.
        2. Разбейте его на логические подзадачи.
        3. Выполните каждую подзадачу шаг за шагом, прозрачно рассуждая.
        4. Проверьте точность и согласованность.
        5. Четко обобщите окончательный результат.
      </instructions>`,
      singleLineText: 'Однострочный текст',
      multimodalModels: 'Мультимодальные модели',
      textOnlyModels: 'Только текстовые модели',
      allModels: 'Все модели',
      codeExecDescription:
        'Напишите свою пользовательскую логику на Python или Javascript.',
      stringTransformDescription:
        'Изменяет текстовое содержимое. В настоящее время поддерживает: разделение или объединение текста.',
      foundation: 'Основа',
      tools: 'Инструменты',
      dataManipulation: 'Манипуляция данными',
      flow: 'Поток',
      dialog: 'Диалог',
      cite: 'Источник',
      citeTip: 'Источник информации',
      name: 'Название',
      nameMessage: 'Введите название',
      description: 'Описание',
      descriptionMessage: 'Это агент для конкретной задачи.',
      examples: 'Примеры',
      to: 'Кому',
      msg: 'Сообщения',
      msgTip:
        'Вывод переменной вышестоящего компонента или введенный вами текст.',
      messagePlaceholder: `Введите текст сообщения, используйте '/' для вставки переменных.`,
      messageMsg: 'Введите сообщение или удалите это поле.',
      addField: 'Добавить поле',
      addMessage: 'Добавить сообщение',
      loop: 'Цикл',
      loopTip:
        'Максимальное количество циклов компонента. При превышении задача не может быть выполнена.',
      yes: 'Да',
      no: 'Нет',
      key: 'Ключ',
      componentId: 'ID компонента',
      add: 'Добавить',
      operation: 'Действие',
      run: 'Запустить',
      save: 'Сохранить',
      title: 'ID:',
      beginDescription: 'Начало потока.',
      answerDescription: `Интерфейс между человеком и ботом, принимает ввод пользователя и отображает ответы.`,
      retrievalDescription: `Извлекает информацию из указанных баз знаний. Убедитесь, что базы используют одну модель эмбеддинга.`,
      generateDescription: `Генерирует ответы с помощью LLM. Убедитесь, что промпт настроен правильно.`,
      categorizeDescription: `Классифицирует ввод пользователя в предопределенные категории. Укажите имя, описание и примеры для каждой категории.`,
      relevantDescription: `Оценивает релевантность вывода вышестоящего компонента последнему запросу пользователя.`,
      rewriteQuestionDescription: `Переписывает пользовательский запрос на основе контекста предыдущих диалогов.`,
      messageDescription:
        'Возвращает итоговый вывод рабочего процесса с предопределенным содержимым.',
      keywordDescription: `Извлекает топ N результатов из ввода пользователя.`,
      switchDescription: `Оценивает условия и направляет поток выполнения.`,
      wikipediaDescription: `Ищет на wikipedia.org. Использует TopN для количества результатов.`,
      promptText: `Обобщите следующие абзацы. Будьте внимательны с числами. Абзацы:
        {input}
  Контент для обобщения.`,
      createGraph: 'Создать агента',
      createFromTemplates: 'Создать из шаблонов',
      retrieval: 'Поиск',
      generate: 'Генерация',
      answer: 'Взаимодействие',
      categorize: 'Классификация',
      relevant: 'Релевантность',
      rewriteQuestion: 'Переписать',
      rewrite: 'Переписать',
      begin: 'Начало',
      message: 'Сообщение',
      blank: 'Пустой',
      createFromNothing: 'Создать агента с нуля',
      addItem: 'Добавить элемент',
      addSubItem: 'Добавить подэлемент',
      nameRequiredMsg: 'Требуется название',
      nameRepeatedMsg: 'Название не должно повторяться',
      keywordExtract: 'Ключевые слова',
      keywordExtractDescription: `Извлекает ключевые слова из запроса пользователя.`,
      baidu: 'Baidu',
      baiduDescription: `Ищет на baidu.com.`,
      duckDuckGo: 'DuckDuckGo',
      duckDuckGoDescription: 'Ищет на duckduckgo.com.',
      searXNG: 'SearXNG',
      searXNGDescription:
        'Компонент, который выполняет поиск по указанному вами URL-адресу экземпляра SearXNG. Укажите TopN и URL-адрес экземпляра.',
      channel: 'Канал',
      channelTip: `Текстовый или новостной поиск`,
      text: 'Текст',
      news: 'Новости',
      messageHistoryWindowSize: 'Размер окна истории',
      messageHistoryWindowSizeTip:
        'Количество сообщений истории, видимых LLM. Учитывайте ограничение токенов модели.',
      wikipedia: 'Wikipedia',
      pubMed: 'PubMed',
      pubMedDescription: 'Ищет на https://pubmed.ncbi.nlm.nih.gov/.',
      email: 'Email',
      emailTip: 'Email обязателен.',
      arXiv: 'ArXiv',
      arXivDescription: 'Ищет на https://arxiv.org/.',
      sortBy: 'Сортировать по',
      submittedDate: 'Дата отправки',
      lastUpdatedDate: 'Дата обновления',
      relevance: 'Релевантность',
      google: 'Google',
      googleDescription:
        'Ищет на https://www.google.com/. Требуется API ключ от serpapi.com.',
      bing: 'Bing',
      bingDescription:
        'Ищет на https://www.bing.com/. Требуется API ключ от microsoft.com.',
      apiKey: 'API КЛЮЧ',
      country: 'Страна и регион',
      language: 'Язык',
      googleScholar: 'Google Scholar',
      googleScholarDescription: 'Ищет на https://scholar.google.com/.',
      yearLow: 'Год от',
      yearHigh: 'Год до',
      patents: 'Патенты',
      data: 'Данные',
      deepL: 'DeepL',
      deepLDescription: 'Перевод с помощью https://www.deepl.com/.',
      authKey: 'Ключ авторизации',
      sourceLang: 'Исходный язык',
      targetLang: 'Целевой язык',
      gitHub: 'GitHub',
      gitHubDescription: 'Ищет репозитории на https://github.com/.',
      baiduFanyi: 'BaiduFanyi',
      baiduFanyiDescription: 'Перевод с помощью https://fanyi.baidu.com/.',
      appid: 'App ID',
      secretKey: 'Секретный ключ',
      domain: 'Домен',
      transType: 'Тип перевода',
      baiduSecretKeyOptions: {
        translate: 'Общий перевод',
        fieldtranslate: 'Специализированный перевод',
      },
      baiduDomainOptions: {
        it: 'Информационные технологии',
        finance: 'Финансы и экономика',
        machinery: 'Машиностроение',
        senimed: 'Биомедицина',
        novel: 'Онлайн литература',
        academic: 'Академические статьи',
        aerospace: 'Аэрокосмическая',
        wiki: 'Гуманитарные науки',
        news: 'Новости',
        law: 'Законы',
        contract: 'Контракты',
      },
      baiduSourceLangOptions: {
        auto: 'Автоопределение',
        zh: 'Китайский',
        en: 'Английский',
        yue: 'Кантонский',
        wyw: 'Классический китайский',
        jp: 'Японский',
        kor: 'Корейский',
        fra: 'Французский',
        spa: 'Испанский',
        th: 'Тайский',
        ara: 'Арабский',
        ru: 'Русский',
        pt: 'Португальский',
        de: 'Немецкий',
        it: 'Итальянский',
        el: 'Греческий',
        nl: 'Голландский',
        pl: 'Польский',
        bul: 'Болгарский',
        est: 'Эстонский',
        dan: 'Датский',
        fin: 'Финский',
        cs: 'Чешский',
        rom: 'Румынский',
        slo: 'Словенский',
        swe: 'Шведский',
        hu: 'Венгерский',
        cht: 'Традиционный китайский',
        vie: 'Вьетнамский',
      },
      qWeather: 'QWeather',
      qWeatherDescription:
        'Получает погодную информацию с https://www.qweather.com/.',
      lang: 'Язык',
      type: 'Тип',
      webApiKey: 'Web API ключ',
      userType: 'Тип пользователя',
      timePeriod: 'Период времени',
      qWeatherLangOptions: {
        zh: 'Упрощенный китайский',
        'zh-hant': 'Традиционный китайский',
        en: 'Английский',
        de: 'Немецкий',
        es: 'Испанский',
        fr: 'Французский',
        it: 'Итальянский',
        ja: 'Японский',
        ko: 'Корейский',
        ru: 'Русский',
        hi: 'Хинди',
        th: 'Тайский',
        ar: 'Арабский',
        pt: 'Португальский',
        bn: 'Бенгальский',
        ms: 'Малайский',
        nl: 'Голландский',
        el: 'Греческий',
        la: 'Латинский

[... Content truncated for brevity ...]
```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/locales/ru.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 1601 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

No significant structural elements detected.

## Code Structure Analysis

- Total lines: 1601
- Blank lines: 3 (0.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~1598


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/locales`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

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

- Other files in `web/src/locales/` directory
- Potential test file: `test_ru.ts`

## Keywords

ACCESS, AISummary, API, APIKey, APIPassword, APISecret, APP, ARK_API_KEY, ASR, AWS, Account, AkShare, App, Application, ArXiv, ArkApiKeyMessage, Audio, Baidu, BaiduFanyi, Bing, CRYPTO, CSV, Chrome, Cloud, Credentials, DOCX, DSL, Days, DeepL, Deepseek, Default, DuckDuckGo, EML, EXCEL, East, Eastmoney, Email, EndpointID, Excel, ExceptionDefaultValue, ExcludeDomains, Extract, FAQ, FENGHUANG, FOREX, FUTURE, Fish, FishAudio, FishAudioLink, FishAudioModelNameMessage...

---
*Generated by RAGFlow Repository Documentation Generator*
