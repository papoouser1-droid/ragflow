# Documentation: web/src/locales/fr.ts

## File Metadata

- **Path**: `web/src/locales/fr.ts`
- **Size**: 65668 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/locales/fr.ts`.

## Original Source Code

```ts
export default {
  translation: {
    common: {
      delete: 'Supprimer',
      deleteModalTitle: 'Êtes-vous sûr de vouloir supprimer cet élément ?',
      ok: 'Oui',
      cancel: 'Non',
      total: 'Total',
      rename: 'Renommer',
      name: 'Nom',
      save: 'Enregistrer',
      namePlaceholder: 'Veuillez saisir le nom',
      next: 'Suivant',
      create: 'Créer',
      edit: 'Modifier',
      upload: 'Télécharger',
      english: 'Anglais',
      french: 'Français',
      portugueseBr: 'Portugais (Brésil)',
      chinese: 'Chinois simplifié',
      traditionalChinese: 'Chinois traditionnel',
      language: 'Langue',
      languageMessage: 'Veuillez saisir votre langue !',
      languagePlaceholder: 'Sélectionnez votre langue',
      copy: 'Copier',
      copied: 'Copié',
      comingSoon: 'Bientôt disponible',
      download: 'Télécharger',
      close: 'Fermer',
      preview: 'Aperçu',
      move: 'Déplacer',
      warn: 'Attention',
      action: 'Action',
      s: 'S',
      pleaseSelect: 'Veuillez sélectionner',
      pleaseInput: 'Veuillez saisir',
      submit: 'Soumettre',
      embedIntoSite: 'Intégrer dans la page web',
      previousPage: 'Précédent',
      nextPage: 'Suivant',
      add: 'Ajouter',
      promptPlaceholder:
        'Veuillez saisir ou utilisez / pour insérer rapidement des variables.',
    },
    login: {
      login: 'Se connecter',
      signUp: 'S’inscrire',
      loginDescription: 'Nous sommes ravis de vous revoir !',
      registerDescription: 'Ravi de vous accueillir !',
      emailLabel: 'Email',
      emailPlaceholder: 'Veuillez saisir votre email',
      passwordLabel: 'Mot de passe',
      passwordPlaceholder: 'Veuillez saisir votre mot de passe',
      rememberMe: 'Se souvenir de moi',
      signInTip: 'Pas encore de compte ?',
      signUpTip: 'Déjà un compte ?',
      nicknameLabel: 'Pseudo',
      nicknamePlaceholder: 'Veuillez saisir votre pseudo',
      register: 'Créer un compte',
      continue: 'Continuer',
      title: 'Commencez à créer vos IA.',
      description:
        'Inscrivez-vous gratuitement pour explorer la technologie RAG. Créez des bases de connaissances et des IA pour booster votre activité.',
      review: 'sur plus de 500 avis',
    },
    header: {
      knowledgeBase: 'Base de connaissances',
      chat: 'Discussion',
      register: 'Inscription',
      signin: 'Connexion',
      home: 'Accueil',
      setting: 'Paramètres utilisateur',
      logout: 'Déconnexion',
      fileManager: 'Gestion des fichiers',
      flow: 'Agent',
      search: 'Recherche',
      welcome: 'Bienvenue sur',
    },
    knowledgeList: {
      welcome: 'Bon retour',
      description:
        'Quelles bases de connaissances allez-vous utiliser aujourd’hui ?',
      createKnowledgeBase: 'Créer une base de connaissances',
      name: 'Nom',
      namePlaceholder: 'Veuillez saisir un nom !',
      doc: 'Documents',
      searchKnowledgePlaceholder: 'Rechercher',
      noMoreData: 'C’est tout. Rien de plus.',
    },
    knowledgeDetails: {
      dataset: 'Ensemble de données',
      testing: 'Test de récupération',
      files: 'Fichiers',
      configuration: 'Configuration',
      knowledgeGraph: 'Graphe de connaissances',
      name: 'Nom',
      namePlaceholder: 'Veuillez saisir un nom !',
      doc: 'Documents',
      datasetDescription:
        '😉 Veuillez attendre la fin de l’analyse de vos fichiers avant de démarrer une discussion avec l’IA.',
      addFile: 'Ajouter un fichier',
      searchFiles: 'Rechercher vos fichiers',
      localFiles: 'Fichiers locaux',
      emptyFiles: 'Créer un fichier vide',
      webCrawl: 'Exploration Web',
      chunkNumber: 'Nombre de segments',
      uploadDate: 'Date de téléversement',
      chunkMethod: 'Méthode de segmentation',
      enabled: 'Activé',
      disabled: 'Désactivé',
      action: 'Action',
      parsingStatus: 'Statut d’analyse',
      parsingStatusTip:
        'Le temps d’analyse dépend de plusieurs facteurs. L’activation de fonctions comme le Graphe de connaissances, RAPTOR, l’extraction automatique de mots-clés ou de questions peut considérablement augmenter ce temps. Si la barre de progression reste bloquée, veuillez consulter ces deux FAQ : https: //ragflow.io/docs/dev/faq#why-does-my-document-parsing-stall-at-under-one-percent.',
      processBeginAt: 'Commencé à',
      processDuration: 'Durée',
      progressMsg: 'Progression',
      testingDescription:
        'Effectuez un test de récupération pour vérifier si RAGFlow peut retrouver le contenu pertinent pour le LLM. Si vous avez modifié les paramètres par défaut, comme le poids de similarité ou le seuil de similarité, ces changements ne seront pas automatiquement sauvegardés. Vous devez les appliquer dans les paramètres de votre assistant de chat ou dans le composant agent de récupération.',
      similarityThreshold: 'Seuil de similarité',
      similarityThresholdTip:
        'RAGFlow utilise une combinaison de similarité par mots-clés pondérée et de similarité cosinus vectorielle, ou bien un score de réordonnancement pondéré. Ce paramètre fixe le seuil en dessous duquel un segment est exclu. Par défaut, le seuil est 0.2 (soit 20%).',
      vectorSimilarityWeight: 'Poids de similarité des mots-clés',
      vectorSimilarityWeightTip:
        'Définit l’importance de la similarité par mots-clés dans le score global. Le total des poids doit être de 1.0.',
      testText: 'Texte de test',
      testTextPlaceholder: 'Saisissez votre question ici !',
      testingLabel: 'Test',
      similarity: 'Similarité hybride',
      termSimilarity: 'Similarité de terme',
      vectorSimilarity: 'Similarité vectorielle',
      hits: 'Résultats',
      view: 'Voir',
      filesSelected: 'Fichiers sélectionnés',
      upload: 'Téléverser',
      run: 'Analyser',
      runningStatus0: 'EN ATTENTE',
      runningStatus1: 'EN ANALYSE',
      runningStatus2: 'ANNULÉ',
      runningStatus3: 'SUCCÈS',
      runningStatus4: 'ÉCHEC',
      pageRanges: 'Plages de pages',
      pageRangesTip:
        'Les pages en dehors de cette plage ne seront pas traitées.',
      fromPlaceholder: 'de',
      fromMessage: 'Numéro de page de début manquant',
      toPlaceholder: 'à',
      toMessage: 'Numéro de page de fin manquant (exclu)',
      layoutRecognize: 'Analyseur PDF',
      layoutRecognizeTip:
        'Utilise un modèle visuel pour détecter les titres, blocs de texte, images et tableaux dans les PDF. Si l’option naïve est choisie, seul le texte brut est extrait. Ne fonctionne que sur les fichiers PDF.',
      taskPageSize: 'Taille de page par tâche',
      taskPageSizeMessage: 'Veuillez saisir une taille de page pour la tâche !',
      taskPageSizeTip:
        'Pendant la reconnaissance, un fichier PDF est divisé en segments traités en parallèle. Une taille plus grande réduit les coupures de texte entre pages.',
      addPage: 'Ajouter une page',
      greaterThan: 'La valeur actuelle doit être supérieure à "à" !',
      greaterThanPrevious:
        'La valeur actuelle doit être supérieure à la précédente !',
      selectFiles: 'Sélectionner des fichiers',
      changeSpecificCategory: 'Changer de catégorie spécifique',
      uploadTitle: 'Glissez-déposez votre fichier ici pour le téléverser',
      uploadDescription:
        'Prise en charge du téléversement unique ou en lot. Pour RAGFlow en local : 1 Go max par téléversement, jusqu’à 32 fichiers. Pour demo.ragflow.io : 10 Mo max par fichier uploadDescription128 fichiers au total.',
      chunk: 'Segment',
      bulk: 'En masse',
      cancel: 'Annuler',
      rerankModel: 'Modèle de réordonnancement',
      rerankPlaceholder: 'Veuillez sélectionner',
      rerankTip: `Optionnel. Si vide, RAGFlow utilisera une combinaison de similarités pondérées. Un modèle de réordonnancement remplace la similarité vectorielle. Attention, cela augmente le temps de réponse. Pour un modèle local, utilisez docker-compose-gpu.yml.`,
      topK: 'Top-K',
      topKTip: 'Nombre de segments à envoyer au modèle de réordonnancement.',
      delimiter: 'Délimiteur de texte',
      delimiterTip:
        'Un délimiteur peut être un ou plusieurs caractères spéciaux. Pour plusieurs caractères, encadrez-les de backticks (``). Ex : \\n`##`;',
      html4excel: 'Excel vers HTML',
      html4excelTip:
        'Utilisé avec la méthode "générale". Si désactivé, les tableaux sont convertis en paires clé-valeur. Sinon, ils deviennent des tableaux HTML divisés toutes les 12 lignes.',
      autoKeywords: 'Mots-clés automatiques',
      autoKeywordsTip:
        'Extrait automatiquement N mots-clés par segment. Consomme des tokens. Voir la documentation : https: //ragflow.io/docs/dev/autokeyword_autoquestion.',
      autoQuestions: 'Questions automatiques',
      autoQuestionsTip:
        'Extrait automatiquement N questions par segment. N’interrompt pas l’analyse si une erreur survient. Consomme aussi des tokens. Voir la documentation.',
      redo: 'Voulez-vous supprimer les {{chunkNum}} segments existants ?',
      setMetaData: 'Définir les métadonnées',
      pleaseInputJson: 'Veuillez saisir du JSON',
      documentMetaTips: `<p>Les métadonnées sont au format JSON (non recherchables) et seront ajoutées au prompt du LLM si des segments du document sont utilisés.</p>`,
      metaData: 'Métadonnées',
      deleteDocumentConfirmContent:
        'Ce document est lié au graphe de connaissances. Sa suppression supprime aussi les nœuds associés. Le graphe ne sera mis à jour qu’à la prochaine analyse.',
      plainText: 'Naïf',
      reRankModelWaring:
        'Le modèle de réordonnancement est très consommateur de temps.',
    },
    knowledgeConfiguration: {
      titleDescription:
        'Modifiez ici la configuration de votre base de connaissances, notamment la méthode de découpage.',
      name: 'Nom de la base de connaissances',
      photo: 'Photo de la base de connaissances',
      photoTip: 'Vous pouvez téléverser un fichier de 4 Mo',
      descriptio

... [Content truncated - file is 64171 bytes] ...

ltrer',
      contain: 'Contient',
      calendarType: 'Type de calendrier',
      calendarDatashape: 'Forme des données du calendrier',
      symbolsDatatype: 'Type de données des symboles',
      symbolsType: 'Type de symboles',
      jin10TypeOptions: {
        flash: 'Actualités rapides',
        calendar: 'Calendrier',
        symbols: 'Cotations',
        news: 'Références',
      },
      jin10FlashTypeOptions: {
        '1': 'Actualités du marché',
        '2': 'Actualités des contrats à terme',
        '3': 'Actualités US-Hong Kong',
        '4': 'Actualités actions A',
        '5': 'Actualités matières premières & Forex',
      },
      jin10CalendarTypeOptions: {
        cj: 'Calendrier des données macroéconomiques',
        qh: 'Calendrier des contrats à terme',
        hk: 'Calendrier du marché boursier de Hong Kong',
        us: 'Calendrier du marché boursier américain',
      },
      jin10CalendarDatashapeOptions: {
        data: 'Données',
        event: 'Événement',
        holiday: 'Jour férié',
      },
      jin10SymbolsTypeOptions: {
        GOODS: 'Cotations des matières premières',
        FOREX: 'Cotations Forex',
        FUTURE: 'Cotations des marchés internationaux',
        CRYPTO: 'Cotations des cryptomonnaies',
      },
      jin10SymbolsDatatypeOptions: {
        symbols: 'Liste des matières premières',
        quotes: 'Dernières cotations du marché',
      },
      concentrator: 'Concentrateur',
      concentratorDescription:
        'Un composant qui reçoit la sortie du composant en amont et la transmet en entrée aux composants en aval.',
      tuShare: 'TuShare',
      tuShareDescription:
        'Un composant qui obtient des brèves d’actualités financières depuis des sites financiers grand public, aidant la recherche sectorielle et quantitative.',
      tuShareSrcOptions: {
        sina: 'Sina',
        wallstreetcn: 'wallstreetcn',
        '10jqka': 'Flush royal',
        eastmoney: 'Eastmoney',
        yuncaijing: 'YUNCAIJING',
        fenghuang: 'FENGHUANG',
        jinrongjie: 'JRJ',
      },
      token: 'Token',
      src: 'Source',
      startDate: 'Date de début',
      endDate: 'Date de fin',
      keyword: 'Mot-clé',
      note: 'Note',
      noteDescription: 'Note',
      notePlaceholder: 'Veuillez entrer une note',
      invoke: 'Requête HTTP',
      invokeDescription: `Un composant capable d’appeler des services distants, utilisant les sorties d’autres composants ou des constantes en entrée.`,
      url: 'URL',
      method: 'Méthode',
      timeout: 'Délai d’attente',
      headers: 'En-têtes',
      cleanHtml: 'Nettoyer le HTML',
      cleanHtmlTip:
        'Si la réponse est au format HTML et que seul le contenu principal est souhaité, activez cette option.',
      invalidUrl:
        'Doit être une URL valide ou une URL avec des espaces réservés de variables au format {nom_variable} ou {composant@variable}',
      reference: 'Référence',
      input: 'Entrée',
      output: 'Sortie',
      parameter: 'Paramètre',
      howUseId: 'Comment utiliser l’ID agent ?',
      content: 'Contenu',
      operationResults: 'Résultats de l’opération',
      autosaved: 'Sauvegardé automatiquement',
      optional: 'Optionnel',
      pasteFileLink: 'Coller le lien du fichier',
      testRun: 'Test',
      template: 'Modèle',
      templateDescription:
        'Un composant qui formate la sortie des autres composants. 1. Supporte les templates Jinja2, convertit d’abord l’entrée en objet puis rend le template, 2. Conserve en parallèle la méthode originale de remplacement de chaîne {parameter}',
      emailComponent: 'Email',
      emailDescription: 'Envoyer un email à une adresse spécifiée.',
      smtpServer: 'Serveur SMTP',
      smtpPort: 'Port SMTP',
      senderEmail: 'Email de l’expéditeur',
      authCode: 'Code d’autorisation',
      senderName: 'Nom de l’expéditeur',
      toEmail: 'Email du destinataire',
      ccEmail: 'Email en copie',
      emailSubject: 'Sujet',
      emailContent: 'Contenu',
      smtpServerRequired: 'Veuillez saisir l’adresse du serveur SMTP',
      senderEmailRequired: 'Veuillez saisir l’email de l’expéditeur',
      authCodeRequired: 'Veuillez saisir le code d’autorisation',
      toEmailRequired: 'Veuillez saisir l’email du destinataire',
      emailContentRequired: 'Veuillez saisir le contenu de l’email',
      emailSentSuccess: 'Email envoyé avec succès',
      emailSentFailed: 'Échec de l’envoi de l’email',
      dynamicParameters: 'Paramètres dynamiques',
      jsonFormatTip:
        'Le composant en amont doit fournir une chaîne JSON au format suivant :',
      toEmailTip: 'to_email : Email du destinataire (Obligatoire)',
      ccEmailTip: 'cc_email : Email en copie (Optionnel)',
      subjectTip: 'subject : Sujet de l’email (Optionnel)',
      contentTip: 'content : Contenu de l’email (Optionnel)',
      jsonUploadTypeErrorMessage: 'Veuillez uploader un fichier JSON',
      jsonUploadContentErrorMessage: 'Erreur dans le fichier JSON',
      iteration: 'Itération',
      iterationDescription: `Un composant de boucle qui itère sur un tableau d’entrée et exécute une logique définie pour chaque élément.`,
      delimiterTip: `
      Ce délimiteur est utilisé pour découper le texte d’entrée en plusieurs morceaux, chacun sera traité comme un élément d’entrée pour chaque itération.`,
      delimiterOptions: {
        comma: 'Virgule',
        lineBreak: 'Saut de ligne',
        tab: 'Tabulation',
        underline: 'Souligné',
        diagonal: 'Slash',
        minus: 'Tiret',
        semicolon: 'Point-virgule',
      },
      addVariable: 'Ajouter une variable',
      variableSettings: 'Paramètres des variables',
      systemPrompt: 'Invite système',
      addCategory: 'Ajouter une catégorie',
      categoryName: 'Nom de la catégorie',
      nextStep: 'Étape suivante',
      variableExtractDescription:
        'Extraire les informations utilisateur dans une variable globale durant toute la conversation',
      variableExtract: 'Variables',
      variables: 'Variables',
      variablesTip: `Définir une variable JSON claire avec une valeur vide. Par ex. : {
        "UserCode": "",
        "NumberPhone": ""
      }`,
      datatype: 'Type MIME de la requête HTTP',
      insertVariableTip: `Entrer / Insérer des variables`,
      historyversion: 'Historique des versions',
      filename: 'Nom du fichier',
      version: {
        created: 'Créé',
        details: 'Détails de la version',
        dsl: 'DSL',
        download: 'Télécharger',
        version: 'Version',
        select: 'Aucune version sélectionnée',
      },
      setting: 'Paramètres',
      settings: {
        agentSetting: 'Paramètres de l’agent',
        title: 'Titre',
        description: 'Description',
        upload: 'Téléverser',
        photo: 'Photo',
        permissions: 'Autorisations',
        permissionsTip:
          'Vous pouvez définir ici les autorisations des membres de l’équipe.',
        me: 'Moi',
        team: 'Équipe',
      },
      noMoreData: 'Plus de données',
      searchAgentPlaceholder: 'Rechercher un agent',
      footer: {
        profile: 'Tous droits réservés @ React',
      },
      layout: {
        file: 'Fichier',
        knowledge: 'Connaissances',
        chat: 'Discussion',
      },
      prompt: 'Invite',
      promptTip:
        'Utilisez l’invite système pour décrire la tâche pour le LLM, préciser comment il doit répondre et indiquer d’autres exigences diverses. L’invite système est souvent utilisée avec des clés (variables), qui servent d’entrées de données pour le LLM. Utilisez une barre oblique `/` ou le bouton (x) pour afficher les clés disponibles.',
      promptMessage: 'L’invite est obligatoire',
      infor: 'Informations d’exécution',
      knowledgeBasesTip:
        'Sélectionnez les bases de connaissances à associer à cet assistant de chat, ou choisissez ci-dessous les variables contenant les IDs des bases de connaissances.',
      knowledgeBaseVars: 'Variables des bases de connaissances',
      code: 'Code',
      codeDescription:
        'Permet aux développeurs d’écrire une logique Python personnalisée.',
      inputVariables: 'Variables d’entrée',
      runningHintText: 'en cours d’exécution...🕞',
      openingSwitch: 'Activation de l’accueil',
      openingCopy: 'Message de bienvenue',
      openingSwitchTip:
        'Vos utilisateurs verront ce message d’accueil au début.',
      modeTip: 'Le mode définit comment le workflow est initié.',
      beginInputTip:
        'En définissant des paramètres d’entrée, ce contenu peut être utilisé par d’autres composants dans les processus suivants.',
      query: 'Variables de requête',
      agent: 'Agent',
      agentDescription:
        'Construit des composants agents équipés de raisonnement, d’utilisation d’outils, et de collaboration multi-agent.',
      maxRecords: 'Nombre maximum d’enregistrements',
      createAgent: 'Créer un agent',
      stringTransform: 'Traitement du texte',
      userFillUp: 'En attente de réponse',
      codeExec: 'Code',
      tavilySearch: 'Recherche Tavily',
      tavilySearchDescription: 'Résultats de recherche via le service Tavily.',
      tavilyExtract: 'Extraction Tavily',
      tavilyExtractDescription: 'Extraction Tavily',
      log: 'Journal',
      management: 'Gestion',
      import: 'Importer',
      export: 'Exporter',
      seconds: 'Secondes',
      subject: 'Sujet',
    },
    llmTools: {
      bad_calculator: {
        name: 'Calculatrice',
        description:
          'Un outil pour calculer la somme de deux nombres (donnera une réponse incorrecte)',
        params: {
          a: 'Le premier nombre',
          b: 'Le deuxième nombre',
        },
      },
    },
    modal: {
      okText: 'Confirmer',
      cancelText: 'Annuler',
    },
    mcp: {
      export: 'Exporter',
      import: 'Importer',
      url: 'URL',
      serverType: 'Type de serveur',
      addMCP: 'Ajouter MCP',
      editMCP: 'Modifier MCP',
    },
  },
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/locales/fr.ts` is located in the `web/src/locales` directory.

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
