```
URL:     http://linuxfr.org/users/illwieckz/journaux/essai-de-lightworks-beta-11-1-h-sous-gnu-linux
Title:   Essai de Lightworks Beta 11.1.h sous GNU/Linux
Authors: Thomas DEBESSE
Date:    2013-05-01T22:13:28+02:00
License: CC by-sa
Tags:    test, vidéo, montage et lightworks
Score:   13
```


Comme annoncé dans le [journal marque-page de suppositoire<](http://linuxfr.org/users/suppositoire--2/journaux/la-beta-de-lightworks-pour-linux-est-enfin-disponible), EditShare [a annoncé hier la publication de Lightworks en bêta sous Linux](http://www.lwks.com/index.php?option=com_kunena&func=view&catid=19&id=44717&Itemid=81).

Alors attention, malgré que [la publication de Lightworks sous Linux avait été annoncée comme « Open Source » en 2010](http://www.phoronix.com/scan.php?page=news_item&px=ODE1MQ), il aura fallu 3 ans pour qu'une première publication voit le jour sous GNU/Linux, et en plus elle n'est pas libre. Mais parce que « libre » ne veut pas forcément dire « sous Linux », on pourrait se poser la questions de son statut sous d'autres systèmes. Depuis trois ans, aucune des versions publiées pour Windows n'ont été libre, non plus.

Plusieurs [soupçonnent EditShare de faire de l'OpenSource Washing](http://linuxfr.org/users/antistress/journaux/editshare-lightworks-et-l-open-source-washing). Le mal est fait, certains sites spécialisés [référencent Lightworks comme un logiciel libre](http://lprod.org/wiki/doku.php/video:lightworks). Pourtant, à moins d'un revirement inattendu, l'avenir de Lightworks n'est pas encourageant pour le libre.

Mais libre ou pas, comme Lightworks est désormais installable sous GNU/Linux, son test devient accessible à beaucoup plus de monde et il pourra inspirer d'autres projets, libres.

J'ai essayé pour vous Lightworks !

Nous allons surtout nous attarder sur son interface et son ergonomie, choses qui peuvent toujours inspirer des projets libres même si Lightworks ne l'est pas.

_Note : les copies d'écran recadrées ou redimensionnées sont cliquables et pointent vers leur version originale non recadrée ni redimensionnée._

# Préparatifs

Je m'étais donc inscrit dès l'annonce en 2010… mais je n'ai jamais pu tester Lightworks. J'avais essayé de l'installer avec [Wine](http://www.winehq.org/) mais si l'installation se faisait bien et que j'arrivais à lancer Lightworks, je n'allais pas très loin…

Pour tester Lightworks sous GNU/Linux, il faut tout d'abord être inscrit sur leur site, avoir un profil utilisateur assez rempli (notamment votre métier et l'outil de montage que vous utilisez actuellement sont des champs obligatoires), puis faire une demande de participation au programme d'essai. Le formulaire demande entre autre la distribution cible. Des dérivés de Debian sont proposés essentiellement (Ubuntu, Mint). Une fois le formulaire rempli, un mail est envoyé automatiquement donnant les instructions nécessaires pour télécharger le ``.deb``. Vous trouverez les informations nécessaires sur [www.lwks.com/betas-linux](http://www.lwks.com/betas-linux).

Lightworks n'est pour le moment compilé que pour l'architecture x86_64. Lightworks recommande d'utiliser une carte graphique ATI ou nVidia avec le pilote propriétaire et parmi les deux, recommande nVidia. Le paquet dépend entre autre du paquet [``nvidia-cg-toolkit``](https://developer.nvidia.com/cg-toolkit). Bon, j'ai testé pour vous avec une carte graphique Intel et son pilote libre…

Alors que l'on télécharge le paquet, on apprend que le test est limité à 7 jours, renouvelables à chaque fois que cette limite est atteinte. On prend également connaissance d'une longue liste de fonctionnalité manquantes, comme l'absence de gestion du Firewire et la non gestion de codec populaires en export, ce qu'il faut probablement imputer d'une part à la nature de cette version bêta, mais surtout aux brevets logiciels. Les codecs peuvent-être achetés.

Nous allons donc tester Lightworks ensemble. Nous nous attarderons surtout sur son interface et comment est pensé le métier d'édition vidéo avec un tel logiciel, parce c'est ce que je trouve le plus intéressant à étudier !

# Lancement de Lightworks

Une fois Lightworks téléchargé et installé, et lancé, il demande le nom d'utilisateur et le mot de passe utilisé sur le site et tente de se connecter au serveur :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-001.lightworks-beta-linux.thomas-debesse.crop-720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-001.lightworks-beta-linux.thomas-debesse.png)

En cas de succès, on obtiens un ticket de 7 jours pour tester, ticket qui peut être renouvelé :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-002.lightworks-beta-linux.thomas-debesse.crop-720.1.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-002.lightworks-beta-linux.thomas-debesse.png)

Lightworks peut être configuré pour interpréter les raccourcis clavier d'Avid ou de Final Cut Pro, n'étant habitué ni à l'un ni à l'autre, je vais laisser par défaut :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-002.lightworks-beta-linux.thomas-debesse.crop-720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-002.lightworks-beta-linux.thomas-debesse.png)

