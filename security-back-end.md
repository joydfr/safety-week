# Sécurisation de l'API

## Table des matières

- [Authentification et autorisation](#Authetification-et-autorisation)
- [Sécurité des données](#Sécurité-des-données)
- [Gestion de l'API et intégration](#Gestion-de-l-API-et-intégration)
- [Documention et journalisation](#Documentation-et-journalisation)
- [Tests et sécurité](#Test-et-sécurité)
- [Mises à jour et maintenance](#Mises-à-jour-et-maintenance)
- [Gestion des incidents](#Gestion-des-incidents)

Comme évoqué précédemment dans la partie API, nous allons entrer un peu plus dans les détails sur comment mettre en place le système de sécurité dans la partie back-end et expliquer comment utiliser le back-end pour sécuriser.

Dans le cadre du développement sécurisé de notre API, il est crucial de mettre en place des mécanismes robustes pour protéger les données et les interactions avec le système.

## Authentification et autorisation

Pour la mise en place de l'authentification multifactorielle ou 2FA, nous devons mettre en place le choix des facteurs d'authentification. Nous vous conseillons d'adapter ces facteurs selon le rôle attribué. Un super-admin aura des contraintes plus robustes lors de l'utilisation de regex que l'utilisateur, pour lequel nous pouvons accorder un peu plus de souplesse dans l'élaboration de son mot de passe.

Le premier facteur, qui est l'utilisation d'un mot de passe robuste avec des contraintes de complexité, peut se traduire dans le back-end par l'utilisation de regex limitant. Nous vous conseillons :

- Utilisation d'un mot de passe long avec un minimum de 15 caractères.

- Utilisation de la complexité en incluant des majuscules, minuscules, chiffres et caractères spéciaux.

- L'utilisation de validateurs ou création de validations pour éviter les mots de passe trop communs comme "password", "azerty", "qwerty".

Vous avez également la possibilité de mettre à disposition des générateurs de mots de passe (par exemple, le générateur du CNIL) avec un coffre fort pour que vos utilisateurs puissent sécuriser leurs mots de passe en évitant la contrainte de devoir les retenir.

Afin de faciliter la révocation de mot de passe pour vos utilisateurs, vous pouvez également rediriger vos utilisateurs vers une page de création de mot de passe une fois celui-ci expiré et mettre en place un système de notification automatique quelques jours avant que celui-ci arrive à échéance.

Pour renforcer la sécurité, nous vous conseillons également de limiter le nombre de tentatives d'authentification incorrectes et de verrouiller temporairement le compte en utilisant le principe de délai croissant, en augmentant progressivement le délai entre les tentatives après chaque échec.

Ces dispositifs doivent être adaptés en fonction des rôles attribués et de l'interaction entre l'utilisateur et l'application.

## référence

Mot de passe :
https://www.cnil.fr/fr/generer-un-mot-de-passe-solide
https://www.economie.gouv.fr/particuliers/creer-mot-passe-securise
