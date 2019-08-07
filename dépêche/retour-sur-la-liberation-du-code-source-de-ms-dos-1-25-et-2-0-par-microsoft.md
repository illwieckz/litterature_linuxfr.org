URL:     https://linuxfr.org/news/retour-sur-la-liberation-du-code-source-de-ms-dos-1-25-et-2-0-par-microsoft
Title:   Retour sur la libération du code source de MS-DOS 1.25 et 2.0 par Microsoft
Authors: Thomas Debesse
         gusterhack, palm123, abriotde, Benoît Sibaud, rapido, Pierre Jarillon, BAud, theojouedubanjo, Davy Defaud, Ysabeau, David Marec, ʭ  ☯ , Cetera, raphj, yeahman et collinm
Date:    2018-09-29T01:52:00+02:00
License: CC by-sa
Tags:    dos, msdos, freedos, microsoft et libération
Score:   9


Microsoft a décidé le 28 septembre 2018 de **libérer** les sources des versions 1.25 et 2.0 de MS-DOS sous licence MIT et de les publier sur GitHub.

[![MS-DOS Logo](https://raw.githubusercontent.com/microsoft/MS-DOS/master/msdos-logo_250x250.png)](https://github.com/Microsoft/MS-DOS/blob/master/msdos-logo.png)

Il s’agit des mêmes versions données au Computer History Museum ([[Musée de l'histoire de l'ordinateur]]) le 25 mars 2014, comme le précise Microsoft dans le fichier [`README.md`](https://github.com/microsoft/MS-DOS/blob/master/README.fr-FR.md).
Microsoft précise aussi dans son billet de blog que les deux versions (1.25 et 2.0) ont été écrites avec l’assembleur de l’Intel 8086, le premier processeur de la famille X86.

L’entreprise précise que beaucoup de fichiers de documentations d’extension `.TXT` et `.DOC` intercalés entre les fichiers sources sont intéressants à lire tout comme de nombreux commentaires directement dans le code source.

On ne peut que se réjouir de cette libération bien qu’elle soit tardive. Mais à quand la libération de `NTKRPAMP.EXE`, le noyau de Windows NT pour un système multiprocesseur ?

----

[Dépot sur GitHub](https://github.com/Microsoft/MS-DOS)
[Billet de blog de Microsoft](https://devblogs.microsoft.com/commandline/re-open-sourcing-ms-dos-1-25-and-2-0/)
[L’annonce sur Phoronix](https://www.phoronix.com/scan.php?page=news_item&px=Microsoft-MS-DOS-GitHub)

----

# Petit historique de MS-DOS

![MS-DOS](https://upload.wikimedia.org/wikipedia/commons/b/b6/StartingMsdos.png)

Le système d’exploitation MS-DOS (_Microsoft Disk Operating System_) s’appelait à l’origine _QDOS_ (_Quick and Dirty OS_) ou _86-DOS_ et fut développé par Tim Patterson de l’entreprise SCP (_Seattle Computer Products_) comme un clone de CP/M, un système d’exploitation développé en 1974 par Gary Kidall de chez _Digital Research_. CP/M était conçu pour les processeurs Intel 8080/Zilog 280, _86-DOS_ visait donc à fournir un environnement familier sur la nouvelle architecture matérielle 8086 d’Intel. QDOS a pris le nom de MS-DOS à la suite de son rachat par Bill Gates.

Le système CP/M (_Control Program/Monitor_) devait être initialement le système d’exploitation de l’IBM PC, mais alors que CP/M-86 était en développement les relations entre Digital Research et IBM se sont dégradées autour des questions de facturation (le premier préférant un achat unique de licence quand le second désirait une redevance), et parce que les accords de non-divulgation (NDA) demandés par IBM ne convenaient pas à Digital Research.

Bill Gates a saisi l’occasion et conclu un accord commercial avec IBM pour que celui-ci fonde son PC-DOS sur MS-DOS. C’est ainsi que PC-DOS, fut distribué avec les premières livraisons de l’IBM PC à l’automne 1981. La version de CP/M compatible avec le processeur 8086 fut disponible dès le printemps 1982 (à peine six mois plus tard) mais ce système d’exploitation fut un échec commercial, pris de vitesse par PC-DOS. Découvrant l’affaire, Digital Research menaça IBM d’une action en justice pour contrefaçon de propriété intellectuelle et obtint d’IBM que CP/M-86 soit proposé comme une alternative à PC-DOS, mais cela ne pu infléchir la route de PC-DOS/MS-DOS et le succès de Microsoft.

Tandis que les premiers clones de l’IBM PC aparaissait, Microsoft conclu des accords avec les autres fabricants pour distribuer MS-DOS. MS-DOS devint le système majoritaire en prenant le pas sur PC-DOS, et les deux branches étant développées séparément, des incompatibilités furent parfois introduites.

La famille CP/M est différente de la famille Unix en termes de fonctionnement architectural. De plus MS-DOS est un système mono-utilisateur car Microsoft réservait cette fonctionnalité pour son Unix maison nommé _Xenix_.


# L’impact d’MS-DOS sur l’industrie et la culture

MS-DOS a conservé de CP/M le concept de BIOS qui permet d’abstraire le matériel afin de faire fonctionner une version unique du système d’exploitation sur différents modèles de machines. L’accord commercial entre Microsoft et IBM autour de MS-DOS et de l’IBM PC ont rendu très populaire ce concept au cœur du développement de l’informatique personnelle, facilitant plus tard l’essor d’un certain noyau _Freax_ visant le processeur 386 (renommé rapidement en _Linux_)…

Microsoft Windows utilise toujours aujourd’hui le caractère `\` comme séparateur de chemin bien que le reste de l’industrie utilise `/`. Cela vient du fait que DOS a hérité de CP/M le caractère `/` comme préfixe d’option (qu’il semble avoir hérité lui-même de VMS). Un [ingénieur de Microsoft](https://blogs.msdn.microsoft.com/larryosterman/2005/06/24/why-is-the-dos-path-character/) a raconté que, parce que les employés de Microsoft développaient DOS sur Xenix, ils étaient habitués à utiliser le caractère `/` comme séparateurs de chemin et `-` comme préfixe d’option. Ceux-ci implémentèrent donc dans certaines parties de DOS (NDLR: mais pas toutes) la capacité de décoder les caractères `/` et `\` indifféremment dans les chemins, et ajoutèrent une variable `SWITCHAR` à mettre dans le fichier `config.sys` pour changer le préfixe d’option de `/` en `-`. Larry Osterman dit dans son billet que l’option a disparu depuis longtemps, mais nous pouvons cependant [la retrouver](https://github.com/microsoft/MS-DOS/search?q=SWITCHAR&unscoped_q=SWITCHAR) dans le code de DOS 2.0 qui vient d’être libéré

MS-DOS fut plusieurs fois adapté sous licence par des tiers (PC-DOS, Compaq DOS…) et des projets complètements indépendants ont été développés pour être compatible avec lui : DR-DOS, FreeDOS (on en parle après), ou encore PTS-DOS, une version russe certifiée par le Ministère de la Défense de la Fédération de Russie.

MS-DOS ou ses clones furent très souvent utilisés au cœur de systèmes industriels ayant une grande longévité, ce qui rend parfois nécessaire sa maintenance aujourd’hui. MS-DOS fut souvent utilisé comme système autonome pour des utilitaires sur disquettes (ancêtres des live-CDs sous Linux et autres live-USBs) mais son usage a largement dépassé le cadre de l’ordinateur personnel.

Tout comme nous trouvons aujourd’hui Linux dans de nombreux appareils du quotidien comme nos téléphones Android, nos box Internet, des télévisions ou même des réfrigérateurs, MS-DOS ou ses clones se sont retrouvés dans certains objets parfois inattendus, comme le téléphone [Nokia 9110 Communicator](https://en.wikipedia.org/wiki/Nokia_9000_Communicator#9110) ou la calculatrice graphique [[Casio Graph 100+]] qui exécutent un ROM-DOS de Datalight (équivalent à MS-DOS 2) sur un processeur AMD dérivé du 486 pour le premier et un processeur Nec dérivé du 286 pour la seconde. Ceci permit à des enthousiastes de développer pour cette calculatrice leurs propres programmes en utilisant par exemple la suite Turbo C 3 de Borland (C/C++/Assembleur), le compilateur C de Digital Mars ou encore QBasic de Microsoft.

DOS est aussi devenu, à l’instar du jeu _DOOM_, un défi pour beaucoup de développeurs : celui de le porter sur le plus d’architecture possible (ou aussi, dans le cas de DOS, de mettre en œuvre des clones). On peut citer par exemple la [version de FreeDOS](https://www.magiclantern.fm/forum/index.php?topic=14853.0) conçue pour tourner sur les apareils photos Canon EOS (comme module de l’extension non-officielle Magic Lantern), la très récente inclusion de DosCard (une version incorporable de DosBox, voir ci-après) [dans le moteur OpenMW](https://www.youtube.com/watch?v=m3_be5weKW8) permettant d’avoir un environnement de travail DOS dans l’univers virtuel du jeu Morrowind. Microsoft lui-même s’est prêté à ce jeu lorsque le 1er avril 2015 l’entreprise a publié une application appelée « MS-DOS Mobile » présentée comme un nouveau système d’exploitation mobile, et qui fournissait une interface similaire à MS-DOS sur leurs téléphones sous Windows.

Enfin, le clône libre FreeDOS (dont nous parlons juste après) est parfois installé sur certaines machines vendues sans Windows ou sans Linux afin de livrer la machine avec un fonctionnement minimal. Certaines machines vendues « sans OS » ont donc parfois bien un système d’exploitation quand-même !


# Les versions de Microsoft DOS

## Version 1.00 (1981)

La première version sortie sur l’IBM PC.

Elle ne prend en charge que les disquettes 5″¼ simple face de 160 Kio.

![Disquette](https://upload.wikimedia.org/wikipedia/commons/thumb/a/aa/Floppy_disk_2009_G1.jpg/640px-Floppy_disk_2009_G1.jpg)

Les répertoires n’existaient pas sur cette version et on ne pouvait mettre que 64 fichiers sur une disquette.


## Version 1.25 (1982)
C’est une des versions qui a été [libérée](https://github.com/microsoft/MS-DOS/tree/master/v1.25).


Une légère évolution de la 1.24. Elle fut la première version vendue par Microsoft et, plus généralement, c’est la première version utilisée par tous les fabricants de clone des ordinateurs d’IBM.


Pour les disquettes 5″¼ on passe sur le support du double face avec une plus grande capacité (320 Kio).

Microsoft précise que le code source de cette version ne contient que 7 fichiers assembleurs comme on peut le voir sur le dépôt GitHub.


## Version 2.0 (1983)
Deuxième version [libérée](https://github.com/microsoft/MS-DOS/tree/master/v2.0) par Microsoft.


Elle introduit la gestion des disques durs avec un système de fichiers FAT12, ainsi que les répertoires.


Les disquettes 5″¼ simple face gagnent en capacité passant de 160 Kio à 180 Kio tandis que les double-face passent de 320 Kio à 360 Kio.


Par rapport à la 1.25, dans le code source on passe de 7 fichiers à 100 fichiers assembleurs, presque 15 fois plus, augmentant en le code sophistication et augmentant la taille d’équipe comme le précise Microsoft.


## Version 3.0 (1984)

Cette version introduit la prise en charge des disquettes de 1 200 Kio et les disques durs de 15 360 Kio.


## Version 6.22 (1994)

_La question qui se pose est : pourquoi ne pas avoir libéré le code de la dernière version commercialisée, la 6.22 ? Peut-être pour ne pas afficher les [pratiques controversées](https://en.wikipedia.org/wiki/MS-DOS#Use_of_undocumented_APIs) de ralentissement des concurrents?_ (NdM : il s’agit de la dernière version commercialisée séparément).

L’[historique des versions sur la page wikipedia](https://fr.m.wikipedia.org/wiki/MS-DOS) évoque une version 7 avec Windows 95 et une 8 avec Windows ME en 2000 (avant une fin de prise en charge en 2001). sont aussi mentionnés les problèmes rencontrés avec les versions 6.2x à cause de des brevets logiciels. Ce pourrait être une explication, du moins pour la la 6.22 (pas forcément pour la 3.0).


# Les divers projets libres autour de MS-DOS

Wikipédia possède des listes très détaillées de [systèmes compatibles DOS ou similaires à DOS](https://en.wikipedia.org/wiki/List_of_disk_operating_systems), ainsi que des [comparaisons](https://en.wikipedia.org/wiki/Comparison_of_DOS_operating_systems) ou des informations très intéressantes sur les [couches de compatibilité](https://en.wikipedia.org/wiki/Virtual_DOS_machine) permettant d’exécuter des programmes DOS sur des systèmes plus récents. Mais _DLFP_ oblige, nous nous concentrons sur les projets libres continuant à entretenir l’environnement DOS.

À noter qu’il existait un système d’exploitation nommé OpenDOS, un descendant de DR-DOS de Digital Research (et donc de CP/M-86 via la branche _Concurrent PC-DOS_, sa version multi-utilisateur). Mais bien que ce système s’appelle OpenDOS et que le source ait été distribué, celui-ci n’a rien de libre. En effet, [selon les termes de Caldera](https://web.archive.org/web/19961018220910/http://caldera.com/news/pr002.html), le source aurait été rendue disponible pour une utilisation personnelle à coût nul, et une redistribution commerciale d’OpenDOS nécessitait une licence payante. L’annonce disait également que les sources de certains composants tierce-partie de _Novell Dos 7_ ne seraient pas publiés, ce qui fait d’OpenDOS une forme d’_[[Open core]]_ dont même le cœur n’est pas libre car soumis à une clause non-commerciale.

Le logiciel [rpix86](http://rpix86.patrickaalto.com/) est similaire à DOSBox que nous présentons ci-après et permet d’exécuter des jeux DOS sur un Raspberry Pi mais n’est malheureusement pas libre, et le seul interpréteur qu’il est capable d’exécuter est [4DOS](https://www.4dos.info/) qui, bien que le source soit disponible, est malheureusement couvert par une licence MIT modifiée qui n’est pas reconnue comme libre ni par l’[OSI](https://fr.wikipedia.org/wiki/Open_Source_Initiative) ni par la [FSF](https://fr.wikipedia.org/wiki/Free_Software_Foundation), une clause interdisant l’usage commercial. Le programme _rpix86_ tournant sous Linux, on peut donc tout de même le citer, parce que dans _LinuxFr_ il y a _Linux_…


## Toujours en développement

### FreeDOS

![FreeDOS](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3d/FreeDOS_logo4_2010.svg/500px-FreeDOS_logo4_2010.svg.png)


[FreeDOS](https://www.freedos.org/), une alternative libre qui a été créée en 1994 du fait de l’arrêt du support par Microsoft.


Il est intéressant de voir que la version stable (1.0) de FreeDOS, sortie en 2006, a mis 12 ans à arriver, chose rare dans le développement logiciel mais qui s’explique aisément par les faibles moyens à disposition. Il a fallu aux développeurs une bonne dose de ténacité pour ne pas abandonner.

FreeDOS est souvent comparé à [ReactOS](https://reactos.org/) qui vise à être une alternative libre à Microsoft Windows, il peut aussi être comparé à [Haiku](https://www.haiku-os.org/), clone de BeOS compatible avec celui-ci.


### DOSBox

![DOSBox](https://upload.wikimedia.org/wikipedia/commons/d/dd/DOSBox_icon.png)


[DOSBox](https://www.dosbox.com/) est un émulateur simulant un environnement compatible MS-DOS dans le but d’exécuter notamment des jeux vidéos développés autrefois pour ce système. Il est aussi possible de l’utiliser pour lancer tout type de logiciels. Ce projet est toujours dynamique et permet d’accéder à la culture du jeu vidéo sur PC de la fin des années 80/début des années 90 sans avoir à dénicher une vieille machine compatible. Bien que DOSBox soit capable de faire tourner FreeDOS, DOSBox fournit sa propre implémentation et son propre interpréteur de commande DOS.

DOSBox permet aux particuliers d’exécuter leurs anciennes applications et jeux DOS mais permet également à des sociétés de redistribuer leurs anciens jeux pour de nouveaux systèmes en empaquetant DOSBox avec leurs jeux, de [très nombreux jeux](https://en.wikipedia.org/wiki/Category:Games_commercially_released_with_DOSBox) ont été distribués officiellement ainsi.


## Projets abandonnés

### PC-MOS/386

PC-MOS/386 était un système d’exploitation compatible DOS multi-utilisateur et multi-tâche développé par une entreprise américaine du nom de _The Software Link_ et qui fut distribué pour la première fois en 1987, et fut [libéré](https://github.com/roelandjansen/pcmos386v501) en 2017 sous licence GPLv3. Comme son nom l’indique, PC-MOS/386 visait les processeurs 386 qui ont une architecture bien plus récente que celles prises en charges par les premières versions de DOS dont nous parlons dans cet article. La dernière version de PC-MOS/386, la version 5.01, était compatible avec MS-DOS 5 et requiert une MMU ([[Unité de gestion mémoire]]) ce qui la rend de fait incompatible avec les processeurs 8086. Il était cependant possible d’installer une extension matérielle entre le _socket_ et un processeur 286 pour rendre l’ordinateur compatible.


### S/DOS

[L’article Wikipédia anglophone sur PTS-DOS](https://en.wikipedia.org/wiki/PTS-DOS) (le DOS russe) rapporte que d’anciens développeurs de _PhysTechSoft_ ont quittés cette société et fondé la société _Paragon Technology Systems_. Ayant emporté avec eux le code source de PTS-DOS, ils auraient distribués une version nommée PTS/DOS 6.51CD ainsi qu’une version diminuée nommée S/DOS 1.0 (« Source-DOS ») qui serait une version « open source ». Il est cependant très difficile de trouver des traces de cette distribution à part de vagues mentions sur un [un document du projet FreeDOS](https://www.freedos.org/technotes/press/2006-doshist.txt) qui évoque la distribution des sources mais pas le caractère « libre » ni la licence utilisée. Même si on en retrouvait la trace, ce projet ne serait pas vraiment utilisable car selon PhysTechSoft, S/DOS violerait leur paternité intellectuelle ainsi que les lois militaires russes…


### CP/M

Ce n’est pas tout à fait DOS, mais étant donné le lien étroit entre DOS et CP/M, il serait dommage de ne pas le citer. Le code source de CP/M [a été libéré en 2001](https://www.theregister.co.uk/2001/11/26/cp_m_collection_is_back/) par l’ayant droit du moment (l’entreprise Lineo). Le code source peut être [retrouvé sur le site du Computer History Museum](https://www.computerhistory.org/atchm/early-digital-research-cpm-source-code/#code) et [sur le site de passionnés _The Unofficial CP/M Web Site_](http://www.cpm.z80.de/source.html). Toutes les versions ne sont pas disponibles (1.1, 1.3, 1.4 et 2.0 sont incluses) et certaines sont incomplètes. Certaines parties ont été obtenues par désassemblage. Initialement CP/M était surtout programmé en langage [[PL/M]] et fut réécrit en assembleur. Sont disponibles également des _scans_ de _listings_ originaux. Selon l’[article Wikipédia anglophone](https://en.wikipedia.org/wiki/CP/M-86), le code serait couvert par une licence similaire aux licences BSD.


### DOSEMU

Un peu comme DOSBox, DOSEMU visait à permettre d’exécuter des applications DOS sous Linux. DOSEMU réalisait ceci en fournissant une couche de compatibilité pour un véritable système d’exploitation comme FreeDOS. Bien que pleinement fonctionnel et toujours disponible dans certaines distributions Linux aujourd’hui, le développement semble s’être arrêté en 2012.


# Cette libération peut-elle profiter au projet FreeDOS ?

Pas directement, le code libéré (version 1.25 et 2.0) est entièrement écrit en assembleur alors que FreeDOS [combine du code C et de l’assembleur](https://sourceforge.net/p/freedos/svn/HEAD/tree/kernel/trunk/kernel/). Cependant cela évite la rétro-ingénierie pour comprendre le fonctionnement… sauf que c’est un peu trop tard : le projet FreeDOS a déjà un peu désossé tout DOS pour en comprendre le comportement. Le code source permet cependant de vérifier que cela a été fait correctement. De plus, FreeDOS ayant pour objectif d’être compatible mais pas de refaire forcément à l’identique, s’il peut faire mieux que MS-DOS, il le fera.


# Quel intérêt y trouver?

- L’**intérêt académique** se situe à la fois sur le plan de la recherche et le plan de l’enseignement. Cela permet aux élèves d’étudier et comprendre comment fonctionnaient les premiers systèmes d’exploitations et donc de mieux appréhender les nouveaux. Cela peut constituer une première étape d’apprentissage de part la relative simplicité de ces systèmes. Il existe aussi d’autres solutions comme [Minix](https://fr.wikipedia.org/wiki/Minix), mais un système comme MS-DOS constitue aussi un cas d’école des avantages et faiblesses de ce type de système.
- L’**intérêt historique / archivistique** réside dans le fait de pouvoir aisément présenter / manipuler un système ancien sans avoir à entretenir un matériel délicat dont les pièces seraient de plus en plus difficiles à trouver. Cela apporte une stabilité qui dépend moins de l’environnement. On peut comparer cet intérêt avec celui de savoir reproduire un objet du Moyen-Âge pour explorer son usage sans avoir à utiliser l’original et le mettre en danger.
- L’**intérêt sécuritaire** est sans doute l’intérêt le plus susceptible de motiver un travail, voire un travail rémunéré (même si cela devient rare et minoritaire car très spécialisé). Outre les passionnés qui ont besoin d’une machine sans trop de failles de sécurité (et stable) pour jouer, il y a encore bien des organisations (de grosses sociétés ou des états) qui ont de très vieux logiciels souvent critiques qui tournent toujours. Parfois les spécifications sont perdues et il est donc bien plus économique de les faire durer ainsi. L’entreprise Microsoft elle-même a déjà été [réduite à modifier un ancien exécutable] pour corriger un problème de sécurité… modification opérée directement sur le binaire, donc sans recompilation depuis le source. Il existe donc nombre de composants qu’il faut « conserver ainsi », parfois même chez les éditeurs de ces composants…
- L’**intérêt pratique** est donc minime et ne vaut pas le travail qu’il demande. Ce qui motive avant tout c’est la passion de l’informatique. La passion de faire revivre un système d’exploitation qui a connu son heure de gloire. C’est un peu comme rouler avec une voiture de collection : c’est inconfortable mais cela évoque un autre monde qui, avec le temps, est enchanté…

Nous pouvons comparer, dans un autre contexte, au langage de programmation [Cobol](https://fr.wikipedia.org/wiki/Cobol) crée en 1959. Parce qu’il a toujours été confidentiel et réservé à des clients fortunés, aucune communauté de passionnés ne l’avait adapté à du matériel moderne. C’est seulement très récemment qu’ont été développées des alternatives à base de logiciels libres pseudo-compatibles pour faire tourner les logiciels Cobol sur des systèmes Linux plus accessibles financièrement. Mais en attendant, les banques ont dû dépenser des sommes considérables pour maintenir ces logiciels en fonction…

Se pose une dernière question : quels outils pour ~~compiler~~ assembler ?
