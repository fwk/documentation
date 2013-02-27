Concepts Généraux
#################

Vue d'ensemble
==============

Une Application Fwk/Core est faite de plusieurs composants:

* Les Listeners: sont des `Observers <http://fr.wikipedia.org/wiki/Observateur_%28patron_de_conception%29>`_ qui traitent les évènements émis par l'Application. 
* Les Services: sont les objets qui exécutent le gros du travail. Généralement, il s'agit de:
    * La connexion a la base de donnée (`DAO <http://fr.wikipedia.org/wiki/Objet_d%27acc%C3%A8s_aux_donn%C3%A9es>`_).
    * Envoi d'emails.
    * Traitements divers.
    * ...
* Les Actions: sont les *Controllers* de notre Application. Ils se servent des Services et des paramètres de la Requête pour traiter cette dernière afin d'en déduire la Réponse adéquate.

.. tip:: Fwk/Core encourage la séparation des couches et l'`Inversion de Contrôle (IoC) <http://fr.wikipedia.org/wiki/Inversion_de_contr%C3%B4le>`_ afin de pouvoir aisément réutiliser les classes de l'Application. Par exemple, si l'on créé une Action permettant de faire du `CRUD <http://fr.wikipedia.org/wiki/CRUD>`_, il est recommandé de créer une classe qui se charge uniquement de la partie base de données (DAO), lui même étant exploité par notre classe Action. De ce fait, si une API doit être crée plus tard pour l'Application, il sera possible d'exploiter directement notre DAO en ne codant que la partie API. Même cas de figure si l'on souhaite faire évoluer la base de données: il suffit de recoder juste le DAO, sans toucher aux Actions.

fwk.xml
=======

Le Descriptor (``Fwk\Core\Descriptor``) permet de "décrire" notre application. Il contient au moins deux définitions:

* Les Listeners
* Les Actions

Le format XML permet cependant de pouvoir alimenter ce fichier comme le développeur le souhaite, afin d'enrichir simplement son Application. D'autres Listeners permettent d'y décrire: la réécriture d'URLs, les options de configuration, les types de résultats ... 

La philosophie de Fwk/Core veut que ce fichier permette d'avoir une vue d'ensemble de l'Application et de tous ses composants. 

.. warning:: Le fichier *fwk.xml* doit se trouver à la racine du package de l'Application. C'est le seul pré-requis nécessaire au bon fonctionnement d'une Application Fwk/Core.

Exemples de fichier fwk.xml
---------------------------

- Exemple simple (Hello World): https://github.com/neiluJ/fwk-helloworld/blob/master/HelloWorld/fwk.xml
- Exemple complet (Todo App): https://github.com/neiluJ/todo-app/blob/master/TodoApp/fwk.xml


Architecture Générale
=====================

Fwk/Core est composé de trois objets principaux:

* ``Fwk\Core\Application``: C'est l'object qui représente notre Application. C'est depuis cet objet que les évènements principaux sont émis (voir plus bas).
* ``Fwk\Core\Descriptor``: C'est l'objet qui lit et exploite le fichier *fwk.xml*.
* ``Fwk\Core\Context``: C'est l'objet qui contient l'intégralité du *contexte* de la requête en cours. Requête, Réponse, Action en cours... tout est là.

L'architecture de Fwk/Core étant basée entièrement sur des évènements applicatifs, ces objets ne sont que de simples conteneurs passifs. Ils sont orchestrés par des *Listeners* qui se chargent de transformer une requête client en une réponse.

L'intégralité du comportement d'une application Fwk/Core décrit dans cette documentation dépend du Listener principal ``Fwk\Core\CoreListener`` qui doit être le premier décrit dans ``fwk.xml``. Cependant, un développeur aguéri peut décider de remplacer ce listener pour implémenter son propre *runtime*.

Evènements principaux
=====================

Les évènements principaux d'un application, lorsque ``Fwk\Core\Listener`` est utilisé, sont les suivants:

============= ===================== ================================= ========================================================================
Evènement     Méthode Listener      Event Type                        Description
============= ===================== ================================= ========================================================================
boot          ``onBoot()``          ``Fwk\Core\Events\BootEvent``     Notifié lorsque l'application démarre
request       ``onRequest()``       ``Fwk\Core\Events\RequestEvent``  Notifié lors de la requête utilisateur
dispatch      ``onDispatch()``      ``Fwk\Core\Events\DispatchEvent`` Notifié lorsque aucune Action n'a été trouvée pour satisfaire la requête
init          ``onInit()``          ``Fwk\Core\CoreEvent``            Notifié juste avant l'intanciation de l'objet Action
actionLoaded  ``onActionLoaded()``  ``Fwk\Core\CoreEvent``            Notifié lorsque la classe Action à été chargée
actionSuccess ``onActionSuccess()`` ``Fwk\Core\CoreEvent``            Notifié lorsque l'Action à été exécutée
result        ``onResult()``        ``Fwk\Core\CoreEvent``            Notifié lors du résultat de l'Action
response      ``onResponse()``      ``Fwk\Core\CoreEvent``            Notifié lorsque la réponse à l'Action à été définie
end           ``onEnd``             ``Fwk\Core\Events\EndEvent``      Notifié lors de la fin du traitement de la requête
finalResponse ``onFinalResponse()`` ``Fwk\Core\CoreEvent``            Notifié lorsque la réponse finale à la requête est définie
error         ``onError``           ``Fwk\Core\Events\ErrorEvent``    Notifié lorsqu'une erreur survient
============= ===================== ================================= ========================================================================

Certains de ces évènements peuvent paraîtres redondants mais un tel découpage permet d'intervenir à n'importe quel moment du runtime de l'Application.

