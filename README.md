FeedBundle
===========

Version 1.2
Changes:
 * adapted for Symfony2.1.
 * support of composer.json

Version 1.1
Changes:
 * in the configuration you need to defined an url, not a route. (Maybe another website ? You can type what you want)
 * the method getFeedRoute become getFeedRoutes in the ItemInterface (it's an array of routes).
 For more informations, see the ItemInterface or the wiki.
 * for naming your feeds you must add |format| (where you want in the filename)


Version 1.0
Warning: finally the new version (1.1) make substantial changes, but not in the code logic.


Features
--------

 * can make atom and rss 2.0 feeds
 * support multiple feeds
 * support edit of feeds
 * extendable
 * On-the-fly and at-save feed generation

Installation
============

Add FeedBundle to your src/ dir
-------------------------------------

Through submodules:
~~~~~~~~~~~~~~~~~~~


::

    $ git submodule add git://github.com/Nek-/FeedBundle.git vendor/bundles/Nekland/Bundle/FeedBundle


Through composer:
~~~~~~~~~~~~~~~~~

::

    "require": {
        ...
        "nekland/feed-bundle": "*"
        ...
    }


Add the FOS namespace to your autoloader (if you don't use composer.phar)
-------------------------------------------------------------------------

::

    // app/autoload.php

    $loader->registerNamespaces(array(
        'Nekland' => __DIR__.'/../vendor/bundles',
        // your other namespaces
    );

Add FeedBundle to your application kernel (also for composer.phar)
------------------------------------------------------------------

::

    // app/AppKernel.php

    public function registerBundles()
    {
        return array(
            // ...
            new Nekland\Bundle\FeedBundle\NeklandFeedBundle(),
            // ...
        );
    }

Configuration
-------------

### app/config.yml

    nekland_feed:
        feeds:
            my_feed:
                class:        My\MyBundle\Entity\Post
                title:       'My fabulous posts'
                description: 'Here is my very fabulous posts'
                url:       'my_posts'
                language:    'fr'
                filename: "blog.|format|"
            my_feed2:
                class:        My\MyBundle\Entity\Comment
                title:       'My fabulous comments'
                description: 'Here is my very fabulous comments'
                url:       'my_posts_comments'
                language:    'fr'
                filename: "blog.|format|"

Optional (default values):

    nekland_feed:
        feeds:
            ...
        renderers:
            rss:
                id: nekland_feed.renderer.rss
        loaders:
            rss_file:
                id: nekland_feed.loader.rss_file

### Models

To use the NeklandFeedBundle, you must have class that implements the ItemInterface. In most of case,
you can do it with your entities/documents

    class Post implements ItemInterface
    {
    .....
    }

Basic usage
-----------
-----------

### Retrieve your feed instance

    $container->get('nekland_feed.factory')->get('my_feed');

If your controller extends the base Symfony controller, you can use

    $this->get('nekland_feed.factory')->get('my_feed');


### Render the feed

    $factory->render('my_feed', 'renderer');

### Add an item

    /** @var $post My\MyBundle\Entity\Post */
    $factory->load('my_feed', 'loader');
    $factory->get('my_feed')->add($post);

### Read more at the wiki

https://github.com/Nek-/FeedBundle/wiki

Tests
-----

NeklandFeedBundle is bundled with some behat flavoured tests. Install BehatBundle and launch it with

    app/console -e=test behat @NeklandFeedBundle

TODO
----

 * Annotation configuration

Author :
-------------
 * Nek <nek.dev+github@gmail.com> ( http://twitter.com/#!/Nekdev )

Contributors :
-------------

 * Yohan Giarelli <yohan@giarelli.org>
 * remontees <remontees@free.fr>
