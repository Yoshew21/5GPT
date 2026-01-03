# Recueil des Documentations Techniques

## Outil 1 : AI Pull Request Reviewer (Catégorie : Code)

### Documentation Technique Approfondie

- **Analyse du Contexte :** L'automatisation de la revue de code répond à un besoin de scalabilité des équipes de développement. L'IA ne remplace pas l'humain mais agit comme un "linter sémantique" capable de comprendre le flux de données, là où les outils statiques (SonarQube) échouent souvent sur la logique métier ou les failles contextuelles.
- **Stratégie de Prompting (Techniques Utilisées) :**
    - Persona Expert 5 : L'assignation du rôle "Senior Software Engineer" permet de calibrer le vocabulaire et le niveau d'exigence du modèle, réduisant les commentaires triviaux.
    - Guided Chain of Thought (CoT) 3 : En forçant le modèle à décomposer l'analyse (Intention -> Sécurité -> Impact -> Classification), on simule un raisonnement de type "Système 2" (lent et réfléchi), ce qui réduit significativement les faux positifs.
    - XML Delimiters & Data Sanitization : L'encapsulation du diff dans les balises `<<<DIFF>>>` crée une frontière sémantique claire entre les instructions système et les données utilisateur, essentielle pour prévenir les injections de prompt indirectes.
- **Évolution et Améliorations :**
    - *De V1 à V2 :* Passage d'une analyse globale floue à une analyse ciblée grâce au CoT. Gain en pertinence sur la sécurité.
    - *De V2 à VFinal :* Sécurisation contre les attaques adverses (règles de non-obéissance) et structuration de la sortie sous forme de tableau Markdown pour une lisibilité immédiate dans les interfaces de Git (GitHub/GitLab).
- **Exemples d'Utilisation :**
    - *Cas 1 (Injection SQL) :*
        - *Input :* Code Python ajoutant `query = "SELECT * FROM users WHERE name = '" + user_input + "'"`
        - *Output VFinal :* Sévérité **CRITIQUE**. Impact : Injection SQL possible. Verdict : **Request changes**. Suggestion : Utiliser des requêtes paramétrées.
    - *Cas 2 (Refactoring propre) :*
        - *Input :* Remplacement d'une boucle `for` par un `map` en JavaScript.
        - *Output VFinal :* Sévérité **MINEUR**. Impact : Amélioration de la lisibilité. Verdict : **Approve**.
- **Limites Connues :** L'outil analyse le diff de manière isolée. Il manque de contexte global (architecture du projet, types définis dans d'autres fichiers), ce qui peut mener à des faux négatifs si la vulnérabilité dépend d'un flux de données externe au diff.

## Outil 2 : Analyseur Intelligent de Logs (Catégorie : Debug)

### Documentation Technique Approfondie

- **Contexte :** La résolution d'incidents (MTTR - Mean Time To Resolution) est un KPI critique. Cet outil accélère l'analyse initiale en pré-digérant les informations techniques complexes.
- **Stratégie de Prompting :**
    - Grounding (Ancrage) : La règle "Preuve par le Log" est essentielle. Elle force le modèle à lier chaque assertion à une donnée d'entrée, éliminant les hallucinations fréquentes dans les modèles génératifs purs.
    - Distinction Causale (System 2 Thinking) : En demandant explicitement de séparer symptôme et cause, on active les capacités de raisonnement logique du modèle, lui permettant de comprendre que l'erreur 500 n'est pas le problème, mais la conséquence d'une erreur de base de données sous-jacente.
    - Safety Constraints : La protection contre le "Log Poisoning" est intégrée via des instructions de neutralité absolue face au contenu textuel des logs.
- **Évolution :**
    - *V1 :* Résumés vagues, peu exploitables.
    - *V2 :* Meilleure structure d'enquête, mais risque de biais de confirmation.
    - *VFinal :* Fiabilité accrue grâce à l'obligation de citation et gestion explicite de l'incertitude ("Informations Manquantes").
- **Exemples d'Utilisation :**
    - *Input :* Logs Java avec `OutOfMemoryError` suivi d'un redémarrage de conteneur.
    - *Output VFinal :* Cause Racine : Fuite de mémoire (Java Heap Space). Preuve : `java.lang.OutOfMemoryError: Java heap space` à 14:02. Action : Analyser le Heap Dump, augmenter `-Xmx`.
