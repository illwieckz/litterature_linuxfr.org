URL:     https://linuxfr.org/news/retour-sur-la-liberation-du-code-source-de-ms-dos-1-25-et-2-0-par-microsoft
Title:   Retour sur la libération du code source de MS DOS 1.25 et 2.0 par Microsoft
Authors: Thomas Debesse
         gusterhack, palm123, abriotde, Benoît Sibaud, rapido, Pierre Jarillon, BAud, theojouedubanjo, Ysabeau, David Marec, Cetera, ʭ ☯ , yeahman, raphj et collinm
Date:    2018-09-29T01:52:00+02:00
License: CC by-sa
Tags:    dos, msdos, freedos, microsoft et libération
Score:   5


Microsoft a décidé le 28 septembre 2018 de **libérer** les sources des versions 1.25 et 2.0 de MS DOS sous licence MIT et de les publier sur GitHub.

Il s'agit des mêmes versions données au Computer History Museum ([[Musée de l'histoire de l'ordinateur]])  le 25 mars 2014, comme le précise Microsoft dans le fichier Readme.
Microsoft précise aussi dans son billet de blog que les deux versions (1.25 et 2.0) ont été écrites avec l'assembleur de l'Intel 8086, le premier processeur de la famille X86. 

L'entreprise précise que beaucoup de fichiers de documentations entrecoupés de code source d'extension .TXT et .DOC sont intéressants à lire tout comme de nombreux commentaires directement dans le code source.


On ne peut que se réjouir de cette libération bien qu'elle soit tardive. Mais à quand la libération de NTKRPAMP.EXE le noyau de Windows NT pour un système multiprocesseur ?

----

