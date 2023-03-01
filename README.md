# Laboratoire 06 - Programmation TCP

Dans ce laboratoire, vous aurez à programmer un jeu de Tic Tac Toe permettant
à 2 joueurs de s'affronter à travers un réseau. Pour ce faire, vous devez partir
d'un jeu de Tic Tac Toe hors ligne qui vous est fourni, et le transformer pour
en faire un jeu en ligne.

Cette fois-ci, il n'y aura pas de programmes distincts pour le client et le serveur.
Les deux joueurs lanceront le même programme, et celui-ci demandera à chaque personne
à l'ouverture si elle veut créer une partie (et donc agir comme serveur) ou bien
joindre une partie créée par quelqu'un d'autre (et donc agir comme client).

Le dépôt de départ contient une solution Visual Studio 2022 comprenant le projet de départ.
Celui-ci est déjà configuré pour pouvoir utiliser le module réseau de SFML.

Commencez par cloner le dépôt localement et vous assurer que vous pouvez démarrer le projet.

## Résultat attendu

[Cette vidéo](https://youtu.be/ibomvG5TbuY) montre le résultat attendu du laboratoire.
Elle montre aussi comment lancer deux exécutions du même programme dans Visual Studio 2022.

## Consignes

### Étape 1 - Prendre connaissance du projet existant

Prenez le temps d'explorer le code existant dans les fichiers de départ, et d'exécuter
le projet pour bien comprendre le fonctionnement du programme. Observez surtout le code
de la fonction `jouer` et assurez-vous de le comprendre.

Durant le laboratoire, vous aurez seulement à modifier le fichier `main.cpp`. Vous
n'aurez pas à modifier la classe `TicTacToe`.

### Étape 2 - Décommenter le code du `main`

Vous devez décommenter le code qui se trouve présentement dans la fonction `main`, puis supprimer
la ligne indiquée par un commentaire. Testez le programme à nouveau. Vous devriez voir ceci:

```console
TIC TAC TOE
===========
Choisir une option:
1. Créer une partie
2. Joindre une partie
Votre choix: 
```

Lorsque l'option 1 est choisie, la fonction `jouer` est appelée en passant `true`
à son paramètre `estServeur`. Lorsque l'option 2 est choisie, la valeur `false`
est passée à ce paramètre.

En d'autres mots, l'hôte qui crée la partie doit agir comme serveur, et celui qui
la joint doit agir comme client.

### Étape 3 - Établir la connexion

*Pour cette étape, vous devez modifier **uniquement** le corps de la fonction `jouer` dans `main.cpp`.*

Au début de la fonction, vous devez ajouter la logique pour établir une connexion TCP
entre les deux joueurs. Le serveur doit donc écouter sur le port reçu en paramètre
et accepter une connexion entrante. Le client, pour sa part, doit plutôt se connecter
au serveur à l'aide de l'adresse fournie par une saisie au clavier.

Voici le pseudocode correspondant à cette logique:

```pseudocode
SI estServeur:
	Écouter sur le port reçu en paramètre
	Afficher « En attente de l'autre joueur... »
	Accepter une connexion entrante et la lier à un socket
	Afficher « L'autre joueur vient de se connecter »
SINON:
	Afficher « Entrer l'adresse du serveur: »
	Lire la saisie au clavier et l'assigner à adresseServeur
	Connecter un socket à adresseServeur au port fourni en paramètre
```

Vous devez bien entendu déclarer toutes les variables nécessaires. Vous n'avez
pas besoin de sélecteur, puisqu'un seul socket est nécessaire de part et d'autre.
Vous n'avez donc pas non plus besoin d'un vecteur de sockets.

Testez deux instances de votre programme (une en mode serveur et une
en mode client). Vous devriez obtenir le résultat suivant:

**Serveur**

```console
TIC TAC TOE
===========

Choisir une option:
1. Créer une partie
2. Joindre une partie

Votre choix: 1
En attente de l'autre joueur...
L'autre joueur vient de se connecter.

   1 2 3
  -------
a | | | |
  -------
b | | | |
  -------
c | | | |
  -------

C'est le tour des x.
Où voulez-vous placer votre x?
ligne colonne :
```

**Client**

```console
TIC TAC TOE
===========

Choisir une option:
1. Créer une partie
2. Joindre une partie

Votre choix: 2
Entrer l'adresse du serveur: 127.0.0.1

   1 2 3
  -------
a | | | |
  -------
b | | | |
  -------
c | | | |
  -------

C'est le tour des x.
Où voulez-vous placer votre x?
ligne colonne :
```

### Étape 4 - Attribuer une lettre à chaque joueur

*Pour cette étape, vous devez modifier **uniquement** le corps de la fonction `jouer` dans `main.cpp`.*

Il faut attribuer une lettre différente (`x` ou `o`) à chacun
des deux joueurs. Ajoutez une variable `lettreJoueur` pour cela, puis modifiez votre
code afin d'attribuer la lettre `x` à cette variable du côté serveur, et `o` du côté client.

### Étape 5 - Demander la saisie au bon joueur

*Pour cette étape, vous devez modifier **uniquement** le corps de la fonction `jouer` dans `main.cpp`.*

Présentement, à chaque tour, le programme affiche « C'est le tour des x » ou bien
« C'est le tour des o », puis demande une saisie, peu importe le joueur.

Il faut modifier ce comportement pour que la saisie soit demandée seulement au joueur
dont c'est le tour. À l'autre joueur, il faut plutôt afficher « C'est le tour de l'autre joueur. ».

Cette modification doit être implémentée dans la boucle de jeu
(`while (ticTacToe.getGagnant() == ' ' && !ticTacToe.estMatchNul())`).
Il ne faut pas modifier la condition de la boucle.

Voici le résultat que vous devriez obtenir après cette modification:

**Serveur**

```console
TIC TAC TOE
===========

Choisir une option:
1. Créer une partie
2. Joindre une partie

Votre choix: 1
En attente de l'autre joueur...
L'autre joueur vient de se connecter.


   1 2 3
  -------
a | | | |
  -------
b | | | |
  -------
c | | | |
  -------

C'est votre tour.
Où voulez-vous placer votre x?
ligne colonne : a 1

   1 2 3
  -------
a |x| | |
  -------
b | | | |
  -------
c | | | |
  -------

C'est le tour de l'autre joueur.
```

**Client**

```console
TIC TAC TOE
===========

Choisir une option:
1. Créer une partie
2. Joindre une partie

Votre choix: 2
Entrer l'adresse du serveur: 127.0.0.1

   1 2 3
  -------
a | | | |
  -------
b | | | |
  -------
c | | | |
  -------

C'est le tour de l'autre joueur.
```

### Étape 6 - Transmettre et traiter les données

*Pour cette étape, vous devez modifier **uniquement** le corps de la fonction `jouer` dans `main.cpp`.*

Nous voilà maintenant à l'étape la plus cruciale pour faire de notre
Tic Tac Toe un jeu en ligne!

Lorsqu'un joueur effectue son tour, la position à laquelle il place sa lettre doit être envoyée à l'autre hôte, et l'autre hôte doit la recevoir et la traiter. Il faut donc ajouter l'envoi, la réception et
le traitement des données à notre programme.

Notre protocole sera très simple: les hôtes s'enverront des objets de type `Packet` contenant le numéro de ligne (entre 0 et 2) suivi du
numéro de colonne (également entre 0 et 2). En d'autres mots, ils
s'enverront les valeurs des variables `ligne` et `colonne` qui existent
déjà dans le programme.

Voici à quoi devrait ressembler la logique de haut niveau de la boucle `while` après avoir implémenté ces aspects du programme. Certaines étapes de l'implémentation réelle sont omises ici (par exemple, la boucle *do-while* déjà présente qui valide le choix du joueur et 
le redemande au besoin).

