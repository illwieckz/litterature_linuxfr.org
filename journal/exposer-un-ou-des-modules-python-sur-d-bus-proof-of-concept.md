```
URL:     http://linuxfr.org/users/illwieckz/journaux/exposer-un-ou-des-modules-python-sur-d-bus-proof-of-concept
Title:   Exposer un ou des modules Python sur D-Bus [proof of concept]
Authors: Thomas DEBESSE
Date:    2013-03-02T23:59:47+01:00
License: CC by-sa
Tags:    dbus, journal_du_mois_2013, python3, python2 et python
Score:   41
```


Réfléchissant au moyen d'utiliser un module [python2 dans mon projet python3](http://linuxfr.org/forums/programmation-python/posts/python-3-et-alsa) (le module [pyalsa](http://git.alsa-project.org/?p=alsa-python.git) en fait), m'est venue l'idée saugrenue de lister toutes les méthodes et des les exposer avec D-Bus, afin d'y avoir accès depuis Python 3, ou n'importe quoi qui cause avec D-Bus. Ce n'est certainement pas la solution que je vais retenir, mais j'ai trouvée l'idée amusante et ça m'a donné envie de jouer ce week-end.

L'idée, c'est donc de parcourir avec python tous les modules qui nous intéressent, et de générer du code python à partir de leur description, code python qui, exécuté, jouera le rôle d'intermédiaire entre les fonctions exposées sur D-Bus et les fonctions des modules.

Nous nommons le générateur de code ``alsadbusgen.py``, et le code généré ``alsadbus.py``.

# Introspection Python

Python permet d'interroger des modules, des classes, des fonctions… enfin tout type d'objet avec le module ``inspect``

Comme son nom le laisse deviner, la fonction ``inspect.getmembers`` reçoit un objet en paramètre et retourne une liste d'objets fils.
En fait, chaque objet fils est associé à un nom, ainsi on peut écrire :

```python
for name, obj in inspect.getmembers(something):
```

Pour parcourir la liste des objets contenus dans un module, associé à son nom.
Ensuite, on peut tester le type d'objet avec ``inspect.ismodule``, ``inspect.isfunction``, ``inspect.isbuiltin``, ``inspect.isclass``, ``inspect.ismethod``, etc.

# Parcourir les modules

Ainsi, pour faire une action arbitraire pour tous les modules parcourus :

```python
for name, obj in inspect.getmembers(something):
	if inspect.ismodule(obj):
		pass
```

Si ``something`` est ``pyalsa``_ et que j'ai préalablement importé ``pyalsa.alsacard``, ``pyalsa.alsacontrol``, ``pyalsa.alsahcontrol``, ``pyalsa.alsamixer``, ``pyalsa.alsaseq``, mon code va donc parcourir tous ces modules.

