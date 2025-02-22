=====================================
Rule ``fully_qualified_strict_types``
=====================================

Removes the leading part of fully qualified symbol references if a given symbol
is imported or belongs to the current namespace. Fixes function arguments,
exceptions in ``catch`` block, ``extend`` and ``implements`` of classes and
interfaces.

Configuration
-------------

``import_symbols``
~~~~~~~~~~~~~~~~~~

Whether FQCNs found during analysis should be automatically imported.

Allowed types: ``bool``

Default value: ``false``

``leading_backslash_in_global_namespace``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Whether FQCN is prefixed with backslash when that FQCN is used in global
namespace context.

Allowed types: ``bool``

Default value: ``false``

Examples
--------

Example #1
~~~~~~~~~~

*Default* configuration.

.. code-block:: diff

   --- Original
   +++ New
    <?php

    use Foo\Bar;
    use Foo\Bar\Baz;
    use Foo\OtherClass;
    use Foo\SomeContract;
    use Foo\SomeException;

    /**
   - * @see \Foo\Bar\Baz
   + * @see Baz
     */
   -class SomeClass extends \Foo\OtherClass implements \Foo\SomeContract
   +class SomeClass extends OtherClass implements SomeContract
    {
        /**
   -     * @var \Foo\Bar\Baz
   +     * @var Baz
         */
        public $baz;

        /**
   -     * @param \Foo\Bar\Baz $baz
   +     * @param Baz $baz
         */
        public function __construct($baz) {
            $this->baz = $baz;
        }

        /**
   -     * @return \Foo\Bar\Baz
   +     * @return Baz
         */
        public function getBaz() {
            return $this->baz;
        }

   -    public function doX(\Foo\Bar $foo, \Exception $e): \Foo\Bar\Baz
   +    public function doX(Bar $foo, Exception $e): Baz
        {
            try {}
   -        catch (\Foo\SomeException $e) {}
   +        catch (SomeException $e) {}
        }
    }

Example #2
~~~~~~~~~~

With configuration: ``['leading_backslash_in_global_namespace' => true]``.

.. code-block:: diff

   --- Original
   +++ New
    <?php

    class SomeClass
    {
   -    public function doY(Foo\NotImported $u, \Foo\NotImported $v)
   +    public function doY(\Foo\NotImported $u, \Foo\NotImported $v)
        {
        }
    }

Example #3
~~~~~~~~~~

With configuration: ``['leading_backslash_in_global_namespace' => true]``.

.. code-block:: diff

   --- Original
   +++ New
    <?php
    namespace {
        use Foo\A;
        try {
            foo();
   -    } catch (\Exception|\Foo\A $e) {
   +    } catch (Exception|A $e) {
        }
    }
    namespace Foo\Bar {
   -    class SomeClass implements \Foo\Bar\Baz
   +    class SomeClass implements Baz
        {
        }
    }

Example #4
~~~~~~~~~~

With configuration: ``['import_symbols' => true]``.

.. code-block:: diff

   --- Original
   +++ New
    <?php

    namespace Foo\Test;
   +use Other\BaseClass;
   +use Other\CaughtThrowable;
   +use Other\FunctionArgument;
   +use Other\FunctionReturnType;
   +use Other\Interface1;
   +use Other\Interface2;
   +use Other\PropertyPhpDoc;
   +use Other\StaticFunctionCall;

   -class Foo extends \Other\BaseClass implements \Other\Interface1, \Other\Interface2
   +class Foo extends BaseClass implements Interface1, Interface2
    {
   -    /** @var \Other\PropertyPhpDoc */
   +    /** @var PropertyPhpDoc */
        private $array;
   -    public function __construct(\Other\FunctionArgument $arg) {}
   -    public function foo(): \Other\FunctionReturnType
   +    public function __construct(FunctionArgument $arg) {}
   +    public function foo(): FunctionReturnType
        {
            try {
   -            \Other\StaticFunctionCall::bar();
   -        } catch (\Other\CaughtThrowable $e) {}
   +            StaticFunctionCall::bar();
   +        } catch (CaughtThrowable $e) {}
        }
    }

Rule sets
---------

The rule is part of the following rule sets:

- `@PhpCsFixer <./../../ruleSets/PhpCsFixer.rst>`_
- `@Symfony <./../../ruleSets/Symfony.rst>`_

Source class
------------

`PhpCsFixer\\Fixer\\Import\\FullyQualifiedStrictTypesFixer <./../../../src/Fixer/Import/FullyQualifiedStrictTypesFixer.php>`_
