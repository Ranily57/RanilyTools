# Modèle de Liste de Packages VPM

Démarrez pour créer vos propres listes de packages, incluant l'automatisation pour leur construction et leur publication.

Une fois que vous êtes prêt, vous pourrez mettre à jour le fichier `source.json`, et générer une liste qui fonctionne dans le VPM pour livrer des mises à jour pour tous les packages répertoriés.

## ▶ Pour Commencer

* Cliquez sur [![Utiliser ce modèle](https://user-images.githubusercontent.com/737888/185467681-e5fdb099-d99f-454b-8d9e-0760e5a6e588.png)](https://github.com/vrchat-community/template-package-listing/generate)
pour démarrer un nouveau projet GitHub basé sur ce modèle, et suivez les instructions là-bas.
  * Choisissez un nom et une description appropriés pour le dépôt.
  * Définissez la visibilité sur 'Public'. Vous pouvez également choisir 'Private' et le modifier ultérieurement.
  * Vous n'avez pas besoin de sélectionner 'Inclure toutes les branches.'
* Modifiez ce projet sur GitHub dans votre navigateur web, ou clonez-le localement en utilisant Git.
  * Si vous n'êtes pas familier avec Git et GitHub, [consultez la documentation de GitHub](https://docs.github.com/en/get-started/quickstart/).

## Configuration de l'Automatisation

Vous devrez modifier certains fichiers de ce modèle, en commençant par [`source.json`](source.json):
- Remplissez des informations générales sur votre liste, telles que le `nom`, `id`, `auteur`, `description`, etc.
- Assurez-vous de mettre à jour le champ "url" à la ligne 4, remplaçant "vrchat-community" par votre nom d'utilisateur GitHub, et "template-package-listing" par le nom de votre dépôt. C'est le lien qui sera utilisé pour télécharger votre liste une fois qu'elle sera publiée sur GitHub. Par exemple, l'utilisateur "thupper" ayant créé un dépôt appelé "thupper-listing" mettrait à jour l'URL en "https://thupper.github.io/thupper-listing/index.json".
- Mettez à jour l'URL dans "infoLink" (à la ligne 11) avec l'URL de ce nouveau dépôt que vous avez créé.
- Si vous souhaitez inclure des packages hébergés sur GitHub, spécifiez-les dans `githubRepos`.
- Si vous souhaitez inclure des packages hébergés ailleurs sous forme de fichier `.zip`, spécifiez-les dans `packages`.
  - Vous pouvez en toute sécurité supprimer soit `githubRepos` soit `packages` si vous ne les utilisez pas.
- Enfin, allez dans la page "Paramètres" de votre dépôt, puis choisissez "Pages", et cherchez l'en-tête "Build and deployment". Changez la liste déroulante "Source" de "Déployer à partir d'une branche" à "GitHub Actions".

## 📃 Reconstruction de la Liste

Chaque fois que vous apportez une modification à la branche `main`, ou lorsque vous le déclenchez manuellement, l'action 'Build Repo Listing' créera un nouvel index de toutes les versions disponibles et les publiera sous forme de site web hébergé gratuitement sur GitHub Pages. Cette liste peut être utilisée par le VPM pour maintenir à jour votre package, et la page d'index générée peut servir de page d'accueil simple avec des informations sur votre package. L'URL de votre package aura le format https://nom_utilisateur.github.io/nom_dépôt.

## 🏠 Personnalisation de la Page d'Accueil

Le contenu du répertoire `Website` peut être personnalisé pour changer l'apparence de la page d'accueil. La plupart des informations seront automatiquement remplies avec les informations de [`source.json`](source.json). Personnaliser la page d'accueil manuellement n'est pas nécessaire.

## Aspects Techniques

Vous pouvez apporter vos propres modifications au processus d'automatisation pour le faire correspondre à vos besoins, et vous pouvez créer des Demandes d'Extraction si vous avez des modifications que vous pensez que nous devrions adopter. Voici quelques informations supplémentaires sur l'automatisation incluse :

### Construction de la Liste
[build-listing.yml](.github/workflows/build-listing.yml)

Il s'agit d'une action composite qui construit une [Liste de Dépôt](https://vcc.docs.vrchat.com/vpm/repos) compatible avec VPM basée sur les éléments que vous avez ajoutés à votre fichier `source.json`. Afin de trouver toutes vos versions et de les regrouper dans une liste, elle extrait [un autre dépôt](https://github.com/vrchat-community/package-list-action) qui possède un projet [Nuke](https://nuke.build/) qui inclut la librairie VPM core pour avoir accès à ses types et méthodes. Ce projet sera étendu pour inclure plus de fonctionnalités à l'avenir - pour l'instant, l'action appelle simplement sa cible `BuildRepoListing`, qui appelle `RebuildHomePage` quand elle est terminée. Si vous vouliez créer une action qui reconstruit simplement la page d'accueil, vous pourriez l'appeler directement à la place - copiez simplement l'appel existant et remplacez les noms des cibles.
