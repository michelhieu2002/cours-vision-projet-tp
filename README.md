# Cours Vision - Projet - 25/04/2019

Nous allons aborder la suite du projet démarré sur la partie NLP.

Avant toute chose, pensez à choisir un créneau pour votre groupe sur : https://doodle.com/poll/zzt9z8cy3ykdbcgs

L'objectif est d'incorporer la validation de l'état du damier avec notre robot. L'idée est de jouer une partie à l'oral contre notre robot, un déroulement serait ainsi :

  - Utilisateur : "I want to play in the middle cell." ** ==> L'utilisateur dessine une croix au milieu**
  - Bot : "Alright, I just played top-left."  ** ==> L'utilisateur dessine un rond en haut à gauche**
  - Bot : "I agree with your board state."
  - Utilisateur : "For this turn, I choose left." ** ==> L'utilisateur dessine une croix à droite **
  - Bot : "Oh, I think you made a mistake. You told me left but you drew your cross right... I'm a bit lost, can you please restart the game !"
  - ...

Dans l'idéal, au cours d'une partie, nous fixerions une caméra au-dessus d'un damier pour le filmer en continu. Notre robot pourrait ainsi confirmer qu'il est d'accord avec l'état ou emettre une opposition dans le cas d'un mauvais mouvement.

Le projet peut être découpé en plusieurs parties :

  1. Découpage du damier
  2. Classification de la case
  3. Reconnaissance du damier
  4. Intégration avec le chatbot

## Partie 1 : Découpage du damier

En partant d'une photo de damier rempli, extraire une image par case. On peut imaginer deux difficultées :
  - Une image de damier propre dessiné sous à l'ordinateur
  - Une photo d'un damier pris sur une feuille de papier ou un tableau blanc. Dans ce cas-là, il faudra certainement faire un seuillage pour binariser l'image.

Dans les deux cas, il sera nécessaire de détecter les traits du damier, on pourra par exemple :

  - Dans un premier temps supposer que les lignes du damier sont les valeurs extrêmes. C'est à dire qu'aucune croix ou rond ne sera ni plus haut, ni plus à gauche, ... que le trait de damier.
  - Dans un second temps, experimenter une détection de lignes avec OpenCV et/ou tenter de matcher la forme attendue...

## Partie 2 : Classification des cases

Cela necessite de générer un jeu de données d'entrainement et d'entrainer un modèle pour classifier nos cases en `cercle`, `croix` ou `vide`.

Il faudra donc :

  - Générer de la donnée d'entrainement
  - La labelliser
  - Faire de l'augmentation (rotations, translations, contraste, ...)
  - Choisir et entrainer un modèle de classification

## Partie 3 : Reconnaissance du damier

Mettre bout à bout les précedentes étapes en écrivant un script permettant, à partir d'une photo, de ressortir la composition du damier. 

En vue d'une future intégration, on peut également faire en sorte que le script se déclenche au moment où on l'on pousse une image. Deux solutions proposées :

  - Soit l'image est envoyée directement à notre API hébergeant la logique du jeu.
  - Soit on se sert du stockage d'AWS (S3) pour déposer notre image et déclencher une lambda pour traiter l'image.

## Partie 4 : Intégration avec le chatbot

Faire en sorte que le robot intéragisse avec le dessin et réagisse en conséquence. Il est possible dans une première étape de demander à l'utilisateur de prendre une photo et de l'envoyer au robot, mais une amélioration pourrait être d'utiliser la webcam et de demander au robot d'analyser en continu l'état du damier.

Le traitement sera possiblement trop long pour être traité par le chatbot. Dans ce cas, on peut imaginer faire appel au service text-2-speech d'AWS (Polly) pour générer une voix à côté qui ferait office "d'arbitre".

## Partie 5 : Proposer des améliorations

Le jeu est loin d'être parfait. Vous êtes libres de proposer des variations/améliorations pour améliorer l'expérience !
