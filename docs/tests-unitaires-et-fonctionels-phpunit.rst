[Partie 6] - Les tests unitaires et fonctionnels avec PHPUnit
====================================================

Introduction
--------

Jusqu'à présent, nous avons exploré une grande quantité d'éléments de base essentiels de Symfony2. Avant de continuer à ajouter des fonctionnalités à Symblog, il est temps de commencer à parler des tests. Nous allons regarder comment tester les fonctions individuellement grâce aux tests unitaires, puis regarder comment nous assurer que plusieurs composants fonctionnent bien ensemble grâce aux tests unitaires. Nous allons utiliser la librairie de tests `PHPUnit <http://www.phpunit.de/manual/current/en/>`_ car elle est au centre des tests dans Symfony2. Comme le sujet des tests est très large, il sera abordé à nouveau dans d'autres chapitres par la suite. A la fin de ce chapitre nous aurons écrit plusieurs tests, à la fois unitaires et fonctionnels. Nous aurons simulé des requêtes du navigateur, rempli des formulaires, et vérifié la réponse pour nous assurer que les pages s'affichent correctement. Nous allons également regarder quel pourcentage du code de l'application couvrent ces tests.


Les tests dans Symfony2
-------------------

`PHPUnit <http://www.phpunit.de/manual/current/en/>`_  est devenu le standard pour l'écriture des tests en PHP, donc si vous ne connaissez pas cette librairie, l'apprendre vous sera également utile pour vos autres projets PHP. N'oubliez également pas que la plupart des concepts abordés dans ce chapitre sont indépendant du langage, et pourront ainsi être transférés dans les autres langages que vous pourrez être amenés à utiliser.

.. tip::

    Si vous comptez écrire vos propres bundles open source pour Symfony2, vous avez plus de chances de gagner en popularité si votre bundle est bien testé (et documenté). Regardez les bundles déjà existant pour Symfony2 chez `KnpBundles <http://bundles.knplabs.org/fr>`_.

Tests unitaires
~~~~~~~~~~~~~~~

Les tests unitaires ont pour mission d'assurer que des unités individuelles de code fonctionnent correctement lorsqu'elles sont utilisées de manière isolée. Dans un cadre objet tel que celui de Symfony2, une unité serait une classe et ses méthodes. Par exemple, nous pourrions écrire les test pour les classes des entités ``Blog`` et ``Comment``. Lorsque l'on écrit des tests unitaires, les tests devraient être écrits indépendemment des autres tests, c'est à dire que le résultat du test B ne devrait pas dépendre du résultat du test A. Il est utile, lorsque l'on fait des tests unitaires, de pouvoir créer des faux objets (des objets mock) qui facilitent les tests unitaires lorsqu'il y a des dépendances. L'utilisation des mocks permet également de simuler un appel de fonction plutôt que l'exécuter; c'est par exemple utile pour simuler une classe qui fait appel à une librairie extérieure. La classe de l'API peut utiliser une couche de transport pour communiquer avec la librairie externe. On peut "mocker" la méthode de requête de la couche de transport pour simuler les résultats que l'on veut, plutôt que de faire appel à la librairie extérieur (dont l'utilisateur dans un cadre isolé peut être pénible à mettre en place, en plus d'être hors sujet, et peut mener à des erreurs lors des mises à jour de librairies, et ainsi de suite).
Les tests unitaires ne testent pas que les composants fonctionnent bien ensemble, c'est un domaine couvert par le thème suivant, les tests fonctionnels.

Tests fonctionnels
~~~~~~~~~~~~~~~~~~

Les tests fonctionnels vérifient l'intégration des différents composants à l'intérieur de l'application, tel que le routage, les controlleurs et les vues. Les tests fonctionnels sont similaires aux tests manuels que vous pouvez effectuer vous même en lançant le navigateur sur la page d'accueil du site, en cliquant sur le lien d'un article et en vérifiant que c'est le bon article qui s'affiche.
Les tests fonctionnels fournissent la possibilité d'automatiser ce processus. Symfony2 propose plusieurs classes très utiles pour les tests fonctionnels, tel qu'un ``Client`` capable d'effectuer des requêtes vers les pages et soumettre des formulaire, et un navigateur de DOM que l'on peut utiliser pour analyser la réponse du client.

.. tip::

    Il y a plusieurs processus de développement logiciel dirigés par les tests. On peut citer le TDD (Test Driven Development, développement orienté tests) et BDD (Behavioral Driven Development, développement orienté comportement). Bien que ces thèmes aillent au dela du but de ce tutoriel, il est bon que vous ayiez connaissance de la librairie écrité par `everzet <https://twitter.com/#!/everzet>`_,  `Behat <http://behat.org/>`_, qui facilite le BDD. Il y a également le `BehatBundle <http://docs.behat.org/bundle/index.html>`_ disponible pour Symfony2, qui permet d'intégrer facilement Behat dans vos projets Symfony2.

PHPUnit
-------

Comme dit plus haut, les tests sont écrits dans Symfony2 avec PHPUnit. Vous devrez l'installer afin de pouvoir lancer ces tests et les tests de ce chapitre. Pour des instructions  d'`installation détaillée <http://www.phpunit.de/manual/current/en/installation.html>`_ rendez vous sur la documentation officielle du site de PHPUnit. Pour lancer les tests dans Symfony2, il vaut faut PHPUnit 3.5.11 ou une version plus récente. PHPUnit  est une librairie de tests très complète, donc des références à la documentation officielle auront lieu lorsque des détails supplémentaires peuvent être apportés.

Assertions
~~~~~~~~~~

L'écriture de tests s'occupe de vérifier que le résultat test du test est bien égal à celui attendu. Il y a plusieurs méthodes d'assertion disponibles dans PHPUnit pour nous assister dans cette tâche. Quelques unes des assertions les plus courantes sont listées ci-dessous.

.. code-block:: php

    // Check 1 === 1 is true
    $this->assertTrue(1 === 1);

    // Check 1 === 2 is false
    $this->assertFalse(1 === 2);

    // Check 'Hello' equals 'Hello'
    $this->assertEquals('Hello', 'Hello');

    // Check array has key 'language'
    $this->assertArrayHasKey('language', array('language' => 'php', 'size' => '1024'));

    // Check array contains value 'php'
    $this->assertContains('php', array('php', 'ruby', 'c++', 'JavaScript'));

Une liste complète des 
`assertions <http://www.phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.assertions>`_
est disponible dans la documentation de PHPUnit.

Lancer les tests de Symfony2
----------------------

