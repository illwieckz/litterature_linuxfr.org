```
URL:     http://linuxfr.org/users/illwieckz/journaux/pyalsacap-python-pointeurs-et-cartes-sons
Title:   PyAlsaCap : Python, pointeurs, et cartes sons…
Authors: Thomas DEBESSE
Date:    2013-03-14T23:58:37+01:00
License: CC by-sa
Tags:    python, journal_du_mois_2013, langage_c et pointeur
Score:   44
```


Pour fêter le retour de [DLFP](http://linuxfr.org) après cette trop longue _vacance_, voici un petit journal pythonesque, mais pas seulement !

Dans mon [dernier journal](http://linuxfr.org/users/illwieckz/journaux/exposer-un-ou-des-modules-python-sur-d-bus-proof-of-concept), nous avions joué avec l'introspection Python et l'export de fonction sur D-Bus. Pour ce faire, nous avions généré du Python avec Python !

Cette fois-ci, je propose de combiner le meilleur de Python avec le meilleur de C ! Nous allons jouer avec des pointeurs… depuis un interpréteur Python !

En plus de tout cela, nous ne sommes plus limité à Python 2. Et nous pouvons sortir avec une peau toute neuve, laissant la vieille mue derrière nous !

Pour cela il faut remercier lolop< qui a partagé une [excellente idée](http://linuxfr.org/users/illwieckz/journaux/exposer-un-ou-des-modules-python-sur-d-bus-proof-of-concept#comment-1434431) : attaquer ``libasound`` directement depuis Python avec le module [ctype](http://docs.python.org/3.3/library/ctypes.html). Si vous ne l'aviez pas encore pertinenté, c'est le moment !

Mais cela n'est pas tout, il faut aussi remercier tankey< de nous avoir fait découvrir [``alsacap``](http://www.volkerschatz.com/noise/alsa.html#alsacap) !

``alsacap`` est un merveilleux petit programme développé par Volker Schatz. Je ne sais pas pourquoi ce programme ne fait pas partie de [alsa-utils](http://alsa.opensrc.org/Alsa-utils), parce que tout en étant un petit bijou, il répond à une problématique légitime et pourtant insatisfaite : décrire le matériel pris en charge par ALSA ! Cela peut paraitre absurde, mais ces informations ne sont accessibles qu'aux développeurs, ALSA ne fournit pas d'interface utilisateur potable pour obtenir ces informations depuis le _shell_.

Si vous n'avez pas pertinenté tankey< pour cette contribution de grande valeur, vous pouvez le faire en suivant le lien suivant :
[lien suivant](http://linuxfr.org/users/illwieckz/journaux/exposer-un-ou-des-modules-python-sur-d-bus-proof-of-concept#comment-1434639). ;)

Ce journal est une grande avancé dans la quête que je poursuis : connaître les noms de mes cartes sons, les échantillonnages pris en charge, le nombre de canaux audio et autres informations très utiles, et ce depuis Python ! Nous allons réécrire ``alsacap`` en Python !

Le serpent est rusé, dit-on… et moi j'ai chuté !

# Python et ctypes

Ctypes est donc une bibliothèque Python qui fournit des _types_ compatibles C et qui permet d'appeler des fonctions appartenant à des bibliothèques C… Avec, on peut donc interfacer C et Python sans compilation !

Pour utiliser une bibliothèque C, rien de plus simple, essayons avec la libc :

```python
In [1]: from ctypes import *

In [2]: libc = cdll.LoadLibrary("libc.so.6")
```

Désormais, nous voilà avec un objet nommé ``libc``, les fonctions sont tout simplement des éléments de cet objet. Malheureusement, l'introspection ne marche pas sur ce type d'objet, il nous faudra donc, pour connaître les fonctions, nous référer à leur documentation officielle ou bien au code source (vive le logiciel libre ) !

Ainsi, pour écrire sur la sortie standard le caractère « a », nous pouvons appeler la fonction ``putchar`` ainsi :

```python
In [3]: libc.putchar(c_char(b'a'))
Out[3]: 97
a
```

Notre caractère « a » est bien affiché !

Nous découvrons une chose, le type ``c_char``. Le module ``ctypes`` fournit une fournée de types de ce type (``c_int``, ``c_bool``…) qui peuvent être convertis vers et depuis les types Python (``int``, ``bool``).

Nous découvrons autre chose : le code de retour est numérique, en effet, c'est un entier.
Nous savons que ``putchar`` renvoie, si succès, la valeur numérique du caractère passé en paramètre. Mais il existe des fonctions qui renvoient d'autres types ! Et bien nous pouvons spécifier le type attendu avec l'attribut ``restype``. Ce n'est pas très utile, mais nous allons le faire pour ``putchar``, nous allons déclarer que le type de retour est ``c_char``.

```python
In [4]: libc.putchar.restype = c_char
In [5]: libc.putchar(c_char(b'a'))
Out[5]: b'a'
a
```

Avant de passer aux choses plus intéressantes, attardons-nous un instant pour adresser une parole pleine de bonnes intentions à nos amis du bouchot :

```python
In [6]: libc.printf(c_char_p(b"salut les moules ! \_o< coincoin"))
Out[6]: 32
salut les moules ! \_o< coincoin
```

Vous aurez remarqué que le nom du type ``c_char_p`` termine par les deux caractères ``_p``, ce qui signifie… que c'est un pointeur. Effectivement, nous savons qu'en C, les chaînes de caractères sont des tableaux de ``char``, et que l'adresse du tableau est l'adresse de son premier caractère ! Avoir bon pointeur c'est s'assurer d'avoir toujours bon caractère…

# Passage par référence

Pour décrire nos cartes, il va falloir appeler des fonctions de ``libasound``. Puisque ``alsacap`` fait tout ce que nous désirons, nous allons l'imiter. Il nous servira de guide spirituel !

Dans [``alsacap.c``](http://www.volkerschatz.com/noise/alsacap.c) nous trouvons très vite une fonction nommée ``scancards``. Dans cette fonction on trouve le code suivant (à peu près) :

```c
int card = -1;
if(snd_card_next(&card) < 0)
	return;
while(card >= 0) {
	/* some code */
 	if (snd_card_next(&card) < 0)
		break;
}
```

Regardons premièrement la forme de ce code, quelque chose nous saute aux yeux. Nous voyons que la fonction ``snd_card_next`` prend pour argument le paramètre ``&card``. Nous devrions plutôt dire que la fonction ``snd_card_next`` prend pour argument la référence de ``card``, c'est à dire son adresse.

En Python avec ``ctypes`` nous pouvons écrire un passage par référence ainsi :

```python
snd_card_next(byref(card))
```

Maintenant, faisons plus attention au sens de ce code, ce qu'il fait. Nous remarquons que la variable ``card`` est initialisée à ``-1``, et qu'ensuite elle passée par référence à la fonction ``snd_card_next`` qui doit donc certainement changer sa valeur par celle de la carte suivante. Nous remarquons aussi que cette fonction retourne un entier négatif en cas d'erreur.

Transposons en python, avec un peu de code supplémentaire pour analyser le fonctionnement. Pour simplifier nous ne nous préoccupons pas des erreurs :

```python
In [1]: from ctypes import *

In [2]: libasound = cdll.LoadLibrary("libasound.so.2")

In [3]: card = c_int(-1)

In [4]: libasound.snd_card_next(byref(card))
Out[4]: 0

In [5]: while card.value >= 0:
   ...:         print(str(card.value))
   ...:         libasound.snd_card_next(byref(card))
   ...:
0
29

In [6]: print(str(card.value))
-1
```

Nous vérifions nos suppositions : la fonction ``snd_card_next`` donne la carte suivante à celle donnée en argument. La valeur -1 signifie que la liste a été parcourue et qu'il faut recommencer à zéro (si la première s'appelle zéro).

Nous pouvons donc écrire la fonction ``GetCards`` suivante :

```python
def GetCards():
	cards = []
	card = c_int(-1)
	if libasound.snd_card_next(byref(card)) < 0:
		return cards
	while card.value >= 0:
		cards.append(card.value)
		if libasound.snd_card_next(byref(card)) < 0:
			break
	return cards
```

```python
In [8]: GetCards()
Out[8]: [0, 29]
```

J'ai effectivement deux cartes sons, ``hw:0`` et ``hw:29``. Première réussite !

# C'était trop facile

Dans ``alsacap``, le code qui nous intéresse ensuite est celui-là :

```c
snd_ctl_card_info(handle, info);
```

Nous lisons un peu plus haut que ces deux variables sont déclarés ainsi :

```c
static snd_ctl_t *handle= NULL;
static snd_ctl_card_info_t *info;
```

Oh, des pointeurs ! Mais quels sont leurs types ?

Nous trouvons dans ``/usr/include/alsa/control.h`` les déclarations suivantes :

```c
/** CTL handle */
typedef struct _snd_ctl snd_ctl_t;
/** CTL card info container */
typedef struct _snd_ctl_card_info snd_ctl_card_info_t;
```

Ce sont des pointeurs de structures, autrement dit des pointeurs de pointeurs !
Aïe aïe aïe… nous ne pourrons pas utiliser ``byref()``, car nous ne connaissons pas ce qui est pointé ! Surtout nous ne savons pas trop comment déclarer ça dans Python…

En fait, nous ne lirons jamais directement ces structures, nous utiliserons des fonctions pour cela. Par exemple, pour obtenir le nom de la carte son, nous ferons :

```c
snd_ctl_card_info_get_name(info))
```

C'est rassurant, nous n'avons pas besoin de connaître exactement les structures, il semble qu'il nous faille seulement savoir passer les pointeurs de fonctions en fonctions.

Nous utiliserons donc le type générique ``c_void_p``. Par souci de clarté, nous préfixerons aussi les noms de variables de type C avec les deux caractères ``c_``.

En C nous avions quelque chose comme :

```c
#define HWCARDTEMPL	"hw:%d"
#define HWDEVLEN	32
card = 0
snprintf(hwdev, HWDEVLEN, HWCARDTEMPL, card);
snd_ctl_open(&handle, hwdev, 0)
snd_ctl_card_info(handle, info)
```

Nous allons essayer en Python :

```python
In [1]: from ctypes import *
In [2]: libasound = cdll.LoadLibrary("libasound.so.2")
In [3]: card = 0
In [4]: hwdev = "hw:" + str(card)
In [5]: b_hwdev = create_string_buffer(str.encode(hwdev))
In [6]: c_handle = c_void_p()
In [7]: c_info = c_void_p()
In [8]: libasound.snd_ctl_open(byref(c_handle), b_hwdev, 0)
Out[8]: 0
```

Bien, ça a l'air de passer ! La valeur retournée aurait été non nulle s'il y avait eu une erreur…

On peut remarquer la conversion de chaîne de caractère entre [les types ``str`` et ``bytes``](http://docs.python.org/3.1/library/stdtypes.html#sequence-types-str-bytes-bytearray-list-tuple-range), ce sera souvent utile. Aussi, lorsque nous passons une chaîne de type ``byte``, il n'est pas nécessaire de la passer via ``byref()``. ``ctypes`` fait cela tout seul.

Continuons…

```python
In [9]: libasound.snd_ctl_card_info(c_handle, c_info)
python3: control.c:234: snd_ctl_card_info: Assertion `ctl && info' failed.
Abandon
```

En fait non, nous ne continuons pas ! :)

# Et là c'est le drame

Nos manipulations de pointeurs semble correctes :

```python
In [6]: handle = c_void_p()
In [7]: info = c_void_p()
In [8]: handle.value

In [9]: libasound.snd_ctl_open(byref(c_handle), b_hwdev, 0)
Out[9]: 0
In [10]: c_handle.value
Out[10]: 51737712
```

Le pointeur ``c_handle`` passe bien de la valeur ``NULL`` à une adresse mémoire !

Qu'est ce qui ne va pas ? Nous avons oublié une chose essentielle…

Déjà, nous aurions pu remarquer que nous passions en paramètre un pointeur non initialisé, ``c_info``. Nous cherchons donc où il est initialisé… 

Nous trouvons ce code :

```c
snd_ctl_card_info_alloca(&info);
```

Nous avons donc trouvé ce qui semble être une fonction qui initialise l'adresse vers laquelle le pointeur pointe… Et nous découvrons que ce que cela semble être une fonction d'allocation. En effet, on peut se douter qu'à l'adresse pointée par ``info``, il n'y aura pas qu'un pointeur, mais des tas de données, chaînes de caractères, listes, etc.

Aïe aïe aïe, pour le pointeur nommé ``handle``, [comme son nom l'indique](http://en.wikipedia.org/wiki/Handle_%28computing%29), nous n'avions pas besoin de nous soucier de son allocation, c'est un pointeur qui contient une référence vers un objet que nous ne gérons pas nous-même. Pour le pointeur nommé ``info``, c'est tout autre chose !

Et si nous essayons, depuis python, d'appeler la fonction ``snd_ctl_card_info_alloca`` ?

Je l'ai tenté pour vous :

```python
In [11]: libasound.snd_ctl_card_info_alloca(byref(c_info))
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-15-13b9fd869a33> in <module>()
----> 1 libasound.snd_ctl_card_info_alloca(byref(c_info))
[…]
AttributeError: /usr/lib/x86_64-linux-gnu/libasound.so.2: undefined symbol: snd_ctl_card_info_alloca
```

C'est un cuisant échec.

# Je veux mes allocs !

Mais que se passe-t'il ?

Ce ne serait pas une fonction ? mais qu'est-ce donc ?

Dans ``/usr/include/alsa/control.h`` je trouve :

```c
#define snd_ctl_card_info_alloca(ptr) __snd_alloca(ptr, snd_ctl_card_info)
```

Dans ``/usr/include/alsa/global.h`` je trouve :

```c
#define __snd_alloca(ptr,type) do { *ptr = (type##_t *) alloca(type##_sizeof()); memset(*ptr, 0, type##_sizeof()); } while (0)
```

Aïe, ce sont des macros ! Je ne pourrais donc pas appeler ce code depuis Python !
Mais rusons, ce qu'il nous faut savoir, c'est la taille de l'espace mémoire à allouer…

Nous pourrions chercher la définition exacte de la structure, et la déclarer en Python en héritant de l'objet [Structure](http://docs.python.org/3.3/library/ctypes.html#structures-and-unions), mais cela serait un trop grand effort sans intérêt. En effet nous ne parcourrons jamais manuellement la structure, nous utiliserons des _getters_ pour cela ! Tout ce qui nous importe, c'est donc la taille de la structure à allouer !

Nous n'allons pas jouer au préprocesseur et interpréter nous même ce code. Nous allons lire le code généré par le préprocesseur lorsqu'il rencontre cette macro d'allocation.

Juste avant la ligne qui nous intéresse, j'ajoute un témoin dans le code source d'``alsacap`` (que je renomme au passage en ``alsacap-trace.c``) :

```c
snd_ctl_card_info_alloca(&info);
printf("THDTHDTHD");
```

Hop, préprocessons tout cela, et regardons :

```
gcc -E alsacap-trace.c -lasound | grep -B1 "THDTHDTHD"
  do { *&info = (snd_ctl_card_info_t *) __builtin_alloca (snd_ctl_card_info_sizeof()); memset(*&info, 0, snd_ctl_card_info_sizeof()); } while (0);
  printf("THDTHDTHD");
```

Nous trouvons la fonction (c'est est bien une cette fois-ci) ``snd_ctl_card_info_sizeof()`` qui retourne la taille mémoire qui doit être allouée ! Essayons…

```python
In [12]: libasound.snd_ctl_card_info_sizeof()
Out[12]: 376
```

Mais comment allouons nous avec Python et ctypes ?
Pour allouer un tableau (au sens de C) d'entiers, la [documentation nous indique](http://docs.python.org/3.3/library/ctypes.html#arrays) que nous pouvons faire ainsi :

```python
TenIntegers = c_int * 10
```

Pour ne pas nous embêter avec le type (nous ne le connaissons pas vraiment, et peut-il être retranscrit ?), nous allons _« multiplier »_ le type ``c_void_p``. Ainsi nous allouerons un espace mémoire dont l'adresse de départ sera de type « pointeur ».

Juste pour tester, tentons de passer un pointeur vers un espace mémoire de taille arbitraire mais conséquente :

```python
In [13]: c_info = (c_void_p * 50)()
In [14]: libasound.snd_ctl_card_info(handle, c_info)
Out[14]: 0
```

Notre code ne plante pas, et la fonction retourne le code _« no problemo »_. Mais attention au _« je reviendrai »_, dès qu'on va tenter d'utiliser un _getter_ sur ``c_info``, on va prendre le risque d'un ``segfault``…

Nous avons une fonction qui retourne une valeur, mais nous ne pouvons pas _« multiplier »_ cette valeur avec le type, puisque ce type a déjà une taille !

```python
In [15]: sizeof(c_void_p)
Out[15]: 8
```

Il nous faut donc _« multiplier »_ le type avec la valeur retournée par ladite fonction, divisée par la taille du type. 

```python
In [16]: int(libasound.snd_ctl_card_info_sizeof() / sizeof(c_void_p))
Out[16]: 47
In [17]: c_info = (c_void_p * int(libasound.snd_ctl_card_info_sizeof() / sizeof(c_void_p)))()
```

Nous utilisons ``int()`` non parce que la division aurait un reste, mais parce qu'une division retourne un nombre de type ``float``, type qui n'a pas de sens pour une taille mémoire.

Nous avons donc un espace alloué, et il est de bonne taille !
Nous pouvons le vérifier :

```python
In [18]: sizeof(c_info)
Out[18]: 376
```

Nous allons donc l'utiliser !

# Récupérer les valeurs

Remplir la structure ``c_info`` de ses valeurs, pour la carte sélectionnée :

```python
In [19]: libasound.snd_ctl_card_info(c_handle, c_info)
Out[19]: 0
```

Appeler un _getter_ :

```python
In [20]: libasound.snd_ctl_card_info_get_id(c_info)
Out[20]: 52202712
```

Nous avons une réponse ! Mais c'est une drôle de réponse !

Mais oui c'est une chaîne de caractère dont nous avons obtenu l'adresse ! Il nous faut définir le type de retour de la fonction et ce sera bon…

```python
In [21]: libasound.snd_ctl_card_info_get_id.restype = c_char_p
In [22]: libasound.snd_ctl_card_info_get_id(c_info)
Out[22]: b'Intel'
```

Et voilà ! Désormais il n'y a plus aucun mystère, aucune surprise, ni dans Python, ni dans ctypes, ni dans libasound… Nous pouvons retranscrire tout ``alsacap`` avec ce que nous avons appris !

# Headers et malloc en Python ?

C'est le petit souci de ``ctypes``, il nous faut reproduire (en partie) une spécificité du langage compilé qu'est le C, les _headers_. Aussi, nous sommes liés aux contraintes d'un langage aujourd'hui considéré comme bas-niveau qu'est le C, comme les allocations mémoires…

Par exemple j'ai du écrire ces déclarations :

```python
libasound.snd_ctl_card_info_get_id.restype = c_char_p
libasound.snd_ctl_card_info_get_name.restype = c_char_p
libasound.snd_pcm_info_get_id.restype = c_char_p
libasound.snd_pcm_info_get_name.restype = c_char_p
libasound.snd_pcm_info_get_subdevice_name.restype = c_char_p
libasound.snd_pcm_format_name.restype = c_char_p
libasound.snd_pcm_format_mask_test.restype = c_bool
```

Ces allocations :

```
c_info = (c_void_p * int(self.libasound.snd_ctl_card_info_sizeof() / sizeof(c_void_p)))()
c_pcminfo = (c_void_p * int(self.libasound.snd_pcm_info_sizeof() / sizeof(c_void_p)))()
c_pars = (c_void_p * int(self.libasound.snd_pcm_hw_params_sizeof() / sizeof(c_void_p)))()
c_fmask = (c_void_p * int(self.libasound.snd_pcm_format_mask_sizeof() / sizeof(c_void_p)))()
```

Et puis j'ai reproduit un certain nombre de constantes de ``libasound`` :

En C nous avons :

```c
/usr/include/alsa/pcm.h
** PCM sample format */
typedef enum _snd_pcm_format {
	/** Unknown */
	SND_PCM_FORMAT_UNKNOWN = -1,
	/** Signed 8 bit */
	SND_PCM_FORMAT_S8 = 0,
	/** Unsigned 8 bit */
	SND_PCM_FORMAT_U8,
	/** Signed 16 bit Little Endian */
	SND_PCM_FORMAT_S16_LE,
	/** Signed 16 bit Big Endian */
	SND_PCM_FORMAT_S16_BE,
[…]
	SND_PCM_FORMAT_U18_3BE,
	SND_PCM_FORMAT_LAST = SND_PCM_FORMAT_U18_3BE,
```

J'ai donc écrit en Python :

```python
SND_PCM_FORMAT_S8 = c_int(0)
SND_PCM_FORMAT_U8 = c_int(1)
SND_PCM_FORMAT_S16_LE = c_int(2)
SND_PCM_FORMAT_S16_BE = c_int(3)
# […]
SND_PCM_FORMAT_U18_3BE = c_int(43)
SND_PCM_FORMAT_LAST = SND_PCM_FORMAT_U18_3BE
```

Mais rassurez-vous, je n'ai pas écrit tout cela à la main, ``sed`` m'a rendu un bien grand service !

# PyAlsaCap

Et voilà, je n'ai plus eu qu'à suivre méthodiquement le code source d'``alsacap`` et à dérouler avec lui la grande boucle qui interroge les _cards_, puis les _devices_, puis les _subdevices_ etc.

Cependant, contrairement à ``alsacap``, je n'imprime pas sur la sortie standard ce que je trouve, je remplis une grande structure utilisable depuis Python ! Pour cela j'ai donc écrit la fonction ``GetCards``. Regardons ce qu'elle nous retourn :

```python
In [1]: from pyalsacap import *
In [2]: insp = Inspector()
In [3]: cards = insp.GetCards()
In [4]: cards
Out[4]:
{
	0: {
		'id': 'Intel',
		'name': 'HDA Intel',
		'devices': {
			0: {
				'channels': [2, 2],
				'formats': ['S16_LE'],
				'id': 'AD198x Analog',
				'name': 'AD198x Analog',
				'rate': [8000, 192000],
				'subdevices_available': 1,
				'subdevices': {
					0: {
						'name': 'subdevice #0'
					}
				}
			},
			1: {
				'channels': [2, 2],
				'formats': ['S16_LE'],
				'id': 'AD198x Digital',
				'name': 'AD198x Digital',
				'rate': [44100, 192000],
				'subdevices_available': 1,
				'subdevices': {
					0: {
						'name': 'subdevice #0'
					}
				}
			}
		}
	},
	29: {
		'id': 'ThinkPadEC',
		'name': 'ThinkPad Console Audio Control',
		'devices': {
		}
	}
}
```

C'est beau !

J'ai ajouté une autre fonction nommée ``PrintCards`` qui reparcourt la structure et l'imprime sur la sortie standard. Regardons ce qu'elle nous écrit !

```python
In [5]: desc = Descriptor()

In [6]: desc.PrintCards(cards)
*** Scanning for playback devices ***
Card 0, ID `Intel', name `HDA Intel'
  Device 0, ID `AD198x Analog', name `AD198x Analog', 1 subdevices (1 available
    2 channels, sampling rate 8000..192000 Hz
    Sample formats: S16_LE
      Subdevice 0, name `subdevice #0'
  Device 1, ID `AD198x Digital', name `AD198x Digital', 1 subdevices (1 available)
    2 channels, sampling rate 44100..192000 Hz
    Sample formats: S16_LE
      Subdevice 0, name `subdevice #0'
Card 29, ID `ThinkPadEC', name `ThinkPad Console Audio Control'
```

Cela nous convient très bien !

Certains demanderont _« mais pourquoi deux classes ? »_. En fait j'ai fait deux classes parce que je considère que la partie qui imprime n'est utile que lorsque PyAlsaCap est utilisé comme un script (en remplacement d'``alsacap``). Lorsque nous utilisons PyAlsaCap comme un module (ce pourquoi je l'ai écrit), nous n'avons pas besoin d'importer tout ce qui a trait à l'impression sur écran !

Pour le moment, toutes les fonctionnalité d'``alsacap`` ne sont pas reproduites, nous ne pouvons pas interroger une carte précise, nous pouvons seulement lister toutes les cartes selon une fonction précise (lecture ou enregistrement). Le paramètre ``-R`` est donc pour le moment le seul paramètre que prend le script. Pour la fonction ``GetCards``, l'argument équivalent est ``"capture"``.

En Python :

```python
In [1]: from pyalsacap import *
In [2]: insp = Inspector()
In [3]: desc = Descriptor()
In [4]: desc.PrintCards(insp.GetCards("capture")
*** Scanning for recording devices ***
Card 0, ID `Intel', name `HDA Intel'
  Device 0, ID `AD198x Analog', name `AD198x Analog', 2 subdevices (2 available
    2 channels, sampling rate 8000..192000 Hz
    Sample formats: S16_LE
      Subdevice 0, name `subdevice #0'
      Subdevice 1, name `subdevice #0'
Card 29, ID `ThinkPadEC', name `ThinkPad Console Audio Control'
```

Depuis le _shell_ :

```
$ ./pyalsacap.py -R
*** Scanning for recording devices ***
Card 0, ID `Intel', name `HDA Intel'
  Device 0, ID `AD198x Analog', name `AD198x Analog', 2 subdevices (2 available)
    2 channels, sampling rate 8000..192000 Hz
    Sample formats: S16_LE
      Subdevice 0, name `subdevice #0'
      Subdevice 1, name `subdevice #0'
Card 29, ID `ThinkPadEC', name `ThinkPad Console Audio Control'
```

À comparer avec la sortie d'``alsacap`` :

```
$ ./alsacap -R
*** Scanning for recording devices ***
Card 0, ID `Intel', name `HDA Intel'
  Device 0, ID `AD198x Analog', name `AD198x Analog', 2 subdevices (2 available)
    2 channels, sampling rate 8000..192000 Hz
    Sample formats: S16_LE, S32_LE
      Subdevice 0, name `subdevice #0'
      Subdevice 1, name `subdevice #1'
Card 29, ID `ThinkPadEC', name `ThinkPad Console Audio Control'
```

# Le code

Si cela vous intéresse, vous pouvez lire [le source de PyAlsaCap](http://dl.illwieckz.net/u/thomas-debesse/bazar/pyalsacap/pyalsacap.py.txt) à la date du 10 mars.

C'est ce que j'ai écrit pour occuper mon dimanche après-midi. Il est probable que le code évolue et que j'y ajoute quelques fonctions supplémentaires (et peut-être pourra-t-il complètement remplacer ``alsacap``).

Il faudra aussi que j'apprenne à utiliser le module [argparse](http://docs.python.org/3.3/library/argparse.html) afin de prendre en compte correctement les paramètres de la ligne de commande.

Mais déjà, si PyAlsaCap ne fait pas tout ce que fait ``alsacap``, il fait quelque chose qu'``alsacap`` ne permet pas : être utilisé comme module Python 3 ! Et c'est là son grand avantage : le code de PyAlsaCap n'utilise rien de spécifique à Python 3 par rapport à Python 2, mais il est utilisable dans un projet plus vaste en Python 3, et ce sans compilation.

* PyAlsaCap est compatible Python 3
* PyAlsaCap n'a pas d'autres dépendances que Python et ``libasound2``.
* Contrairement à d'autres modules (``python-pyalsa``, ``python-alsaaudio``), PyAlsaCap est un module en python qui n'a pas besoin d'être compilé.
* PyAlsaCap est donc utilisable dans toute distribution GNU/Linux qui fournit Python et ``libasound``, dès aujourd'hui.
* PyAlsaCap ne fait pas beaucoup de choses, mais c'est peut-être aussi un avantage ! PyAlsaCap liste une sélection de caractéristiques et capacités des cartes audio, et il le fait bien. :)

# Conclusion

Python avec ctypes, c'est extrêmement puissant !

Bon, parfois, chercher par quel bout prendre la bibliothèque C pour obtenir ce que l'on souhaite est un peu prise de tête, surtout lorsque le code fait usage de macros !

Parfois la recherche de solution prend des airs de rétro-ingénierie, mais sur du code ouvert, c'est vraiment trop facile. :)

Python avec ctypes, c'est grisant…

Ça m'a rappelé les sensations que j'avais quand je codait en C étant enfant, mais cette fois-ci sans compilation ! Les pointeurs, c'est fun ! Les langages interprétés, c'est fun aussi ! On peut combiner les deux… :o

D'ailleurs je crois que c'est une bonne manière de décrire ctypes : on n'échappe pas au C, il faut y avoir été initié et maîtriser les concepts de bases, mais par contre on évite toute la compilation !

Surtout, surtout, ce qui est super sympa, c'est de pouvoir jouer avec des pointeurs avec un langage interprété, depuis un _shell_ dudit langage… De quoi nous occuper les longues soirées d'hiver, même si l'hiver ne dure pas très longtemps sur la côte d'Azur ! :p
