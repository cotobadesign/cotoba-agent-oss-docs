.. _aiml_pattern_matching:

Pattern Matching
=======================

AIML pattern matching provides wildcard matching for user input. 
There are ``*``, ``^`` , ``_`` and ``#`` wildcards, each of which is used differently.

``*`` Wildcard
-------------------------

``*`` can be used to extract user input words, and determines one or more matches.

.. code:: xml

    <pattern>Hello *</pattern>

In the example below, the grammar will match any sentence that starts with 'Hello' and 1 or more additional words.

::

    Hello!
    Hello, Mr. Yamada.
    Hello, who is this ?



``^`` Wildcard
-------------------------
``^`` can be used to specify zero or more word matches.
That is, even if there is no word corresponding to the wildcard, the evaluation result is obtained even if only the expression of the specified word matches.

.. code:: xml

    <pattern>Hello ^</pattern>


In the example below, the grammar will match any sentence that is either 'Hello' or a sentence that starts with 'Hello' and 1 or more additional words.


::

    Hello
    Hello!
    Hello, Mr. Yamada.
    Hello, who is this?



``_`` and ``#`` Wildcards
-------------------------------------
``*`` and ``^`` evaluate with a lower priority than word matching, i.e.

.. code:: xml

    <pattern> Hello, it's a nice day. </pattern>
    <pattern> Hello ^</pattern>
    <pattern> Hello * </pattern>

is defined, the input "Hello, it's a nice day." matches the pattern at the top.

In contrast, ``_`` and ``#`` are evaluated with higher precedence than word matching.
``_`` matches one or more matches, as in  ``*`` .
``#`` matches zero or more as in ``^`` .

.. code:: xml

    <pattern> Hello, it's a nice day. </pattern>
    <pattern> Hello _ </pattern>
    <pattern> Hello # </pattern>

is defined, if  the input is "Hello, it's a nice day. ", matches ``#`` .


Preferred Word
-------------------------
A word specified by ``$``  is evaluated before ``_`` , ``#`` .

.. code:: xml

    <pattern> Hello, $it's a nice day. </pattern>
    <pattern> Hello _ </pattern>
    <pattern> Hello # </pattern>

In case of above, the input "Hello, it's a nice day." matches the pattern at the top.


Decision priority
-------------------------
The priority when specifying a wildcard in the pattern is as follows.


-  ``$(Priority Word)``
-  ``#`` ( 0 or greater )
-  ``_`` ( 1 or more )
-  ``(word)``
-  ``^`` ( 0 or greater )
-  ``*`` ( 1 or more )