Avant de commencer à écrire des tests, regardons comment lancer des tests dans Symfony2. On peut spécifier à PHPUnit de se lancer avec un fichier de configuration particulier. Dans notre projet Symfony2, ce fichier est ``app/phpunit.xml.dist``. Comme le nom du fichier est suffixé avec ``.dist``, vous devez copier son contenu dans un fichier ``app/phpunit.xml``.

.. tip::

    Si vous utilisez un gestionnaire de version tel que Git, vous devriez ajouter le nouveau fichier ``app/phpunit.xml`` dans la liste des fichiers à ignorer.

Si vous regardez maintenant le contenu du fichier de configuration PHPUnit, vous y verrez ce qui suit :

.. code-block:: xml

    <!-- app/phpunit.xml -->
    
    <testsuites>
        <testsuite name="Project Test Suite">
            <directory>../src/*/*Bundle/Tests</directory>
            <directory>../src/*/Bundle/*Bundle/Tests</directory>
        </testsuite>
    </testsuites>

Ces paramètres configurent des répertoires qui font partie de notre ensemble de test. Lorsqu'on lance PHPUnit, il va chercher dans ces répertoire s'il existe des tests à lancer. On peut également lui fournir des paramètres de ligne de commande additionnels pour lancer les tests seulement sur un répertoire particulier, plutôt que sur une suite de tests complète. Nous verrons comment faire celà un peu plus tard.

Vous pouvez également remarquer que ce fichier spécifie le fichier de démarrage ``app/bootstrap.php.cache``. Ce fichier est utilisé par PHPUnit pour obtenir des paramètres de l'environnement de test.

.. code-block:: xml

    <!-- app/phpunit.xml -->
    
    <phpunit
        bootstrap                   = "bootstrap.php.cache" >

.. tip::

    Pour plus d'informations concernant la configuration de PHPUnit à l'aide d'un fichier XML, reportez vous à la 
    `documentation de PHPUnit <http://www.phpunit.de/manual/current/en/organizing-tests.html#organizing-tests.xml-configuration>`_.

Lancer les tests
----------------

Comme nous avons utilisé le générateur de Symfony2 pour créer le ``BloggerBlogBundle`` dans le chapitre 1, une classe de test pour le controlleur par défaut  ``DefaultController`` a également été crée. On peut exécuter ce test en lançant la commande suivante depuis le répertoire racine du projet. L'option ``-c`` précise que PHPUnit doit charger sa configuration depuis le répertoire ``app``.

.. code-block:: bash

    $ phpunit -c app

Une fois que le test est terminé, vous devriez être averti que les tests ont échoué. Si vous regardez dans la classe ``DefaultControllerTest`` dans ``src/Blogger/BlogBundle/Tests/Controller/DefaultControllerTest.php``, vous y trouverez le contenu suivant :

.. code-block:: php

    <?php
    // src/Blogger/BlogBundle/Tests/Controller/DefaultControllerTest.php

    namespace Blogger\BlogBundle\Tests\Controller;

    use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

    class DefaultControllerTest extends WebTestCase
    {
        public function testIndex()
        {
            $client = static::createClient();

            $crawler = $client->request('GET', '/hello/Fabien');

            $this->assertTrue($crawler->filter('html:contains("Hello Fabien")')->count() > 0);
        }
    }

C'est un test fonctionnel pour la classe ``DefaultController`` que Symfony2 a généré. Si vous vous souvenez du chapitre 1, ce controlleur avait une action qui gérait les requêtes vers ``/hello/{name}``. Puisque nous avons enlevé cette classe, le test échoue. Essayez de vous rendre à l'adresse ``http://symblog.dev/app_dev.php/hello/Fabien`` avec un navigateur. Symfony2 devrait vous informer que la route n'a pas pu être trouvée. Comme ce test fait appel à la même adresse, il obtient la même réponse, d'où l'échec du test. Les tests fonctionnels vont couvrir une grosse partie de ce chapitre et seront abordés en détails par la suite.

COmme nous avons supprimé la classe ``DefaultController``, vous pouvez également supprimer cette classe de test. Supprimez la classe  ``DefaultControllerTest`` située dans ``src/Blogger/BlogBundle/Tests/Controller/DefaultControllerTest.php``.

Tests unitaires
---------------

Comme expliqué précédemment, les tests unitaires ont pour mission de tester des unités de l'application, individuellement, en isolation. Lorsque vous écrivez des tests unitaires, il est conseillé de reproduire la structure du Bundle dans le répertoire Tests. Si par exemple vous voulez tester la classe de l'entité ``Blog``, située dans ``src/Blogger/BlogBundle/Entity/Blog.php``, le fichier de test devrait se trouver dans
``src/Blogger/BlogBundle/Tests/Entity/BlogTest.php``. Une structure de répertoire pourrait être la suivante :

.. code-block:: text

    src/Blogger/BlogBundle/
                    Entity/
                        Blog.php
                        Comment.php
                    Controller/
                        PageController.php
                    Twig/
                        Extensions/
                            BloggerBlogExtension.php
                    Tests/
                        Entity/
                            BlogTest.php
                            CommentTest.php
                        Controller/
                            PageControllerTest.php
                        Twig/
                            Extensions/
                                BloggerBlogExtensionTest.php

Vous pouvez remarquer que tous les fichiers de tests sont suffixés par Test.

Testing the Blog Entity - Slugify method
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Nous allons commencer par tester la méthode slugify de l'entité ``Blog``. Ecrivons quelques tests afin de nous assurer qu'elle fonctionne correctement. Créez un nouveau fichier dans ``src/Blogger/BlogBundle/Tests/Entity/BlogTest.php`` et ajoutez-y le contenu suivant :

.. code-block:: php

    <?php
    // src/Blogger/BlogBundle/Tests/Entity/BlogTest.php

    namespace Blogger\BlogBundle\Tests\Entity;

    use Blogger\BlogBundle\Entity\Blog;

    class BlogTest extends \PHPUnit_Framework_TestCase
    {

    }

Nous avons créé une classe de test pour l'entité ``Blog``. Remarquez que la localisation de se fichier est conforme à la structure de répertoire évoquée plus haut. La classe ``BlogTest`` extends la classe de base de PHPUnit ``PHPUnit_Framework_TestCase``. Tous les tests que vous écrirez pour PHPUnit hériteront de cette classe. Vous vous souvenez peut être depuis un chapitre précédent que le ``\`` doit être placé devant le nom de la classe ``PHPUnit_Framework_TestCase`` car elle est déclarée dans l'espace de nom public de PHP.

Nous avons un squelette de la classe de test pour l'entité ``Blog``, écrivons donc maintenant un scénario de test. Les scénarios de test sont, avec PHPUnit, des méthodes de la classe de test préfixées par ``test``, tel que ``testSlugify()``. Mettez à jour la classe ``BlogTest`` dans
``src/Blogger/BlogBundle/Tests/Entity/BlogTest.php`` avec ce qui suit.

.. code-block:: php

    // src/Blogger/BlogBundle/Tests/Entity/BlogTest.php

    // ..

    class BlogTest extends \PHPUnit_Framework_TestCase
    {
        public function testSlugify()
        {
            $blog = new Blog();

            $this->assertEquals('hello-world', $blog->slugify('Hello World'));
        }
    }

C'est un scénario très simple. On instancie une nouvelle entité ``Blog`` et lance un ``assertEquals()`` sur le résultat de la méthode ``slugify``. La méthode ``assertEquals()`` prend au moins 2 arguments en paramètres, le résultat attendu et celui obtenu. Un 3eme argument permet de choisir le message à afficher en cas d'échec.

Lançons notre nouveau test unitaire. Lancez la commande suivante.

.. code-block:: bash

    $ phpunit -c app

Vous devriez voir la sortie suivante :

.. code-block :: bash

    PHPUnit 3.5.11 by Sebastian Bergmann.

    .

    Time: 1 second, Memory: 4.25Mb

    OK (1 test, 1 assertion)

La sortie de l'exécution de PHPUnit est très simple à comprendre. Le programme commence par afficher certaines informations à propos de PHPUnit, et affiche un ``.`` pour chaque test lancé. Dans notre cas il y a seulement 1 test, donc seulement 1 ``.`` est affiché. La dernière ligne nous informe du résultat du tests. Pour notre ``BlogTest`` nous avons seulement lancé 1 test avec 1 assertion. Si vous avez l'affichage en couleur sur votre ligne de commande, vous verrez également la dernière ligne en vert, ce qui montre que tout s'est bien exécuté.
Modifions la méthode ``testSlugify()`` pour voir ce qui se passe lorsque le test échoue.

.. code-block:: php

    // src/Blogger/BlogBundle/Tests/Entity/BlogTest.php

    // ..

    public function testSlugify()
    {
        $blog = new Blog();

        $this->assertEquals('hello-world', $blog->slugify('Hello World'));
        $this->assertEquals('a day with symfony2', $blog->slugify('A Day With Symfony2'));
    }

Relancez le test unitaire comme précédement. Vous aurez alors la sortie suivante :

.. code-block :: bash

    PHPUnit 3.5.11 by Sebastian Bergmann.

    F

    Time: 0 seconds, Memory: 4.25Mb

    There was 1 failure:

    1) Blogger\BlogBundle\Tests\Entity\BlogTest::testSlugify
    Failed asserting that two strings are equal.
    --- Expected
    +++ Actual
    @@ @@
    -a day with symfony2
    +a-day-with-symfony2

    /var/www/html/symblog/symblog/src/Blogger/BlogBundle/Tests/Entity/BlogTest.php:15

    FAILURES!
    Tests: 1, Assertions: 2, Failures: 1.

La sortie est un peu plus évoluée cette fois ci. On peut voir que les ``.`` ont laissé place à un ``F``, qui nous indique qu'un des tests a échoué. Vous verrez également ``E`` lorsque le test contient des erreurs. PHPUnit nous informe ensuite des détails des échecs, donc dans le cas présent, de l'échec. On peut voir que la méthode ``Blogger\BlogBundle\Tests\Entity\BlogTest::testSlugify`` a échoué car les valeurs attendues et obtenues sont différentes. Si vous avez l'affichage en couleur dans la console, vous verrez la dernière ligne en rouge, indiquant des échecs dans les tests. Corrigez la méthode ``testSlugify()`` afin de réussir le test.

.. code-block:: php

    // src/Blogger/BlogBundle/Tests/Entity/BlogTest.php

    // ..

    public function testSlugify()
    {
        $blog = new Blog();

        $this->assertEquals('hello-world', $blog->slugify('Hello World'));
        $this->assertEquals('a-day-with-symfony2', $blog->slugify('A Day With Symfony2'));
    }

Avant d'avancer, ajoutons quelques autres tests pour la méthode ``slugify()``.

.. code-block:: php

    // src/Blogger/BlogBundle/Tests/Entity/BlogTest.php

    // ..

    public function testSlugify()
    {
        $blog = new Blog();

        $this->assertEquals('hello-world', $blog->slugify('Hello World'));
        $this->assertEquals('a-day-with-symfony2', $blog->slugify('A Day With Symfony2'));
        $this->assertEquals('hello-world', $blog->slugify('Hello    world'));
        $this->assertEquals('symblog', $blog->slugify('symblog '));
        $this->assertEquals('symblog', $blog->slugify(' symblog'));
    }

Maintenant que nous avons testé la méthode slugify de l'entité  ``Blog``, il faut nous assurer que le membre 
``$slug`` est correctement affecté lorsque le membre ``$title`` de ``Blog`` est mis à jour. Ajoutez la méthode suivante dans la classe ``BlogTest`` du fichier ``src/Blogger/BlogBundle/Tests/Entity/BlogTest.php``.

.. code-block:: php

    // src/Blogger/BlogBundle/Tests/Entity/BlogTest.php

    // ..

    public function testSetSlug()
    {
        $blog = new Blog();

        $blog->setSlug('Symfony2 Blog');
        $this->assertEquals('symfony2-blog', $blog->getSlug());
    }

    public function testSetTitle()
    {
        $blog = new Blog();

        $blog->setTitle('Hello World');
        $this->assertEquals('hello-world', $blog->getSlug());
    }

On commence par tester la méthode ``setSlug`` pour s'assurer que le membre ``$slug`` est correctement slugifié lorsqu'il est mis à jour. Ensuite on vérifie que le membre ``$slug`` est correctement mis à jour lorsque la méthode ``setTitle`` de l'entité ``Blog`` est appelée.

Lancez ces tests pour vérifier que l'entité ``Blog`` fonctionne correctement.

Test de l'extension Twig
~~~~~~~~~~~~~~~~~~~~~~~~

Dans le chapitre précédent, nous avons créé une extension Twig pour convertir une instance d'un objet ``\DateTime`` en une chaine de caractères contenant la durée écoulée depuis. Nous allons tester que cette méthode se comporte bien comme nous l'attendons.
Créez un nouveau fichier de test dans ``src/Blogger/BlogBundle/Tests/Twig/Extensions/BloggerBlogExtensionTest.php`` et mettez-y le ccontenu suivant :

.. code-block:: php

    <?php
    // src/Blogger/BlogBundle/Tests/Twig/Extensions/BloggerBlogExtensionTest.php

    namespace Blogger\BlogBundle\Tests\Twig\Extensions;

    use Blogger\BlogBundle\Twig\Extensions\BloggerBlogExtension;

    class BloggerBlogExtensionTest extends \PHPUnit_Framework_TestCase
    {
        public function testCreatedAgo()
        {
            $blog = new BloggerBlogExtension();

            $this->assertEquals("0 seconds ago", $blog->createdAgo(new \DateTime()));
            $this->assertEquals("34 seconds ago", $blog->createdAgo($this->getDateTime(-34)));
            $this->assertEquals("1 minute ago", $blog->createdAgo($this->getDateTime(-60)));
            $this->assertEquals("2 minutes ago", $blog->createdAgo($this->getDateTime(-120)));
            $this->assertEquals("1 hour ago", $blog->createdAgo($this->getDateTime(-3600)));
            $this->assertEquals("1 hour ago", $blog->createdAgo($this->getDateTime(-3601)));
            $this->assertEquals("2 hours ago", $blog->createdAgo($this->getDateTime(-7200)));

            // Cannot create time in the future
            $this->setExpectedException('\Exception');
            $blog->createdAgo($this->getDateTime(60));
        }

        protected function getDateTime($delta)
        {
            return new \DateTime(date("Y-m-d H:i:s", time()+$delta));
        }
    }

La classe est construite de la même manière que précédemment, et une méthode ``testCreatedAgo()`` teste l'extension Twig. Nous utilisons une nouvelle méthode de PHPUnit ici, la méthode ``setExpectedException()``. Cette méthode doit être appelée avant une méthode dont on attend qu'elle lance une exception. Nous savons que la méthode ``createdAgo`` ne peut gérer des durées dans le futur, et que si cela arrive, elle va lancer une ``\Exception``. La méthode ``getDateTime()`` est simplement une petite fonction auxilliaire pour créer une instance de ``\DateTime`` facilement. Vous pouvez remarquer que cette méthode n'est pas préfixée par ``test``, donc PHPUnit n'essayera pas de l'exécuter. Ouvrez une ligne de commande et lancez les tests pour ce fichier. On pourrait simplement lancer le test comme avant, mais nous pouvons également préciser à PHPUnit de lancer les tests sur tout un répertoire (et ses sous-répertoires) ou bien sur un seul fichier. Lancez la commande suivante :

.. code-block:: bash

    $ phpunit -c app src/Blogger/BlogBundle/Tests/Twig/Extensions/BloggerBlogExtensionTest.php

Cela va lancer les tests uniquement pour les fichiers de tests de notre extension. PHPUnit va alors nous informer que les tests ont échoué. Regardons la sortie, et essayons de comprendre pourquoi :

.. code-block:: bash

    1) Blogger\BlogBundle\Tests\Twig\Extension\BloggerBlogExtensionTest::testCreatedAgo
    Failed asserting that two strings are equal.
    --- Expected
    +++ Actual
    @@ @@
    -0 seconds ago
    +0 second ago

    /var/www/html/symblog/symblog/src/Blogger/BlogBundle/Tests/Twig/Extensions/BloggerBlogExtensionTest.php:14

Nous voulions que la première assertion renvoie ``0 seconds ago`` mais ce n'est pas arrivé, le mot ``second`` n'était pas au pluriel. Mettons à jour l'extension Twig dans ``src/Blogger/BlogBundle/Twig/Extensions/BloggerBlogBundle.php`` pour corriger cela.

.. code-block:: php

    <?php
    // src/Blogger/BlogBundle/Twig/Extensions/BloggerBlogBundle.php

    namespace Blogger\BlogBundle\Twig\Extensions;

    class BloggerBlogExtension extends \Twig_Extension
    {
        // ..

        public function createdAgo(\DateTime $dateTime)
        {
            // ..
            if ($delta < 60)
            {
                // Seconds
                $time = $delta;
                $duration = $time . " second" . (($time === 0 || $time > 1) ? "s" : "") . " ago";
            }
            // ..
        }

        // ..
    }

Relancez les tests. La première assertion passe désormais, mais le jeu de test échoue plus loin quand même. Regardons à nouveau pourquoi :

.. code-block:: bash

    1) Blogger\BlogBundle\Tests\Twig\Extension\BloggerBlogExtensionTest::testCreatedAgo
    Failed asserting that two strings are equal.
    --- Expected
    +++ Actual
    @@ @@
    -1 hour ago
    +60 minutes ago

    /var/www/html/symblog/symblog/src/Blogger/BlogBundle/Tests/Twig/Extensions/BloggerBlogExtensionTest.php:18

Nous pouvons maintenant remarquer que c'est la 5ème assertion qui échoue. Prenez note du 18 à la fin de la sortie, qui nous indique la ligne dans le fichie où l'assertion a échouée. En regardant le jeu de tests, on peut voir que l'extension ne se comporte pas comme attendu : là où il aurait fallu donner ``1 hour ago`` (il y a 1 heure), il a été répondu ``60 minutes ago`` (Il y a 60 minutes). Nous pouvons en trouver la raison en examinant le code du ``BloggerBlogExtension``. On compare les durées non strictement, c'est à dire avec ``<=`` plutôt ``<``. Nous le faisons également lorsque nous vérifions les heures. Mettez à jour le code de ``src/Blogger/BlogBundle/Twig/Extensions/BloggerBlogBundle.php`` pour corriger celà.

.. code-block:: php

    <?php
    // src/Blogger/BlogBundle/Twig/Extensions/BloggerBlogBundle.php

    namespace Blogger\BlogBundle\Twig\Extensions;

    class BloggerBlogExtension extends \Twig_Extension
    {
        // ..

        public function createdAgo(\DateTime $dateTime)
        {
            // ..

            else if ($delta < 3600)
            {
                // Mins
                $time = floor($delta / 60);
                $duration = $time . " minute" . (($time > 1) ? "s" : "") . " ago";
            }
            else if ($delta < 86400)
            {
                // Hours
                $time = floor($delta / 3600);
                $duration = $time . " hour" . (($time > 1) ? "s" : "") . " ago";
            }

            // ..
        }

        // ..
    }

Relancez à nouveau tous nos tests avec la commande suivante :

.. code-block:: bash

    $ phpunit -c app

Cela lance tous nos tests, et montre qu'ils passent tous sans erreurs. Bien que nous n'avons écrit que quelques tests unitaires, vous devriez déjà sentir à quel point il est important et puissant de tester unitairement son code. Bien que les tests ci-dessus soient mineurs, il s'agit tout de même d'erreurs. Tester permet également de s'assurer que les nouvelles fonctionnalités ajoutées au projet ne cassent pas ce qui est déjà en place. Cela conclut la page sur les tests unitaires pour cette fois-ci, mais nous y reviendrons par la suite. En attendant, vous pouvez essayer d'ajouter vos propres tests unitaires pour tester ce qui ne l'a pas encore été.

Tests fonctionnels
------------------

Maintenant que nous avons écrit quelques tests unitaires, passons aux tests de plusieurs composants à la fois. La première section des tests fonctionnels va nous faire simuler des requêtes dans un navigateur, afin d'analyser la réponse qui est générée.

Test de la page A propos
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Nous commençons par tester la classe ``PageController`` pour la page ``A propos``. C'est un bon point de départ, car cette page est très simple. Créez un nouveau fichier dans ``src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php`` et ajoutez-y le contenu suivant.

.. code-block:: php

    <?php
    // src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php

    namespace Blogger\BlogBundle\Tests\Controller;

    use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

    class PageControllerTest extends WebTestCase
    {
        public function testAbout()
        {
            $client = static::createClient();

            $crawler = $client->request('GET', '/about');

            $this->assertEquals(1, $crawler->filter('h1:contains("About symblog")')->count());
        }
    }

Nous avons déjà vu un test de controlleur très similaire lorsque nous avons brièvement jeté un oeil à la classe ``DefaultControllerTest``. Pour tester la page ``À propos``, on vérifie que la chaine ``About symblog`` est présente dans le HTML qui est généré, plus précisémment à l'intérieur d'un tag ``H1``. La classe ``PageControllerTest`` n'étend pas le ``\PHPUnit_Framework_TestCase`` comme nous l'avons vu dans les exemples des tests unitaires, elle étend à la place la classe ``WebTestCase``. Cette classe fait partie du FrameworkBundle, un bundle de Symfony2.

Comme expliqué plus tôt, les classes de tests de PHPUnit doivent étendre ``\PHPUnit_Framework_TestCase``, mais lorsque des fonctionnalités communes ou supplémentaires sont nécessaires dans plusieurs jeux de tests, il est utile de les encapsuler dans une classe à part et de faire que les classes de test étendent alors cette classe. C'est ce que fait ``WebTestCase``, qui fournit plusieurs méthode utiles aux tests fonctionnels dans Symfony2. Ouvrez la définition de ``WebTestCase`` dans
``vendor/symfony/src/Symfony/Bundle/FrameworkBundle/Test/WebTestCase.php``, vous y verrez que cette classe étend en fait la classe ``\PHPUnit_Framework_TestCase``.

.. code-block:: php

    // vendor/symfony/src/Symfony/Bundle/FrameworkBundle/Test/WebTestCase.php

    abstract class WebTestCase extends \PHPUnit_Framework_TestCase
    {
        // ..
    }

Si vous regardez la méthode ``createClient()`` de la classe ``WebTestCase``, vous verrez qu'elle instancie un kernel (noyau) de Symfony2. EN suivant les méthodes, vous pouvez également voir que l' ``environment`` est défini à ``test``, à moins qu'il soit surdéfini comme argument à ``createClient()``. Il s'agit là de l'environnement de test dont nous parlions dans le chapitre précédent.
En revenant à notre classe de test, on peut voir que la méthode ``createClient()`` est appelée pour mettre en marche le test. On appelle en suite ``request()`` sur le client pour simuler une requête  HTTP de type GET de la part d'un navigateur vers la page ``/about``, exactement comme si nous nous rendions sur ``http://symblog.dev/about`` dans un navigateur). La requête nous fournit en retour un objet ``Crawler`` qui contient la  ``Response``. La classe  ``Crawler`` est très utile car elle nous laisse traverser le HTML qui nous est renvoyé. Nous utilisons le l'instance du  ``Crawler`` pour vérifier que le tag ``H1`` de la réponse HTML contient les mots ``About symblog``. Vous remarquerez que, bien que nous étendons désormais la classe ``WebTestCase``, nous continuons à utiliser les méthodes d'assertions comme avant : souvenez vous que la classe ``PageControllerTest`` hérite de ``\PHPUnit_Framework_TestCase`` .

Lançons le ``PageControllerTest`` avec la commande suivante. Lorsque l'on écrit des tests, il est utile de ne lancer que les tests sur le fichier sur lequel on est en train de travailler : lorsque les tests deviennent nombreux, tous les exécuter peut alors prendre beaucoup de temps.

.. code-block:: bash

    $ phpunit -c app/ src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php

Vous devriez recevoir le message ``OK (1 test, 1 assertion)``, qui nous informe qu'un test (``testAboutIndex()``) a tourné avec une assertion (``assertEquals()``) et que le résultat est celui attendu. Tout va pour le mieux !

Essayez de changer la chaine ``About symblog`` pour ``Contact``, et lancez à nouveau le test. Vous verrez alors le test échouter car ``Contact`` n'est pas trouvé, et le ``assertEquals`` donne alors une erreur.

.. code-block:: bash

    1) Blogger\BlogBundle\Tests\Controller\PageControllerTest::testAboutIndex
    Failed asserting that <boolean:false> is true.

Remettez le test à ``About symblog`` avant d'avancer.

L'instance du  ``Crawler`` que nous avons utilisé nous permet de traverser les documents HTML ou XML, ce qui signifie qu'il ne va marcher que pour des réponses de ce type. Nous pouvons utiliser le ``Crawler`` pour traverser les réponses générées à l'aide de méthodes telles que  ``filter()``, ``first()``, ``last()``, et ``parents()``. Si vous avez déjà utilisé `jQuery <http://jquery.com/>`_ auparavant, vous ne serez pas perdu avec la classe ``Crawler``. Une liste complète des méthodes de traversée supportées par le ``Crawler`` se trouve dans le chapitre sur les `tests
<http://symfony.com/doc/current/book/testing.html#traversing>`_ du livre Symfony2. Nous allons en évoquer quelques uns en avançant.

Page d'accueil
~~~~~~~~~~~~~~

Bien que le test de la page ``A propos`` ait été très simple, il nous a permis de mettre en avant les principes de base des tests fonctionnels des pages du site :

 1. Créer le client
 2. Effectuer une requête sur une page
 3. Vérifier la réponse

C'est une présentation simple du processus, car il y a en fait plusieurs autres étapes qui peuvent s'ajouter, comme cliquer sur les liens, ou remplir et soumettre des formulaires.

Créons une méthode pour tester la page d'accueil. Nous savons que la page d'accueil est disponible via l'URL ``/`` et qu'elle doit afficher les derniers articles. Ajoutez une nouvelle méthode ``testIndex()`` à la classe ``PageControllerTest`` dans le fichier
``src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php``, avec le contenu suivant :

.. code-block:: php

    // src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php

    public function testIndex()
    {
        $client = static::createClient();

        $crawler = $client->request('GET', '/');

        // Check there are some blog entries on the page
        $this->assertTrue($crawler->filter('article.blog')->count() > 0);
    }

Vous pouvez voir que les mêmes étaptes ont lieu que pour les tests de la page ``A propos``. Lancez le test pour vérifier que tout fonctionne comme prévu.

.. code-block:: bash

    $ phpunit -c app/ src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php

Allons maintenant un peu plus loin dans les tests. Une partie des tests fonctionnels consiste à répliquer ce qu'un utilisateur pourrait faire sur le site. Les utilisateurs cliquent sur les liens pour naviguer entre les pages. Simulons maintenant cette action pour tester que les liens vers la page d'affichage des articles fonctionnent amènent bien vers la bonne page lorsque l'on clique sur les titres des articles.

Mettez à jour la méthode ``testIndex()`` de la classe ``PageControllerTest`` avec le contenu suivant :

.. code-block:: php

    // src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php

    public function testIndex()
    {
        // ..

        // Find the first link, get the title, ensure this is loaded on the next page
        $blogLink   = $crawler->filter('article.blog h2 a')->first();
        $blogTitle  = $blogLink->text();
        $crawler    = $client->click($blogLink->link());

        // Check the h2 has the blog title in it
        $this->assertEquals(1, $crawler->filter('h2:contains("' . $blogTitle .'")')->count());
    }

La première chose que nous faisons, c'est utiliser le ``Crawler`` pour extraitre le texte du premier lien vers un article, à l'aide du filtre ``article.blog h2 a``. Ce filtre est utilisé pour renvoyer un tag ``a`` dans un tag ``H2`` de la section ``article.blog``. Pour mieux comprendre cela, regardez le code HTML utilisé pour afficher les articles sur la page d'accueil.

.. code-block:: html

    <article class="blog">
        <div class="date"><time datetime="2011-09-05T21:06:19+01:00">Monday, September 5, 2011</time></div>
        <header>
            <h2><a href="/app_dev.php/1/a-day-with-symfony2">A day with Symfony2</a></h2>
        </header>

        <!-- .. -->
    </article>
    <article class="blog">
        <div class="date"><time datetime="2011-09-05T21:06:19+01:00">Monday, September 5, 2011</time></div>
        <header>
            <h2><a href="/app_dev.php/2/the-pool-on-the-roof-must-have-a-leak">The pool on the roof must have a leak</a></h2>
        </header>

        <!-- .. -->
    </article>

Vous pouvez voir le filtre ``article.blog h2 a`` en place dans le code de la page d'accueil. Vous pouvez également remarquer qu'il y a plus d'une section ``<article class="blog">`` dans le code, ce qui signifie que le filtre du ``Crawler`` va nous renvoyer une collection. Comme nous voulons seulement le premier lien, nous utilisons la méthode ``first()`` sur la collection. Nous utilisons enfin la méthode ``text()`` pour extraire le texte du lien, dans ce cas précis il s'agit de ``A day with Symfony2``. On clique ensuite sur le lien du titre de l'article pour naviguer vers la page d'affichage des articles. La méthode ``click()`` du client prend un objet lien en paramètre et renvoit la ``Response`` dans une instance de ``Crawler``. Vous devriez maintenant remarquer que l'objet ``Crawler`` est un élément essentiel des tests fonctionnels.

L'objet ``Crawler`` contient maintenant la réponse vers la page d'affichage des articles. Nous devons tester que la page vers laquelle nous avons navigué est bien la bonne. Nous pouvons utiliser la variable ``$blogTitle`` que nous avons récupéré auparavant pour vérifier le titre de la réponse.

Lancez les tests pour vérifier que la navigation entre la page d'accueil et la page d'affichage des articles fonctionne correctement.

.. code-block:: bash

    $ phpunit -c app/ src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php

Vous savez maintenant comment naviguer entre les pages du site à travers les tests fonctionnels. Apprenons maintenant à tester les formulaires.

Test de la page de contact
~~~~~~~~~~~~~~~~~~~~~~~~~~

Les utilisateurs de Symblog peuvent envoyer des demandes de contact en utilisant le formulaire sur la page de contacts ``http://symblog.dev/contact``. Testons que la soumission de ce formulaire marche comme il faut. Nous devons tout d'abord savoir ce qui doit arriver si le formulaire est correctement soumis, ce qui, dans le cas présent, doit arriver lorsqu'il n'y a pas d'erreurs dans le formulaire. Le processus est le suivant :

 1. Se rendre sur la page de contact
 2. Remplir les éléments du formulaire avec des valeurs
 3. Soumettre le formulaire
 4. Vérifier que l'email a été envoyé à symblog
 5. Vérifier que la réponse au client contient une notification de réussite d'envoi
 
Pour le moment, nous en savons assez pour faire seulement les étapes 1 et 5. Nous allons maintenant regarder comment faire les 3 étapes intermédiaires.

Ajoutez une nouvelle méthode ``testContact()`` à la classe ``PageControllerTest`` dans
``src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php``.

.. code-block:: php

    // src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php

    public function testContact()
    {
        $client = static::createClient();

        $crawler = $client->request('GET', '/contact');

        $this->assertEquals(1, $crawler->filter('h1:contains("Contact symblog")')->count());

        // Select based on button value, or id or name for buttons
        $form = $crawler->selectButton('Submit')->form();

        $form['blogger_blogbundle_enquirytype[name]']       = 'name';
        $form['blogger_blogbundle_enquirytype[email]']      = 'email@email.com';
        $form['blogger_blogbundle_enquirytype[subject]']    = 'Subject';
        $form['blogger_blogbundle_enquirytype[body]']       = 'The comment body must be at least 50 characters long as there is a validation constrain on the Enquiry entity';

        $crawler = $client->submit($form);

        $this->assertEquals(1, $crawler->filter('.blogger-notice:contains("Your contact enquiry was successfully sent. Thank you!")')->count());
    }

On commence de la manière habituelle, en effectuant une requête vers l'URL ``/contact``, et en vérifiant que la page contient le bon titre ``H1``. On utilise ensuite le ``Crawler`` pour choisir le bouton de soumission du formulaire. A partir du bouton, il est ensuite possible de retrouver le formulaire; on remplit ensuite les éléments du formulaire en utilisant la notation de tableau ``[]``. Le formulaire est ensuite fourni à la méthode du client ``submit()`` pour soumettre le formulaire. Comme d'hbaitude, on reçoit en retour une instance de ``Crawler``. On utilise cette réponse pour vérifier que le message flash est présent dans la réponse qui nous est renvoyée. Lancez le test pour vérifier que tout fonctionne correctement.

.. code-block:: bash

    $ phpunit -c app/ src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php

Le test échoue, PHPUnit nous donne la sortie suivante :

.. code-block:: bash

    1) Blogger\BlogBundle\Tests\Controller\PageControllerTest::testContact
    Failed asserting that <integer:0> matches expected <integer:1>.

    /var/www/html/symblog/symblog/src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php:53

    FAILURES!
    Tests: 3, Assertions: 5, Failures: 1.

La sortie nous informe que le message flash n'a pas pu être trouvé dans la réponse obtenue après soumission du formulaire. C'est parce que dans l'environnement de test, les redirections ne sont pas suivies. Lorsque le formulaire est effectivement validé dans la classe ``PageController``, une redirection a lieu, mais n'est pas suivie. Nous devons dire explicitement que la redirection doit être suivie. La raison en est simple : il est possible que l'on veuille vérifier la réponse actuelle d'abord. C'est ce que l'on verra bientôt pour vérifier que l'envoi de l'email a bien eu lieu. Mettez à jour la classe ``PageControllerTest`` pour forcer le client à suivre la redirection.

.. code-block:: php

    // src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php

    public function testContact()
    {
        // ..

        $crawler = $client->submit($form);

        // Need to follow redirect
        $crawler = $client->followRedirect();

        $this->assertEquals(1, $crawler->filter('.blogger-notice:contains("Your contact enquiry was successfully sent. Thank you!")')->count());
    }

Les tests devraient maintenant passer dans PHPUnit. Regardons maintenant l'étape finale du processus de vérification de soumission des formulaires, qui consiste à s'assurer qu'un email a bien été envoyé à Symblog. On sait déjà que les emails ne seront pas réellement envoyés dans l'environnement de test, grâce à la configuration de cet environnement :


.. code-block:: yaml

    # app/config/config_test.yml

    swiftmailer:
        disable_delivery: true

Il est néanmoins possible de vérifier que les emails sont envoyés en utilisant les informations rapportées par la barre d'outils de debug. C'est là où l'importance pour le client de ne pas suivre les redirections rentre en jeu. La vérification de l'outil de debug doit être réalisée avant que la redirection arrive, sinon les informations du profiler seraient perdues. Mettez à jour la méthode de test ``testContact()`` avec ce qui suit :

.. code-block:: php

    // src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php

    public function testContact()
    {
        // ..

        $crawler = $client->submit($form);

        // Check email has been sent
        if ($profile = $client->getProfile())
        {
            $swiftMailerProfiler = $profile->getCollector('swiftmailer');

            // Only 1 message should have been sent
            $this->assertEquals(1, $swiftMailerProfiler->getMessageCount());

            // Get the first message
            $messages = $swiftMailerProfiler->getMessages();
            $message  = array_shift($messages);

            $symblogEmail = $client->getContainer()->getParameter('blogger_blog.emails.contact_email');
            // Check message is being sent to correct address
            $this->assertArrayHasKey($symblogEmail, $message->getTo());
        }

        // Need to follow redirect
        $crawler = $client->followRedirect();

        $this->assertTrue($crawler->filter('.blogger-notice:contains("Your contact enquiry was successfully sent. Thank you!")')->count() > 0);
    }

Après la soumission du formulaire, on vérifie que le profiler est disponible, car il aurait pu être désactivé par un paramètre de l'environnement.

.. tip::

    Souvenez vous que les tests n'ont pas besoin d'être réalisés dans l'environnement de test. Ils pourraient très bien être lancés dans l'environnement de production, où des éléments tel que le profiler ne sont pas disponibles.

Si nous arrivons à récupérer le profiler, on effectue une requête pour obtenir l'espion de ``swiftmailer``. L'espion de ``swiftmailer`` se charge de surveiller, en arrière plan, comment le service d'emails est utilisé. On peut l'utiliser pour obtenir des informations sur quels emails ont été envoyés.

On utilise ensuite la méthode ``getMessageCount()`` pour vérifier qu'un email a été envoyé. C'est peut-être suffisant pour s'assurer qu'un email a bien été envoyé, mais cela ne vérifie pas vers où il est envoyé. Il peut être très embarassant, voire dangereux, que des emails soient envoyés à des adresses incorrectes. Afin de nous en assurer, on vérifie que l'adresse du destinataire est bien correcte.

Lancez maintenant le test pour vérifier que tout fonctionne correctement :

.. code-block:: bash

    $ phpunit -c app/ src/Blogger/BlogBundle/Tests/Controller/PageControllerTest.php

Test de l'ajout de commentaire d'articles
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Utilisons maintenant les connaissances que nous avons acquises dans les précédents tests de la page de contact pour tester le processus de soumission des commentaires pour un article. Voici ce qui doit arriver lorsqu'un commentaire est correctement soumis :

 1. Se rendre sur la page d'un article
 2. Remplir le formulaire de commentaire
 3. Soumettre le formulaire
 4. Vérifier que le nouveau commentaire est ajouté à la fin de la liste des commentaires
 5. Vérifier également dans la barre latérale que le commentaire ajouté est au sommet de la liste des derniers ccommentaires

Créez un nouveau fichier dans ``src/Blogger/BlogBundle/Tests/Controller/BlogControllerTest.php`` et ajoutez-y le code suivant :

.. code-block:: php

    <?php
    // src/Blogger/BlogBundle/Tests/Controller/BlogControllerTest.php

    namespace Blogger\BlogBundle\Tests\Controller;

    use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

    class BlogControllerTest extends WebTestCase
    {
        public function testAddBlogComment()
        {
            $client = static::createClient();

            $crawler = $client->request('GET', '/1/a-day-with-symfony');

            $this->assertEquals(1, $crawler->filter('h2:contains("A day with Symfony2")')->count());

            // Select based on button value, or id or name for buttons
            $form = $crawler->selectButton('Submit')->form();

            $crawler = $client->submit($form, array(
                'blogger_blogbundle_commenttype[user]'          => 'name',
                'blogger_blogbundle_commenttype[comment]'       => 'comment',
            ));

            // Il faut suivre la redirection
            $crawler = $client->followRedirect();

            // On vérifie que le comment s'affiche, et que c'est le dernier. Cela assure que les commentaires
            // vont du plus vieux au plus récent.
            $articleCrawler = $crawler->filter('section .previous-comments article')->last();

            $this->assertEquals('name', $articleCrawler->filter('header span.highlight')->text());
            $this->assertEquals('comment', $articleCrawler->filter('p')->last()->text());

            // On vérifie que la barre latérale affiche bien 10 derniers articles.

            $this->assertEquals(10, $crawler->filter('aside.sidebar section')->last()
                                            ->filter('article')->count()
            );

            $this->assertEquals('name', $crawler->filter('aside.sidebar section')->last()
                                                ->filter('article')->first()
                                                ->filter('header span.highlight')->text()
            );
        }
    }

Il s'agit cette fois-ci du code entier du test directement. Avant de dissecter ce code, lancez le test pour vérifier que tout fonctionne :

.. code-block:: bash

    $ phpunit -c app/ src/Blogger/BlogBundle/Tests/Controller/BlogControllerTest.php

    
PHPUnit devrait vous dire qu'un test a été correctement exécuté. En regardant le code de ``testAddBlogComment()``, on peut voir que les choses se passent de la manière habituelle : on crée un client, effectue des requêtes sur les pages et on vérifie qu'elles contiennent bien ce qu'il faut.
On continue alors par obtenir le formulaire d'ajout des commentaires et le soumet. Cette fois-ci, nous peuplons le formulaire d'une manière un peu différente de la précédente. Nous utilisons le 2nd argument de la méthode ``submit()`` pour fournir les valeurs du formulaire.

.. tip::

    Nous pourrions également utiliser l'interface objet pour remplir les champs du formulaire, comme dans les exemples suivantes :

    .. code-block:: php

        // Cocher une case à cocher
        $form['show_emal']->tick();
        
        // Choisir une option ou un élément radio
        $form['gender']->select('Male');

Après avoir soumis le formulaire, on effectue la redirection du client afin de pouvoir vérifier la réponse. On utilise à nouveau le ``Crawler`` afin d'obtenir le dernier commentaire de l'article, qui devrait être celui qui vient d'être soumis. On vérifie enfin que le premier élément de la liste des derniers commentaires de la barre latérale est bien le dernier commentaire proposé.

Dépôt d'articles
~~~~~~~~~~~~~~~~

Dans la dernière partie sur les tests fonctionnels de ce chapitre, nous allons regarder comment tester un dépôt Doctrine 2. Créez un fichier dans
``src/Blogger/BlogBundle/Tests/Repository/BlogRepositoryTest.php`` et ajoutez-y le contenu suivante.

.. code-block:: php

    <?php
    // src/Blogger/BlogBundle/Tests/Repository/BlogRepositoryTest.php

    namespace Blogger\BlogBundle\Tests\Repository;

    use Blogger\BlogBundle\Repository\BlogRepository;
    use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

    class BlogRepositoryTest extends WebTestCase
    {
        /**
         * @var \Blogger\BlogBundle\Repository\BlogRepository
         */
        private $blogRepository;

        public function setUp()
        {
            $kernel = static::createKernel();
            $kernel->boot();
            $this->blogRepository = $kernel->getContainer()
                                           ->get('doctrine.orm.entity_manager')
                                           ->getRepository('BloggerBlogBundle:Blog');
        }

        public function testGetTags()
        {
            $tags = $this->blogRepository->getTags();

            $this->assertTrue(count($tags) > 1);
            $this->assertContains('symblog', $tags);
        }

        public function testGetTagWeights()
        {
            $tagsWeight = $this->blogRepository->getTagWeights(
                array('php', 'code', 'code', 'symblog', 'blog')
            );

            $this->assertTrue(count($tagsWeight) > 1);

            // Test case where count is over max weight of 5
            $tagsWeight = $this->blogRepository->getTagWeights(
                array_fill(0, 10, 'php')
            );

            $this->assertTrue(count($tagsWeight) >= 1);

            // Test case with multiple counts over max weight of 5
            $tagsWeight = $this->blogRepository->getTagWeights(
                array_merge(array_fill(0, 10, 'php'), array_fill(0, 2, 'html'), array_fill(0, 6, 'js'))
            );

            $this->assertEquals(5, $tagsWeight['php']);
            $this->assertEquals(3, $tagsWeight['js']);
            $this->assertEquals(1, $tagsWeight['html']);

            // Test empty case
            $tagsWeight = $this->blogRepository->getTagWeights(array());

            $this->assertEmpty($tagsWeight);
        }
    }