Pour chacun, nous allons donc générer un code python qui monte un service D-Bus avec comme ``service name`` « org.alsa.Service », comme ``object_path`` « /org/alsa/<nom du module> », et comme ``interface`` « org.alsa.Controller ». Comme on bidouille, on va faire ça avec des ``print``, et on se souciera peu de la beauté du code généré (on mélangera imports, code et définitions, tant pis, c'est une démo pour voir si ça marche).

Chose pratique, chaque objet semble avoir un objet ``__name__`` qui contient son nom, ainsi, si l'on reçoit un module en paramètre, on peut tout de même savoir son nom.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from __future__ import print_function

import pyalsa.alsacard
import pyalsa.alsacontrol
import pyalsa.alsahcontrol
import pyalsa.alsamixer
import pyalsa.alsaseq

import inspect
import re

SERVICE_NAME_BASE = "org.alsa"
OBJECT_PATH_BASE = "/org/alsa/"
SERVICE_NAME = SERVICE_NAME_BASE + '.Service'
CONTROLLER_NAME = SERVICE_NAME_BASE + '.Controller'

print("#!/usr/bin/env python")
print("# -*- coding: utf-8 -*-")
print('')
print("from gi.repository import GObject")
print("import dbus")
print("import dbus.service")
print("from dbus.mainloop.glib import DBusGMainLoop")
print('')

def FindModulesIn(something):
	print("import " + something.__name__)
	print('')
	for name, obj in inspect.getmembers(something):
		if inspect.ismodule(obj):
			print("import " + obj.__name__)
			print('')
			print("class Service_" + name + "(dbus.service.Object):")
			print("\tdef __init__(self):")
			print("\t\tself.service_name = \"" + SERVICE_NAME + "\"")
			print("\t\tself.object_path = \"" + OBJECT_PATH_BASE + name + "\"")
			print("\t\tself.main_loop = DBusGMainLoop()")
			print("\t\tself.bus=dbus.SessionBus(mainloop = self.main_loop)")
			print("\t\tself.bus_name = dbus.service.BusName(self.service_name, self.bus)")
			print("\t\tdbus.service.Object.__init__(self, self.bus_name, self.object_path)")
			# inspect.getmembers(obj) here
			print('')

FindModulesIn(pyalsa)

print("mainloop = GObject.MainLoop()")
print("mainloop.run()")
```

Nous enregistrons ça dans ``alsadbusgen.py``, puis nous faisons :

```sh
python alsadbusgen.py > alsadbus.py
```

Nous obtenons un code qui semble interprétable par Python, c'est bon ça !

# Parcourir les fonctions

En fait nous n'utiliserons pas ``inspect.isfunction``, puisque nous inspectons un module écrit en C (pyalsa), il faut donc tester avec ``inspect.isbuiltin``.

```python
for name, obj in inspect.getmembers(module):
	if inspect.isbuiltin(obj):
		pass
```

Mais il manque une chose importante, il faut définir les fonctions (``def …``) avec les bons paramètres. Normalement, ``inspect.getargspec`` est fait pour décrire les arguments des fonctions, mais ça ne marche pas avec les fonction du module pyalsa parce que ce module n'est pas écrit en Python !

```python
In [16]: def mafonction(a, b):
   ....:     pass
   ....: 

In [17]: inspect.getargspec(mafonction)
Out[17]: ArgSpec(args=['a', 'b'], varargs=None, keywords=None, defaults=None)

In [18]: inspect.getargspec(pyalsa.alsacard.card_list)
TypeError: <built-in function card_list> is not a Python function
```

Nous allons donc utiliser l'objet ``__doc__`` que nous allons découper à coup de ``regexp`` !

```python
In [19]: pyalsa.alsacard.card_list.__doc__
Out[19]: 'card_list() -- Get a card number list in tuple.'

In [20]: pyalsa.alsacard.card_get_name.__doc__
Out[20]: 'card_get_name(card) -- Get card name string using card index.'
```

Je vous épargne les ``regexp``, surtout que j'ai écris les miennes à la va-vite pour voir si ça marche, je n'ai pas du tout cherché l'élégance, et que de toute manière, les ``regexp``, c'est du code en écriture seule !

Aussi, certaines méthodes ont des paramètres optionnels, nous n'allons pas nous en préoccuper, nous allons tous les indiquer comme s'ils étaient obligatoires, à nous de savoir quelle est la valeur par défaut à passer !

Mais donc voilà, nous pouvons donc générer un code qui ressemble à :

```python
@dbus.service.method("org.alsa.Controller")
def card_get_name(self, card):
	return str(pyalsa.alsacard.card_get_name(card))

@dbus.service.method("org.alsa.Controller")
def card_list(self):
	return Py2Dbus(pyalsa.alsacard.card_list())
```

# Parcourir les classes et les méthodes.

Plus compliqué cette fois-ci, nous exposerons sur D-Bus des fonctions qui correspondent à des méthodes de classe.
En fait pour chaque module nous parcourons les classes :

```python
for name, obj in inspect.getmembers(module):
	if inspect.isclass(obj):
		pass
```

Et pour chaque classe, nous parcourons les méthodes :

```python
for name, obj in inspect.getmembers(a_class):
	if inspect.ismethoddescriptor(obj):
		pass
```

Nous utilisons la fonction ``inspect.ismethoddescriptor``, je n'ai pas cherché à vérifier, mais je suppose que ``ismethoddescriptor`` est à ``ismethod`` ce que ``isbuiltin`` est à ``isfunction`` : notre module est un module en C, et non en python.

Là nous ça se corse un peu. Nous allons déclarer une fonction dont le nom est composé du nom de la classe suivi du nom de la méthode, qui prendra en paramètre à la fois les paramètres du « constructeur », et à la fois les paramètres de la méthode. Pour éviter les conflits de nom (si le constructeur prend un paramètre qui a le même nom que le paramètre d'une de ses méthodes), nous allons précéder le nom de chaque paramètre de la classe par son nom de classe.

Ensuite, après une telle définition, nous instancions un objet depuis ladite classe avec les paramètres de classe, et nous appliquons la méthode sur ledit objet avec les paramètres de la méthode.

Ainsi, nous générons un code qui, par exemple, ressemble à cela :

```python
@dbus.service.method("org.alsa.Controller")
def Control_card_info(self,  Control_name, Control_ode):
	obj = pyalsa.alsacontrol.Control(Control_name, Control_ode)
	return Py2Dbus(obj.card_info())

@dbus.service.method("org.alsa.Controller")
def Element_get_volume(self,  Element_mixer, Element_name, Element_index, channel, capture):
	obj = pyalsa.alsamixer.Element(Element_mixer, Element_name, Element_index)
	return Py2Dbus(obj.get_volume(channel, capture))

@dbus.service.method("org.alsa.Controller")
def Sequencer_create_queue(self,  Sequencer_name, Sequencer_clientname, Sequencer_streams, Sequencer_mode, Sequencer_maxreceiveevents, name):
	obj = pyalsa.alsaseq.Sequencer(Sequencer_name, Sequencer_clientname, Sequencer_streams, Sequencer_mode, Sequencer_maxreceiveevents)
	return Py2Dbus(obj.create_queue(name))
```

Noter que sur le dernier exemple, il y avait à l'origine deux paramètres nommés ``name``.

# Retourner les valeurs retour

Les fonctions et méthodes retournent parfois des _types_ qui ne sont pas compris par D-Bus, libre à nous d'écrire une fonction ``Py2Dbus`` qui fasse une conversion.

Pour faire simple (c'est juste une démo), nous transformons tout retour en chaîne de caractère.

```python
def Py2Dbus(var):
	return str(var)
```

# Générer le code, l'exécuter.

Nous avons donc quelque chose de fonctionnel !

Générons le code :

```sh
python alsadbusgen.py > alsadbus.py 
```

Exécutons le :

```sh
python alsadbus.py 
```

Maintenant nous allons parcourir les services D-Bus avec l'outil graphique [d-feet](https://live.gnome.org/DFeet/).

![d-feet](http://dl.illwieckz.net/u/thomas-debesse/bazar/alsadbusgen/d-feet.alsadbus.png)

Nous trouvons très vite un service nommé ``org.alsa.Service`` qui contient cinq objets :

* ``/org/alsa/alsacard``
* ``/org/alsa/alsacontrol``
* ``/org/alsa/alsahcontrol``
* ``/org/alsa/alsamixer``
* ``/org/alsa/alsaseq``

Chacun de ces objets possède bien une interface nommée ``org.alsa.Controllers``, et ces interfaces contiennent bien des méthodes au nom des fonctions du module ``pyalsa``, ou bien dont le nom est bien un assemblage de classe et de méthode.

# Tester

J'identifie dans l'objet ``/org/alsa/alsacard``, l'interface ``org.alsa.Controller``, la méthode ``card_list`` sans paramètres que j'appelle sans paramètre. Je reçois « ``(0, 29)`` ».

J'identifie dans l'objet ``/org/alsa/alsacontrol``, l'interface ``org.alsa.Controller``, la méthode ``Control_card_info`` avec les paramètres ``Control_name`` et ``Control_ode``. J'appelle donc cette méthode avec les paramètres « ``"hw:0", 1`` », je reçois « ``{'name': 'HDA-Intel', 'longname': 'HDA Intel at 0xf8220000 irq 46', 'driver': 'HDA-Intel', 'components': 'HDA:11d41984,17aa20dd,00100400', 'mixername': 'Analog Devices AD1984', 'id': 'Intel', 'card': 0}`` ».

J'appelle la même méthode mais avec comme paramètres « ``"hw:29", 1`` », je reçois « ``{'name': 'ThinkPad EC', 'longname': 'ThinkPad Console Audio Control at EC reg 0x30, fw 7RHT16WW-1.02', 'driver': 'ThinkPad EC', 'components': '', 'mixername': 'ThinkPad EC 7RHT16WW-1.02', 'id': 'ThinkPadEC', 'card': 29}`` ».

Et oui, je suis bien l'heureux possesseur d'un [Thinkpad X61 Tablet](http://www.thinkwiki.org/wiki/Category:X61_Tablet) !

# Et ensuite…

Tout cela est donc fonctionnel. Vous pouvez trouver mon ``alsadbusgen.py`` [ici](http://dl.illwieckz.net/u/thomas-debesse/bazar/alsadbusgen/alsadbusgen.py.txt) et le fichier ``alsadbus.py`` [là](http://dl.illwieckz.net/u/thomas-debesse/bazar/alsadbusgen/alsadbus.py.txt). Évidemment, à ne pas utiliser en production, et ne pas être trop regardant, c'est du code écrit pour le plaisir de la bidouille, pas pour l'élégance.

Pour aller plus loin, il serait peut-être même possible d'exposer les méthodes via D-Bus sans passer par la génération de code et son interprétation, que le même code qui fait l'introspection se charge d'exposer les méthodes. Mais pour cela, il faudrait le faire sans utiliser les _décorateurs_, ce qui complique la tâche.

Pour mon projet, je ne sais toujours pas ce que je vais faire… Mais en attendant, si ce journal peut donner des idées aux petits gars du projet Alsa ! Les gars de Jackd ont déjà fourni une API D-bus, donc je peux contrôler jack directement via D-Bus, j'aimerai tant pouvoir faire la même chose avec Alsa !

Et tout ça c'est rigolo, mais ça ne répond pas à mon premier problème, parler à Alsa depuis Python 3 !
En plus de la méthode exposée ici (que je me refuse à utiliser en prod, c'est juste un jeu), je pourrais essayer une [autre méthode décrite dans un article GLMF](http://www.inspyration.org/articles-glmf/utiliser-le-meilleur-de-python-2.x-et-de-python-3.x-au-sein-d2019une-seule-et-meme-application), où deux threads, l'un en Python 2 l'autre en Python 3 partagent leurs objets avec [PyRO](https://pypi.python.org/pypi/Pyro4). Mais l'idée de lancer deux machines virtuelles Python me chiffonne un peu ! Si vous avez des idées pour discuter avec Alsa autrement qu'en grepant la sortie d'``aplay``, je suis preneur ! ;)
