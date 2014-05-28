```
URL:     http://linuxfr.org/users/illwieckz/journaux/sortie-de-punix-beta5-et-d-un-emulateur-68k-en-javascript
Title:   Sortie de Punix beta5, et d'un émulateur 68k en javascript !
Authors: Thomas DEBESSE
Date:    2012-04-28T14:16:13+02:00
License: CC by-sa
Tags:    calculatrice, javascript, emulateur et kernel
Score:   20
```


# Une cinquième beta pour Punix #

Pour ceux qui ont apprécié ma [dernière dépêche sur Punix](/news/punix-le-bapteme-du-feu), voici quelques nouvelles !

Christopher Williams a [sorti une 5ème version beta](http://punix-os.blogspot.fr/2012/04/beta-5-is-released.html) de son système d'exploitation Unix pour calculatrice à base de Motorolla 68000.
Cette version est disponible pour TI89 et TI92+, et les sources sont disponibles.
Si vous souhaitez compiler depuis les sources, il vous faudra vous munir de :

* [GCC4TI](http://trac.godzil.net/gcc4ti/)
* [RabbitSign](http://www.ticalc.org/archives/files/fileinfo/383/38392.html) 
* tib2xxu de [FreeFlash](http://www.ticalc.org/archives/files/fileinfo/368/36829.html)
(Pour compiler ce dernier sous Unix, il suffit de commenter la ligne ``#define WINDOWS``, et de compiler ainsi : ``gcc tib2xxu.c -o tib2xxu``)

C'est la première beta a tourner sur de matériel et non sur émulateur, le test qui avait motivé ma dépêche n'avait pas été publié officiellement. Pour la démonstration audio, cette version inclut un extrait du [Beau Danube Bleu](http://fr.wikipedia.org/wiki/Le_Beau_Danube_bleu). On retrouve donc le support officiel des niveaux de gris ainsi que l'antialiasing des caractères qui avait été annoncé. L'extrait audio inclut explique l'écart de taille entre cette beta 5 et la beta 4 (280 ko contre 76 ko), sans l'extrait audio elle ne ferait 96 ko).

Il faut noter que suite à un petit [article OSNews](http://www.osnews.com/story/25777/Punix_Unix-like_operating_system_for_the_TI-92_graphing_calculator) qui a mit un gros coup de projecteur sur le projet, Christopher avait publié une [petite FAQ](http://punix-os.blogspot.fr/2012/04/how-to-install-punix-on-calculator.html). À cette occasion il a annoncé officiellement travailler à supporter les modèles Voyages 200 et TI 89 Titanium.

Pour finir, Christopher a découvert avec une heureuse surprise que son projet était référencé sur la page [historique d'Unix d'Éric Lévénez](http://www.levenez.com/unix/).

# Un émulateur 68000 en javascript #

Vous aviez peut-être [entendu parler](/users/jiyuu/journaux/linux-dans-votre-navigateur-web) de [jslinux](http://bellard.org/jslinux/), un émulateur d'architecture PC exécutant Linux dans un navigateur et développé par Fabrice Bellard, et bien il n'est pas le seul à se lancer dans ce genre de projet un peu fou, et cette fois-ci, c'est Patrick Davidson qui s'y colle.

Patrick Davidson est un développeur très réputé dans le monde des calculatrices, il est l'auteur de jeux parmi les plus téléchargés sur plus de 10 ans, comme les différents Phoenix et sa version Platinum, disponible sur TI 82, 83, 83+, 84+, 85, 86 (Zilog z80), TI 89, 92, 92+, V200 (Motorola m68k), et Casio Graph 100+ (Nec V30Mx, compatible intel 286), les jeux Mercury, Monster…

Patrick a donc annoncé sur [son site web](http://www.ocf.berkeley.edu/~pad/) travailler sur un émulateur en javascript de calculatrice TI 68k, la 92+ précisémment.
Tout comme jslinux démarre linux, il fallait bien que l'émulateur démarre un système d'exploitation pour qu'il soit utile, et c'est [PedroM](http://www.yaronet.com/t3/?id=19) (en version 0.72) qui est démarré, préchargé avec les jeux de Patrick, évidemment !

L'émulateur est disponible à l'adresse suivante : http://www.ocf.berkeley.edu/~pad/emu/v3.html
Il fonctionne bien sous Firefox et Chrome, à vitesse normale, et devrait fonctionner sur tout navigateur supportant l'élément Canvas, et intégrant un interpréteur javascript assez rapide, évidemment. Attention, le code est très gros à charger (16Mo).

On peut donc lancer les jeux :

* [Phoenix 7](http://www.ticalc.org/archives/files/fileinfo/82/8207.html) (shoot'em'up vertical en noir et blanc)
* [Platinum Edition](http://www.ticalc.org/archives/files/fileinfo/203/20318.html) (shoot'em'up vertical en niveaux de gris et scrolling différentiel)
* [Mercury](http://www.ticalc.org/archives/files/fileinfo/235/23500.html) (shoot'em'up horizontal en niveaux de gris en vidéo inversée)
* [Monster](http://www.ticalc.org/archives/files/fileinfo/223/22382.html) (casse brique en niveaux de gris)
* [Smiley's Adventure](http://www.ticalc.org/archives/files/fileinfo/248/24824.html) (jeux de plateforme en noir et blanc)

![copie d'écran de Phoenix Platinum](http://thomas.debesse.free.fr/fourretout/patrick-davidson-emu-platinum-illwieckz.png)

Note : malheureusement les touches ont été pensées pour un clavier qwerty, et le résultat est un peu malheureux en azerty. Par exemple, dans les shoot'em'up, la touche "Lock" qui sert habituellement a tirer a été mappée sur la touche \ pas très accessible en azerty (AltGr+8).

Les codes de ces jeux sont couverts par une license très permissive.
License du code de Platinum, Phoenix, Mercury, Smiley's Adventure et Monster (noter le « I would appreciate » non obligatoire) :

> All portions of the program made by me are copyrighted by me, but may be freely used, copied, and/or modified with no restrictions.  There are a few
small sections based on the TIGCC library, which also may be freely redistributed.
> 
> However, I would appreciate if you at least do the following if you are making a modified version:
> 
> - Don't add any restrictions to its distribution or modification
> - Supply complete source code
> - Give me a reasonable amount of credit

Si le [code source de l'émulateur est disponible](http://www.ocf.berkeley.edu/~pad/emu/), ainsi que celui de quelques outils (conversion de rom), je n'ai pas encore trouvé de license. Mais il faut garder à l'esprit que ce n'est qu'une seconde alpha, et qu'il n'y a pas de nom non-plus, à part « Javascript TI-92 Plus emulator ». L'émulateur est donc en javascript, les outils de conversion en python, et les jeux en assembleur 68k.

Les plus curieux pourront se [repaître de ce code](http://www.ocf.berkeley.edu/~pad/emu/v3inst.js), attention c'est du lourd ;)

Maintenant, certains doivent se poser la même question que moi… À quand Punix dans un navigateur avec cet émulateur ?