Comme on veut effectuer des tests qui nécessitent une connection effective vers la base de donnée, il faut à nouveau étendre la classe ``WebTestCase``, car cela nous permet de démarrer le noyau Symfony2. Lancez le test sur ce fichier avec la commande suivante :

.. code-block:: bash

    $ phpunit -c app/ src/Blogger/BlogBundle/Tests/Repository/BlogRepositoryTest.php

Couverture de code
------------------

Avant d'avancer, quelques mots sur la couverture de code. Il s'agit d'un aperçu de quelles parties du code sont executées lorsque les tests sont lancés. Cela permet de savoir quelles sont les parties du code qui ne sont pas testées, et de déterminer s'il faut ou non écrire des test pour eux.

Afin d'obtenir l'analyse de couverture de code de votre application, lancez la commande suivante :

.. code-block:: bash

    $ phpunit --coverage-html ./phpunit-report -c app/

Cela donnera l'analyse de couverture de code dans le répertoire ``phpunit-report``. Lancez le fichier ``index.html`` dans votre navigateur pour obtenir les résultats de l'analyse.

Reportez vous au chapitre sur `l'analyse de la couverture de code <http://www.phpunit.de/manual/current/en/code-coverage-analysis.html>`_ de PHPUnit pour plus d'informations.

Conclusion
----------

Nous avons couvert un grand nombre d'aspects clés de la question des tests. Nous avons exploré à la fois les tests unitaires et fonctionnels pour nous assurer que notre site web fonctionne correctement. Nous avons vu comment manipuler les requêtes les requêtes du navigateur et comment utiliser la classe ``Crawler`` de Symfony2 pour vérifier les réponses de ces requêtes.

Dans le prochain chapitre, nous parlerons du composant de sécurité de Symfony2, et plus spécifiquement de comment s'en servir pour la gestion des utilisateurs. Nous intégrerons également FOSUserBundle, qui va nous permettre de travailler directement sur la section d'administration de Symblog.











