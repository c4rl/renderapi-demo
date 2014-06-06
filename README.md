# Object-Oriented (Drupal) RenderAPI

## Installing

Via [Composer](http://getcomposer.org/):

    $> cd path/to/renderapi
    $> curl -sS https://getcomposer.org/installer | php
    $> php composer.phar install

## Getting started

* ./index.php Shows how some sample renderables are built.
* This project isn't yet in Drupal core, it's using a skeleton (FakeDrupal) to
  simulate some of Drupal's behavior and concepts using [Silex](http://silex.sensiolabs.org/)
* In ./index.php, uncomment the lines within `FakeDrupal::setEnabledModules()`
  and `FakeDrupal::setEnabledThemes()` to see module and theme overrides in
  action. Check out `colorado.module.php` for examples of how alter-like hooks
  can affect builders.
* Querystring parameters:
  * `rendervar` delegates the response to return JSON per an accessor-like syntax
    to represent the hierarchical tree of a given renderable. For example:
    `/node/123?rendervar=.` will return a JSON interpretation of the
    renderable, while `/node/123?rendervar=node.title` will just return the title
    of the node. (This does not yet distinguish sanitized variables).
  * `prepare` provided along with `rendervar` processes the RenderableBuilder as a
    Renderable preparing any relevant template variables per the final
    rendered state. For example: if a later preprocessor provides a `subtitle`
    variable to the node template, this can be available via
    `/node/123?rendervar=subtitle&prepare=1`.

## @todo

* Extract submodule, convert this project into new demo, submit to https://packagist.org
* Unit tests
* Drupal sandbox integration
* Front-end template example
* Benchmarking

## Things we need

* Logical template-based markup that can be extended and overridden.
   * `hook_theme()`, PHPTemplate (Twig), `template_preprocess()`, hook
     suggestions, theme registry.
* Abstracted, alterable structure.
   * Render arrays, `hook_node_view()`, `hook_page_alter()`
* Sensible, accessible, API.
   * `theme()`, `drupal_render()`, `render()`/`hide()`/`show()`

## Render workflow

* (1) Define renderable. `$build = new RenderableBuilder('ThemeFoo', array('foo' => $foo));`
* (2) Alter renderable. `$build->set('param', $myParam); $build->setThemeClass('ThemeBar');`
* (3) Render renderable. `$renderable = $build->create(); $renderable instanceOf ThemeBar;`
   * (A) Prepare/alter vars. `$renderable = new BazThemeBarDecorator($renderable);`
   * (B) Invoke theme engine & template. `print (string) $renderable;`

## Wishlist

"Make it faster. Don't break caching." -- Moshe

## Changelog

* 2014-05-30 Version 2.3 Clean-up structure, revise "Fake Drupal" integration,
  revise interfaces, addition of dependency-injected RenderManager and
  ThemeEngine.
* 2014-05-28 Version 2.2 Better code structure and autoloading
* 2013-10-08 Version 2.1 Accessor class and URL-based API to access for themed
  and non-themed structure.
* 2013-10-05 Drillable syntax via find() method to delegate from
  RenderableBuilder to Renderable.
* 2013-10-05 Weight support for RenderableBuilder.
* 2013-10-05 Silex and composer integration.
* 2013-09-25 Version 2.0 - After many moons, a complete rewrite via Builder and
  Decorator patterns.
* 2012-12-03 Version 1.1 - Somewhat looser coupling with HTML tags, still not
  really what we want.
* 2012-11-30 Version 1.0 - Basically an abstracted HTML builder. Not really what
  we want.

## Acknowledgements

* msonnabaum for OO reality checks
* jessebeach for realizing the connection to headless Drupal
