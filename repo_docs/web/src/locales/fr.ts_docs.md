# File Documentation: web/src/locales/fr.ts

## File Metadata

- **Path**: `web/src/locales/fr.ts`
- **Extension**: `.ts`
- **Lines**: 1,269
- **Characters**: 64,171
- **Size**: 65,668 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
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
      description: 'Description',
      language: 'Langue du document',
      languageMessage: 'Veuillez saisir votre langue !',
      languagePlaceholder: 'Veuillez saisir votre langue !',
      permissions: 'Autorisations',
      embeddingModel: 'Modèle d’embedding',
      chunkTokenNumber: 'Taille de segment recommandée',
      chunkTokenNumberMessage: 'Le nombre de tokens par segment est requis',
      embeddingModelTip:
        'Modèle d’embedding par défaut. Ne peut pas être modifié si la base contient déjà des segments. Pour le changer, vous devez supprimer tous les segments existants.',
      permissionsTip:
        "Si défini sur 'Équipe', tous les membres de votre équipe pourront gérer cette base.",
      chunkTokenNumberTip:
        'Définit un seuil de tokens pour créer un segment. Les textes courts sont regroupés jusqu’à dépasser ce seuil, sauf si un délimiteur est détecté.',
      chunkMethod: 'Méthode de découpage',
      chunkMethodTip: 'Voir les conseils à droite.',
      upload: 'Téléverser',
      english: 'Anglais',
      chinese: 'Chinois',
      portugueseBr: 'Portugais (Brésil)',
      embeddingModelPlaceholder: 'Veuillez sélectionner un modèle d’embedding.',
      chunkMethodPlaceholder: 'Veuillez sélectionner une méthode de découpage.',
      save: 'Enregistrer',
      me: 'Moi uniquement',
      team: 'Équipe',
      cancel: 'Annuler',
      methodTitle: 'Description de la méthode de découpage',
      methodExamples: 'Exemples',
      methodExamplesDescription:
        'Les captures d’écran suivantes sont fournies à titre explicatif.',
      dialogueExamplesTitle: 'voir',
      methodEmpty:
        'Affichera une explication visuelle des catégories de base de connaissances',
      // Les contenus HTML comme "book", "laws", etc. sont laissés en l’état pour ne pas altérer leur structure technique.
      useRaptor: 'Utiliser RAPTOR pour améliorer la récupération',
      useRaptorTip:
        'Activez RAPTOR pour les questions nécessitant plusieurs étapes. Voir https: //ragflow.io/docs/dev/enable_raptor pour plus d’informations.',
      prompt: 'Prompt',
      promptTip:
        'Décrivez la tâche attendue du LLM, ses réponses, ses exigences, etc. Utilisez `/` pour afficher les variables disponibles.',
      promptMessage: 'Le prompt est requis',
      promptText: `Veuillez résumer les paragraphes suivants. Attention aux chiffres, ne pas inventer. Paragraphes suivants : {cluster_content}
  Le contenu à résumer est ci-dessus.`,
      maxToken: 'Nombre maximal de tokens',
      maxTokenTip: 'Nombre maximal de tokens générés par résumé.',
      maxTokenMessage: 'Nombre maximal de tokens requis',
      threshold: 'Seuil',
      thresholdTip:
        'Dans RAPTOR, les segments sont groupés par similarité. Un seuil élevé = petits groupes, un seuil faible = grands groupes.',
      thresholdMessage: 'Le seuil est requis',
      maxCluster: 'Nombre maximal de groupes',
      maxClusterTip: 'Nombre maximal de groupes à créer.',
      maxClusterMessage: 'Le nombre de groupes est requis',
      randomSeed: 'Graine aléatoire',
      randomSeedMessage: 'La graine est requise',
      entityTypes: 'Types d’entités',
      vietnamese: 'Vietnamien',
      pageRank: 'Rang de page',
      pageRankTip:
        'Attribuez un score PageRank plus élevé à certaines bases pour les faire remonter dans les résultats. Voir documentation.',
      tagName: 'Étiquette',
      frequency: 'Fréquence',
      searchTags: 'Rechercher des étiquettes',
      tagCloud: 'Nuage',
      tagTable: 'Tableau',
      tagSet: 'Jeux d’étiquettes',
      tagSetTip: `
    <p>Sélectionnez une ou plusieurs bases pour taguer automatiquement les segments. Voir documentation.</p>
    <p>Les questions utilisateurs seront aussi taguées.</p>
    <p>Différences avec les mots-clés automatiques :</p>
    <ul>
      <li>Les tags viennent d’un ensemble fermé ; les mots-clés sont extraits librement par le LLM.</li>
      <li>Vous devez téléverser les sets de tags au préalable.</li>
      <li>Les mots-clés consomment plus de tokens.</li>
    </ul>`,
      topnTags: 'Top-N Étiquettes',
      tags: 'Étiquettes',
      addTag: 'Ajouter une étiquette',
      useGraphRag: 'Extraire le graphe de connaissances',
      useGraphRagTip:
        'Construit un graphe basé sur les segments de cette base pour répondre à des questions complexes. Voir documentation.',
      graphRagMethod: 'Méthode',
      graphRagMethodTip: `Light : (Par défaut) utilise les prompts de github.com/HKUDS/LightRAG. Moins de consommation.
    General : utilise ceux de github.com/microsoft/graphrag.`,
      resolution: 'Résolution d’entités',
      resolutionTip:
        'Fusionne des entités similaires comme "2025" et "l’année 2025".',
      community: 'Génération de rapports communautaires',
      communityTip: `Un "community" est un groupe d’entités liées. Le LLM peut générer un résumé pour chaque groupe. Voir plus ici : https: //www.microsoft.com/en-us/research/blog/graphrag-improving-global-search-via-dynamic-community-selection/`,
      theDocumentBeingParsedCannotBeDeleted:
        'Le document en cours d’analyse ne peut pas être supprimé',
    },
    chunk: {
      chunk: 'Segment',
      bulk: 'En masse',
      selectAll: 'Tout sélectionner',
      enabledSelected: 'Activer la sélection',
      disabledSelected: 'Désactiver la sélection',
      deleteSelected: 'Supprimer la sélection',
      search: 'Rechercher',
      all: 'Tous',
      enabled: 'Activé',
      disabled: 'Désactivé',
      keyword: 'Mot-clé',
      function: 'Fonction',
      chunkMessage: 'Veuillez saisir une valeur !',
      full: 'Texte complet',
      ellipse: 'Ellipse',
      graph: 'Graphe de connaissances',
      mind: 'Carte mentale',
      question: 'Question',
      questionTip: `S’il y a des questions fournies, l'embedding du segment se basera sur celles-ci.`,
      chunkResult: 'Résultat du segment',
      chunkResultTip: `Affiche les segments utilisés pour l'embedding et la récupération.`,
      enable: 'Activer',
      disable: 'Désactiver',
      delete: 'Supprimer',
    },
    chat: {
      newConversation: 'Nouvelle conversation',
      createAssistant: 'Créer un assistant',
      assistantSetting: 'Paramètres de l’assistant',
      promptEngine: 'Moteur de prompt',
      modelSetting: 'Paramètres du modèle',
      chat: 'Discussion',
      newChat: 'Nouvelle discussion',
      send: 'Envoyer',
      sendPlaceholder: 'Envoyez un message à l’assistant...',
      chatConfiguration: 'Configuration de la discussion',
      chatConfigurationDescription:
        ' Configurez un assistant de chat pour vos ensembles de données sélectionnés (bases de connaissances) ici ! 💕',
      assistantName: 'Nom de l’assistant',
      assistantNameMessage: 'Le nom de l’assistant est requis',
      namePlaceholder: 'ex. Resume Jarvis',
      assistantAvatar: 'Avatar de l’assistant',
      language: 'Langue',
      emptyResponse: 'Réponse vide',
      emptyResponseTip: `Définissez ceci comme réponse si aucun résultat n’est récupéré des bases de connaissances pour votre requête, ou laissez ce champ vide pour permettre au LLM d’improviser lorsqu’aucune information n’est trouvée.`,
      emptyResponseMessage: `La réponse vide sera déclenchée lorsqu’aucune information pertinente n’est récupérée des bases de connaissances. Vous devez vider le champ 'Réponse vide' si aucune base de connaissances n’est sélectionnée.`,
      setAnOpener: 'Message d’accueil',
      setAnOpenerInitial: `Bonjour ! Je suis votre assistant, que puis-je faire pour vous ?`,
      setAnOpenerTip: 'Définissez un message d’accueil pour les utilisateurs.',
      knowledgeBases: 'Bases de connaissances',
      knowledgeBasesMessage: 'Veuillez sélectionner',
      knowledgeBasesTip:
        'Sélectionnez les bases de connaissances à associer à cet assistant de chat. Une base de connaissances vide n’apparaîtra pas dans la liste déroulante.',
      system: 'Prompt système',
      systemInitialValue: `Vous êtes un assistant intelligent. Veuillez résumer le contenu de la base de connaissances pour répondre à la question. Veuillez lister les données de la base de connaissances et répondre en détail. Lorsque tout le contenu de la base de connaissances est sans rapport avec la question, votre réponse doit inclure la phrase "La réponse que vous cherchez ne se trouve pas dans la base de connaissances !" Les réponses doivent prendre en compte l’historique de la discussion.
      Voici la base de connaissances : {knowledge}
      Ce qui précède est la base de connaissances.`,
      systemMessage: 'Veuillez saisir !',
      systemTip:
        'Vos prompts ou instructions pour le LLM, incluant mais non limités à son rôle, la longueur souhaitée, le ton et la langue de ses réponses. Si votre modèle supporte nativement le raisonnement, vous pouvez ajouter //no_thinking dans le prompt pour stopper le raisonnement.',
      topN: 'Top N',
      topNTip: `Tous les segments avec un score de similarité supérieur au 'seuil de similarité' ne seront pas forcément envoyés au LLM. Cela sélectionne les 'Top N' segments parmi ceux récupérés.`,
      variable: 'Variable',
      variableTip: `Utilisé avec les API de gestion d’assistant de chat de RAGFlow, les variables aident à développer des stratégies de prompt système plus flexibles. Les variables définies seront utilisées dans le 'Prompt système' comme partie des prompts pour le LLM. {knowledge
      } est une variable spéciale réservée représentant les segments récupérés des bases de connaissances spécifiées. Toutes les variables doivent être entourées d’accolades {} dans le 'Prompt système'. Voir https: //ragflow.io/docs/dev/set_chat_variables pour plus de détails.`,
      add: 'Ajouter',
      key: 'Clé',
      optional: 'Optionnel',
      operation: 'Action',
      model: 'Modèle',
      modelTip: 'Modèle de chat basé sur un grand modèle de langage',
      modelMessage: 'Veuillez sélectionner !',
      modelEnabledTools: 'Outils activés',
      modelEnabledToolsTip:
        'Veuillez sélectionner un ou plusieurs outils que le modèle de chat peut utiliser. N’a aucun effet pour les modèles ne supportant pas l’appel d’outil.',
      freedom: 'Liberté',
      improvise: 'Improviser',
      precise: 'Précis',
      balance: 'Équilibre',
      freedomTip: `Un raccourci vers les paramètres 'Température', 'Top P', 'Pénalité de présence' et 'Pénalité de fréquence', indiquant le niveau de liberté du modèle. Ce paramètre a trois options : choisissez 'Improviser' pour des réponses plus créatives ; 'Précis' (par défaut) pour des réponses plus conservatrices ; 'Équilibre' est un compromis entre 'Improviser' et 'Précis'.`,
      temperature: 'Température',
      temperatureMessage: 'La température est requise',
      temperatureTip: `Ce paramètre contrôle l’aléa des prédictions du modèle. Une température basse donne des réponses plus conservatrices, une température élevée des réponses plus créatives et variées.`,
      topP: 'Top P',
      topPMessage: 'Top P est requis',
      topPTip: `Aussi appelé "échantillonnage par noyau", ce paramètre fixe un seuil pour sélectionner un ensemble plus restreint de mots les plus probables à échantillonner, en éliminant les moins probables.`,
      presencePenalty: 'Pénalité de présence',
      presencePenaltyMessage: 'La pénalité de présence est requise',
      presencePenaltyTip: `Cela décourage le modèle de répéter les mêmes informations en pénalisant les mots déjà apparus dans la conversation.`,
      frequencyPenalty: 'Pénalité de fréquence',
      frequencyPenaltyMessage: 'La pénalité de fréquence est requise',
      frequencyPenaltyTip: `Similaire à la pénalité de présence, cela réduit la tendance du modèle à répéter fréquemment les mêmes mots.`,
      maxTokens: 'Nombre max de tokens',
      maxTokensMessage: 'Le nombre max de tokens est requis',
      maxTokensTip: `Définit la longueur maximale de la sortie du modèle, mesurée en nombre de tokens (mots ou morceaux de mots). Par défaut 512. Si désactivé, aucune limite maximale n’est imposée, le modèle décide alors du nombre de tokens dans ses réponses.`,
      maxTokensInvalidMessage:
        'Veuillez saisir un nombre valide pour le nombre max de tokens.',
      maxTokensMinMessage:
        'Le nombre max de tokens ne peut pas être inférieur à 0.',
      quote: 'Afficher la citation',
      quoteTip: 'Afficher ou non le texte original en référence.',
      selfRag: 'Self-RAG',
      selfRagTip:
        'Veuillez vous référer à : https: //huggingface.co/papers/2310.11511',
      overview: 'ID de discussion',
      pv: 'Nombre de messages',
      uv: 'Nombre d’utilisateurs actifs',
      speed: 'Vitesse de sortie des tokens',
      tokens: 'Nombre de tokens consommés',
      round: 'Nombre d’interactions de session',
      thumbUp: 'Satisfaction client',
      preview: 'Aperçu',
      embedded: 'Intégré',
      serviceApiEndpoint: 'Point d’API du service',
      apiKey: 'Clé API',
      apiReference: 'Documents API',
      dateRange: 'Plage de dates :',
      backendServiceApi: 'Serveur API',
      createNewKey: 'Créer une nouvelle clé',
      created: 'Créé',
      action: 'Action',
      embedModalTitle: 'Intégrer dans une page web',
      comingSoon: 'Bientôt disponible',
      fullScreenTitle: 'Intégration complète',
      fullScreenDescription:
        'Intégrez l’iframe suivant dans votre site web à l’emplacement désiré',
      partialTitle: 'Intégration partielle',
      extensionTitle: 'Extension Chrome',
      tokenError: 'Veuillez d’abord créer une clé API.',
      betaError:
        'Veuillez d’abord obtenir une clé API RAGFlow depuis la page Paramètres système.',
      searching: 'Recherche en cours...',
      parsing: 'Analyse en cours',
      uploading: 'Téléversement en cours',
      uploadFailed: 'Échec du téléversement',
      regenerate: 'Régénérer',
      read: 'Lire le contenu',
      tts: 'Texte en parole',
      ttsTip:
        'Assurez-vous de sélectionner un modèle TTS dans la page Paramètres avant d’activer cette option pour jouer le texte en audio.',
      relatedQuestion: 'Question associée',
      answerTitle: 'R',
      multiTurn: 'Optimisation multi-tours',
      multiTurnTip:
        'Optimise les requêtes utilisateur en utilisant le contexte d’une conversation multi-tours. Lorsqu’activé, cela consomme des tokens LLM supplémentaires.',
      howUseId: 'Comment utiliser l’ID de discussion ?',
      description: 'Description de l’assistant',
      descriptionPlaceholder: 'ex. Un assistant de chat pour CV.',
      useKnowledgeGraph: 'Utiliser le graphe de connaissances',
      useKnowledgeGraphTip:
        'Utiliser les graphes de connaissances dans les bases sélectionnées lors de la récupération pour les questions multi-sauts. Cela implique des recherches itératives sur entités, relations, et rapports communautaires, augmentant fortement le temps de récupération.',
      keyword: 'Analyse de mots-clés',
      keywordTip: `Utiliser le LLM pour analyser les questions utilisateur, extraire des mots-clés qui seront mis en avant lors du calcul de pertinence. Fonctionne bien avec les requêtes longues mais augmente le temps de réponse.`,
      languageTip:
        'Permet la réécriture de phrases dans la langue spécifiée ou utilise la dernière question si aucune sélection.',
      avatarHidden: 'Cacher l’avatar',
      locale: 'Paramètres régionaux',
      selectLanguage: 'Sélectionner une langue',
      reasoning: 'Raisonnement',
      reasoningTip: `Activer un flux de raisonnement lors de la réponse aux questions, comme dans les modèles Deepseek-R1 ou OpenAI o1. Cela permet au modèle d’accéder à des connaissances externes et de traiter des questions complexes étape par étape, en utilisant des techniques comme le chain-of-thought. Cette méthode améliore la précision des réponses en décomposant les problèmes en étapes gérables, augmentant les performances sur les tâches nécessitant un raisonnement logique et multi-étapes.`,
      tavilyApiKeyTip:
        'Si une clé API est correctement configurée ici, les recherches web basées sur Tavily seront utilisées pour compléter la récupération des bases de connaissances.',
      tavilyApiKeyMessage: 'Veuillez saisir votre clé API Tavily',
      tavilyApiKeyHelp: 'Comment l’obtenir ?',
      crossLanguage: 'Recherche inter-langues',
      crossLanguageTip: `Sélectionnez une ou plusieurs langues pour la recherche inter-langues. Si aucune langue n’est sélectionnée, le système recherche avec la requête originale.`,
    },
    setting: {
      profile: 'Profil',
      avatar: 'Avatar',
      avatarTip: 'Ceci sera affiché sur votre profil.',
      profileDescription:
        'Mettez à jour votre photo et vos informations personnelles ici.',
      maxTokens: 'Nombre maximum de tokens',
      maxTokensMessage: 'Le nombre maximum de tokens est requis',
      maxTokensTip: `Cela définit la longueur maximale de la sortie du modèle, mesurée en nombre de tokens (mots ou morceaux de mots). Par défaut à 512. Si désactivé, la limite maximale de tokens est levée, permettant au modèle de déterminer le nombre de tokens dans ses réponses.`,
      maxTokensInvalidMessage:
        'Veuillez saisir un nombre valide pour le nombre maximum de tokens.',
      maxTokensMinMessage:
        'Le nombre maximum de tokens ne peut pas être inférieur à 0.',
      password: 'Mot de passe',
      passwordDescription:
        'Veuillez entrer votre mot de passe actuel pour le changer.',
      model: 'Fournisseurs de modèles',
      modelDescription:
        'Configurez les paramètres du modèle et la clé API ici.',
      team: 'Équipe',
      system: 'Système',
      logout: 'Déconnexion',
      api: 'API',
      username: "Nom d'utilisateur",
      usernameMessage: "Veuillez saisir votre nom d'utilisateur !",
      photo: 'Votre photo',
      photoDescription: 'Ceci sera affiché sur votre profil.',
      colorSchema: 'Schéma de couleurs',
      colorSchemaMessage: 'Veuillez sélectionner votre schéma de couleurs !',
      colorSchemaPlaceholder: 'sélectionnez votre schéma de couleurs',
      bright: 'Clair',
      dark: 'Sombre',
      timezone: 'Fuseau horaire',
      timezoneMessage: 'Veuillez saisir votre fuseau horaire !',
      timezonePlaceholder: 'sélectionnez votre fuseau horaire',
      email: 'Adresse e-mail',
      emailDescription:
        "Une fois enregistrée, l'adresse e-mail ne peut pas être modifiée.",
      currentPassword: 'Mot de passe actuel',
      currentPasswordMessage: 'Veuillez saisir votre mot de passe !',
      newPassword: 'Nouveau mot de passe',
      changePassword: 'Changer le mot de passe',
      newPasswordMessage: 'Veuillez saisir votre mot de passe !',
      newPasswordDescription:
        'Votre nouveau mot de passe doit contenir plus de 8 caractères.',
      confirmPassword: 'Confirmer le nouveau mot de passe',
      confirmPasswordMessage: 'Veuillez confirmer votre mot de passe !',
      confirmPasswordNonMatchMessage:
        'Le nouveau mot de passe que vous avez saisi ne correspond pas !',
      cancel: 'Annuler',
      addedModels: 'Modèles ajoutés',
      modelsToBeAdded: 'Modèles à ajouter',
      addTheModel: 'Ajouter le modèle',
      apiKey: 'Clé API',
      apiKeyMessage:
        'Veuillez saisir la clé API (pour un modèle déployé localement, ignorez ceci).',
      apiKeyTip:
        'La clé API peut être obtenue en vous enregistrant auprès du fournisseur LLM correspondant.',
      showMoreModels: 'Voir les modèles',
      hideModels: 'Masquer les modèles',
      baseUrl: 'URL de base',
      baseUrlTip:
        "Si votre clé API provient d'OpenAI, ignorez ceci. Tout autre fournisseur intermédiaire fournira cette URL de base avec la clé API.",
      tongyiBaseUrlTip:
        'Pour les utilisateurs chinois, pas besoin de remplir ou utiliser https://dashscope.aliyuncs.com/compatible-mode/v1. Pour les utilisateurs internationaux, utilisez https://dashscope-intl.aliyuncs.com/compatible-mode/v1',
      tongyiBaseUrlPlaceholder:
        "(Utilisateurs internationaux uniquement, veuillez consulter l'astuce)",
      modify: 'Modifier',
      systemModelSettings: 'Définir les modèles par défaut',
      chatModel: 'Modèle de chat',
      chatModelTip:
        'Le modèle de chat par défaut pour chaque base de connaissances nouvellement créée.',
      embeddingModel: "Modèle d'embedding",
      embeddingModelTip:
        "Le modèle d'embedding par défaut pour chaque base de connaissances nouvellement créée. Si vous ne trouvez pas de modèle d'embedding dans la liste déroulante, vérifiez si vous utilisez l'édition RAGFlow slim (qui n'inclut pas les modèles d'embedding) ou consultez https://ragflow.io/docs/dev/supported_models pour voir si votre fournisseur de modèle supporte ce modèle.",
      img2txtModel: 'Modèle Img2txt',
      img2txtModelTip:
        'Le modèle img2txt par défaut pour chaque base de connaissances nouvellement créée. Il décrit une image ou une vidéo. Si vous ne trouvez pas de modèle dans la liste déroulante, consultez https://ragflow.io/docs/dev/supported_models pour voir si votre fournisseur le supporte.',
      sequence2txtModel: 'Modèle Speech2txt',
      sequence2txtModelTip:
        'Le modèle ASR par défaut pour chaque base de connaissances nouvellement créée. Utilisez ce modèle pour traduire les voix en texte correspondant.',
      rerankModel: 'Modèle de rerank',
      rerankModelTip:
        'Le modèle de rerank par défaut pour le rerank des segments. Si vous ne trouvez pas de modèle dans la liste déroulante, consultez https://ragflow.io/docs/dev/supported_models pour voir si votre fournisseur le supporte.',
      ttsModel: 'Modèle TTS',
      ttsModelTip:
        'Le modèle de synthèse vocale par défaut. Si vous ne trouvez pas de modèle dans la liste déroulante, consultez https://ragflow.io/docs/dev/supported_models pour voir si votre fournisseur le supporte.',
      workspace: 'Espace de travail',
      upgrade: 'Mettre à jour',
      addLlmTitle: 'Ajouter LLM',
      editLlmTitle: 'Modifier le modèle {{name}}',
      editModel: 'Modifier le modèle',
      modelName: 'Nom du modèle',
      modelID: 'ID du modèle',
      modelUid: 'UID du modèle',
      modelNameMessage: 'Veuillez saisir le nom de votre modèle !',
      modelType: 'Type de modèle',
      modelTypeMessage: 'Veuillez saisir le type de votre modèle !',
      addLlmBaseUrl: 'URL de base',
      baseUrlNameMessage: 'Veuillez saisir votre URL de base !',
      vision: 'Supporte-t-il la vision ?',
      ollamaLink: 'Comment intégrer {{name}}',
      FishAudioLink: 'Comment utiliser FishAudio',
      TencentCloudLink: 'Comment utiliser TencentCloud ASR',
      volcModelNameMessage: 'Veuillez saisir le nom de votre modèle !',
      addEndpointID: 'EndpointID du modèle',
      endpointIDMessage: "Veuillez saisir l'EndpointID du modèle",
      addArkApiKey: 'VOLC ARK_API_KEY',
      ArkApiKeyMessage: 'Veuillez saisir votre ARK_API_KEY',
      bedrockModelNameMessage: 'Veuillez saisir le nom de votre modèle !',
      addBedrockEngineAK: "CLÉ D'ACCÈS",
      bedrockAKMessage: "Veuillez saisir votre CLÉ D'ACCÈS",
      addBedrockSK: 'CLÉ SECRÈTE',
      bedrockSKMessage: 'Veuillez saisir votre CLÉ SECRÈTE',
      bedrockRegion: 'Région AWS',
      bedrockRegionMessage: 'Veuillez sélectionner !',
      'us-east-1': 'US Est (Virginie du Nord)',
      'us-west-2': 'US Ouest (Oregon)',
      'ap-southeast-1': 'Asie Pacifique (Singapour)',
      'ap-northeast-1': 'Asie Pacifique (Tokyo)',
      'eu-central-1': 'Europe (Francfort)',
      'us-gov-west-1': 'AWS GovCloud (US-Ouest)',
      'ap-southeast-2': 'Asie Pacifique (Sydney)',
      addHunyuanSID: 'ID secret Hunyuan',
      HunyuanSIDMessage: 'Veuillez saisir votre ID secret',
      addHunyuanSK: 'Clé secrète Hunyuan',
      HunyuanSKMessage: 'Veuillez saisir votre clé secrète',
      addTencentCloudSID: 'ID secret TencentCloud',
      TencentCloudSIDMessage: 'Veuillez saisir votre ID secret',
      addTencentCloudSK: 'Clé secrète TencentCloud',
      TencentCloudSKMessage: 'Veuillez saisir votre clé secrète',
      SparkModelNameMessage: 'Veuillez sélectionner un modèle Spark',
      addSparkAPIPassword: 'Mot de passe API Spark',
      SparkAPIPasswordMessage: 'Veuillez saisir votre mot de passe API',
      addSparkAPPID: 'ID application Spark',
      SparkAPPIDMessage: 'Veuillez saisir votre ID application',
      addSparkAPISecret: 'Secret API Spark',
      SparkAPISecretMessage: 'Veuillez saisir votre secret API',
      addSparkAPIKey: 'Clé API Spark',
      SparkAPIKeyMessage: 'Veuillez saisir votre clé API',
      yiyanModelNameMessage: 'Veuillez saisir le nom du modèle',
      addyiyanAK: 'Clé API yiyan',
      yiyanAKMessage: 'Veuillez saisir votre clé API',
      addyiyanSK: 'Clé secrète yiyan',
      yiyanSKMessage: 'Veuillez saisir votre clé secrète',
      FishAudioModelNameMessage:
        'Veuillez donner un nom à votre modèle de synthèse vocale',
      addFishAudioAK: 'Clé API Fish Audio',
      addFishAudioAKMessage: 'Veuillez saisir votre clé API',
      addFishAudioRefID: 'ID de référence FishAudio',
      addFishAudioRefIDMessage:
        "Veuillez saisir l'ID de référence (laissez vide pour utiliser le modèle par défaut).",
      GoogleModelIDMessage: 'Veuillez saisir votre ID modèle !',
      addGoogleProjectID: 'ID du projet',
      GoogleProjectIDMessage: 'Veuillez saisir votre ID projet',
      addGoogleServiceAccountKey:
        "Clé du compte de service (laissez vide si vous utilisez les identifiants par défaut de l'application)",
      GoogleServiceAccountKeyMessage:
        'Veuillez saisir la clé du compte de service Google Cloud en format base64',
      addGoogleRegion: 'Région Google Cloud',
      GoogleRegionMessage: 'Veuillez saisir la région Google Cloud',
      modelProvidersWarn: `Veuillez ajouter d'abord à la fois un modèle d'embedding et un LLM dans <b>Paramètres > Fournisseurs de modèles</b>. Ensuite, définissez-les dans 'Définir les modèles par défaut'.`,
      apiVersion: 'Version API',
      apiVersionMessage: "Veuillez saisir la version de l'API",
      add: 'Ajouter',
      updateDate: 'Date de mise à jour',
      role: 'Rôle',
      invite: 'Inviter',
      agree: 'Accepter',
      refuse: 'Refuser',
      teamMembers: "Membres de l'équipe",
      joinedTeams: 'Équipes rejointes',
      sureDelete: 'Êtes-vous sûr de vouloir supprimer ce membre ?',
      quit: 'Quitter',
      sureQuit:
        "Êtes-vous sûr de vouloir quitter l'équipe que vous avez rejointe ?",
      secretKey: 'Clé secrète',
      publicKey: 'Clé publique',
      secretKeyMessage: 'Veuillez entrer la clé secrète',
      publicKeyMessage: 'Veuillez entrer la clé publique',
      hostMessage: "Veuillez entrer l'hôte",
      configuration: 'Configuration',
      langfuseDescription:
        'Traces, évaluations, gestion des prompts et métriques pour déboguer et améliorer votre application LLM.',
      viewLangfuseSDocumentation: 'Voir la documentation de Langfuse',
      view: 'Voir',
      modelsToBeAddedTooltip:
        'Si votre fournisseur de modèle n\'est pas listé mais prétend être "compatible OpenAI", sélectionnez la carte compatible OpenAI-API pour ajouter le(s) modèle(s) pertinent(s).',
      mcp: 'MCP',
    },
    message: {
      registered: 'Enregistré !',
      logout: 'Déconnexion',
      logged: 'Connecté !',
      pleaseSelectChunk: 'Veuillez sélectionner un segment !',
      registerDisabled: "L'inscription des utilisateurs est désactivée",
      modified: 'Modifié',
      created: 'Créé',
      deleted: 'Supprimé',
      renamed: 'Renommé',
      operated: 'Opéré',
      updated: 'Mis à jour',
      uploaded: 'Téléversé',
      200: 'Le serveur a renvoyé les données demandées avec succès.',
      201: 'Création ou modification des données réussie.',
      202: 'La requête a été mise en file d’attente en arrière-plan (tâche asynchrone).',
      204: 'Données supprimées avec succès.',
      400: 'Erreur dans la requête émise, le serveur n’a pas créé ou modifié les données.',
      401: 'Veuillez vous reconnecter.',
      403: "L'utilisateur est autorisé, mais l'accès est interdit.",
      404: "La requête concernait un enregistrement inexistant, le serveur n'a pas effectué l'opération.",
      406: 'Le format demandé n’est pas disponible.',
      410: 'La ressource demandée a été définitivement supprimée et ne sera plus disponible.',
      413: 'La taille totale des fichiers téléversés d’un coup est trop grande.',
      422: "Une erreur de validation s'est produite lors de la création de l'objet.",
      500: 'Erreur serveur, veuillez vérifier le serveur.',
      502: 'Erreur de passerelle.',
      503: 'Service indisponible, le serveur est temporairement surchargé ou en maintenance.',
      504: 'Délai d’attente de la passerelle dépassé.',
      requestError: 'Erreur de requête',
      networkAnomalyDescription:
        'Il y a une anomalie sur votre réseau et vous ne pouvez pas vous connecter au serveur.',
      networkAnomaly: 'Anomalie réseau',
      hint: 'Astuce',
    },
    fileManager: {
      name: 'Nom',
      uploadDate: 'Date de téléversement',
      knowledgeBase: 'Base de connaissances',
      size: 'Taille',
      action: 'Action',
      addToKnowledge: 'Lier à la base de connaissances',
      pleaseSelect: 'Veuillez sélectionner',
      newFolder: 'Nouveau dossier',
      file: 'Fichier',
      uploadFile: 'Téléverser un fichier',
      parseOnCreation: 'Analyser à la création',
      directory: 'Répertoire',
      uploadTitle: 'Glissez-déposez votre fichier ici pour téléverser',
      uploadDescription:
        'Prise en charge du téléversement de fichiers uniques ou en lot. Pour un déploiement local de RAGFlow : la taille totale des fichiers par téléversement est limitée à 1 Go, avec un maximum de 32 fichiers par lot. Il n’y a pas de limite sur le nombre total de fichiers par compte. Pour demo.ragflow.io, la taille totale des fichiers par téléversement est limitée à 10 Mo, chaque fichier ne devant pas dépasser 10 Mo, avec un maximum de 128 fichiers par compte.',
      local: 'Téléversements locaux',
      s3: 'Téléversements S3',
      preview: 'Aperçu',
      fileError: 'Erreur de fichier',
      uploadLimit:
        'Chaque fichier ne doit pas dépasser 10 Mo, et le nombre total de fichiers ne doit pas dépasser 128.',
      destinationFolder: 'Dossier de destination',
    },
    flow: {
      cite: 'Citation',
      citeTip: 'Astuce citation',
      name: 'Nom',
      nameMessage: 'Veuillez saisir un nom',
      description: 'Description',
      examples: 'Exemples',
      to: 'Vers',
      msg: 'Messages',
      msgTip:
        'Afficher le contenu de la variable du composant en amont ou le texte que vous saisissez vous-même.',
      messagePlaceholder:
        "Veuillez entrer le contenu de votre message, utilisez '/' pour insérer rapidement des variables.",
      messageMsg: 'Veuillez saisir un message ou supprimer ce champ.',
      addField: 'Ajouter une option',
      addMessage: 'Ajouter un message',
      loop: 'Boucle',
      loopTip:
        "Boucle est la limite supérieure du nombre de répétitions du composant actuel. Lorsque le nombre de boucles dépasse cette valeur, cela signifie que le composant ne peut pas terminer la tâche actuelle, veuillez réoptimiser l'agent.",
      yes: 'Oui',
      no: 'Non',
      key: 'Clé',
      componentId: 'ID du composant',
      add: 'Ajouter',
      operation: 'Opération',
      run: 'Exécuter',
      save: 'Enregistrer',
      title: 'ID :',
      beginDescription: "C'est ici que le flux commence.",
      answerDescription:
        "Un composant qui sert d'interface entre l'humain et le bot, recevant les entrées utilisateur et affichant les réponses de l'agent.",
      retrievalDescription:
        "Un composant qui récupère des informations à partir de bases de connaissances spécifiées (ensembles de données). Assurez-vous que les bases sélectionnées utilisent le même modèle d'encodage.",
      generateDescription:
        'Un composant qui demande au LLM de générer des réponses. Assurez-vous que le prompt est correctement configuré.',
      categorizeDescription:
        'Un composant qui utilise le LLM pour classer les entrées utilisateur en catégories prédéfinies. Assurez-vous de spécifier le nom, la description et les exemples pour chaque catégorie, ainsi que le composant suivant correspondant.',
      relevantDescription:
        "Un composant qui utilise le LLM pour évaluer si la sortie en amont est pertinente par rapport à la dernière requête de l'utilisateur. Assurez-vous de spécifier le composant suivant pour chaque résultat de jugement.",
      rewriteQuestionDescription:
        'Un composant qui réécrit une requête utilisateur du composant Interact, basé sur le contexte des dialogues précédents.',
      messageDescription:
        'Ce composant renvoie la sortie finale du workflow avec un contenu de message prédéfini.',
      keywordDescription:
        "Un composant qui récupère les N meilleurs résultats de recherche à partir de la saisie de l'utilisateur. Assurez-vous que la valeur TopN est correctement définie avant utilisation.",
      switchDescription:
        "Un composant qui évalue des conditions basées sur la sortie des composants précédents et dirige le flux d'exécution en conséquence. Il permet une logique de branchement complexe en définissant des cas et des actions pour chaque cas ou une action par défaut si aucune condition n'est remplie.",
      wikipediaDescription:
        'Un composant qui effectue une recherche sur wikipedia.org, utilisant TopN pour spécifier le nombre de résultats. Il complète les bases de connaissances existantes.',
      promptText:
        'Veuillez résumer les paragraphes suivants. Faites attention aux chiffres, ne les inventez pas. Paragraphes suivants : {input} Le contenu ci-dessus est celui que vous devez résumer.',
      createGraph: 'Créer un agent',
      createFromTemplates: 'Créer à partir de modèles',
      retrieval: 'Récupération',
      generate: 'Générer',
      answer: 'Interaction',
      categorize: 'Catégoriser',
      relevant: 'Pertinent',
      rewriteQuestion: 'Réécrire',
      rewrite: 'Réécrire',
      begin: 'Commencer',
      message: 'Message',
      blank: 'Vide',
      createFromNothing: 'Créer votre agent de zéro',
      addItem: 'Ajouter un élément',
      addSubItem: 'Ajouter un sous-élément',
      nameRequiredMsg: 'Le nom est requis',
      nameRepeatedMsg: 'Le nom ne peut pas être répété',
      keywordExtract: 'Mot-clé',
      keywordExtractDescription:
        "Un composant qui extrait des mots-clés à partir d'une requête utilisateur, avec Top N spécifiant le nombre de mots-clés à extraire.",
      baidu: 'Baidu',
      baiduDescription:
        'Un composant qui recherche sur baidu.com, utilisant TopN pour spécifier le nombre de résultats. Il complète les bases de connaissances existantes.',
      duckDuckGo: 'DuckDuckGo',
      duckDuckGoDescription:
        'Un composant qui recherche sur duckduckgo.com, vous permettant de spécifier le nombre de résultats avec TopN. Il complète les bases de connaissances existantes.',
      searXNG: 'SearXNG',
      searXNGDescription:
        "Un composant qui effectue des recherches via la URL de l'instance de SearXNG que vous fournissez. Spécifiez TopN et l'URL de l'instance.",
      channel: 'Canal',
      channelTip:
        "Effectuer une recherche de texte ou d'actualités sur l'entrée du composant",
      text: 'Texte',
      news: 'Actualités',
      messageHistoryWindowSize:
        "Taille de la fenêtre d'historique des messages",
      messageHistoryWindowSizeTip:
        "La taille de la fenêtre de l'historique de la conversation visible par le LLM. Plus c'est grand, mieux c'est, mais attention à la limite maximale de tokens du LLM.",
      wikipedia: 'Wikipedia',
      pubMed: 'PubMed',
      pubMedDescription:
        'Un composant qui recherche sur https://pubmed.ncbi.nlm.nih.gov/, vous permettant de spécifier le nombre de résultats avec TopN. Il complète les bases de connaissances existantes.',
      email: 'Email',
      emailTip:
        "L'email est un champ obligatoire. Vous devez saisir une adresse email ici.",
      arXiv: 'ArXiv',
      arXivDescription:
        'Un composant qui recherche sur https://arxiv.org/, vous permettant de spécifier le nombre de résultats avec TopN. Il complète les bases de connaissances existantes.',
      sortBy: 'Trier par',
      submittedDate: 'Date de soumission',
      lastUpdatedDate: 'Date de dernière mise à jour',
      relevance: 'Pertinence',
      google: 'Google',
      googleDescription:
        'Un composant qui recherche sur https://www.google.com/, vous permettant de spécifier le nombre de résultats avec TopN. Il complète les bases de connaissances existantes. Notez que cela nécessite une clé API de serpapi.com.',
      bing: 'Bing',
      bingDescription:
        'Un composant qui recherche sur https://www.bing.com/, vous permettant de spécifier le nombre de résultats avec TopN. Il complète les bases de connaissances existantes. Notez que cela nécessite une clé API de microsoft.com.',
      apiKey: 'Clé API',
      country: 'Pays & Région',
      language: 'Langue',
      googleScholar: 'Google Scholar',
      googleScholarDescription:
        'Un composant qui recherche sur https://scholar.google.com/. Vous pouvez utiliser Top N pour spécifier le nombre de résultats.',
      yearLow: 'Année de début',
      yearHigh: 'Année de fin',
      patents: 'Brevets',
      data: 'Données',
      deepL: 'DeepL',
      deepLDescription:
        'Un composant qui obtient des traductions spécialisées de https://www.deepl.com/.',
      authKey: "Clé d'authentification",
      sourceLang: 'Langue source',
      targetLang: 'Langue cible',
      gitHub: 'GitHub',
      gitHubDescription:
        'Un composant qui recherche des dépôts sur https://github.com/. Vous pouvez utiliser Top N pour spécifier le nombre de résultats.',
      baiduFanyi: 'BaiduFanyi',
      baiduFanyiDescription:
        'Un composant qui obtient des traductions spécialisées de https://fanyi.baidu.com/.',
      appid: "ID d'application",
      secretKey: 'Clé secrète',
      domain: 'Domaine',
      transType: 'Type de traduction',
      baiduSecretKeyOptions: {
        translate: 'Traduction générale',
        fieldtranslate: 'Traduction spécialisée',
      },
      baiduDomainOptions: {
        it: "Technologie de l'information",
        finance: 'Finance et économie',
        machinery: 'Fabrication de machines',
        senimed: 'Biomédecine',
        novel: 'Littérature en ligne',
        academic: 'Article académique',
        aerospace: 'Aérospatial',
        wiki: 'Sciences humaines et sociales',
        news: 'Actualités et informations',
        law: 'Lois et règlements',
        contract: 'Contrat',
      },
      baiduSourceLangOptions: {
        auto: 'Détection automatique',
        zh: 'Chinois',
        en: 'Anglais',
        yue: 'Cantonais',
        wyw: 'Chinois classique',
        jp: 'Japonais',
        kor: 'Coréen',
        fra: 'Français',
        spa: 'Espagnol',
        th: 'Thaï',
        ara: 'Arabe',
        ru: 'Russe',
        pt: 'Portugais',
        de: 'Allemand',
        it: 'Italien',
        el: 'Grec',
        nl: 'Néerlandais',
        pl: 'Polonais',
        bul: 'Bulgare',
        est: 'Estonien',
        dan: 'Danois',
        fin: 'Finnois',
        cs: 'Tchèque',
        rom: 'Roumain',
        slo: 'Slovène',
        swe: 'Suédois',
        hu: 'Hongrois',
        cht: 'Chinois traditionnel',
        vie: 'Vietnamien',
      },
      qWeather: 'QWeather',
      qWeatherDescription:
        "Un composant qui récupère les informations météorologiques, telles que la température et la qualité de l'air, de https://www.qweather.com/.",
      lang: 'Langue',
      type: 'Type',
      webApiKey: 'Clé API Web',

[... Content truncated for brevity ...]
```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/locales/fr.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 1269 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

No significant structural elements detected.

## Code Structure Analysis

- Total lines: 1269
- Blank lines: 1 (0.1%)
- Comment lines: ~1 (0.1%)
- Code lines: ~1267


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
- Potential test file: `test_fr.ts`

## Keywords

ANALYSE, API, ARK_API_KEY, ASR, ATTENTE, AWS, Accepter, Accueil, Action, Actions, Activation, Activer, Activez, Adresse, Affiche, Afficher, Affichera, Agent, Ajouter, AkShare, Allemand, Analyse, Analyser, Analyseur, Anglais, Annuler, Anomalie, ArXiv, Arabe, ArkApiKeyMessage, Article, Asie, Assurez, Astuce, Attention, Attribuez, Aucune, Audio, Aussi, Autorisations, Avatar, Baidu, BaiduFanyi, Base, Bases, Bengali, Bienvenue, Bilan, Bing, Bon...

---
*Generated by RAGFlow Repository Documentation Generator*
