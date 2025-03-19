# 2. Menaces Potentielles

Afin de répondre aux menaces de vulnérabilités de plus en plus présentes dans le domaine du développement web, nous allons vous exposer, dans un premier temps, les menaces potentielles qui peuvent affectent votre site de formation en peer-to-peer. Nous analyserons ces menaces couche par couche, en commençant par le back-end et l'API, puis en abordant la partie front-end. Nous expliquerons pourquoi la sécurité de votre application est primordiale et comment nos services peuvent vous aider à éviter ces problématiques.

## Back-End/ API

### Injection SQL

Votre application nécessite des entrées de la part des utilisateurs, comme par exemple les formulaires d'inscription. Cela expose votre système à une menace courante : l'injection SQL. Un attaquant peut insérer du code malveillant via les formulaires ou les liens des pages dans les champs de saisie ou les requêtes. Cela lui permet de manipuler la base de données, ce qui peut entraîner un accès non autorisé, la modification ou la suppression de données sensibles telles que des mots de passe, des coordonnées bancaires ou des informations personnelles. Ces attaques se produisent généralement lorsque les entrées utilisateurs ne sont pas correctement validées ou filtrées en amont.

Par exemple, un utilisateur malveillant peut entrer une commande comme DROP TABLE, ce qui peut entraîner la suppression de la table entière si vos entrées ne sont pas sécurisées. Il existe plusieurs types d'injections SQL, mais la plus courante est l'injection classique via les failles dans les champs de saisie, permettant de modifier directement les requêtes SQL.

Les conséquences des injections SQL peuvent avoir plusieurs impacts graves :

- Vol ou modification de données sensibles : Les attaquants peuvent accéder à des informations confidentielles.

- Compromission totale de la base de données : La perte de contrôle sur les données peut avoir des conséquences catastrophiques.

- Accès aux comptes administrateurs ou utilisateurs : Les attaquants peuvent prendre le contrôle des comptes pour effectuer des actions malveillantes.

- Perturbation du fonctionnement des applications : Les attaques peuvent entraîner des dysfonctionnements ou des arrêts de service.

#### Comment se Protéger contre les Injections SQL

Les bonnes pratiques pour prévenir les injections SQL que votre entreprise peut mettre en place sont les suivantes :

- Validation Stricte des Entrées : Assurer une validation rigoureuse des données fournies par les utilisateurs pour empêcher les attaquants d'injecter du code malveillant. Cela inclut le nettoyage et la validation des entrées pour éviter toute insertion de code SQL malveillant.

- Utilisation de Procédures Stockées : Définir des instructions SQL directement dans la base de données pour limiter les interactions et réduire les risques d'injection. Bien que les regex puissent être utilisés pour filtrer et valider les données, il est crucial de s'assurer que toutes les entrées sont correctement nettoyées.

- Utilisation d'Instructions Préparées et de Requêtes Paramétrées : Séparer le code SQL des données d'entrée pour empêcher la manipulation des requêtes. Cela permet de traiter les données comme des entrées non exécutables, ce qui est essentiel pour éviter les injections.

- Principe des Moindres Privilèges : Limiter les permissions accordées aux utilisateurs qui interagissent avec la base de données pour réduire l'impact potentiel d'une attaque.

- Utilisation d'un Pare-feu d'Application Web (WAF) : Inspecter et filtrer les requêtes entrantes pour identifier et bloquer les attaques malveillantes.

- Tests de Sécurité Réguliers : Effectuer des tests de pénétration pour identifier et corriger les vulnérabilités avant qu'elles ne soient exploitées. Ces tests permettent de maintenir une sécurité proactive et de garantir que les mesures de protection sont efficaces.

### Faiblesse dans l'Authentification et l'Autorisation

Votre application nécessite une authentification des utilisateurs, que ce soit pour la gestion des rôles au sein de votre entreprise (Super-Admin, Admin, Formateur) ou pour les personnes qui consomment votre site (étudiant, futur étudiant). Les faiblesses dans un système d'authentification et d'autorisation peuvent devenir des vulnérabilités critiques, compromettant ainsi la sécurité de vos données et de vos systèmes. Voici les points de faiblesse les plus courants :

#### Faiblesse dans l'Authentification

- Mots de passe faibles ou politique inadéquate : Les mots de passe courts ou faciles à deviner sont une porte ouverte pour les attaquants. Une politique de mots de passe robuste est essentielle.

- Authentification simpliste : Une authentification simple pour chaque utilisateur, sans tenir compte de leurs rôles ou de la manière dont ils interagissent avec votre application, peut mettre en péril votre sécurité.

- Réinitialisation de mot de passe vulnérable : Si le processus de réinitialisation est mal sécurisé, cela peut permettre aux attaquants de prendre le contrôle des comptes.

- Conservation des mots de passe en clair dans la base de données : C'est une pratique extrêmement dangereuse, car elle expose directement les informations sensibles en cas de compromission de la base de données.

#### Comment se protéger des faiblesse de l'Authentification

Les bonnes pratiques pour se prévenir des faiblesse d'authentifiaction que vous pouvez appliquer sont :

