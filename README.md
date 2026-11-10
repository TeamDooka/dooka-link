# dooka-link

Ce dépôt est servi tel quel par GitHub Pages sur **link.dooka.fr** (voir
`CNAME`). Il ne contient que des fichiers statiques : les liens
d'invitation des deux applications, et les fichiers de vérification qui
leur permettent d'ouvrir l'app au lieu du navigateur.

`.nojekyll` est indispensable — sans lui, GitHub Pages ignore les dossiers
qui commencent par un point, donc `.well-known/`.

## Dooka — `/join-group/<code>` et `/amis`

Le lien d'invitation est de la forme `https://link.dooka.fr/join-group/K4P9RX`.
Aucun fichier ne correspond à ce chemin : c'est `404.html` qui le rattrape
et lit le code dans l'URL (`?code=` aussi, pour tester en local). La page
affiche le code en grand avec un bouton pour le copier, un bouton qui ouvre
l'app quand même (`intent://` sur Android, `dooka://` ailleurs), les étapes
d'installation adaptées à l'appareil, et un QR code sur ordinateur. Toute
autre adresse inexistante tombe sur la même page, qui dit alors qu'elle
n'existe pas.

`amis/index.html` est le repli du bouton « Ouvrir Dooka » du mail de
demande d'ami, sur le même principe.

## Dooka — `/mdp/?token_hash=…&type=recovery`

Le bouton du mail « mot de passe oublié » (gabarit Supabase versionné
dans `supabase/email-templates/` du dépôt de l'app) mène à
`mdp/index.html`, avec le jeton à usage unique de Supabase. La page ne
le consomme pas : elle le transmet à l'app (`dooka://mdp?…`, par
`intent://` sur Android), qui le vérifie et affiche l'écran du nouveau
mot de passe, ou au formulaire de `dooka.fr/auth.html` (ordinateur,
version de l'app trop ancienne, demande faite depuis un autre
téléphone). Le chemin n'est volontairement déclaré dans aucun fichier
d'association : la page s'affiche toujours.

Ces pages suivent le style de dooka.fr : `assets/dooka.css`, polices
chargées depuis dooka.fr. La racine renvoie vers dooka.fr.

Pour les voir en local avec le comportement de GitHub Pages (un chemin
inconnu reçoit `404.html`), un simple `python -m http.server` ne suffit
pas : il faut un serveur qui serve `404.html` sur les chemins absents.
`?os=ios|android|desktop` force la variante, `?still` coupe l'animation.

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
