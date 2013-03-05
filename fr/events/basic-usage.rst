Utilisation de Fwk/Events
#########################

L'objet ``Fwk\Events\Dispatcher`` est très simple et va droit au but. Il stocke les listeners pour chaque évènement et appèle leur fonction lorsque l'évènement est notifié. ``Fwk\Events\Dispatcher`` peut être utilisé tel quel où être étendu par des objets de plus haut niveau dans l'application.

Les évènements sont eux des classes/conteneurs qui possèdent au moins un nom, et éventuellement un jeu de données. Il est possible d'étendre l'objet ``Fwk\Events\Event`` pour créer des classes pour chaque évènement distinct.

::

    <?php
    use Fwk\Events\Dispatcher;

    $dispatcher = new Dispatcher();


Notifier un évènement
=====================

Voici la manière la plus simple de notifier un évènement:

::

    <?php
    use Fwk\Events\Event;

    // création de l'évènement
    $event = new Event("exemple", array(
        "donnee1" => "valeur1"
    ));
    $event->extraData = "extraValue"; // ArrayAccess

    // notification de l'évènement
    $dispatcher->notify($event);

Les listeners écoutant l'évènement *exemple* seront alors appelés dans leur ordre d'ajout au ``Dispatcher``.

Ecouter les évènements
======================

Grâce aux `Closures <http://php.net/closure>`_ introduites avec PHP 5.3, il est possible de créer très facilement des listeners qui traiteront les évènements applicatifs.

::

    <?php
    use Fwk\Events\Event;

    $dispatcher->on("exemple", function(Event $event) {
        // faire quelque chose avec notre évènement et ses données.
    });

Lorsque l'on souhaite avoir une vue plus synthétique de l'application, ses évènements et ses listeners, il est toutes fois conseillé de créer des classes Listeners. Ces classes ne doivent étendre aucun autre objet et les méthodes préfixée par ``on`` suivi du nom de l'évènement seront appelées lorsque l'évènement sera notifié. 

Voici une l'équivalent en objet du précédent listener:

::

    <?php
    use Fwk\Events\Event;

    class MyListener 
    {
        public function onExemple(Event $event) {
            // faire quelque chose avec notre évènement et ses données.
        }
    }

    $dispatcher->addListener($listener = new MyListener());


.. note:: Lorsque l'on ajoute un listener sous forme d'objet, ``Fwk\Events\Dispatcher`` stocke uniquement les fonctions appelées lors d'évènements (``callables``). Ceci n'est pas très important à savoir sauf lorsque l'on souhaite retirer des listeners.
    
Suppression de listeners
========================

Il peut arriver d'avoir a supprimer des listeners déjà ajoutés au ``Dispatcher``. Pour ce faire, il existe deux méthodes:

::

    <?php
    /*
     * tous les listeners pour l'évènement "exemple" sont retirés
     */
    $dispatcher->removeAllListeners("exemple"); 

    /*
     * suppression uniquement du listener d'exemple ci-dessus
     */
    $dispatcher->removeListener("exemple", array($listener, "onExemple")); 

.. note:: Généralement, il convient d'ajouter uniquement des listeners qui serviront tout le long du runtime. Retirer des listeners n'est pas pratique car le développeur doit conserver sous la main les instances des listeners/callables. Cela peut également refléter un problème de conception.



