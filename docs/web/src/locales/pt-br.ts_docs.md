# Documentation: web/src/locales/pt-br.ts

## File Metadata

- **Path**: `web/src/locales/pt-br.ts`
- **Size**: 62902 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/locales/pt-br.ts`.

## Original Source Code

```ts
export default {
  translation: {
    common: {
      delete: 'Excluir',
      deleteModalTitle: 'Tem certeza de que deseja excluir este item?',
      ok: 'Sim',
      cancel: 'Não',
      total: 'Total',
      rename: 'Renomear',
      name: 'Nome',
      save: 'Salvar',
      namePlaceholder: 'Por favor, insira o nome',
      next: 'Próximo',
      create: 'Criar',
      edit: 'Editar',
      upload: 'Carregar',
      english: 'Inglês',
      portugueseBr: 'Português (Brasil)',
      chinese: 'Chinês Simplificado',
      traditionalChinese: 'Chinês Tradicional',
      language: 'Idioma',
      languageMessage: 'Por favor, insira seu idioma!',
      languagePlaceholder: 'selecione seu idioma',
      copy: 'Copiar',
      copied: 'Copiado',
      comingSoon: 'Em breve',
      download: 'Baixar',
      close: 'Fechar',
      preview: 'Pré-visualizar',
      move: 'Mover',
      warn: 'Aviso',
      action: 'Ação',
      s: 'S',
      pleaseSelect: 'Por favor, selecione',
      pleaseInput: 'Por favor, insira',
      submit: 'Enviar',
      embedIntoSite: 'Incorporar no site',
      previousPage: 'Anterior',
      nextPage: 'Próxima',
    },
    login: {
      login: 'Entrar',
      signUp: 'Inscrever-se',
      loginDescription: 'Estamos muito animados para vê-lo novamente!',
      registerDescription: 'Feliz por tê-lo a bordo!',
      emailLabel: 'Email',
      emailPlaceholder: 'Por favor, insira o email',
      passwordLabel: 'Senha',
      passwordPlaceholder: 'Por favor, insira a senha',
      rememberMe: 'Lembrar-me',
      signInTip: 'Não tem uma conta?',
      signUpTip: 'Já tem uma conta?',
      nicknameLabel: 'Apelido',
      nicknamePlaceholder: 'Por favor, insira o apelido',
      register: 'Criar uma conta',
      continue: 'Continuar',
      title: 'Comece a construir seus assistentes inteligentes.',
      description:
        'Inscreva-se gratuitamente para explorar a tecnologia RAG de ponta. Crie bases de conhecimento e IAs para capacitar seu negócio.',
      review: 'de 500+ avaliações',
    },
    header: {
      knowledgeBase: 'Base de Conhecimento',
      chat: 'Chat',
      register: 'Registrar',
      signin: 'Entrar',
      home: 'Início',
      setting: 'Configurações do usuário',
      logout: 'Sair',
      fileManager: 'Gerenciamento de Arquivos',
      flow: 'Agente',
      search: 'Buscar',
    },
    knowledgeList: {
      welcome: 'Bem-vindo de volta',
      description: 'Quais bases de conhecimento você usará hoje?',
      createKnowledgeBase: 'Criar base de conhecimento',
      name: 'Nome',
      namePlaceholder: 'Por favor, insira o nome!',
      doc: 'Documentos',
      searchKnowledgePlaceholder: 'Buscar',
      noMoreData: 'Isso é tudo. Nada mais.',
    },
    knowledgeDetails: {
      dataset: 'Conjunto de dados',
      testing: 'Teste de recuperação',
      files: 'Arquivos',
      configuration: 'Configuração',
      name: 'Nome',
      namePlaceholder: 'Por favor, insira o nome!',
      doc: 'Documentos',
      datasetDescription:
        '😉 Por favor, aguarde o término da análise do seu arquivo antes de iniciar um chat com IA.',
      addFile: 'Adicionar arquivo',
      searchFiles: 'Buscar seus arquivos',
      localFiles: 'Arquivos locais',
      emptyFiles: 'Criar arquivo vazio',
      webCrawl: 'Rastreamento na web',
      chunkNumber: 'Número de fragmentos',
      uploadDate: 'Data de upload',
      chunkMethod: 'Método de fragmentação',
      enabled: 'Habilitar',
      disabled: 'Desabilitar',
      action: 'Ação',
      parsingStatus: 'Status da análise',
      parsingStatusTip:
        'O tempo de processamento do documento varia conforme vários fatores. Ativar recursos como Knowledge Graph, RAPTOR, Extração Automática de Perguntas ou Extração Automática de Palavras-chave aumentará significativamente o tempo de processamento. Se a barra de progresso travar, consulte estas duas FAQs: https://ragflow.io/docs/dev/faq#why-does-my-document-parsing-stall-at-under-one-percent.',
      processBeginAt: 'Início em',
      processDuration: 'Duração',
      progressMsg: 'Progresso',
      testingDescription:
        'Realize um teste de recuperação para verificar se o RAGFlow pode recuperar o conteúdo pretendido para o LLM. Por favor, note que as alterações feitas aqui não são salvas automaticamente. Se você ajustar as configurações padrão aqui, como o peso de similaridade de palavras-chave, certifique-se de atualizar as configurações relacionadas de forma sincronizada nas configurações do assistente de chat ou nas configurações do operador de recuperação.',
      similarityThreshold: 'Limite de similaridade',
      similarityThresholdTip:
        'O RAGFlow emprega uma combinação de similaridade de palavras-chave ponderada e similaridade de cosseno vetorial ponderada, ou uma combinação de similaridade de palavras-chave ponderada e pontuação de reranking ponderada durante a recuperação. Este parâmetro define o limite para similaridades entre a consulta do usuário e os fragmentos. Qualquer fragmento com uma pontuação de similaridade abaixo deste limite será excluído dos resultados. Por padrão, o limite é definido como 0,2. Isso significa que apenas os trechos com uma pontuação de similaridade híbrida de 20 ou mais serão recuperados.',
      vectorSimilarityWeight: 'Peso da similaridade de palavras-chave',
      vectorSimilarityWeightTip:
        'Define o peso da similaridade de palavras-chave na pontuação de similaridade combinada, usada com a similaridade de cosseno vetorial ou com a pontuação de reranking. O total dos dois pesos deve ser igual a 1.0.',
      testText: 'Texto de teste',
      testTextPlaceholder: 'Insira sua pergunta aqui!',
      testingLabel: 'Testando',
      similarity: 'Similaridade híbrida',
      termSimilarity: 'Similaridade de termos',
      vectorSimilarity: 'Similaridade vetorial',
      hits: 'Acertos',
      view: 'Visualizar',
      filesSelected: 'Arquivos selecionados',
      upload: 'Carregar',
      run: 'Analisar',
      runningStatus0: 'Não analisado',
      runningStatus1: 'Analisando',
      runningStatus2: 'Cancelado',
      runningStatus3: 'Sucesso',
      runningStatus4: 'Falha',
      pageRanges: 'Intervalos de páginas',
      pageRangesTip:
        'Intervalo de páginas a serem analisadas; páginas fora deste intervalo não serão processadas.',
      fromPlaceholder: 'de',
      fromMessage: 'Página inicial ausente',
      toPlaceholder: 'até',
      toMessage: 'Página final ausente (excluída)',
      layoutRecognize: 'Reconhecimento de layout',
      layoutRecognizeTip:
        'Use modelos visuais para análise de layout para entender melhor a estrutura do documento e localizar efetivamente títulos, blocos de texto, imagens e tabelas. Se desativado, apenas o texto simples no PDF será recuperado. Para mais informações, acesse https://ragflow.io/docs/dev/select_pdf_parser.',
      taskPageSize: 'Tamanho da página da tarefa',
      taskPageSizeMessage: 'Por favor, insira o tamanho da página da tarefa!',
      taskPageSizeTip:
        'Durante o reconhecimento de layout, um arquivo PDF é dividido em fragmentos e processado em paralelo para aumentar a velocidade de processamento. Este parâmetro define o tamanho de cada fragmento. Um tamanho de fragmento maior reduz a probabilidade de dividir texto contínuo entre páginas.',
      addPage: 'Adicionar página',
      greaterThan: 'O valor atual deve ser maior que!',
      greaterThanPrevious: 'O valor atual deve ser maior que o anterior!',
      selectFiles: 'Selecionar arquivos',
      changeSpecificCategory: 'Alterar categoria específica',
      uploadTitle:
        'Clique ou arraste o arquivo para esta área para fazer o upload',
      uploadDescription:
        'Suporte para upload único ou em massa. Estritamente proibido fazer upload de dados da empresa ou outros arquivos proibidos.',
      chunk: 'Fragmento',
      bulk: 'Em massa',
      cancel: 'Cancelar',
      rerankModel: 'Modelo de reranking',
      rerankPlaceholder: 'Por favor, selecione',
      rerankTip:
        'Opcional. Se deixar em branco, o RAGFlow usará uma combinação de similaridade ponderada por palavra-chave e similaridade ponderada do cosseno vetorial; se um modelo de rerank for selecionado, uma pontuação ponderada de reranking substituirá a similaridade ponderada do cosseno vetorial. Esteja ciente de que usar um modelo de rerank aumentará significativamente o tempo de resposta do sistema. Se desejar usar um modelo de rerank, certifique-se de usar um reranker SaaS; se preferir um modelo de rerank implantado localmente, certifique-se de iniciar o RAGFlow com docker-compose-gpu.yml.',
      topK: 'Top-K',
      topKTip:
        'Usado em conjunto com o Rerank model, essa configuração define o número de trechos de texto a serem enviados ao modelo reranking especificado.',
      delimiter: 'Delimitadores para segmentação de texto',
      delimiterTip:
        'Um delimitador ou separador pode consistir em um ou vários caracteres especiais. Se for múltiplos caracteres, certifique-se de que estejam entre crases (``). Por exemplo, se você configurar seus delimitadores assim: \\n`##`;, seus textos serão separados em quebras de linha, símbolos de hash duplo (##) ou ponto e vírgula. Defina os delimitadores apenas após entender o mecanismo de segmentação e particionamento de texto.',
      html4excel: 'Excel para HTML',
      html4excelTip:
        'Use em conjunto com o método de fragmentação General. Quando desativado, arquivos de planilhas (XLSX, XLS (Excel 97-2003)) serão analisados linha por linha como pares chave-valor. Quando ativado, os arquivos de planilhas serão convertidos em tabelas HTML. Se a tabela original tiver mais de 12 linhas, o sistema dividirá automaticamente em várias tabelas HTML a cada 12 linhas. Para mais informações, consulte https://ragflow.io/docs/dev/enable_excel2html.',
      autoKeywords: 'Palavras-chave automáticas',
      autoKeywordsTip:
        'Extraia automaticamente N palavras-chave d

... [Content truncated - file is 61865 bytes] ...

ueguês',
      },

      qWeatherTypeOptions: {
        weather: 'Previsão do tempo',
        indices: 'Índice de qualidade de vida relacionado ao clima',
        airquality: 'Qualidade do ar',
      },
      qWeatherUserTypeOptions: {
        free: 'Assinante gratuito',
        paid: 'Assinante pago',
      },

      qWeatherTimePeriodOptions: {
        now: 'Agora',
        '3d': '3 dias',
        '7d': '7 dias',
        '10d': '10 dias',
        '15d': '12 dias',
        '30d': '30 dias',
      },

      publish: 'API',
      exeSQL: 'ExeSQL',
      exeSQLDescription:
        'Um componente que executa consultas SQL em um banco de dados relacional, suportando consultas de MySQL, PostgreSQL ou MariaDB.',

      dbType: 'Tipo de banco de dados',
      database: 'Banco de dados',
      username: 'Nome de usuário',
      host: 'Hospedeiro',
      port: 'Porta',
      password: 'Senha',
      switch: 'Trocar',
      logicalOperator: 'Operador lógico',
      switchOperatorOptions: {
        equal: 'igual',
        notEqual: 'diferente',
        gt: 'Maior que',
        ge: 'Maior ou igual',
        lt: 'Menor que',
        le: 'Menor ou igual',
        contains: 'Contém',
        notContains: 'Não contém',
        startWith: 'Começa com',
        endWith: 'Termina com',
        empty: 'Vazio',
        notEmpty: 'Não vazio',
      },

      switchLogicOperatorOptions: {
        and: 'E',
        or: 'Ou',
      },

      operator: 'Operador',
      value: 'Valor',
      useTemplate: 'Usar este modelo',
      wenCai: 'WenCai',
      queryType: 'Tipo de consulta',
      wenCaiDescription:
        'Um componente que obtém informações financeiras, incluindo preços de ações e notícias de financiamento, de uma ampla variedade de sites financeiros.',

      wenCaiQueryTypeOptions: {
        stock: 'Ação',
        zhishu: 'Índice',
        fund: 'Fundo',
        hkstock: 'Ações de Hong Kong',
        usstock: 'Mercado de ações dos EUA',
        threeboard: 'Mercado OTC Novo',
        conbond: 'Título Conversível',
        insurance: 'Seguro',
        futures: 'Futuros',
        lccp: 'Financiamento',
        foreign_exchange: 'Câmbio',
      },

      akShare: 'AkShare',
      akShareDescription:
        'Um componente que obtém notícias sobre ações de https://www.eastmoney.com/.',

      yahooFinance: 'YahooFinance',
      yahooFinanceDescription:
        'Um componente que consulta informações sobre uma empresa de capital aberto usando seu símbolo de ticker.',

      crawler: 'Rastreador Web',
      crawlerDescription:
        'Um componente que rastreia o código-fonte HTML de um URL especificado.',

      proxy: 'Proxy',
      crawlerResultOptions: {
        html: 'Html',
        markdown: 'Markdown',
        content: 'Conteúdo',
      },

      extractType: 'Tipo de extração',
      info: 'Informações',
      history: 'Histórico',
      financials: 'Financeiro',
      balanceSheet: 'Balanço patrimonial',
      cashFlowStatement: 'Demonstração de fluxo de caixa',
      jin10: 'Jin10',
      jin10Description:
        'Um componente que obtém informações financeiras da Plataforma Aberta Jin10, incluindo notícias, calendários, cotações e referências.',

      flashType: 'Tipo de Flash',
      filter: 'Filtro',
      contain: 'Contém',
      calendarType: 'Tipo de calendário',
      calendarDatashape: 'Formato de dados do calendário',
      symbolsDatatype: 'Tipo de dados de símbolos',
      symbolsType: 'Tipo de símbolos',
      jin10TypeOptions: {
        flash: 'Notícias rápidas',
        calendar: 'Calendário',
        symbols: 'Cotações',
        news: 'Referência',
      },

      jin10FlashTypeOptions: {
        '1': 'Notícias do Mercado',
        '2': 'Notícias de Futuros',
        '3': 'Notícias EUA-Hong Kong',
        '4': 'Notícias A-Share',
        '5': 'Notícias de Commodities & Forex',
      },

      jin10CalendarTypeOptions: {
        cj: 'Calendário de dados macroeconômicos',
        qh: 'Calendário de Futuros',
        hk: 'Calendário do mercado de ações de Hong Kong',
        us: 'Calendário do mercado de ações dos EUA',
      },

      jin10CalendarDatashapeOptions: {
        data: 'Dados',
        event: 'Evento',
        holiday: 'Feriado',
      },

      jin10SymbolsTypeOptions: {
        GOODS: 'Cotações de commodities',
        FOREX: 'Cotações de Forex',
        FUTURE: 'Cotações do mercado internacional',
        CRYPTO: 'Cotações de criptomoedas',
      },

      jin10SymbolsDatatypeOptions: {
        symbols: 'Lista de commodities',
        quotes: 'Últimas cotações do mercado',
      },
      concentrator: 'Concentrador',
      concentratorDescription:
        'Um componente que recebe a saída do componente anterior e a passa como entrada para os componentes subsequentes.',

      tuShare: 'TuShare',
      tuShareDescription:
        'Um componente que obtém resumos de notícias financeiras de sites financeiros principais, auxiliando pesquisas industriais e quantitativas.',

      tuShareSrcOptions: {
        sina: 'Sina',
        wallstreetcn: 'wallstreetcn',
        '10jqka': 'Straight flush',
        eastmoney: 'Eastmoney',
        yuncaijing: 'YUNCAIJING',
        fenghuang: 'FENGHUANG',
        jinrongjie: 'JRJ',
      },

      token: 'Token',
      src: 'Fonte',
      startDate: 'Data de início',
      endDate: 'Data de término',
      keyword: 'Palavra-chave',
      note: 'Nota',
      noteDescription: 'Nota',
      notePlaceholder: 'Por favor, insira uma nota',

      invoke: 'Invocar',
      invokeDescription:
        'Um componente capaz de chamar serviços remotos, usando saídas de outros componentes ou constantes como entradas.',

      url: 'Url',
      method: 'Método',
      timeout: 'Tempo de espera',
      headers: 'Cabeçalhos',
      cleanHtml: 'Limpar HTML',
      cleanHtmlTip:
        'Se a resposta for formatada em HTML e apenas o conteúdo principal for desejado, ative esta opção.',
      invalidUrl:
        'Deve ser uma URL válida ou uma URL com marcadores de posição de variáveis no formato {nome_variável} ou {componente@variável}',

      reference: 'Referência',
      input: 'Entrada',
      output: 'Saída',
      parameter: 'Parâmetro',
      howUseId: 'Como usar o ID do agente?',
      content: 'Conteúdo',
      operationResults: 'Resultados da operação',
      autosaved: 'Autossalvo',
      optional: 'Opcional',
      pasteFileLink: 'Cole o link do arquivo',
      testRun: 'Executar teste',
      template: 'Modelo',
      templateDescription:
        'Um componente que formata a saída de outro componente.',

      emailComponent: 'Email',
      emailDescription: 'Enviar um email para um endereço especificado.',
      smtpServer: 'Servidor SMTP',
      smtpPort: 'Porta SMTP',
      senderEmail: 'Email do remetente',
      authCode: 'Código de autorização',
      senderName: 'Nome do remetente',
      toEmail: 'Email do destinatário',
      ccEmail: 'Email CC',
      emailSubject: 'Assunto',
      emailContent: 'Conteúdo',
      smtpServerRequired: 'Por favor, insira o endereço do servidor SMTP',
      senderEmailRequired: 'Por favor, insira o email do remetente',
      authCodeRequired: 'Por favor, insira o código de autorização',
      toEmailRequired: 'Por favor, insira o email do destinatário',
      emailContentRequired: 'Por favor, insira o conteúdo do email',
      emailSentSuccess: 'Email enviado com sucesso',
      emailSentFailed: 'Falha ao enviar o email',

      dynamicParameters: 'Parâmetros dinâmicos',
      jsonFormatTip:
        'O componente anterior deve fornecer a string JSON no seguinte formato:',
      toEmailTip: 'to_email: Email do destinatário (Obrigatório)',
      ccEmailTip: 'cc_email: Email CC (Opcional)',
      subjectTip: 'subject: Assunto do email (Opcional)',
      contentTip: 'content: Conteúdo do email (Opcional)',
      jsonUploadTypeErrorMessage: 'Por favor, carregue um arquivo json',
      jsonUploadContentErrorMessage: 'Erro no arquivo json',

      iteration: 'Iteração',
      iterationDescription:
        'Este componente primeiramente divide a entrada em um array pelo "delimitador". Realiza os mesmos passos de operação nos elementos do array em sequência até que todos os resultados sejam gerados, o que pode ser entendido como um processador de tarefas em lote. Por exemplo, dentro do nó de tradução de texto longo, se todo o conteúdo for enviado ao nó LLM, o limite de conversação pode ser atingido. O nó anterior pode primeiro dividir o texto longo em fragmentos e cooperar com o nó iterativo para realizar a tradução em lote de cada fragmento para evitar atingir o limite de mensagem do LLM em uma única conversa.',

      delimiterTip:
        'Este delimitador é usado para dividir o texto de entrada em várias partes, cuja saída será realizada como entrada de cada iteração.',

      delimiterOptions: {
        comma: 'Vírgula',
        lineBreak: 'Quebra de linha',
        tab: 'Tabulação',
        underline: 'Sublinhado',
        diagonal: 'Forward slash',
        minus: 'Dash',
        semicolon: 'Ponto e vírgula',
      },
      addVariable: 'Adicionar variável',
      variableSettings: 'Configurações da variável',
      systemPrompt: 'Prompt do sistema',
      addCategory: 'Adicionar categoria',
      categoryName: 'Nome da categoria',
      nextStep: 'Próximo passo',
      prompt: 'Prompt',
      promptTip:
        'Use o prompt do sistema para descrever a tarefa para o LLM, especificar como ele deve responder e esboçar outros requisitos diversos. O prompt do sistema é frequentemente usado em conjunto com chaves (variáveis), que servem como várias entradas de dados para o LLM. Use uma barra `/` ou o botão (x) para mostrar as chaves a serem usadas.',
      promptMessage: 'O prompt é obrigatório',
      runningHintText: 'está rodando...🕞',
    },
    footer: {
      profile: 'Todos os direitos reservados @ React',
    },
    layout: {
      file: 'arquivo',
      knowledge: 'conhecimento',
      chat: 'bate-papo',
    },
  },
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/locales/pt-br.ts` is located in the `web/src/locales` directory.

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
- [ru.ts](ru.ts_docs.md)
- [until.ts](until.ts_docs.md)
- [vi.ts](vi.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
