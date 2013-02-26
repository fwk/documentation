Présentation des Packages
#########################

Fwk est segmenté en plusieurs packages indépendants dont voici la liste. 

.. note::
   Vous trouverez des exemples où ces packages sont utilisés en même temps dans la section des Tutoriels.

Core
====

Fwk/Core est le package qui permet de créér une application type MVC de A à Z. Sa structure modulable et robuste en fait un outil puissant qui facilite le travail du développeur tout au long du cycle de vie de l'application. Il s'inspire d'autres grands frameworks (comme Struts) mais garde un côté léger et simple propre à ce que peut espérer un développeur PHP.

.. hint:: 
   Les sources du package sont disponibles sur Github: http://github.com/fwk/Core

Db
==

Fwk/Db est un framework d'accès aux bases de données qui ne s'inspire lui d'aucun autre ORM/framework. Simple et efficace, il permet d'utiliser des classes PHP comme entités (Models) et fait de la persistance de données un jeu d'enfant. De plus, il fonctionne sans configuration (pas de mapping XML) et supporte les relations entre entités classiques (One2One, One2Many, Many2Many). Il se base sur le fameux Doctrine/DBAL ce qui le rend compatible avec un maximum de moteurs de bases de données.

.. hint:: 
   Les sources du package sont disponibles sur Github: http://github.com/fwk/Db

Xml
===

Fwk/Xml est un outil de parsing de fichiers XML. Le développeur spécifie une Map, passe son fichier XML à la moulinette et en tire un tableau de données prêt à être utilisé dans le reste de son application. L'essayer c'est l'adopter! 

.. hint:: 
   Les sources du package sont disponibles sur Github: http://github.com/fwk/Xml

Cache
=====

Fwk/Cache est une bibliothèque qui permet d'écrire et/ou de lire des objets en cache. Sa syntaxe de fonctions chainées permet de rendre la manipulation du cache complètement transparente pour le développeur.

.. hint:: 
   Les sources du package sont disponibles sur Github: http://github.com/fwk/Cache

Form
====

Fwk/Form est un framework de formulaires HTML. Basé sur un découplage total des différents outils, il facilite l'intégration et la customisation pour le développeur final.

.. hint:: 
   Les sources du package sont disponibles sur Github: http://github.com/fwk/Form

Security
========

Fwk/Security permet de gérer l'Authentification et les Autorisations au sein d'une application PHP. C'est une aggrégation de multiples composants ZendFramework (version 2). Ce package contient également des outils pour la génération de mots de passes sécurisés.

.. hint:: 
   Les sources du package sont disponibles sur Github: http://github.com/fwk/Security

Events
======

Fwk/Events est une micro-librairie permettant d'émettre ou de traiter des évènements applicatifs. Simple, rapide et efficace, elle est utilisée par la plupart des composants Fwk et peut s'avérer très utile pour développer aisément des applications orientées-évènements.

.. hint:: 
   Les sources du package sont disponibles sur Github: http://github.com/fwk/Events

