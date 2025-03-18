# Sécurisation de l'API

## Table des matières

- [Authentification et autorisation](#Authetification-et-autorisation)
- [Gestion des données](#Gestion-des-données)
- [Gestion de l'API](#Gestion-de-l-API)
- [Tests et sécurité](#Test-et-sécurité)

## Authentification et autorisation

## Autorisation

Les autorisations API sont essentielles pour contrôler l'accès aux ressources via l'API. Elles permettent de définir précisément les actions que peuvent effectuer un utilisateur ou une application sur une ressource spécifique.

Pour gérer ces autorisations efficacement, nous utilisons une gestion de rôles. Les rôles typiques incluent le super administrateur, l'administrateur, le formateur et l'utilisateur.

### Super-Admin

- Gestion complète du système
- Création/suppression de comptes administrateurs
- Configuration globale de la plateforme
- Accès aux données analytiques et financières
- Gestion des API et intégrations externes
- Audit des actions des autres utilisateurs
- Modification de la structure des permissions

Afin de respecter les bonnes pratiques de sécurité et de conformité RGPD/CNIl au principes de mimination des accès même pour les super utilisateur, nous segmenterons les privilèges entre plusieur Super-Admin et mettrons en place une approbation multi personne pour les actions critique(ex : suppresions massives de données, modification des paramèttre de sécurité, Export complet des données personnelles ...)

### Admin

- Gestion des utilisateurs (création, modification, suspension)
- Validation des cours proposés
- Modération des contenus et commentaires
- Gestion des catégories de cours
- Accès aux rapports et statistiques
- Gestion des paiements et remboursements
- Résolution des litiges entre formateurs et étudiants

### Formateur

- Création et gestion de ses propres cours
- Publication de contenus pédagogiques (vidéos, documents, quiz)
- Visualisation des statistiques de ses cours
- Communication avec ses étudiants
- Gestion de son planning avec ses dipsonibilités
- Accès aux données de ses étudiants inscrits

### Étudiant

- Inscription/désinscription aux cours
- Accès aux contenus des cours auxquels il est inscrit
- Soumission de travaux et exercices
- Participation aux forums de discussion des cours
- Évaluation des cours suivis
- Suivi de sa progression d'apprentissage

### Utilisateur (non inscrit à un cours)

- Navigation dans le catalogue de cours
- Consultation des descriptions de cours
- Visualisation des profils publics des formateurs
- Création et gestion de son profil personnel

Chaque rôle est associé à des privilèges spécifiques, ce qui permet de limiter les données visibles grâce à un filtrage basé sur le rôle de l'utilisateur. Cela évite d'accorder des permissions trop étendues. Pour renforcer la sécurité, nous mettons en place un middleware d'autorisation qui vérifie les permissions entre la réceptions de la requête par l'application et l'envoie de la réponse avant d'exécuter les requêtes API. Cette approche est cruciale pour prévenir les problèmes de sécurité qui pourraient survenir si les permissions ne sont pas correctement gérées.

Pour maintenir la sécurité, il est important d'examiner et d'évaluer régulièrement les permissions attribuées aux utilisateurs. Cela garantit qu'elles restent appropriées et nécessaires. De plus, nous recommandons de journaliser toutes les activités de l'API pour détecter toute action non autorisée.

Enfin, pour protéger les interactions avec notre base de données, nous appliquons des pratiques d'authentification robustes. Ces pratiques sont adaptées au niveau d'implication de chaque utilisateur, garantissant ainsi une sécurité maximale.

## Authentification robuste selon les autorisations

Dans le cadre de notre application, nous visons à atteindre deux objectifs principaux : la sécurité et la cohérence avec les besoins. Pour le système d'authentification, nous devons réaliser une analyse des risques avant de mettre en œuvre les moyens d'authentification. Cela nous permettra d'adapter la robustesse des mots de passe au contexte spécifique. En effet, notre super-admin ayant plus de droits que notre utilisateur, le système d'authentification sera différent pour chacun des rôles. Par exemple pour

### Nos super-administrateurs et les administrateurs

Un système Multifactorielle serait un impératif car c'est deux roles ont accès à des données sensible, je priviligérai le mots de passe avec plus de contraintes gérer par regex dans la partie back-end ainsi que la saissie d'un code envoyer par mail avec révocations au bout de 10 minutes et la présence avec géolocalosation pour vérifier si l'utilisateur se connecte depuis un endroit attendu et l'heure qui permettra de potentiellement limiter l'accès a certaines heures. Révocations du mot de passe tout les 90 jours.

### Nos formateur et étudiants

Un système authentification à deux facteurs (2FA) car chacun a accès à des données les étudiants au contenu et les formateur au données des étudiants, ceci nécéssite une protection supplémentaire. Le système d'authentification permettra de protèger leurs compte contre des accès non autorisés tout en étant simple à mettre en place. Mise en place d'un mot de passe robuste avec regex dans la partie back-end moins contraintes que pour les super-adminasteur et Admin et pour le deuxième facteur l'envoie d'un mail avec un code à saisir. Pour garder la sécurité mais éviter l'aspect contrainiant de la double authentification priviligié une solution 2FA permettant de mémoirisé les appareils utilisés régulierement. L'utilisateur n'aura pas à saissir le code à chaque connexion mais uniquement lors de la connexion à un nouvel appareil. Revocation de mots de passe à la demande de l'utilisateur.

### Utilisateur(non-inscrit à un cours)

Les utilisateurs non inscrit n'auront dans un pas de néccesités d'inscription pour parcourir le site.

Afin d'améliorer la sécurité d'authentification nous vous conseillons de choisir votre système d'authentification avec un verrouillage de compte pour verouillez temporairement les comptes s'il y a trop de tentatives d'authentification refusé, la posibilité de signaler une activité suspecte et blocage et déblocage des utilisateurs.

Nous vous recommandons également une sensibilisation à vos utilisateur dans la partie front-end pour indiquer les bonnes pratique lors du choix du mot de passe.

## Gestion des données

### Minimisation des données

Pour éviter le vol de données, nous vous conseillons de limiter les données collectées et stockées, en ne collectant que les informations strictement nécessaires pour le fonctionnement de l'API. Il est également important de stocker les données uniquement pendant la période nécessaire et de les supprimer ou les anonymiser une fois qu'elles ne sont plus nécessaires. Pour protéger les données personnelles des utilisateurs, vous pouvez les anonymiser en remplaçant les entités personnelles (prénoms, noms, adresses) par des pseudonymes aléatoires. Le masquage des données sensibles, telles que les numéros de carte de crédit, adresses e-mail et numéros de téléphone, peut également être utilisé pour renforcer la sécurité.

### Chiffrement des données sensibles

Pour sécuriser les données sensibles, nous recommandons l'utilisation de tokens et de jetons. Par exemple, les numéros de carte de crédit peuvent être remplacés par des tokens, permettant ainsi de stocker et de traiter les transactions sans risque d'exposition des informations sensibles. De plus, l'utilisation du protocole HTTPS est essentielle pour chiffrer toutes les communications entre l'API et les utilisateurs, limitant ainsi les attaques de type "Man-in-the-middle". Cela garantit que les données échangées restent confidentielles et sécurisées tout au long du processus.

## Gestion de l'API

### Documentation et journalisation

#### Documentation de l'API

Pour votre documentation, nous vous recommandons d'expliquer l'objectif et les recommandations de votre API. Une documentation API complète, qui fournit des informations détaillées sur l'API, est une ressource essentielle pour les développeurs. Elle leur donne des instructions claires et des exemples pour les aider à comprendre et à utiliser l'API efficacement, ainsi que pour comprendre comment elle est sécurisée. La documentation des processus de sécurité et des politiques de gestion des incidents aidera à démontrer la conformité aux réglementations. Cela facilite également la collaboration entre les équipes de développement et de sécurité.

La documentation de chaque endpoint doit inclure les méthodes HTTP, les paramètres requis, ainsi que des exemples de requêtes et réponses. Elle doit également inclure une liste des codes d'erreur possibles et leur signification. Les codes d'erreur sont divisés en cinq catégories, basées sur le premier chiffre : réponses informatives (1xx), réponses réussies (2xx), réponses de redirection (3xx), réponses d’erreur client (4xx) et réponses d’erreur serveur (5xx). Il est important d’utiliser le code d’état approprié pour chaque scénario d’erreur, et de ne pas créer le vôtre ou d’utiliser des codes qui ne correspondent pas à la sémantique d’erreur. Par exemple, 400 Bad Request doit être utilisé pour les paramètres invalides ou manquants, 401 Unauthorized pour les erreurs d’authentification, 403 Forbidden pour les erreurs d’autorisation, et 404 Not Found pour les erreurs de ressource introuvable. Assurez-vous que la documentation est régulièrement mise à jour pour refléter les changements apportés à l'API et réalisez des tests pour vérifier leur exactitude.

#### Journalisation

La journalisation est un outil essentiel pour assurer la sécurité des traitements de données. Elle permet la conservation des données de journalisation des actions d'accès, de création, de modification et de suppression sur un traitement de données personnelles. Dans le contexte de systèmes multi-utilisateurs, elle assure une traçabilité des accès et des actions des différentes personnes accédant aux systèmes d'informations, et plus précisément, aux traitements de données personnelles mis en œuvre. Les journaux contiennent des informations sur les personnes administrant ou accédant aux ressources, telles que l'identifiant utilisateur, la date et l'heure de l'accès, ainsi que l'identifiant de l'équipement utilisé. Ces données constituent un outil efficace de détection et d'investigation en cas d'incident, d'intrusion dans les systèmes informatiques, ou de détournement d'usage des traitements de données par les personnes habilitées.

Il est recommandé que les données de journalisation soient régulièrement analysées à l'aide d'analyses automatiques afin de permettre la détection rapide des éventuelles utilisations indues du traitement. Pour cela, il est possible d'utiliser un SIEM (Solutions de Gestion des Informations et des Événements de Sécurité) qui permet d'examiner et d'interpréter plus facilement ce qui se passe sur le réseau et d'obtenir des informations exploitables. Cela aide à identifier les incidents en corrélant les événements et en détectant les modèles de menace. Nous pouvons également utiliser un UEBA (Analyse du Comportement des Utilisateurs et des Entités) qui utilise des modèles de comportement pour détecter les actions anormales des utilisateurs ou des entités, ce qui peut indiquer une menace.

La durée de conservation des journaux doit être comprise entre six mois et un an, sauf cas spécifique où une durée plus longue pourrait être justifiée.

### Mise à jour et maintanance

La mise à jour et la maintenance de la documentation et de la journalisation sont cruciales, comme expliqué précédemment. Elles permettent la collaboration entre l'équipe de développement et l'équipe de sécurité. Grâce à l'analyse effectuée avec la journalisation, les équipes peuvent comprendre comment les données circulent au sein du système, ce qui permet d'identifier les points où les données pourraient être vulnérables. La documentation est également utilisée pour suivre les mises à jour et les correctifs de sécurité appliqués au système, ce qui aide à garantir que les vulnérabilités connues sont corrigées. La documentation et la journalisation peuvent être utilisées pour planifier les tests de pénétration en identifiant les points d'entrée potentiels et les vulnérabilités à tester.

## Test et sécurité

### Test de sécurité

Dans le paragraphe précédent, nous avons vu que la journalisation et la documentation pouvaient être utiles pour planifier des tests de pénétration. Pour réaliser des tests de sécurité, nous pouvons commencer par des tests simples comme :

- Fuzz Testing : Cela consiste à injecter des données aléatoires dans l'API pour tester sa robustesse face à des entrées invalides ou malveillantes. Ce type de test peut révéler des vulnérabilités telles que les injections SQL ou les attaques XSS.

- Verb Fuzzing : Ce test consiste à tester les différentes méthodes HTTP (GET, POST, PUT, DELETE, etc.) pour identifier les vulnérabilités dans les endpoints REST.

L'intégration des tests de sécurité dès le début du cycle de développement permet d'identifier et de corriger les vulnérabilités tôt.
Tests avancés

Pour aller plus loin, nous pouvons également :

- Simulation d'Attaques (DAST : Dynamic Application Security Testing) : Cette méthode permet de tester l'API en simulant des attaques réelles pour identifier les vulnérabilités qui ne sont apparentes qu'à l'exécution. Cela inclut la validation des mécanismes d'authentification et d'autorisation.

- Simulation d'Attaques Réalistes : En effectuant des tests de pénétration pour simuler des attaques réalistes et identifier les vulnérabilités qui pourraient être exploitées par des attaquants.

Outils de test

Des outils comme Postman peuvent être utilisés pour automatiser les tests de sécurité et simuler divers scénarios d'attaque. OWASP ZAP est un outil open source pour la simulation d'attaques et la détection de vulnérabilités.

## Ressources

https://aws.amazon.com/fr/what-is/mfa/
https://www.microsoft.com/fr-fr/security/business/security-101/what-is-two-factor-authentication-2fa
https://learn.microsoft.com/fr-fr/entra/identity/authentication/howto-mfa-mfasettings
https://support.microsoft.com/fr-fr/account-billing/sauvegarder-les-informations-d-identification-du-compte-dans-microsoft-authenticator-bb939936-7a8d-4e88-bc43-49bc1a700a40
https://www.cnil.fr/fr/securite-api-interfaces-de-programmation-applicative
https://api.gouv.fr/les-api/api-pseudonymisation-documents
https://fr.linkedin.com/advice/0/how-do-you-secure-encrypt-your-api-data?lang=fr
https://www.linkedin.com/advice/3/how-do-you-document-api-errors-exceptions-status?lang=fr&originalSubdomain=fr
https://api.gerermesaffaires.com/documentation/rest-api/error-codes/
https://www.cnil.fr/fr/la-cnil-publie-une-recommandation-relative-aux-mesures-de-journalisation
https://fr.parasoft.com/learning-center/api-security-testing-guide/
https://www.astera.com/fr/type/blog/api-test-automation/