```pseudocode
TANT QUE ticTacToe.getGagnant() = ' ' ET PAS ticTacToe.estMatchNul():
    Afficher ticTacToe
    SI lettreJoueur = tour:
        Afficher « C'est votre tour. »
        Lire la ligne et la colonne saisies par le joueur
        ticTacToe.jouer(ligne, colonne, tour)

        Créer un paquet contenant la ligne et la colonne
        Envoyer le paquet sur le socket
    SINON:
        Afficher « C'est le tour de l'autre joueur. »

        Recevoir un paquet
        Extraire la ligne et la colonne du paquet reçu
        ticTacToe.jouer(ligne, colonne, tour)
        Afficher « L'autre joueur a joué. »

    SI tour = 'x':
        tour = 'o'
    SINON:
        tour = 'x'
```

Si vous réutilisez le même `Packet` à chaque itération pour créer
votre paquet sortant, n'oubliez pas de le réinitialiser à la fin 
de la boucle.

### Étape 7 - Gérer la déconnexion

*Pour cette étape, vous devez modifier **uniquement** le corps de la fonction `jouer` dans `main.cpp`.*

Pour compléter notre programme, il faut aussi détecter lorsque l'autre joueur se déconnecte, et mettre
fin à la partie lorsque cela survient.

Les sockets TCP de SFML retournent l'état de la connexion (`status`) chaque fois qu'on fait un
`send` ou un `receive`. On peut donc s'en servir pour détecter une
déconnexion. Suite à cette détection, on veut afficher un message à
l'écran indiquant que l'autre joueur s'est déconnecté, puis mettre fin
à la fonction `jouer` (avec un `return;`).

Par ailleurs, à la fin de la fonction `jouer`, on veut appeler la méthode `disconnect` sur le socket
pour les deux hôtes afin de fermer la connexion proprement avant de passer à une autre partie.

Référez-vous aux notes de cours pour cette étape.

### Étape 8 (facultatif) - Tester sur des ordinateurs différents

Une fois que le tout est bien testé et fonctionnel, testez maintenant votre solution en exécutant le client et le serveur sur deux ordinateurs distincts!

## Références utiles

* Notes de cours du chapitre 06 sur Moodle
* [Tutoriel du module réseau de SFML](https://www.sfml-dev.org/tutorials/2.5/index-fr.php#module-rceseau)
* [Documentation des classes du module réseau de SFML](https://www.sfml-dev.org/documentation/2.5.1-fr/group__network.php)
