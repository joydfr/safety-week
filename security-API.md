# Sécurisation de l'API

## Table des matières

- [Authentification et autorisatio](#Authetification-et-autorisation)
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

### utilisateur(non-inscrit à un cours)

Les utilisateurs non inscrit n'auront dans un pas de néccesités d'inscription pour parcourir le site.

Afin d'améliorer la sécurité d'authentification nous vous conseillons de choisir votre système d'authentification avec un verrouillage de compte pour verouillez temporairement les comptes s'il y a trop de tentatives d'authentification refusé, la posibilité de signaler une activité suspecte et blocage et déblocage des utilisateurs.

Nous vous recommandons également une sensibilisation à vos utilisateur dans la partie front-end pour indiquer les bonnes pratique lors du choix du mot de passe.

## Ressources

https://aws.amazon.com/fr/what-is/mfa/
https://www.microsoft.com/fr-fr/security/business/security-101/what-is-two-factor-authentication-2fa
https://learn.microsoft.com/fr-fr/entra/identity/authentication/howto-mfa-mfasettings
https://support.microsoft.com/fr-fr/account-billing/sauvegarder-les-informations-d-identification-du-compte-dans-microsoft-authenticator-bb939936-7a8d-4e88-bc43-49bc1a700a40
