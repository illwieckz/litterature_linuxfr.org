```
URL:     http://linuxfr.org/users/illwieckz/journaux/quelques-actus-videoludiques
Title:   Quelques actus vidéoludiques
Authors: Thomas DEBESSE
Date:    2013-04-03T12:15:14+02:00
License: CC by-sa
Tags:    jeux_linux, unvanquished, ioquake et idtech
Score:   18
```


J'aurai voulu poster ce journal le mois dernier mais le temps est passé plus vite que prévu. Je voulais juste annoncer deux choses, une à propos d'Unvanquished, et une autre à propos d'ET:Legacy.

# Unvanquished a un an

La première annonce c'est qu'au mois de mars, le projet Unvanquished a soufflé sa première bougie !
L'anniversaire était le 2 mars, c'est pourquoi, pour honorer cette fête, l'alpha 13 est sortie un samedi et non un dimanche comme d'habitude.
Unvanquished a été évoqué dans de multiples journaux et dépêches [⁰](http://linuxfr.org/users/gillux/journaux/nouvelle-version-d-unvanquished-alpha-11) [¹](http://linuxfr.org/news/les-avancees-des-jeux-pour-gnu-linux-au-mois-d-octobre#toc_2) [²](http://linuxfr.org/users/illwieckz/journaux/sortie-d-unvanquished-alpha-7-et-une-riche-retrospective-en-anglais) [³](http://linuxfr.org/users/mcmic/journaux/venez-jouer-a-unvanquished) [⁴](https://linuxfr.org/users/illwieckz/journaux/une-histoire-de-fork), c'est un jeu mêlant stratégie et tir à la première personne. Comme le jeu a été abondamment décrit dans ces publications précédentes, je ne vais pas trop m'étendre dessus et si vous avez des questions, je vous invite à vous réferer aux dites publications.

Pour faire une petite rétrospective depuis la première fois que j'ai évoqué ce projet dans ces pages, et bien je dirais que le projet est bien vivant.

Premièrement le moteur et les données du jeu évoluent doucement mais sûrement au gré des versions alpha (une tous les premiers dimanches du moi, la prochain étant donc dimanche 7 avril, bientôt !).

La logique du jeu est modifiée aussi, la gestion des ressources de constructions passe d'un stock de point de constructions à utiliser pendant la partie à un système où il faut disposer intelligemment des _extracteurs_ pour gagner ces points. En plus de gérer la construction d'une base, assurer sa défense et établir des postes avancés en ligne ennemie, il faut désormais prendre soin d'extraire correctement ses ressources !

Ces derniers mois est apparu sur certains serveurs un mode _mission_ avec des _bots_ afin de s'entraîner. Pour donner un exemple, un scénario est que tous les joueurs sont obligatoirement des humains, et les bots des aliens sur une carte saturée d'œufs et autres tubes d'acides… En fait ce qui est étonnant, c'est qu'ils ont fait cela dans le but de faire un _tutoriel_ pour les nouveaux, le but premier n'était pas de faire au sens propre des _missions_. Ces missions sont un moyen d'apprentissage. Pour ceux qui se souviennent de l'arlésienne Tremulous 1.2 qui n'est jamais sortie et qui ne sortira probablement jamais, il était promis un mode _mission_, et une [carte avait fuité](http://tremulous-fr.tuxfamily.org/viewtopic.php?id=151&p=1), nommée _Siege_. Là, le projet Unvanquished vient de développer accessoirement en marge de ses objectifs et peut-être même sans vraiment s'en rendre compte, un point prioritaire et probablement le plus attendu de Tremulous 1.2. C'est très encourageant pour le projet et cela montre que le développement est efficace.

J'en profite pour publier ici un petit graph de l'historique d'Unvanquished, pour les curieux :

![historique d'Unvanquished](http://dl.illwieckz.net/u/thomas-debesse/bazar/path-unvanquished-game.svg)

Le développement est toujours aussi actif et le projet communique bien. Kharnov publie chaque dimanche un article décrivant l'avancement du projet et chaque premier dimanche du mois sort une nouvelle version. Il faut noter que malgré une faible publicité (le projet se déclare toujours en version alpha), la base de joueurs croît de manière assurée et il est désormais difficile de ne pas trouver d'autres joueurs en ligne (quand bien même ils seraient tous sur le même serveur ;)). Tout se passe sur [unvanquished.net](http://unvanquished.net).

Bon anniversaire Unvanquished !

# Première Release Candidate d'ET:Legacy

ET:Legacy est une reprise du développement du jeu [Wolfenstein: Enemy Territory](http://fr.wikipedia.org/wiki/Wolfenstein: Enemy Territory "Définition Wikipédia"). Wolfenstein: Enemy Territory est un jeu sorti en 2003 et développé sur une variante du moteur de Quake 3. Si le moteur id Tech 3 a été libéré le 19 août 2005 par id Software pour être repris par le projet ioQuake3, le moteur de Wolfenstein: Enemy Territory (et de son frère Return To Castle Wolfenstein) n'a été publié que le 17 août 2010. Je ne saurais pas décrire les différences entre les moteurs, mais ce doit être un patch assez imposant pour justifier un surplus de 5 ans… Deux projets sont nés, ioRTCW et ioWolfET.

Pendant tout ce temps le moteur XReaL (une modification très avancée d'id Tech 3) avançait toujours, développé par Tr3b (Robert Beckebans). Très vite après l'annonce de la publication d'ioWolfET, une petite équipe a annoncé travailler sur le projet ET:XreaL, rejoint par Tr3b qui abandonna aussitôt son propre projet de FPS qui n'était qu'un prétexte pour son moteur.

Malheureusement, ce n'est pas allé beaucoup plus loin… Il y a eu une [première sortie le 10 novembre 2010](http://sourceforge.net/projects/xreal/files/ETXreaL/) mais depuis plus rien.

Malheureusement (bis), ET:XreaL n'est pas aisé à installer pour un joueur du dimanche : il faut d'abord installer Wolfenstein: Enemy Territory à partir de l'installeur officiel, avec tous les désagréments qu'on peut rencontrer sous GNU/Linux lorsqu'on tente d'exécuter d'un binaire vieux de plusieurs années… puis copier manuellement les données extraites dans certains dossiers précis d'ET:XreaL, et je passe les détails d'installation d'XreaL. Perso j'avais réussi, mais quelle galère ! J'arrivais à lancer le jeu mais je ne crois pas avoir fait l'effort de vraiment rejoindre une partie…

Et puis de toute manière, Wolfenstein: Enemy Territory était compliqué à installer même sans XreaL, les libristes pouvant recompiler tout et n'importe quoi ne s'en privent pas, ce qui fait que les quelques binaires qui tentent de survivre sans recompilation passent pour des ovnis, et essayer d'exécuter un truc aussi vieux, c'est un truc à se casser les dents (même tenter d'installer Doom 3 qui est plus récent mêne souvent à un échec, surtout si on utilise une distribution 64bit).

Et puis pendant tout ce temps le développeur d'XreaL, Tr3b, a abandonné tout développement sur id Tech 3 (et donc sur son dérivé XreaL) pour travailler sur id Tech 4… laissant le travail en plan… Au passage, on notera que le seul jeu qui bénéficie du travail de Tr3b sur XreaL, c'est Unvanquished qui en a importé une bonne partie.

Et puis est venue une excellente nouvelle, le projet ET:Legacy est né.

Le but de ce projet ? Maintenir le jeu Wolfenstein:Enemy Territory vivant et à jour.. Les améliorations graphiques promises par XreaL ne sont pas recherchées, le but premier est de faire en sorte que ça marche et qu'on puisse jouer, et c'est une très bonne idée. Une attention toute particulière est aussi portée à la compatibilité avec les serveurs existants.

Si on devait faire un graph de l'historique d'ET:Legacy, cela donnerai quelque chose comme :
![historique d'ET:Legacy](http://dl.illwieckz.net/u/thomas-debesse/bazar/path-enemy-territory-legacy.svg).

Contrairement à Unvanquished qui est parti de Tremulous dans le but d'en dériver, ET:Legacy propose simplement de jouer au même jeu. Et en fait, quand on mesure le parcours du combattant qu'il fallait faire avant pour pouvoir y jouer, on peut s'exclamer que c'est déjà énorme !

Le projet ET:Legacy a donc publié sa première RC le 3 mars 2013 (il y a tout juste un mois) avec des binaires pour Windows, GNU/Linux, Mac OS X et [AROS](http://aros.sourceforge.net/fr/). J'ai testé sous GNU/Linux et jamais je n'avais installé Wolf:ET d'une manière aussi simple et aussi efficace. Aussi, fort de ce succès et satisfait, j'ai rejoint une partie pour la première fois de ma vie et ai joué quelques heures !

Alors je souhaite bon courage au projet ET:Legacy !

Toutes les informations intéressantes se trouvent sur le site [etlegacy.com](http://www.etlegacy.com/).

![Pièce 6 du concours Plee the Bear](http://www.stuff-o-matic.com/ptb/linuxfr/puzzle/puzzle-6.png)