# Création d'un projet

Ensuite on crée un projet, est demandé le nom et le _frame rate_ :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-003.lightworks-beta-linux.thomas-debesse.crop-720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-003.lightworks-beta-linux.thomas-debesse.png)
_Attention c'est très important, les vidéos d'un frame rate différent ne pourront pas être importés._

# Import de fichiers

Dès la création d'un projet, l'interface d'import de fichier s'ouvre.

On remarque que les chemins « pertinents » sont bien gérés (raccourcis de l'environnement de bureau, média amovibles…), EditShare semble avoir le souci de respecter les « bonnes pratiques » du bureau sous GNU/Linux :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-004.lightworks-beta-linux.thomas-debesse.crop-720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-004.lightworks-beta-linux.thomas-debesse.png)

Notez que plusieurs manières d'importer les fichier existent, faire un lien, faire une copie locale, ou transcoder :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-005.lightworks-beta-linux.thomas-debesse.crop-720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-005.lightworks-beta-linux.thomas-debesse.png)

Vous remarquerez sur cette copie d'écran que le clip à 30 fps est indiqué en rouge alors que les clips à 25 fps apparaissent normalement, en effet, le projet est en 25 fps et ce clip ne pourra pas être importé :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-005.lightworks-beta-linux.thomas-debesse.crop-720.1.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-005.lightworks-beta-linux.thomas-debesse.png)

# Système de fichier

On remarque que par défaut Lightworks stocke les données dans ``$HOME/Lightworks``. J'ai vu que certains chemins peuvent être changés, je n'ai pas testé.

On remarque également que les liens (lors d'import de fichier) ne sont pas des liens symboliques, certainement pour être agnostique au sujet de la manière de faire des liens, en prévision d'une cohabitation Windows & Mac OS X & GNU/Linux.

Un liens Lightworks est un fichier binaire qui contient, entre autre, le chemin du fichier cible :

```
illwieckz@montage ~ $ strings ~/Lightworks/Media/Material/V901004S.MTS 
/media/illwieckz/rush/police-civils-00.MTS
```

# Espace de travail

Nous importons déjà des fichiers mais nous allons un peu vite… attardons nous un peu sur l'interface, nous sommes avant tout en visite :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-006.lightworks-beta-linux.thomas-debesse.720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-006.lightworks-beta-linux.thomas-debesse.png)

Nous sommes donc devant l'espace de travail de Lightworks, qui s'affiche en plein écran et reproduit donc un environnement de bureau complet : fenêtrage, dock, etc.

Ce n'est pas une surprise, on retrouve ce genre de comportement chez d'autres outils professionnels, comme Blender (mais avec un fenêtrage en tuile pour ce dernier).

On remarque en bas de l'écran une barre d'outil vidéo (lecture, suivant, précédent, remplacer, insérer, supprimer) qui est globale à toute les fenêtres :

[![legende](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-007.lightworks-beta-linux.thomas-debesse.crop-720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-007.lightworks-beta-linux.thomas-debesse.crop-720.png)

Je ne sais pas si c'est très pratique, mais comme je peste déjà contre les menu globaux à la Mac OS trouvant cela anti-ergonomique à l'encontre de tant d'avis contraires… Avoir les boutons si loin n'est pas très pratique.

Cependant, il faut voir que Lightworks s'utilise habituellement avec une [console](http://www.lwks.com/index.php?page=shop.product_details&flypage=yagendoo_flypage_1.tpl&product_id=20&category_id=13&option=com_virtuemart&Itemid=45) ou bien avec des raccourcis clavier, et donc ne pas encombrer chaque fenêtre de ces boutons n'est pas une mauvaise idée…

Dans tous les cas, ces boutons sont toujours au dessus des fenêtres.

# Logique multi-fenêtre

Tandis que nous lisons nos clips, une fenêtre « Vidéo Analysis » peut nous montrer diverses analyse de l'image d'un clip lu dans une autre fenêtre :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-008.lightworks-beta-linux.thomas-debesse.crop-720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-008.lightworks-beta-linux.thomas-debesse.png)

Si la logique est plutôt au multi fenêtre, avec une fenêtre pour l'affichage d'un clip et une autre fenêtre pour le contrôle et la navigation à l'intérieur dudit clip, les fenêtres d'un même clip sont entourées d'un même liseré de couleur :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-009.lightworks-beta-linux.thomas-debesse.crop-720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-009.lightworks-beta-linux.thomas-debesse.png)

# Salles ou bureaux virtuels

Chaque projet possède des salles, « Rooms », qui sont en fait des bureaux virtuels. Leur gestion fait penser à ce que fait [Gnome Shell](http://www.gnome.org/gnome-3/), la création des salles est dynamique :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-010.lightworks-beta-linux.thomas-debesse.crop-720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-010.lightworks-beta-linux.thomas-debesse.png)

Dans le gestionnaire de projet, chaque projet affiche un miniature de la salle en cours, et on peut ouvrir un projet directement dans une salle voulue. Les salles sont donc propres à chaque projet et leur contenu est persistant, de même, la salle en cours pour un projet est une information persistante :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-011.lightworks-beta-linux.thomas-debesse.crop-720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-011.lightworks-beta-linux.thomas-debesse.png)

À noter que les menus sont en faites des fenêtres qui peuvent être « punaisées », cela rappelle certaines ergonomies qu'on voit de plus en plus rarement :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-012.lightworks-beta-linux.thomas-debesse.crop-720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-012.lightworks-beta-linux.thomas-debesse.png)

# Dock, fenêtres et miniatures

Question gestion de l'espace de travail on retrouve plusieurs idées reprises de plusieurs bureaux :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-013.lightworks-beta-linux.thomas-debesse.720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-013.lightworks-beta-linux.thomas-debesse.png)

Les icônes du dock peuvent être déplacées sur le bureau, mais contrairement aux ergonomies de bureaux habituelles, elles sont toujours _au dessus_ des fenêtres. C'est original.

Tout n'est pas très cohérent, certains fenêtres se réduisent en une seul icône, ce sont des fenêtres qui ne peuvent avoir qu'une seule instance, comme l'outil d'import de fichier et la calculette. Mais certains icônes disparaissent lorsque la fenêtre est ouverte (comme la calculette) cela signifie que leur réduction sera une icône. Certains icônes ne disparaissent pas lorsque la fenêtre est ouverte, elles ne peuvent-être que fermées (comme l'outil d'import de fichier). Les fenêtres qui peuvent avoir plusieurs instances (comme le lecteur de clip) se réduisent en une barre de titre repliée. La gestion des fenêtres réduites se fait donc « en bazar », il n'y a pas de barre d'icônes et le dock n'est qu'un lanceur. On devine une certaine logique, mais c'est un peu fouillis à utiliser.

# Bugs

Je ne suis pas allé jusqu'au montage d'un vrai film et son export. La principale cause est la présence d'un bug fâcheux concernent la synchronisation du son et de l'image. Sur quelques capture d'écrans précédentes vous auriez remarqué que parfois la bande son n'était pas présente sur les pistes, et sur la copie d'écran suivante, vous verrez que la bande son est anormalement plus courte que la bande vidéo :

[![legende](http://illwieckz.net/media/reduc/20130501.lightworks-beta-linux.thomas-debesse/20130501-014.lightworks-beta-linux.thomas-debesse.crop-720.png)](http://illwieckz.net/media/image/20130501.lightworks-beta-linux.thomas-debesse/20130501-014.lightworks-beta-linux.thomas-debesse.png)

Nous remarquons d'ailleurs, au passage, que les pistes ne montrent ni les miniatures pour les pistes vidéo, ni une courbe sonore pour les pistes audio. C'est dommage, mais peut-être que ça viendra. En tout cas, les pistes d'un fichiers différents ont des couleurs différentes et c'est pas bête !

Ce test n'a donc pas exploré toutes les facettes du montage avec Lightworks, mais déjà nous avons étudié une interface et une ergonomie originale, dont certains aspects pourront inspirer nombre de projets libre…
