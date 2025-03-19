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

### Faiblesse dans l'Authentification et l'Autorisation

Votre application nécessite une authentification des utilisateurs, que ce soit pour la gestion des rôles au sein de votre entreprise (Super-Admin, Admin, Formateur) ou pour les personnes qui consomment votre site (étudiant, futur étudiant). Les faiblesses dans un système d'authentification et d'autorisation peuvent devenir des vulnérabilités critiques, compromettant ainsi la sécurité de vos données et de vos systèmes. Voici les points de faiblesse les plus courants :

#### Faiblesse dans l'Authentification

- Mots de passe faibles ou politique inadéquate : Les mots de passe courts ou faciles à deviner sont une porte ouverte pour les attaquants. Une politique de mots de passe robuste est essentielle.

- Authentification simpliste : Une authentification simple pour chaque utilisateur, sans tenir compte de leurs rôles ou de la manière dont ils interagissent avec votre application, peut mettre en péril votre sécurité.

- Réinitialisation de mot de passe vulnérable : Si le processus de réinitialisation est mal sécurisé, cela peut permettre aux attaquants de prendre le contrôle des comptes.

- Conservation des mots de passe en clair dans la base de données : C'est une pratique extrêmement dangereuse, car elle expose directement les informations sensibles en cas de compromission de la base de données.

#### Faiblesse dans l'Autorisation des Droits

Contrôle d'accès désorganisé : Si une gestion de rôles n'est pas mise en place, cela peut permettre à des utilisateurs d'accéder à des ressources ou d'exécuter des actions pour lesquelles ils ne devraient pas avoir d'autorisation. L'absence de gestion de rôles peut donner à tous les utilisateurs un accès aux données sensibles, ce qui est une très mauvaise pratique et crée une vulnérabilité, entraînant une perte de confidentialité.

## Ressource

Injection SQl :
https://www.cnil.fr/fr/definition/injection-sql
https://www.oracle.com/fr/security/injection-sql-attaque/
https://openclassrooms.com/fr/courses/7727176-realisez-un-test-dintrusion-web/7917166-attaquez-la-base-de-donnees-avec-les-injections-sql

Faiblesse d'authentification :
https://www.cnil.fr/fr/mots-de-passe-une-nouvelle-recommandation-pour-maitriser-sa-securite
