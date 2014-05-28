```
URL:     http://linuxfr.org/users/illwieckz/journaux/the-timeless-hacke-ta-machine-et-ton-cerveau
Title:   The Timeless hacke ta machine et ton cerveau
Authors: Thomas DEBESSE
Date:    2014-04-30T13:41:48+02:00
License: CC by-sa
Tags:    demoscène, optimisation et opengl
Score:   48
```


Du 18 au 21 avril à Saarbrücken (Allemagne), se tenait l’événement _[Revision 2014](http://2014.revision-party.net/)_, une des plus grandes _[[demoparty]]_ du monde. L’équipe [Mercury](http://mercury.sexy/) a publié une impressionnante démo dans la catégorie 64K (l’exécutable ne doit pas dépasser 64 Ko en taille) : _The Timeless_.

[![Mer](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/reduc/00001085.png)](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/00001085.png)

La démo dure environ 6 minutes (étape de pré-calcul omise). La première scène nous plonge dans une eau trouble que nous quittons pour contempler un soleil couchant sur une mer agitée, et dès les premières secondes cela m’a remémoré une autre démo, _[Elevated](http://www.pouet.net/prod.php?which=52938)_ par [Rgba](http://www.rgba.org/) et [TBC](http://www.pouet.net/groups.php?which=1623). Ces deux démos partagent le fait d‘utiliser intensivement la [génération procédurale](http://en.wikipedia.org/wiki/Procedural_generation) ainsi que le fait d’implémenter des effets de flous avancés afin de rendre la projection réaliste (ou de tromper son monde \^\^). Et puis la musique est sympa mais ça c’est commun à beaucoup de démos. ;)

# Visite des lieux

On pourrait distinguer deux parties. Durant la première, différentes scènes sont présentées, des vagues, des colonnades de marbres ([on me glisse à l’oreille que c’est un marbre rubanné de type onyx](http://marthe-debesse.net/)), un fond marin avec de drôles d’objets, les rues d’une cité de grattes ciels, les vitraux et les colonnes d’une cathédrale… Comme _Elevated_, _The Timeless_ use et abuse de génération procédurale. Certaines scènes sont moins léchées que d’autres (notamment certaines vagues ou les textures simplistes des immeubles), mais l’intérêt porte aussi sur cette diversité de scènes qui multiplient les procédures (il faut bien justifier les 64 ko !)…

[![Lumières](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/reduc/00008951.png)](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/00008951.png)

Après cette visite des lieux, les scènes sont revisitées et transformées, parfois il s’agit de simples transformations graphiques, comme lorsque la ville semble projetée dans un kaléidoscope, parfois ils s’agit de déformations des objets (comme les étranges blocs sous marins) ou des ajouts d’éléments (lumières supplémentaires principalement).

# Quand l’imperfection est dans l’équation

On notera particulièrement la composition de l’eau ou de l’air et ses poussières qui matérialisent ces fluides difficiles à rendre en immersion. On est habitué à représenter des fluides dans les démos, moins à les représenter de l’intérieur.

[![Eau](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/reduc/00000641.png)](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/00000641.png)

Un point notable de cette démo est le soin apporté aux effets d’imperfection : les scènes sont parfois vues avec un voile comme si la scène était vue à travers une vitre sale (dans certaines scènes je trouve que c’est tout de même un peu excessif), ou encore comme s’il y avait des poussières sur l’objectif, poussières qui s’illuminent selon la lumière de la scène, et gouttes d’eau sur l’objectif. On retrouve évidemment des effets de mise au point (et donc de hors-champ), des effets de _flare_ et autres aberrations optiques.

[![Humide](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/reduc/00003290.png)](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/00003290.png)

Ce n’est pas nouveau, on avait déjà vu ça dans leur précédente démo 64K (_[Luma](https://www.youtube.com/watch?v=WBuoyegCeXI)_). Alors dans _The Timeless_ cela fait parfois cache misère, mais utiliser des artifices pour impressionner son public avec très peu, c’est le principe des démos limités en tailles…

# Les secrets de l’illusionniste

Mais ce que je trouve le plus intéressant dans cette démo n’est pas la débauche d’information générée et d’effets appliqués par un si petit code, mais l’utilisation intelligente des imperfections pour bluffer le spectateur. La partie des _greetings_ aurait mérité une démo à elle seule (peut-être une 4K ?). La ville précédemment présentée est rendue cette fois-ci de nuit, sous la pluie, alors qu’une circulation tendue de véhicules laissent leur trainée de phares se refléter dans l’asphalte détrempé, sous les lumières des panneaux publicitaires…

[![Pluie](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/reduc/00009959.png)](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/00009959.png)

Mais non, point de véhicules, il n’y a que des points lumineux, des reflets et beaucoup de flou savamment maîtrisé… C’est cela qui m’intéresse le plus, il ne s’agit pas là d’exploiter une fonctionnalité non documentée du matériel pour rendre une scène indécemment détaillée, il s’agit ici d’exploiter les défauts de perception humaine pour suggérer ce que l’on ne peut pas coder dans 64K de code, et ça c’est vraiment cool.

# La part belle à OpenGL 4.4

Malheureusement la démo a été développée pour Windows et au moment de sa sortie seules les cartes nVidia étaient capable de la rendre (c’est [la config de référence](http://2014.revision-party.net/compos/pc) pour la compétition)).

Mais c’est intéressant de voir que cette démo a été développée pour OpenGL, alors certes ce n’est ni la première ni la dernière démo en OpenGL, mais je me souviens d’_Elevated_ qui avait été commencée pour OpenGL, mais l’une des optimisation avait été de la réécrire pour Direct3D en cours de route ! Dommage…

[![Marbre](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/reduc/00001619.png)](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/00001619.png)

Au moment de sa sortie, seules les cartes nVidia pouvaient rendre la démo parce qu’elle exploite des fonctionnalités d’OpenGL 4.4 et AMD avait du retard dans ses pilotes. Cependant une RC de Catalyst 14.4 apportant le support complet d’OpenGL 4.4 [est sortie dès le 22 avril](http://www.phoronix.com/scan.php?px=MTY2OTY&page=news_item) et une [sortie officielle a pointé son nez les jours suivant](http://www.phoronix.com/scan.php?page=news_item&px=MTY3MzA)… à essayer sous wine ?

[![Désert](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/reduc/00002836.png)](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/00002836.png)

On peut voir une démo comme un reflet de son époque, les choix techniques traduisent les intérêts et les compétences des compétiteurs. Quand une démo OpenGL gagne une compétition, cela signifie aussi que cette technologie est pertinente. OpenGL n’étais pas pertinent pour _Elevated_ en 2009, OpenGL est pertinent pour _The Timeless_ en 2014.

# Astuces et Tutoriaux

Il est encore trop tôt pour pouvoir lire de la littérature technique sur _The Timeless_, dans l’éventualité où il y en aurait car certains gardent jalousement leurs secrets… Je citais plus haut la migration de _Elevated_ depuis OpenGL vers Direct3D, c’est un renseignement que l’on retrouve dans le très intéressant document _[Behind Elevated](http://iquilezles.org/www/material/function2009/function2009.pdf)_ qui décrit des détails croustillants sur les dessous de cette démo légendaire. Il y a aussi de nombreux commentaires sur les effets de post-prod (en temps réel toujours) qui rendent l’image moins « parfaite » et plus réaliste (_motion blur_ etc.) et sur d’autres détails (brouillard, faire en sorte que la caméra tremble et semble affectée d’une masse dans ses mouvements). Iq a [publié sur son site personnel des tutoriaux](http://iquilezles.org/prods/#elevated) expliquant des astuces utilisée dans sa démo.

Si vous voulez vous initier à la génération procédurale, je ne peux que vous recommander les articles de rewind< publiés ici même :

* [Je crée mon jeu vidéo E10 : génération procédurale de carte (partie 1)](http://linuxfr.org/news/je-cree-mon-jeu-video-e10-generation-procedurale-de-carte-partie-1) ;
* [Je crée mon jeu vidéo E11 : génération procédurale de carte (partie 2)](http://linuxfr.org/news/je-cree-mon-jeu-video-e10-generation-procedurale-de-carte-partie-2).

# La démo, que dis-je, le monument

[![Cathédrale](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/reduc/00008433.png)](http://dl.illwieckz.net/u/thomas-debesse/bazar/demo-hg-the-timeless/00008433.png)

Chose amusante, le zip qui distribue la démo fournit une capture d’écran compressée en png… cette capture d’écran est 15 fois plus grosse que le binaire.

```
illwieckz@computer hg_the_timeless $ ls -lh
total 984K
-rw-rw-r-- 1 illwieckz illwieckz  61K avril 19 18:14 hg_the_timeless.exe
-rw-rw-r-- 1 illwieckz illwieckz 1,3K avril 19 14:33 hg_the_timeless.nfo
-rw-rw-r-- 1 illwieckz illwieckz 913K avril 19 11:07 hg_the_timeless.png
```

Une fois exécuté, le binaire alloue près de 200mo, et la quantité d’information envoyée à l’écran et à votre ampli audio doit avoisiner les 70Go… On est loin encore de la concision et la performativité d’un _« que la lumière soit, et la lumière fut »_, mais c’est déjà pas mal.

Mais voici donc le plus important :

* [Une vidéo de la démo _The Timeless_](https://www.youtube.com/watch?v=lwFVlNytq0Q), pour ceux qui ne pourront pas exécuter la démo (ils sont nombreux sur _LinuxFr_) ;
* [La fiche de _The Timeless_ sur pouet.net](http://www.pouet.net/prod.php?which=62935) ;
* [Le site de l’équipe Mercury](http://mercury.sexy/).
