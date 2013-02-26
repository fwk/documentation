Installation des Packages
#########################

Le meilleur moyen d'installer et de maintenir les packages Fwk est `Composer <http://getcomposer.org/>`_, un outil de gestion de dépendances largement utilisé par la communauté PHP. De plus, Composer génères un `Autoloader <http://php.net/manual/fr/language.oop5.autoload.php>`_ sur-mesure pour votre application, quelques soient les librairies demandées.

Dans le cas d'une installation traditionnelle (décrite plus bas) vous devrez vous même coder votre `Autoloader <http://php.net/manual/fr/language.oop5.autoload.php>`_.

Composer
========

Installer Composer
------------------

Vous pouvez, au choix, `Télécharger manuellement Composer <http://getcomposer.org/download/>`_ où l'installer directement via la ligne de commande suivante (PHP et Curl doivent être installés sur votre système):

::

    curl -s https://getcomposer.org/installer | php

.. note::
   Composer sera installé dans le répertoire courrant.


Installer un package
--------------------

Pour installer un package à être utilisé dans votre application, il vous suffit maintenant d'ajouter la clause correspondante dans votre fichier composer.json. 

.. note::
   Retrouvez les clauses nécessaires aux packages Fwk sur `Packagist <https://packagist.org/packages/fwk/>`_.

Une fois chose faite, allez dans la console et tapez:

::

    php composer.phar install

ou

::

    php composer.phar update

afin d'installer le package désiré. C'est tout!

Installation Traditionnelle
===========================

L'installation traditionnelle n'est pas recommandée car:

* Il est difficile de maintenir à jour ses dépendances
* Implique une écriture manuelle de l'`Autoloader <http://php.net/manual/fr/language.oop5.autoload.php>`_
* Peut s'avérer être un calvaire (gestion des sous-dépendances)

Afin d'installer un package Fwk, il suffit de télécharger le Zip du repository sur `Github <http://github.com/fwk>`_ et d'en extraire les fichiers où vous le souhaitez (généralement dans votre include_path)

Voici une liste des différents packages Fwk ainsi que leurs dépendances nécessaires à leur utilisation.

===========  ============================================================================
Package Fwk  Dépendances
===========  ============================================================================
Events       --
Core         * HttpFoundation -> https://github.com/symfony/HttpFoundation/tree/2.2
             * Fwk/Events     -> https://github.com/fwk/Events/tree/master
             * Fwk/Xml        -> https://github.com/fwk/Xml/tree/master
Xml          --
Db           * Doctrine/DBAL  -> http://www.doctrine-project.org/projects/dbal.html
             * Fwk/Events     -> https://github.com/fwk/Events/tree/master
             * Psr/log        -> https://github.com/php-fig/log/tree/1.0.0
Form         * Fwk/Events     -> https://github.com/fwk/Events/tree/master
Cache        * FunctionParser -> https://github.com/jeremeamia/FunctionParser/tree/master
             * SuperClosure   -> https://github.com/jeremeamia/super_closure/tree/master
Security     * Zend Authentication -> https://packages.zendframework.com/
             * Zend Permissions Acl -> https://packages.zendframework.com/
             * Zend Crypt     -> https://packages.zendframework.com/
             * Fwk/Events     -> https://github.com/fwk/Events/tree/master
===========  ============================================================================

