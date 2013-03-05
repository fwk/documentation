Utilisation avancée
###################

Stopper un évènement
====================

Si un listener souhaite arrêter la propagation d'un évènement, il peut appeler la méthode ``stop()`` de l'évènement.

:: 

    <?php

    $dispatcher->on("exemple", function(Event $event) {
        // faire quelque chose avec notre évènement et ses données.
        // ...
        // arreter le traitement futur de l'évènement
        $event->stop();
    });


Gestion des priorités
=====================

Le choix à été fait de ne pas autoriser la gestion des priorités au sein du ``Dispatcher`` pour des raisons de simplicité mais également car cela peut rendre difficile la maintenance/debug de l'application. Il est donc du ressort du développeur d'ajouter dans l'ordre d'éxecution souhaité ses listeners.


Savoir si l'évènement est écouté/traité par un listener
=======================================================

Lorsqu'un évènement est notifié et qu'un listener le traite, un flag est alors activé dans l'objet ``Fwk\Events\Event`` afin de le savoir.

::

    <?php
    use Fwk\Events\Event;

    // création de l'évènement
    $event = new Event("exemple");
    $dispatcher->notify($event);

    // mon évènement as-t'il été traité?
    $event->isProcessed(); // true/false
    