- Utilisation des méthodes d'authetification en fonction de l'intéraction des utilisateurs avec votre site. Pour se faire vous pouvez utiliser l'authentification multifacteur en exigant plusieurs preuves d'itentité, comme le mot de passe, l'envoie de code via les canaux (SMS, Email ...)

- L'utilisation de mot de passe forts, vous pouvez encourager vos utilisateurs a choisir des mots de passe robuste avec des regex et des validations en choissant des mots de passes long avec des lettres masjuscules, miniscules, chiffres et caractère spéciaux. Voir dans certains cas et selon les privilièges accordés le changement régulier de mots de passe avec revocation pour renforcer la sécurité.

- L'utilisation de système d'authentification standardisé comme OAuth ou JWT pour une authentification sécurisé avec l'utilisation de jetons et de tokens.

- Utilisation de vérouillage temporaire de session en cas d'échec de connection avec délais croissant.

- L'encryptage des mots de passes dans la base de données

#### Faiblesse dans l'Autorisation des Droits

Contrôle d'accès désorganisé : Si une gestion de rôles n'est pas mise en place, cela peut permettre à des utilisateurs d'accéder à des ressources ou d'exécuter des actions pour lesquelles ils ne devraient pas avoir d'autorisation. L'absence de gestion de rôles peut donner à tous les utilisateurs un accès aux données sensibles, ce qui est une très mauvaise pratique et crée une vulnérabilité, entraînant une perte de confidentialité.

#### Comment se protéger des faiblesse de L'autorisation

Pour vous protéger des faiblesses d'autorisation, voici quelques bonnes pratiques que vous pourriez mettre en place :

- Appliquer le Principe du Moindre Privilège : Autoriser vos utilisateurs selon leurs rôles, en leur accordant uniquement les permissions nécessaires pour leurs interactions avec votre site. Cela réduit le risque d'abus en cas d'accès non autorisé.

- Vérification des Requêtes : Vérifier les requêtes envoyées par le client via l'API REST sur le serveur pour s'assurer que l'utilisateur a les droits nécessaires pour accéder à la ressource demandée ou effectuer l'action souhaitée.

- Journalisation des Tentatives d'Accès : Mettre en place une journalisation pour enregistrer toutes les tentatives d'accès non autorisé. Cela permet de détecter des attaques potentielles et de prendre des mesures correctives en temps réel.

- Utilisation des Tokens d'Autorisation : Utiliser des tokens d'autorisation pour vérifier les permissions à chaque requête, garantissant ainsi que les utilisateurs n'accèdent qu'aux ressources pour lesquelles ils sont autorisés.

- Segmentation des Privilèges : Mettre en place une segmentation des privilèges pour les super-utilisateurs, avec des demandes d'autorisation pour l'accès à certaines ressources. Cela peut inclure un accès temporaire et limité aux ressources sensibles, réduisant ainsi les risques associés aux comptes privilégiés.

### Exposition excessive de données sensible

Votre application, en tant que plateforme de formation en ligne, va collecter des données sensibles. La prochaine vulnérabilité que nous allons aborder est l'exposition excessive de données, qui consiste à divulguer plus d'informations qu'il n'est nécessaire aux utilisateurs et potentiellement aux attaquants. Le problème avec cette faille est la fuite de données sensibles telles que les informations personnelles, qui peuvent entraîner une usurpation d'identité, des problèmes financiers avec un accès aux moyens de paiement et aux comptes bancaires ou d'entreprise.

Cette faille se manifeste souvent lors de la phase de développement, lorsque l'exposition des données est faite sans considérer leur sensibilité et sans filtrer les données avant de les afficher à l'utilisateur. Comme mentionné précédemment, si le filtrage est réalisé uniquement côté client avec le front-end, il ne sera pas protégé côté serveur pour empêcher l'exposition excessive de données.

#### Comment se Prévenir de l'Exposition Excessive

Afin d'éviter cette exposition pour votre site, voici quelques suggestions :

- Analyse en Amont du Besoin de Données : Utiliser la minimisation des données en ne collectant que les informations strictement nécessaires. Éviter de donner la priorité au fonctionnel plutôt qu'à la sécurité.

- Chiffrement des Données Sensibles : Utiliser le protocole HTTPS pour chiffrer toutes les communications entre l'API et les utilisateurs, ce qui limite les attaques de type "Man-in-the-Middle". Encrypter les données sensibles comme les mots de passe et les informations bancaires. Utiliser des pseudonymes aléatoires pour les données d'entités (nom, prénom et adresse), et utiliser des UUID (identifiants uniques universels) plutôt que les identifiants standards, car ils sont plus difficiles à deviner.

- Filtrage au Niveau de l'API : Ne pas déléguer le filtrage des données à la partie front-end, mais privilégier le filtrage au niveau de l'API pour garantir que seules les informations nécessaires sont transmises.

- Contrôles d'Accès : Mettre en place des contrôles d'accès, comme expliqué dans la partie autorisation, pour s'assurer que les utilisateurs n'accèdent qu'aux données pour lesquelles ils sont autorisés.