- **Limites :** L'outil ne peut pas corréler des logs s'ils ne contiennent pas d'identifiants communs (Trace ID, Request ID) ou si les horodatages sont désynchronisés.

## Outil 3 : Générateur de Dockerfile Optimisé (Catégorie : DevOps)

### Documentation Technique Approfondie

- **Contexte :** La sécurité de la chaîne d'approvisionnement logicielle (*Supply Chain Security*) commence dès la construction de l'image. Ce prompt automatise l'application des règles de sécurité souvent négligées.
- **Stratégie de Prompting :**
    - Implicit Few-Shot & Constraints : En listant les contraintes comme une "Checklist", on agit sur l'attention du modèle pour qu'il vérifie chaque point lors de la génération. Cela remplace avantageusement des exemples longs (Few-Shot classique) en consommant moins de tokens.
    - **Role-Playing Expert :** Le rôle "DevOps Senior" active les connaissances liées aux benchmarks de sécurité (CIS), induisant l'utilisation de techniques comme le nettoyage des caches `apt`.
- **Évolution :**
    - *V1 :* Dockerfile basique, souvent insécurisé.
    - *V2 :* Optimisation de la taille (Multi-stage), mais sécurité variable.
    - *VFinal :* Sécurité par défaut (Non-root, version pinning) et optimisation maximale (layer caching).
- **Exemples d'Utilisation :**
    - *Input :* Application Node.js simple.
    - *Output VFinal :* Dockerfile avec `FROM node:18-alpine AS builder`, `npm ci` (pour la reproductibilité), puis `FROM node:18-alpine AS runner`, copie des `node_modules` de prod uniquement, création utilisateur `node`, et `USER node`.
- **Limites :** L'outil ne peut pas deviner les dépendances système binaires (ex: librairies C++ pour certains modules Python) si elles ne sont pas spécifiées dans l'input.

## Outil 4 : Générateur de README Professionnel (Catégorie : Documentation)

### Documentation Technique Approfondie

- **Contexte :** La documentation est l'interface utilisateur du code. Une structure standardisée réduit la charge cognitive des développeurs.
- **Stratégie de Prompting :**
    - Template Structuring : En imposant le squelette Markdown, on contraint la génération du modèle ("Slot Filling"). Cela garantit que toutes les sections critiques sont présentes.
    - **Meta-constraints (Anti-hallucination) :** L'instruction explicite d'utiliser des placeholders (``) transforme l'incertitude du modèle en une fonctionnalité utile plutôt qu'en une erreur factuelle.
- **Évolution :**
    - *V1 :* Structure correcte mais contenu parfois inventé.
    - *V2 :* Meilleur ton, mais manque de rigueur technique.
    - *VFinal :* Documentation "prête à l'emploi" avec gestion explicite des manques d'information.
- **Exemples d'Utilisation :**
    - *Input :* "Script Python pour scraper Amazon avec Selenium".
    - *Output VFinal :* README incluant les prérequis (Chrome Driver), l'installation (`pip install -r requirements.txt`) et un avertissement légal généré sur le scraping.
- **Limites :** L'outil ne peut pas documenter l'architecture interne ou les choix de design si ceux-ci ne sont pas fournis dans le contexte d'entrée.

## Outil 5 : Générateur de Messages de Commit (Catégorie : Productivité)

### Documentation Technique Approfondie

- **Contexte :** La sémantique des commits permet l'automatisation des versions (Semantic Versioning).
- **Stratégie de Prompting :**
    - **Standard Enforcement :** Le modèle agit comme un traducteur de langage naturel vers un langage formel (la spec Conventional Commits).
    - Few-Shot Learning : L'inclusion de 3 paires Input/Output dans le prompt améliore drastiquement la performance du modèle. Cela lui permet de comprendre par l'exemple ce qu'est un "scope" et quel ton adopter, bien mieux que des instructions abstraites.
- **Évolution :**
    - *V1 :* Format respecté mais contenu faible.
    - *V2 :* Grammaire corrigée.
    - *VFinal :* Précision du scope et de la terminologie technique grâce au Few-Shot.
- **Exemples d'Utilisation :**
    - *Input :* "Optimisation de la requête SQL pour récupérer les commandes, c'est 2x plus rapide."
    - *Output VFinal :* `perf(db): optimize orders retrieval query for latency`
- **Limites :** Si l'input décrit plusieurs changements décorrélés, l'outil aura du mal à générer un message atomique et risque de produire un commit "fourre-tout".

