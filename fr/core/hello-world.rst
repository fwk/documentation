Hello World !
#############

Comme pour tout framework d'applications, l'exemple du Hello World s'impose :) 
Nous allons donc développer de A à Z une application répondant "Hello World" sur la page d'accueil. 

Préparation du système de fichiers
==================================

Dans un premier temps, nous allons créer le répertoire principal de l'application.
Pour cet exemple, le "package" de l'application sera *HelloWorld* et le dossier
(webroot) de l'application sera *public*. Ces deux dossiers seront stockés dans 
le répertoire *app*.

::

    mkdir -p app && mkdir -p app/HelloWorld app/public
    cd app


Nous allons ensuite télécharger `Composer <http://getcomposer.org/>`_ et installer les dépendances requises pour notre application. (:doc:`voir la page de documentation <../intro/install-package>`). Une fois `Composer <http://getcomposer.org/>`_ installé, nous exécutons la commande suivante pour créer le fichier composer.json.

::

    php composer.phar init

Nous allons maintenant éditer le fichier et rajouter Fwk/Core en dépendance.

::

    {
        "name": "neiluj/fwk-helloworld",
        "minimum-stability": "dev",
        "require": {
            "fwk/core": "dev-master"
        },
        "autoload": {
            "psr-0": {
                "HelloWorld": ""
            }
        }
    }

Ensuite, il suffit de finaliser l'installation:

::

    php composer.phar install


Si tout s'est bien déroulé, un dossier *vendor* est venu s'ajouter dans notre répertoire courant, contenant les dépendances et l'autoloader de notre application.


index.php
=========

Les requêtes provenant du web arriveront vers un fichier *index.php* placé dans notre répertoire racine *public*. Ce fichier doit rester le plus simple possible.

::

    <?php
    require_once __DIR__ .'/../vendor/autoload.php';

    use Fwk\Core\Application, Fwk\Core\Descriptor;

    Application::autorun(new Descriptor(__DIR__ .'/../HelloWorld/fwk.xml'));

Nous passons sur les détails. 

fwk.xml (Descriptor)
====================

Le fichier *fwk.xml* localisé à la racine du répertoire *HelloWorld* permet de configurer notre application. C'est la seule forte dépendance/pré-requis du framework. C'est ce que l'on appele la "description" de l'application. 

Voici son contenu:

::

    <?xml version="1.0" encoding="UTF-8"?>
    <fwk id="HelloWorld" version="1.0-dev">
        <listener class="Fwk\Core\CoreListener" />
        <actions>
            <action name="Hello" class="HelloWorld\actions\Hello" method="show" />
        </actions>
    </fwk>

Notre application étant simplissime, ce fichier est très simple à comprendre:

* On souhaite utiliser CoreListener, qui est le chef d'orchestre général du framework.
* On déclare une seule action (nom, classe et méthode), celle qui dit "bonjour"

Hello.action
============

Nous devons maintenant créer le dossier *actions* ou nous mettrons nos "Actions" (lire *Controllers*), ici "Hello".

::

    mkdir HelloWorld/actions && cd HelloWorld/actions
    touch Hello.php

Le contenu de notre classe Hello est lui aussi très simple:

:: 

    <?php
    namespace HelloWorld\actions;

    class Hello
    {
        public function show()
        {
            return "Hello World";
        }
    }


Premier test
============

Notre action "Hello" est maintenant disponible à l'URL suivante (en fonction de la configuration de votre environnement): http://localhost/~neiluj/helloworld/public/index.php/Hello.action

Ceci devrait afficher le texte "Hello World" !

URL-Rewritting
==============

Core dispose d'un listener permettant de réécrire les URLs de manière simple et efficace. Pour des raisons de brièveté, nous n'allons pas en détailler son usage en détail ici. Tout se passe dans le fichier *fwk.xml* :

::

    <?xml version="1.0" encoding="UTF-8"?>
    <fwk id="HelloWorld" version="1.0-dev">
        <listener class="Fwk\Core\CoreListener" />
        <listener class="Fwk\Core\Components\UrlRewriter\UrlRewriterListener" />
        <actions>
            <!-- ... -->
        </actions>

        <url-rewrite>
            <url route="/" action="Hello" />
        </url-rewrite>
    </fwk>

à présent, notre application répond à l'adresse suivante: http://localhost/~neiluj/helloworld/public/

Hello-who?
==========

Maintenant que notre application est en place, il est temps de l'améliorer un petit peu. Nous allons préciser à l'aide d'un paramètre de requête le nom de la personne qui souhaite être saluée. On commence donc par éditer notre action *Hello* 

::

    <?php
    namespace HelloWorld\actions;

    class Hello
    { 
        protected $name = "World";

        public function show()
        {
            return "Hello ". htmlspecialchars($this->name);
        }

        public function getName() 
        {
            return $this->name;
        }

        public function setName($name) 
        {
            $this->name = $name;
        }
    }

Notre paramètre *$name* est accessible depuis la requête HTTP, par exemple: http://localhost/~neiluj/helloworld/public/?name=JohnDoe

Ajoutons maintenant une règle de réécriture dans notre fichier *fwk.xml* 

::

    <!-- http://localhost/~neiluj/helloworld/public/hello/john -->
    <url route="/hello/:name" action="Hello">
        <param name="name" required="true" />
    </url>


Fin
===

Vous pouvez télécharger/forker cet exemple sur Github: http://github.com/neiluj/fwk-helloworld
