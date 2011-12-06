Création d'un blog avec Symfony2
================================

Introduction
------------

Ce tutoriel va vous guider dans le processus de création d’un blog complet avec `Symfony2 <http://symfony.com/>`_. La distribution standard du framework sera utilisée, qui inclut les principaux composants nécessaires à la création de vos propres sites web. Le tutoriel est découpé en plusieurs parties, qui couvrent chacune des aspects différents de Symfony2 et de ses composants. Il est prévu pour être utilisé de la même manière que `Jobeet <http://www.symfony-project.org/jobeet/1_4/Doctrine/en/>`_ pour Symfony 1.


Chapitres du tutoriel
~~~~~~~~~~~~~~~~~~~~~

.. toctree::
    :maxdepth: 1

    docs/configuration-et-templates
    docs/validateurs-et-formulaires
    docs/doctrine-2-et-les-articles
    docs/maj-des-articles-ajout-de-commentaires
    docs/personnalisation-de-la-vue-twig-avance
    

Site démo
---------


Le site original de Symblog se trouve à l’adresse : `http://symblog.co.uk <http://symblog.co.uk/>`_. Le code source est disponible sur `Github <https://github.com/dsyph3r/symblog>`_. Il suit chaque partie du tutoriel.


Contenu
--------

Le but de ce tutoriel est de couvrir les tâches que vous allez régulièrement être amené à réaliser lors de la création d’un site web avec Symfony2.

    1.  Les bundles
    2.  Les controlleurs
    3.  Les templates (avec Twig)
    4.  Le modèle - Doctrine 2
    5.  Les migrations
    6.  Les données factices
    7.  Les validateurrs
    8.  Les formulaires
    9.  Le routage
    10. Gestion des fichiers externes
    11. Les emails
    12. les environnements
    13. Personnalisation des pages d'erreur
    14. La securité
    15. L'utilisateur et les sessions
    16. Generation de CRUD
    17. Le cache
    18. Les tests
    19. Le deploiement

Symfony2 est fortement personnalisable et propose différentes manières de réaliser un même tâche. On peut citer par exemple le format de configuration qui peut être le YAML, le XML, le PHP ou les annotations, ainsi que la création de template en PHP ou à l’aide de Twig. Par souci de simplicité, nous utiliserons le format YAML et les annotations pour la configuration, et Twig pour les templates. Le `livre Symfony <http://symfony.com/doc/current/book/index.html>`_ propose de nombreux exemples de l’utilisation des autres méthodes. Si d’autres personnes souhaitent contribuer à compléter des méthodes alternatives, n’hésitez pas à faire un fork du projet sur `Github <https://github.com/dsyph3r/symblog-docs>`_ puis proposer un pull :)

Traductions
------------

Espagnol
~~~~~~~~

Symblog a été traduit en `espagnol <http://udelabs.com/symfony/symblog/index.html>`_ grâce à `Lisper <https://twitter.com/#!/esymfony>`_.

Français
~~~~~~~

La version de Symblog que vous lisez actuellement a été traduite en `français <http://keiruaprod.fr/blog/symblog/>`_ grâce à `Keirua <https://twitter.com/#!/clemkeirua>`_.

Auteur
------

Ce tutoriel a été originalement écrit par `dsyph3r <http://twitter.com/#!/dsyph3r>`_.

Participer
------------

La  de ce tutoriel est disponible sur Github, en `français <https://github.com/Keirua/symblog-docs>`_ et en `anglais <https://github.com/dsyph3r/symblog-docs>`_. Si vous voulez améliorer et étendre ce tutoriel, vous pouvez faire un fork du projet et proposer un pull. Vous pouvez également rapporter les problèmes via le `gestionnaire de problèmes <https://github.com/dsyph3r/symblog-docs/issues>`_ de Github. Si quelqu’un est intéressé par la création d’un design plus joli, qu’il n’hésite pas à contacter `l’auteur original <http://twitter.com/#%21/dsyph3r>`_!

Credits
-------

Remerciement particulier aux contributeurs de la `documentation officielle de Symfony2  <http://symfony.com/doc/current/>`_., qui a été une source inestimable d’information.

Les icones proviennent de `famfamfam <http://www.famfamfam.com/lab/icons/flags/>`_.