### Absence de limitation de taux

L'absence de limitation de taux dans votre application de formation en pair-à-pair peut exposer vous exposer à des risques significatifs, notamment en termes de surcharge du réseau et d'exploitation par des attaquants. Voici les attaques en l'absence de limitation de taux :

#### Attaques par déni de service (DoS)

Une attaque par déni de service consiste à inonder un serveur avec un grand nombre de requêtes pour le rendre inaccessible aux utilisateurs légitimes. Si aucune limitation de taux n'est mise en place, le serveur peut être submergé par un volume élevé de trafic, entraînant une perte de disponibilité et de performance.

##### Conséquences

- Perte de disponibilité : Le site devient temporairement inaccessible ou difficile à utiliser.

- Pertes financières : Perte de revenus potentiels, notamment si le site est utilisé pour des transactions commerciales.

- Impact sur la réputation : La confiance des utilisateurs est ébranlée, ce qui peut nuire à l'image de l'entreprise.

#### Attaques par force brute

Une attaque par force brute consiste à tester un grand nombre de combinaisons de mots de passe pour accéder à un compte. Sans limitation de taux, un attaquant peut essayer un nombre illimité de combinaisons en peu de temps, augmentant ainsi ses chances de réussite.

##### Méthodes utilisées

- Dictionnaires de mots de passe : Utilisation de listes de mots de passe couramment utilisés.

- Exploration de combinaisons : Test de toutes les combinaisons possibles jusqu'à trouver une correspondance.

##### Conséquences

- Accès non autorisé : L'attaquant peut accéder aux profils d'utilisateurs, aux boîtes de messagerie électronique, ou même compromettre des comptes bancaires.

#### Attaques par stuffing de credentials

Le stuffing de credentials consiste à réaliser des tentatives massives d'authentification sur un site à partir d'identifiants et de mots de passe volés ou obtenus par d'autres moyens. Les dégâts incluent des pertes financières, une mauvaise image pour l'entreprise, et une perte d'accès au service pour les utilisateurs.

##### Conséquences

- Vol d'informations personnelles : Les attaquants peuvent accéder à des données sensibles.

- Perte de confiance : Les utilisateurs peuvent perdre confiance dans le service.

#### Comment se protéger de ces attaques ?

Pour se protéger contre ces attaques, il est crucial de mettre en place une limitation de taux efficace. Voici quelques mesures :

- Utilisation d'outils dédiés :

  - Limiter le nombre de requêtes autorisées dans un temps donné.

  - Utiliser des jetons pour contrôler les requêtes et ajuster les limites en fonction du trafic en temps réel.

- Surveillance du trafic :

  - Utiliser des journaux pour surveiller le trafic et détecter les abus potentiels.

  - Adapter les limitations en fonction du trafic réel pour éviter les faux positifs.

- Équilibre entre sécurité et expérience utilisateur :

  - Assurer que les limitations ne nuisent pas à l'expérience utilisateur tout en protégeant contre les attaques.

## Ressource

Injection SQl :
https://www.cnil.fr/fr/definition/injection-sql
https://www.oracle.com/fr/security/injection-sql-attaque/
https://openclassrooms.com/fr/courses/7727176-realisez-un-test-dintrusion-web/7917166-attaquez-la-base-de-donnees-avec-les-injections-sql

Faiblesse d'authentification :
https://www.cnil.fr/fr/mots-de-passe-une-nouvelle-recommandation-pour-maitriser-sa-securite
https://aws.amazon.com/fr/what-is/mfa/
https://www.microsoft.com/fr-fr/security/business/security-101/what-is-two-factor-authentication-2fa
https://learn.microsoft.com/fr-fr/entra/identity/authentication/howto-mfa-mfasettings
https://support.microsoft.com/fr-fr/account-billing/sauvegarder-les-informations-d-identification-du-compte-dans-microsoft-authenticator-bb939936-7a8d-4e88-bc43-49bc1a700a40

Exposition excessive des données :

https://api.gouv.fr/les-api/api-pseudonymisation-documents
https://fr.linkedin.com/advice/0/how-do-you-secure-encrypt-your-api-data?lang=fr

Absence de limitation de taux :

- déni de service :
  https://www.cybermalveillance.gouv.fr/tous-nos-contenus/fiches-reflexes/attaque-en-deni-de-service-ddos
  https://www.microsoft.com/fr-fr/security/business/security-101/what-is-a-ddos-attack
  https://www.altospam.com/glossaire/deni-de-service-ddos/

- force brute :
  https://www.cnil.fr/fr/definition/force-brute-attaque-informatique
  https://www.oracle.com/fr/cloud/attaque-brute-force/
  https://www.mailinblack.com/ressources/glossaire/attaque-bruteforce-comprendre-et-contrer-la-menace/

- Credential stuffing :
  https://www.cnil.fr/fr/definition/credential-stuffing-attaque-informatique
  https://www.cnil.fr/fr/violation-du-trimestre-attaque-par-credential-stuffing-sur-un-site-web
