# dooka-link

Ce dépôt est servi tel quel par GitHub Pages sur **link.dooka.fr** (voir
`CNAME`). Il ne contient que des fichiers statiques : les liens
d'invitation des deux applications, et les fichiers de vérification qui
leur permettent d'ouvrir l'app au lieu du navigateur.

`.nojekyll` est indispensable — sans lui, GitHub Pages ignore les dossiers
qui commencent par un point, donc `.well-known/`.

## Dooka — `/join-group/<code>`

Le lien est de la forme `https://link.dooka.fr/join-group/PIT447`. Aucun
fichier ne correspond à ce chemin : c'est `404.html` qui le rattrape, lit
le code dans l'URL et propose d'ouvrir `dooka://join-group/<code>`.

## Quali — `/quali/?code=<code>`

Le lien est de la forme `https://link.dooka.fr/quali/?code=PIT447`, servi
par `quali/index.html`. Le code passe en paramètre de requête et non dans
le chemin, pour que la page réponde 200 : une 404 prive le lien de son
aperçu dans WhatsApp et iMessage, alors que c'est justement là qu'il se
partage. `quali/og.png` est l'image de cet aperçu (1200×630).

## Vérification de propriété du domaine

| Fichier | Ce qu'il active |
|---|---|
| `.well-known/apple-app-site-association` (+ copie à la racine) | Liens universels iOS, pour Dooka et Quali |
| `.well-known/assetlinks.json` | App Links Android |

Contraintes qu'iOS applique **sans rien signaler** quand elles ne sont pas
respectées : pas d'extension `.json` sur le fichier Apple, et aucune
redirection sur son URL. iOS ne le télécharge qu'à l'installation de
l'app : après une correction, il faut désinstaller et réinstaller.

Un domaine ne sert qu'**un seul** exemplaire de chacun de ces deux
fichiers : ils déclarent donc les deux apps, et ne doivent jamais être
remplacés par une version qui n'en mentionne qu'une.

Manquent encore :

- l'empreinte SHA-256 de Quali dans `assetlinks.json` (lisible en Play
  Console une fois le premier envoi fait) ;
- l'identifiant de la fiche App Store de Quali dans `quali/index.html`,
  aujourd'hui un placeholder.