[Dépot sur GitHub](https://github.com/Microsoft/MS-DOS)
[Billet de blog de Microsoft](https://devblogs.microsoft.com/commandline/re-open-sourcing-ms-dos-1-25-and-2-0/)
[L'annonce sur Phoronix](https://www.phoronix.com/scan.php?page=news_item&px=Microsoft-MS-DOS-GitHub)

----

Petit historique de MS-DOS
--------------------------
![MS-DOS](https://upload.wikimedia.org/wikipedia/commons/b/b6/StartingMsdos.png)
Le système d'exploitation MS-DOS (Microsoft Disk Operating System) s'appelait à l'origine QDOS (Quick and Dirty OS) et fut développé par Tim Patterson de l'entreprise SCP (Seattle Computer Products) comme un clone de CP/M un système d'exploitation développé en 1974 par Gary Kidall.
La famille CP/M est différente de la famille Unix en termes de fonctionnement architectural.

QDOS a pris le nom de MS-DOS à la suite de son rachat par Bill Gates. Et fut porté sur l'IBM PC. 



### Version 1.00 (1981)
La première version sortie sur l'IBM PC.



Elle ne supporte que les disquettes 5″¼ simple face de 160 Kio.
![Disquette](https://upload.wikimedia.org/wikipedia/commons/thumb/a/aa/Floppy_disk_2009_G1.jpg/640px-Floppy_disk_2009_G1.jpg)

Les répertoires n'existaient pas sur cette version et on ne pouvait mettre que 64 fichiers sur une disquette.


### Version 1.25 (1982)
C'est une des versions qui a été [libérée](https://github.com/microsoft/MS-DOS/tree/master/v1.25).


Une légère évolution de la 1.24 et fut la première version vendue par Microsoft et, plus généralement, c'est la première version utilisée par tous les fabricants de clone des ordinateurs d'IBM.


Pour les disquettes 5″¼ on passe sur le support du double face avec une plus grande capacité (320 Kio). 

Microsoft précise que le code source de cette version ne contient que 7 fichiers assembleurs comme on peut le voir sur le dépôt GitHub.


### Version 2.0 (1983)
Deuxième version [libérée](https://github.com/microsoft/MS-DOS/tree/master/v2.0) par Microsoft.


Elle introduit la gestion des disques durs avec un système de fichiers FAT12, ainsi que les répertoires.


Les disquettes 5″¼ simple face gagnent en capacité passant de 160 Kio à 180 Kio tandis que les double face passent de 320 Kio à 360 Kio.


Par rapport à la 1.25 dans le code source, on passe de 7 fichiers à 100 fichiers assembleurs, presque 15 fois plus, augmentant en sophistication et en taille d'équipe comme le précise Microsoft. 


### Version 3.0 (1984)
Pour cette version on passe au support des disquettes de 1 200 Kio et les disques durs de 15 360 Kio.


### Version 6.22 (1994)
_La question qui se pose, est pourquoi ne pas avoir libéré le code de la dernière version commercialisée, la 6.22 ? Peut-être pour ne pas afficher les [pratiques controversées](https://en.wikipedia.org/wiki/MS-DOS#Use_of_undocumented_APIs) de ralentissement des concurrents?_
NdM : il s'agit de la dernière version commercialisée séparément. L'[historique des versions sur la page wikipedia](https://fr.m.wikipedia.org/wiki/MS-DOS) évoque une version 7 avec Windows 95 et une 8 avec Windows ME en 2000 (avant une fin de support en 2001). Et il mentionne aussi les problèmes rencontrés avec les versions 6.2x avec des brevets logiciels, ce qui pourrait être une autre explication (pour la 6.22 en tout cas, pas forcément pour la 3.0).

Les divers projets libres autour de MS-DOS
=========================================
Cet article nous permet aussi d'évoquer les projets continuant à entretenir l'environnement DOS.
Toujours en développement
-------------------------


### FreeDOS
![FreeDOS](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3d/FreeDOS_logo4_2010.svg/500px-FreeDOS_logo4_2010.svg.png)


[FreeDOS](https://www.freedos.org/), une alternative libre, a été créé en 1994 du fait de l'arrêt du support par Microsoft. 


Il est intéressant de voir que la version stable (1.0) de FreeDos, sortie en 2006, a mis 12 ans à arriver, chose rare dans le développement logiciel mais qui s'explique aisément par les faibles moyens à disposition. Il leur a fallu une bonne dose de ténacité pour ne pas abandonner.


#### Cette libération peut-elle profiter au projet FreeDOS ?


Pas directement, le code libéré (version 1.25 et 2.0) est entièrement écrit en assembleur alors que FreeDOS [combine du code C et de l'assembleur](https://sourceforge.net/p/freedos/svn/HEAD/tree/kernel/trunk/kernel/). Cependant cela évite la rétro-ingénierie pour comprendre le fonctionnement... sauf que c'est un peu trop tard, le projet FreeDOS a déjà un peu désossé tout DOS pour en comprendre le comportement. Cela permet juste de vérifier que cela n'a pas été trop mal fait. De plus FreeDOS a pour but d'être compatible mais pas de refaire forcément à l'identique. S'il peut faire mieux que MS-DOS, il le fera.

### DosBox



![DosBox](https://upload.wikimedia.org/wikipedia/commons/d/dd/DOSBox_icon.png)



[DOSBox](https://www.dosbox.com/) est un émulateur simulant un environnement compatible MS-DOS dans le but d'exécuter notamment des jeux vidéo développés autrefois pour ce système. Il est aussi possible de l'utiliser pour lancer tout type de logiciels. Ce projet est toujours dynamique et permet d'accéder à la culture du jeu vidéo sur PC de la fin des années 80, début des années 90 sans avoir à dénicher une vieille machine compatible.
## Abandonné
### PC-MOS/386


# Quel intérêt pratique?



- L'**intérêt académique** se situe à la fois sur le plan recherche et le plan enseignement. Cela permet aux élèves de comprendre comment fonctionnaient les premiers OS et donc de mieux appréhender les nouveaux. Cela peut constituer une première étape dans l'apprentissage de part leur relative simplicité. (en soi il existe aussi d'autres solutions comme [Minix](https://fr.wikipedia.org/wiki/Minix).) Après dans la recherche de nouveaux OS cela constitue un cas d'école des avantages/faiblesses de ce type d'OS.
- L'**intérêt historique / archivistique** réside dans le fait de pouvoir aisément présenter / manipuler un système ancien sans avoir à entretenir un matériel délicat dont les pièces seront de plus en plus difficiles à trouver. Par rapport à l'OS original il apporte une meilleure stabilité. C'est un peu comme l'intérêt de reproduire un objet du moyen-âge par rapport à utiliser l'original.
- L'**intérêt sécuritaire** est sans doute l'intérêt majeur, celui qui apporte le plus de fonds (même s'ils restent bien minimes). Outre les passionnés qui ont besoin d'une machine sans trop de failles de sécurité (et stable) pour jouer, il y a encore bien des organisations (de grosses sociétés ou des états) qui ont de très vieux logiciels souvent critiques qui tournent toujours. Parfois les spécifications sont perdues et il est donc bien plus économique de les faire durer ainsi. Citons le cas connu de [Haiku](https://en.wikipedia.org/wiki/Haiku_(operating_system)), clone de BeOS qui a été en partie financé par Jean-Louis Gassée,
- L'**intérêt réel** est minime et n'en vaut pas le travail qu'il demande. Ce qui motive avant tout c'est la passion de l'informatique. La passion de faire revivre un OS qui a connu une heure de gloire. Un peu comme rouler en voiture de collection. C'est inconfortable, mais cela évoque un autre monde qui avec le temps est enchanté... 

D'ailleurs dans un autre contexte [Cobol](https://fr.wikipedia.org/wiki/Cobol) un langage de programmation crée en 1959, du fait qu'il a toujours été confidentiel et réservé à des clients très riches, aucune communauté de passionnés ne l'a adapté à du matériel moderne, et c'est seulement très récemment qu'ont été développées des alternatives à base d'open-sources pseudo-compatibles pour les faire tourner sur des Linux bien meilleur marché. En attendant, les banques ont dû dépenser des sommes considérables pour les maintenir en fonction...


Se pose la question : quels outils pour ~~compiler~~ assembler ?
