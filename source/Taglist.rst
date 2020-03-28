==============================
COTOBA AIML: Basic Elements
==============================

| This chapter describes the basic AIML elements supported by COTOBA AIML (cAIML).
| The [...] at the top of the section refers to the AIML version in which the element was originally defined.
| [custom] is an element of the cAIML proprietary extension.

For more information about the pattern matching precedence rules for cAIML, see :doc:`Pattern Matching <./rst/AIML-Pattern-Matching>`.

-  `aiml <#aiml>`__
-  `category <#category>`__
-  `pattern <#pattern>`__
-  `template <#template>`__
-  `topic <#topic>`__

aiml
=====================

[1.0]

| The aiml element is the root element for cAIML. All other cAIML elements must be written as the contents of the aiml element.

* Attribute

.. list-table::
    :widths: 20 20 5 55
    :header-rows: 1

    *
      + Patameter
      + Type
      + Required
      + Description

    *
      + version
      + String
      + Yes
      + Specifies the AIML version being described.


* Use Case

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <!-- cAIML element should be described here -->
    </aiml>


category
=====================

[1.0]

| The category element corresponds to the base unit of dialog rules in cAIML.
| The content of the category element consists of a  `pattern <#pattern>`__ element that specifies a matching pattern for the user's utterances and a `template <#template>`__ element that specifies the system's response statements to form a single dialog rule.
| The contents of the aiml element can contain blocks of category elements.
| All cAIML elements, except the `aiml <#aiml>`__ and `topic <#topic>`__ elements, must be contained within a block of category elements.

* Attribute

None

* Use Case

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>Hello.</pattern>
            <template>The weather is nice today, too.</template>
        </category>

	<category>
	    <pattern>Goodbye.</pattern>
	    <template>See you tomorrow.</template>
	</category>
    </aiml>

| Input: Hello.
| Output: The weather is nice today, too.
| Input: Goodbye.
| Output: See you tomorrow.

See also: `aiml <#aiml>`__, `topic <#topic>`__ , :doc:`pattern <./rst/Pattern_Tags>`, :doc:`template <./rst/Template_Tags>`

pattern
=====================

[1.0]

| The pattern element describes the contents of the category element and the content of the pattern element describes a pattern for pattern matching with the user's utterance.
| If the character string described within a pattern element and the character string of the user utterance are matched, a dialog rule (processing within a block of category elements) containing that pattern element is executed.

* Attribute

    None

* Use Case

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>Hello</pattern>
            <template>The weather is nice today, too.</template>
        </category>
    </aiml>

| A description including cAIML elements other than character strings can be done in the content of the pattern element.
| This allows complex pattern matching operations.
| For more information on cAIML elements that can be described as the contents of a pattern element, see :doc:`pattern element <./rst/Pattern_Tags>` .

template
=====================

[1.0]

| The template element is described as the content of the category element, and the content of the template element is the system response statement.
| When a dialog rule (Block category element) is executed, the string described in the content of the template element for the category element block is replied as the system response statement.

* Attribute

None

* Use Case

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>Hello</pattern>
            <template>The weather is nice today, too.</template>
        </category>
    </aiml>

| A description including cAIML elements other than character strings can be done in the content of the template element.
| This allows for complex answer sentence generation processing.
| For more information on cAIML elements that can be described in the contents of template elements, see :doc:`template elements <./rst/Template_Tags>`.

topic
=====================

[1.0]

| The dialog rules can be contextualized by describing multiple dialog rule `category <#category>`__ elements in the topic elements block.
| When a dialog rule is contextual, the dialog rule is evaluated only if the value of the topic variable held by the Dialog Engine matches the attribute value specified in the name attribute of the topic element.
| The dialog rules (non-contextual dialog rules) `category <#category>`__ elements that are not in the block for the topic element are treated in the same way as the attribute value of the name attribute were specified as a wildcard "*", and their dialog rules are evaluated regardless of the value of the topic variable, which is maintained by the Dialog Engine.
| However, the dialog rule that is not contextual is evaluated only if the dialog rule that is contextual in the topic element is evaluated first and the contextual dialog rule is not executed.

"topic" is a reserved word and cannot be used as a user-defined variable name.

* Attribute

.. list-table::
    :widths: 20 20 5 55
    :header-rows: 1

    *
      + Parameter
      + Type
      + Required
      + Description
    *
      + name
      + String
      + Yes
      + Specify the topic name.

| The topic element allows the matching behavior of the same  `pattern <#pattern>`__ to be used differently depending on the context (topic value), as shown in the example below.
| In this use case, the response to the user's "I don't put anything in." utterance is changed by switching the dialog rule to be evaluated for the user's "~" utterance is decided according to the topic value set before the utterance and the response corresponding to the rule is returned.

* Use Case

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>I like * . </pattern>
            <template>
                I also like <set name = "topic"><star /></set>.
            </template>
        </category>

        <topic name = "coffee">
            <category>
                <pattern>I like it black.</pattern>
                <template>I like it with cream and sugar.</template>
            </category>
        </topic>

        <topic name = "tea">
            <category>
                <pattern>I like it black.  </pattern>
                <template>I like it with lemon. </template>
            </category>
        </topic>
    </aiml>


| Input: I like coffee.
| Output: I also like coffee.
| Input: I like it black.
| Output: I like it with cream and sugar.
| Input: I like tea.
| Output: I also like tea.
| Input: I like it black.
| Output: I  like it with lemon.

See also: :ref:`that<pattern_that>`, :ref:`set<template_set>`, :ref:`think<template_think>`
