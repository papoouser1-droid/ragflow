# Documentation: web/src/locales/de.ts

## File Metadata

- **Path**: `web/src/locales/de.ts`
- **Size**: 72207 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/locales/de.ts`.

## Original Source Code

```ts
export default {
  translation: {
    common: {
      delete: 'Löschen',
      deleteModalTitle:
        'Sind Sie sicher, dass Sie diesen Eintrag löschen möchten?',
      ok: 'Ja',
      cancel: 'Nein',
      total: 'Gesamt',
      rename: 'Umbenennen',
      name: 'Name',
      save: 'Speichern',
      namePlaceholder: 'Bitte Namen eingeben',
      next: 'Weiter',
      create: 'Erstellen',
      edit: 'Bearbeiten',
      upload: 'Hochladen',
      english: 'Englisch',
      portugueseBr: 'Portugiesisch (Brasilien)',
      chinese: 'Vereinfachtes Chinesisch',
      traditionalChinese: 'Traditionelles Chinesisch',
      german: 'Deutsch',
      language: 'Sprache',
      languageMessage: 'Bitte geben Sie Ihre Sprache ein!',
      languagePlaceholder: 'Wählen Sie Ihre Sprache',
      copy: 'Kopieren',
      copied: 'Kopiert',
      comingSoon: 'Demnächst verfügbar',
      download: 'Herunterladen',
      close: 'Schließen',
      preview: 'Vorschau',
      move: 'Verschieben',
      warn: 'Warnung',
      action: 'Aktion',
      s: 'S',
      pleaseSelect: 'Bitte auswählen',
      pleaseInput: 'Bitte eingeben',
      submit: 'Absenden',
      embedIntoSite: 'In Webseite einbetten',
      previousPage: 'Zurück',
      nextPage: 'Weiter',
    },
    login: {
      login: 'Anmelden',
      signUp: 'Registrieren',
      loginDescription: 'Wir freuen uns, Sie wiederzusehen!',
      registerDescription: 'Schön, Sie an Bord zu haben!',
      emailLabel: 'E-Mail',
      emailPlaceholder: 'Bitte E-Mail eingeben',
      passwordLabel: 'Passwort',
      passwordPlaceholder: 'Bitte Passwort eingeben',
      rememberMe: 'Angemeldet bleiben',
      signInTip: 'Noch kein Konto?',
      signUpTip: 'Bereits ein Konto?',
      nicknameLabel: 'Spitzname',
      nicknamePlaceholder: 'Bitte Spitznamen eingeben',
      register: 'Konto erstellen',
      continue: 'Fortfahren',
      title: 'Beginnen Sie mit dem Aufbau Ihrer intelligenten Assistenten.',
      description:
        'Registrieren Sie sich kostenlos, um führende RAG-Technologie zu erkunden. Erstellen Sie Wissensdatenbanken und KIs, um Ihr Unternehmen zu stärken.',
      review: 'von über 500 Bewertungen',
    },
    header: {
      knowledgeBase: 'Wissensdatenbank',
      chat: 'Chat',
      register: 'Registrieren',
      signin: 'Anmelden',
      home: 'Startseite',
      setting: 'Benutzereinstellungen',
      logout: 'Abmelden',
      fileManager: 'Dateiverwaltung',
      flow: 'Agent',
      search: 'Suche',
    },
    knowledgeList: {
      welcome: 'Willkommen zurück',
      description: 'Welche Wissensdatenbanken möchten Sie heute nutzen?',
      createKnowledgeBase: 'Wissensdatenbank erstellen',
      name: 'Name',
      namePlaceholder: 'Bitte Namen eingeben!',
      doc: 'Dokumente',
      searchKnowledgePlaceholder: 'Suchen',
      noMoreData: `Das war's. Nichts mehr zu sehen.`,
    },
    knowledgeDetails: {
      dataset: 'Datensatz',
      testing: 'Abruftest',
      files: 'Dateien',
      configuration: 'Konfiguration',
      knowledgeGraph: 'Wissensgraph',
      name: 'Name',
      namePlaceholder: 'Bitte Namen eingeben!',
      doc: 'Dokumente',
      datasetDescription:
        '😉 Bitte warten Sie, bis die Analyse Ihrer Datei abgeschlossen ist, bevor Sie einen KI-gestützten Chat starten.',
      addFile: 'Datei hinzufügen',
      searchFiles: 'Durchsuchen Sie Ihre Dateien',
      localFiles: 'Lokale Dateien',
      emptyFiles: 'Leere Datei erstellen',
      webCrawl: 'Web-Crawling',
      chunkNumber: 'Chunk-Anzahl',
      uploadDate: 'Hochladedatum',
      chunkMethod: 'Chunk-Methode',
      enabled: 'Aktiviert',
      disabled: 'Deaktiviert',
      action: 'Aktion',
      parsingStatus: 'Analysestatus',
      parsingStatusTip:
        'Die Verarbeitungszeit für Dokumente variiert je nach mehreren Faktoren. Das Aktivieren von Funktionen wie Knowledge Graph, RAPTOR, automatischer Frage- oder Schlüsselwort-Extraktion verlängert die Bearbeitungszeit deutlich. Wenn der Fortschrittsbalken stehen bleibt, konsultieren Sie bitte diese beiden FAQs: https://ragflow.io/docs/dev/faq#why-does-my-document-parsing-stall-at-under-one-percent.',
      processBeginAt: 'Beginn',
      processDuration: 'Dauer',
      progressMsg: 'Fortschritt',
      testingDescription:
        'Führen Sie einen Abruftest durch, um zu prüfen, ob RAGFlow die beabsichtigten Inhalte für das LLM wiederherstellen kann.',
      similarityThreshold: 'Ähnlichkeitsschwelle',
      similarityThresholdTip:
        'RAGFlow verwendet entweder eine Kombination aus gewichteter Schlüsselwortähnlichkeit und gewichteter Vektorkosinus-Ähnlichkeit oder eine Kombination aus gewichteter Schlüsselwortähnlichkeit und gewichteter Neuordnungsbewertung während des Abrufs. Dieser Parameter legt den Schwellenwert für Ähnlichkeiten zwischen der Benutzeranfrage und den Chunks fest. Jeder Chunk mit einer Ähnlichkeitsbewertung unter diesem Schwellenwert wird von den Ergebnissen ausgeschlossen. Standardmäßig ist der Schwellenwert auf 0,2 festgelegt. Das bedeutet, dass nur Textblöcke mit einer hybriden Ähnlichkeitsbewertung von 20 oder höher abgerufen werden.',
      vectorSimilarityWeight: 'Schlüsselwortähnlichkeitsgewicht',
      vectorSimilarityWeightTip:
        'Dies legt das Gewicht der Schlüsselwortähnlichkeit im kombinierten Ähnlichkeitswert fest, entweder in Verbindung mit der Vektorkosinus-Ähnlichkeit oder mit der Neuordnungsbewertung. Die Summe der beiden Gewichte muss 1,0 ergeben.',
      testText: 'Testtext',
      testTextPlaceholder: 'Geben Sie hier Ihre Frage ein!',
      testingLabel: 'Testen',
      similarity: 'Hybride Ähnlichkeit',
      termSimilarity: 'Begriffsähnlichkeit',
      vectorSimilarity: 'Vektorähnlichkeit',
      hits: 'Treffer',
      view: 'Ansehen',
      filesSelected: 'Dateien ausgewählt',
      upload: 'Hochladen',
      run: 'Analysieren',
      runningStatus0: 'AUSSTEHEND',
      runningStatus1: 'WIRD ANALYSIERT',
      runningStatus2: 'ABGEBROCHEN',
      runningStatus3: 'ERFOLGREICH',
      runningStatus4: 'FEHLGESCHLAGEN',
      pageRanges: 'Seitenbereiche',
      pageRangesTip:
        'Bereich der zu analysierenden Seiten; Seiten außerhalb dieses Bereichs werden nicht verarbeitet.',
      fromPlaceholder: 'von',
      fromMessage: 'Anfangsseitennummer fehlt',
      toPlaceholder: 'bis',
      toMessage: 'Endseitennummer fehlt (ausgeschlossen)',
      layoutRecognize: 'Dokumentenparser',
      layoutRecognizeTip:
        'Verwendet ein visuelles Modell für die PDF-Layout-Analyse, um Dokumententitel, Textblöcke, Bilder und Tabellen effektiv zu lokalisieren. Wenn die einfache Option gewählt wird, wird nur der reine Text im PDF abgerufen. Bitte beachten Sie, dass diese Option derzeit NUR für PDF-Dokumente funktioniert. Weitere Informationen finden Sie unter https://ragflow.io/docs/dev/select_pdf_parser.',
      taskPageSize: 'Aufgabenseitengröße',
      taskPageSizeMessage: 'Bitte geben Sie die Größe der Aufgabenseite ein!',
      taskPageSizeTip:
        'Während der Layouterkennung wird eine PDF-Datei in Chunks aufgeteilt und parallel verarbeitet, um die Verarbeitungsgeschwindigkeit zu erhöhen. Dieser Parameter legt die Größe jedes Chunks fest. Eine größere Chunk-Größe verringert die Wahrscheinlichkeit, dass fortlaufender Text zwischen den Seiten aufgeteilt wird.',
      addPage: 'Seite hinzufügen',
      greaterThan: 'Der aktuelle Wert muss größer als "bis" sein!',
      greaterThanPrevious:
        'Der aktuelle Wert muss größer als der vorherige "bis"-Wert sein!',
      selectFiles: 'Dateien auswählen',
      changeSpecificCategory: 'Spezifische Kategorie ändern',
      uploadTitle: 'Ziehen Sie Ihre Datei hierher, um sie hochzuladen',
      uploadDescription:
        'RAGFlow unterstützt das Hochladen von Dateien einzeln oder in Batches. Für lokal bereitgestelltes RAGFlow: Die maximale Dateigröße pro Upload beträgt 1 GB, mit einem Batch-Upload-Limit von 32 Dateien. Es gibt keine Begrenzung der Gesamtanzahl an Dateien pro Konto. Für demo.ragflow.io: Die maximale Dateigröße pro Upload beträgt 10 MB, wobei jede Datei nicht größer als 10 MB sein darf und maximal 128 Dateien pro Konto erlaubt sind.',
      chunk: 'Chunk',
      bulk: 'Masse',
      cancel: 'Abbrechen',
      rerankModel: 'Neuordnungsmodell',
      rerankPlaceholder: 'Bitte auswählen',
      rerankTip:
        'Wenn leer gelassen, verwendet RAGFlow eine Kombination aus gewichteter Schlüsselwortähnlichkeit und gewichteter Vektorkosinus-Ähnlichkeit; wenn ein Neuordnungsmodell ausgewählt wird, ersetzt eine gewichtete Neuordnungsbewertung die gewichtete Vektorkosinus-Ähnlichkeit. Bitte beachten Sie, dass die Verwendung eines Neuordnungsmodells die Antwortzeit des Systems erheblich erhöht.',
      topK: 'Top-K',
      topKTip:
        'In Verbindung mit dem Rerank model wird mit dieser Einstellung die Anzahl der Textblöcke festgelegt, die an das angegebene reranking model gesendet werden.',
      delimiter: 'Trennzeichen für Textsegmentierung',
      delimiterTip:
        'Ein Trennzeichen oder Separator kann aus einem oder mehreren Sonderzeichen bestehen. Bei mehreren Zeichen stellen Sie sicher, dass sie in Backticks (` `) eingeschlossen sind. Wenn Sie beispielsweise Ihre Trennzeichen so konfigurieren: \\n`##`;, dann werden Ihre Texte an Zeilenumbrüchen, doppelten Rautenzeichen (##) oder Semikolons getrennt. Setzen Sie Trennzeichen nur nachdem Sie das Mechanismus der Textsegmentierung und -chunking verstanden haben.',
      html4excel: 'Excel zu HTML',
      html4excelTip:
        'Verwenden Sie dies zusammen mit der General-Schnittmethode. Wenn deaktiviert, werden Tabellenkalkulationsdateien (XLSX, XLS (Excel 97-2003)) zeilenweise in Schlüssel-Wert-Paare analysiert. Wenn aktiviert, werden Tabellenkalkulationsdateien in HTML-Tabellen umgewandelt. Wenn die ursprüngliche Tabelle mehr als 12 Zeilen enthält, teilt das System sie automatisch alle 12 Zeilen in mehrere HTML-Tabellen auf. Für weitere Informationen siehe ht

... [Content truncated - file is 71557 bytes] ...

': '7 Tage',
        '10d': '10 Tage',
        '15d': '12 Tage',
        '30d': '30 Tage',
      },
      publish: 'API',
      exeSQL: 'ExeSQL',
      exeSQLDescription:
        'Eine Komponente, die SQL-Abfragen in einer relationalen Datenbank ausführt und Abfragen von MySQL, PostgreSQL oder MariaDB unterstützt.',
      dbType: 'Datenbanktyp',
      database: 'Datenbank',
      username: 'Benutzername',
      host: 'Host',
      port: 'Port',
      password: 'Passwort',
      switch: 'Schalter',
      logicalOperator: 'Logischer Operator',
      switchOperatorOptions: {
        equal: 'Gleich',
        notEqual: 'Ungleich',
        gt: 'Größer als',
        ge: 'Größer gleich',
        lt: 'Kleiner als',
        le: 'Kleiner gleich',
        contains: 'Enthält',
        notContains: 'Enthält nicht',
        startWith: 'Beginnt mit',
        endWith: 'Endet mit',
        empty: 'Ist leer',
        notEmpty: 'Nicht leer',
      },
      switchLogicOperatorOptions: {
        and: 'UND',
        or: 'ODER',
      },
      operator: 'Operator',
      value: 'Wert',
      useTemplate: 'Diese Vorlage verwenden',
      wenCai: 'WenCai',
      queryType: 'Abfragetyp',
      wenCaiDescription:
        'Eine Komponente, die Finanzinformationen, einschließlich Aktienkursen und Finanzierungsnachrichten, von einer Vielzahl von Finanzwebsites abruft.',
      wenCaiQueryTypeOptions: {
        stock: 'Aktie',
        zhishu: 'Index',
        fund: 'Fonds',
        hkstock: 'Hongkong-Aktien',
        usstock: 'US-Aktienmarkt',
        threeboard: 'Neuer OTC-Markt',
        conbond: 'Wandelanleihe',
        insurance: 'Versicherung',
        futures: 'Futures',
        lccp: 'Finanzierung',
        foreign_exchange: 'Fremdwährung',
      },
      akShare: 'AkShare',
      akShareDescription:
        'Eine Komponente, die Nachrichten über Aktien von https://www.eastmoney.com/ abruft.',
      yahooFinance: 'YahooFinance',
      yahooFinanceDescription:
        'Eine Komponente, die Informationen über ein börsennotiertes Unternehmen anhand seines Tickersymbols abfragt.',
      crawler: 'Web-Crawler',
      crawlerDescription:
        'Eine Komponente, die HTML-Quellcode von einer angegebenen URL crawlt.',
      proxy: 'Proxy',
      crawlerResultOptions: {
        html: 'Html',
        markdown: 'Markdown',
        content: 'Inhalt',
      },
      extractType: 'Extraktionstyp',
      info: 'Info',
      history: 'Verlauf',
      financials: 'Finanzen',
      balanceSheet: 'Bilanz',
      cashFlowStatement: 'Kapitalflussrechnung',
      jin10: 'Jin10',
      jin10Description:
        'Eine Komponente, die Finanzinformationen von der Jin10 Open Platform abruft, einschließlich Nachrichtenaktualisierungen, Kalendern, Kursen und Referenzen.',
      flashType: 'Flash-Typ',
      filter: 'Filter',
      contain: 'Enthalten',
      calendarType: 'Kalendertyp',
      calendarDatashape: 'Kalender-Datenform',
      symbolsDatatype: 'Symboldatentyp',
      symbolsType: 'Symboltyp',
      jin10TypeOptions: {
        flash: 'Schnellnachrichten',
        calendar: 'Kalender',
        symbols: 'Kurse',
        news: 'Referenz',
      },
      jin10FlashTypeOptions: {
        '1': 'Marktnachrichten',
        '2': 'Futures-Nachrichten',
        '3': 'US-Hongkong-Nachrichten',
        '4': 'A-Aktien-Nachrichten',
        '5': 'Rohstoff- und Devisennachrichten',
      },
      jin10CalendarTypeOptions: {
        cj: 'Makroökonomischer Datenkalender',
        qh: 'Futures-Kalender',
        hk: 'Hongkong-Aktienmarktkalender',
        us: 'US-Aktienmarktkalender',
      },
      jin10CalendarDatashapeOptions: {
        data: 'Daten',
        event: 'Ereignis',
        holiday: 'Feiertag',
      },
      jin10SymbolsTypeOptions: {
        GOODS: 'Rohstoffkurse',
        FOREX: 'Devisenkurse',
        FUTURE: 'Internationale Marktkurse',
        CRYPTO: 'Kryptowährungskurse',
      },
      jin10SymbolsDatatypeOptions: {
        symbols: 'Rohstoffliste',
        quotes: 'Aktuelle Marktkurse',
      },
      concentrator: 'Konzentrator',
      concentratorDescription:
        'Eine Komponente, die die Ausgabe der vorgelagerten Komponente empfängt und als Eingabe an die nachgelagerten Komponenten weitergibt.',
      tuShare: 'TuShare',
      tuShareDescription:
        'Eine Komponente, die Finanznahrichten-Kurzmeldungen von führenden Finanzwebsites abruft und bei Branchen- und quantitativer Forschung hilft.',
      tuShareSrcOptions: {
        sina: 'Sina',
        wallstreetcn: 'wallstreetcn',
        '10jqka': 'Straight Flush',
        eastmoney: 'Eastmoney',
        yuncaijing: 'YUNCAIJING',
        fenghuang: 'FENGHUANG',
        jinrongjie: 'JRJ',
      },
      token: 'Token',
      src: 'Quelle',
      startDate: 'Startdatum',
      endDate: 'Enddatum',
      keyword: 'Schlüsselwort',
      note: 'Notiz',
      noteDescription: 'Notiz',
      notePlaceholder: 'Bitte geben Sie eine Notiz ein',
      invoke: 'Aufrufen',
      invokeDescription:
        'Eine Komponente, die Remote-Dienste aufrufen kann und dabei die Ausgaben anderer Komponenten oder Konstanten als Eingaben verwendet.',
      url: 'URL',
      method: 'Methode',
      timeout: 'Zeitüberschreitung',
      headers: 'Header',
      cleanHtml: 'HTML bereinigen',
      cleanHtmlTip:
        'Wenn die Antwort im HTML-Format vorliegt und nur der Hauptinhalt gewünscht wird, schalten Sie dies bitte ein.',
      invalidUrl:
        'Muss eine gültige URL oder eine URL mit Variablenplatzhaltern im Format {Variablenname} oder {Komponente@Variable} sein',
      reference: 'Referenz',
      input: 'Eingabe',
      output: 'Ausgabe',
      parameter: 'Parameter',
      howUseId: 'Wie verwendet man die Agenten-ID?',
      content: 'Inhalt',
      operationResults: 'Operationsergebnisse',
      autosaved: 'Automatisch gespeichert',
      optional: 'Optional',
      pasteFileLink: 'Dateilink einfügen',
      testRun: 'Testlauf',
      template: 'Vorlage',
      templateDescription:
        'Eine Komponente, die die Ausgabe anderer Komponenten formatiert. 1. Unterstützt Jinja2-Vorlagen, konvertiert zuerst die Eingabe in ein Objekt und rendert dann die Vorlage, 2. Behält gleichzeitig die ursprüngliche Methode der Verwendung von {parameter} Zeichenkettenersetzung bei',
      emailComponent: 'E-Mail',
      emailDescription: 'Sendet eine E-Mail an eine angegebene Adresse.',
      smtpServer: 'SMTP-Server',
      smtpPort: 'SMTP-Port',
      senderEmail: 'Absender-E-Mail',
      authCode: 'Autorisierungscode',
      senderName: 'Absendername',
      toEmail: 'Empfänger-E-Mail',
      ccEmail: 'CC-E-Mail',
      emailSubject: 'Betreff',
      emailContent: 'Inhalt',
      smtpServerRequired: 'Bitte geben Sie die SMTP-Serveradresse ein',
      senderEmailRequired: 'Bitte geben Sie die Absender-E-Mail ein',
      authCodeRequired: 'Bitte geben Sie den Autorisierungscode ein',
      toEmailRequired: 'Bitte geben Sie die Empfänger-E-Mail ein',
      emailContentRequired: 'Bitte geben Sie den E-Mail-Inhalt ein',
      emailSentSuccess: 'E-Mail erfolgreich gesendet',
      emailSentFailed: 'E-Mail konnte nicht gesendet werden',
      dynamicParameters: 'Dynamische Parameter',
      jsonFormatTip:
        'Die vorgelagerte Komponente sollte einen JSON-String im folgenden Format bereitstellen:',
      toEmailTip: 'to_email: Empfänger-E-Mail (Erforderlich)',
      ccEmailTip: 'cc_email: CC-E-Mail (Optional)',
      subjectTip: 'subject: E-Mail-Betreff (Optional)',
      contentTip: 'content: E-Mail-Inhalt (Optional)',
      jsonUploadTypeErrorMessage: 'Bitte laden Sie eine JSON-Datei hoch',
      jsonUploadContentErrorMessage: 'JSON-Dateifehler',
      iteration: 'Iteration',
      iterationDescription:
        'Diese Komponente teilt zunächst die Eingabe durch "Trennzeichen" in ein Array auf. Führt die gleichen Operationsschritte nacheinander für die Elemente im Array aus, bis alle Ergebnisse ausgegeben sind, was als Aufgaben-Batch-Prozessor verstanden werden kann.\n\nZum Beispiel kann innerhalb des Iterationsknotens für lange Textübersetzungen, wenn der gesamte Inhalt in den LLM-Knoten eingegeben wird, das Limit für eine einzelne Konversation erreicht werden. Der vorgelagerte Knoten kann den langen Text zuerst in mehrere Fragmente aufteilen und mit dem Iterationsknoten zusammenarbeiten, um eine Batch-Übersetzung für jedes Fragment durchzuführen, um zu vermeiden, dass das LLM-Nachrichtenlimit für eine einzelne Konversation erreicht wird.',
      delimiterTip:
        'Dieses Trennzeichen wird verwendet, um den Eingabetext in mehrere Textstücke aufzuteilen, von denen jedes als Eingabeelement jeder Iteration ausgeführt wird.',
      delimiterOptions: {
        comma: 'Komma',
        lineBreak: 'Zeilenumbruch',
        tab: 'Tabulator',
        underline: 'Unterstrich',
        diagonal: 'Schrägstrich',
        minus: 'Bindestrich',
        semicolon: 'Semikolon',
      },
      addVariable: 'Variable hinzufügen',
      variableSettings: 'Variableneinstellungen',
      systemPrompt: 'System-Prompt',
      addCategory: 'Kategorie hinzufügen',
      categoryName: 'Kategoriename',
      nextStep: 'Nächster Schritt',
      datatype: 'MIME-Typ der HTTP-Anfrage',
      insertVariableTip: 'Eingabe / Variablen einfügen',
      prompt: 'Prompt',
      promptTip:
        'Verwenden Sie den Systemprompt, um die Aufgabe für das LLM zu beschreiben, festzulegen, wie es antworten soll, und andere verschiedene Anforderungen zu skizzieren. Der Systemprompt wird oft in Verbindung mit Schlüsseln (Variablen) verwendet, die als verschiedene Dateninputs für das LLM dienen. Verwenden Sie einen Schrägstrich `/` oder die (x)-Schaltfläche, um die zu verwendenden Schlüssel anzuzeigen.',
      promptMessage: 'Prompt ist erforderlich',
      runningHintText: 'läuft...🕞',
    },
    footer: {
      profile: 'Alle Rechte vorbehalten @ React',
    },
    layout: {
      file: 'Datei',
      knowledge: 'Wissen',
      chat: 'Chat',
    },
  },
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/locales/de.ts` is located in the `web/src/locales` directory.

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
- [en.ts](en.ts_docs.md)
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
