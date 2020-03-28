====================
pattern element
====================

| This chapter describes child elements that can be described as the content of the pattern element.

| The list of pattern matching elements supported by cAIML is in below.
| It adds its own set of elements to the official AIML 2.x specification.


-  `bot <#bot>`__
-  `iset <#iset>`__
-  `nlu <#nlu>`__
-  `oneormore <#oneormore>`__
-  `priority <#priority>`__
-  `regex <#regex>`__
-  `set <#set>`__
-  `topic <#topic>`__
-  `that <#that>`__
-  `word <#word>`__
-  `zeroormore <#zeroormore>`__

| Most of the elements described here can use the variety of data that the Dialog Engine can access by specifying attributes and describing child elements as content.
| See :doc:`Pattern Matching <AIML-Pattern-Matching>`  for details on how to match patterns by writing pattern elements.
| The [...] at the beginning of each element indicates the AIML version in which the element was originally defined.

.. _pattern_bot:

bot
---------
[1.0]

| The bot element is used to call a custom bot property. These variables are accessible to all users accessing the bot.
| You can set custom bot properties in the properties.txt file.

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
      + yes
      + Specifies the bot property name.

* Use Case

The following use case returns the name of the bot. (Assuming properties.txt is set to 'name: Bot')

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>Are you <bot name="name" />? </pattern>
            <template>My name is <bot name="name" />. </template>
        </category>
    </aiml>

| Input: Are you Bot?
| Output: My name is Bot.

See also: :ref:`File Management：properties<storage_entity>`

iset
----------
[1.0]

The iset element is used to describe a small number of target words without using a sets file.

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
      + words
      + String
      + yes
      + Enter the words to be matched by separating them with commas.

* Use Case

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>I live in <iset words="Tokyo, Kanagawa, Chiba, Gunma, Saitama, Tochigi" />. </pattern>
            <template>
                I live in Kanto, too.
            </template>
        </category>
    </aiml>


| Input: I live in Tokyo.
| Output: I live in Kanto, too.

See also: `set <#set>`__

.. _pattarn_nlu :

nlu
----------
[custom]

| The nlu element is used to interact with the intent recognition results of user utterances by the intent recognition engine.
| See :doc:`NLU <NLU>` for more information on attributes, how to set child elements, and how to use the intent recognition engine.

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
      + intent
      + String
      + yes
      + Specifies the intent name to match.
    *
      + scoreGt
      + String
      + No
      + Specifies the confidence level to match. Matches if the confidence of the target intent is greater than the specified value.
    *
      + scoreGe
      + String
      + No
      + Specifies the confidence level to match. Matches if the subject intent's confidence is greater than or equal to the specified value.
    *
      + score
      + String
      + No
      + Specifies the confidence level to match. Matches the confidence of the target intent to the specified value.
    *
      + scoreLe
      + String
      + No
      + Specifies the confidence level to match. Matches if the subject intent's confidence is less than or equal to the specified value.
    *
      + scoreLt
      + String
      + No
      + Specifies the confidence level to match. Matches if the confidence of the target intent is less than the specified value.
    *
      + maxLikelihood
      + String
      + No
      + Specifies ``true`` or ``false``. Specifies whether the confidence of the subject must be the maximum likelihood. If  ``true`` , the target intent will match only at maximum likelihood. If ``false`` , matches even if the subject intent is not one of a maximum likelihood. If not specified, it is  ``true`` .

| The available intent names are limited to the range of intent names described in the learning data that creates the intent recognition model used by the intent recognition engine.
| Only one scoreXx can be specified. If there is more than one entry, use scoreGt, scoreGe, score, scoreLe, and scoreLt in that order. (If scoreGt and score are listed, scoreGt is adopted.)

* Use Case

Example of around search intent name set to 'aroundsearch' in intent recognition model.

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>
                <nlu intent="aroundsearch" />
            </pattern>
            <template>
                Around search.
            </template>
        </category>
    </aiml>


| Input: Look around this area.
| Output: Around search.

See also: :doc:`NLU <NLU>`

oneormore
---------------
[1.0]

This element is a wildcard that matches at least one arbitrary word.
If the wildcard is at the end of the description in the pattern element, it matches up to the end of the user's utterance.
Also, if the wildcard is between other AIML pattern matching elements in the pattern element description, match processing continues until the next pattern matching element in the wildcard is matched.

| There is a priority that matches are applied between this wildcard matching and the matching of other AIML pattern matching elements.
| Wildcards "_" are matched before the AIML pattern matching elements  ``set`` , ``iset`` , ``regex`` , and ``bot``.
| Wildcards "*" are matched after these AIML pattern matching elements.
| For more information on the pattern matching process, see :doc:`Pattern Matching <AIML-Pattern-Matching>` .

The following two use cases evaluate the match of 1 word "Hello" with one or more words that follow.

* Use Case

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>Hello _</pattern>
            <template>
                Hello
            </template>
       </category>
    </aiml>

| Input: Hello, the weather is nice.
| Output: Hello

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
       <category>
           <pattern>Hello *</pattern>
           <template>
               How are you?
           </template>
       </category>
    </aiml>

| Input: Hello, the weather is nice.
| Output: How are you?

See also: `zeroormore <#zeroormore>`__ , :doc:`Pattern Matching <AIML-Pattern-Matching>`

priority
--------------
[1.0]

| Describe patterns to match mapping words to ``wildcard``, ``set``, ``iset``, ``regex`` , ``bot`` and so on.
| In AIML, "$" is written at the beginning of a word to be matched in a pattern element to give priority to matching for that word over matching for other AIML pattern matching elements.
| In the following use case, even if a dialog rule contains a pattern element with wildcards "Hello *", "Hello * friend", and so on, it will match the word "fantastic" with "$" in preference.

* Use Case

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">

        <category>
            <pattern>Hello $fantastic day, today. </pattern>
            <template>
                That's right.
            </template>
        </category>

        <category>
	        <pattern>Hello * </pattern>
	        <template>
	            Hello.
	        </template>
	    </category>

	    <category>
	        <pattern>Hello * friend.</pattern>
	        <template>
	            Hi.
	        </template>
	    </category>

    </aiml>

| Input: Hello fantastic day, today.
| Output: That's right.
| Input: Hello fantastic.
| Output: Hello.
| Input: Hello fantastic friend.
| Output: Hi.

See also: `word <#word>`__, :doc:`Pattern Matching <AIML-Pattern-Matching>`

.. _pattern_regex:

regex
-----------------
[custom]

The use of regex elements allows pattern matching by regular expressions to user utterances.
It supports word-by-word regular expressions and regular expressions for strings.

* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "pattern","String","No","Describing Words with Regular Expressions"
    "template","String","No","Uses word-by-word regular expressions defined in the regex.txt file"
    "form","String","No","Write regular expressions for strings containing multiple words"

When describing a regex element, one of the following attributes is required: pattern, template, or form.

* Use Case

| There are three ways to specify a regular expression word for an attribute of a regex element.
| The first is to write a regular expression directly into pattern (word by word).
| One area where this is useful is in handling the difference between UK-English and American-English spelling. The first is by specifying the regular expression as the 'pattern' attribute as below

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>I did not <regex pattern="reali[z|s]e" />it was that.</pattern>
            <template>
                Did you realize it was that?
            </template>
        </category>
    </aiml>

| The second way is to use template. Specify the template name corresponding to the specified template file.
| Define with 'template_name.txt'.
| Regular expression template files allow you to use a regular expression in a template file that can be referenced by multiple dialog rules.
| The contents of the description are the same as the words specified in pattern.

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern><regex template="color" /></pattern>
            <template>
                color
            </template>
        </category>
    </aiml>

| Third, you can use form as an attribute to specify a regular expression for the string.

| The regular expression you specify for form is a combination of the following:

.. csv-table::
    :header: "Notation","Meaning"
    :widths: 10,70

    "'[...]'","Matches any character in '[...]'."
    "'A|B'","Matches either of the left or right strings of '|'."
    "'(X)'","A subpattern of the regular expression X. It is OK if it matches X."
    "'()?'","Matches or does not match the subpattern just before '?'."

The following example matches any of the following statements.

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>
                <regex form="I like analy[z|s]ing (football|soccer) matches " />
            </pattern>
            <template>
                That's nice.
            </template>
        </category>
    </aiml>

"I like analyzing football matches" "I like analysing football matches"
"I like analyzing soccer matches" "I like analysing soccer matches"

See also: :ref:`File Management：regex_templates<storage_entity>`


.. _pattern_set:

set
---------
[1.0]

| Specify the match processing with a word set.
| The words to be matched are listed in the sets file in a list format and stored under the directory specified by config.
| In the attribute name of the set, set a character string excluding the extension from the file name of the sets file.

* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "name","String","Yes","Character string excluding the extension from the sets file name"

* Use Case

The example below assumes that prefecture.txt contains the names of prefectures in Japan.

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>I live in <set name = "preference" />. </pattern>
            <template>
                I live in Tokyo.
            </template>
        </category>
   </aiml>

| Input: I live in Chiba.
| Output: I live in Tokyo.

See also: `iset <#iset>`__ , :ref:`File Management：sets<storage_entity>`

.. _pattern_topic:

topic
----------
[1.0]

Using the topic element, you can add a condition that the value of the system reserved variable topic matches the value specified for name. 
"topic" can specify a value using the set of template elements as follows.

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern><!-- pattern description goes here --></pattern>
            <template>
                <think><set name="topic">FISHING</set></think>
		<!-- response sentence goes here-->
            </template>
        </category>
    </aiml>

The condition specified in the topic element is evaluated before pattern matching.
For example, you can vary the response statement by subdividing the pattern matching process into conditional branches based on the currently set topic value.

In the example below,  for the user utterance "Why do you know that?",  the response sentence changes depending on whether the value of topic is set to "FISHING" or "COOKING" in the previous dialog.

* Use Case

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>Why do you know that? </pattern>
            <topic>FISHING</topic>
            <template>
                My father taught me when I was a child.
            </template>
        </category>

        <category>
            <pattern>Why do you know that? </pattern>
            <topic>COOKING</topic>
            <template>
                My mother taught me when I was a child.
            </template>
        </category>
    </aiml>

See also: `that <#that>`__, :ref:`set(template element)<template_set>`, :ref:`think<template_think>`

.. _pattern_that:

that
----------
[1.0]

By using the that element, you can add to the condition that the response of the system in the previous dialog matches the specified character string.
If the pattern element and that element are contained within the category element, the match process of the pattern element is done only if the last response in the previous response of the system matched the specified character string by the that element.
This function of the that element makes possible to write dialog rules considering the flow of dialog contents.
For example, A system response sentence to a general user utterance sentence such as "Yes" or "No" can be classified according to the content of the previous dialog.


* Use Case

In the example of specification below, depending on whether the system response sentence of the previous conversation was "Would you like sugar and milk in your coffee?" or "Would you like some lemon slices with your tea?",
the dialog rules that match the "Yes" and "No" of user utterances are classified so that system response sentences that match the contents of the previous dialog are returned.

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>I like coffee.</pattern>
            <template>Would you like sugar and milk in your coffee?</template>
        </category>

        <category>
            <pattern>I like tea.</pattern>
            <template>Would you like some lemon slices with your tea?</template>
        </category>

        <category>
            <pattern>Yes.</pattern>
            <that>Would you like sugar and milk in your coffee?</that>
            <template>Okay.</template>
        </category>

        <category>
            <pattern>No.</pattern>
            <that>Would you like sugar and milk in your coffee?</that>
            <template>I like black coffee.</template>
        </category>

        <category>
            <pattern>Yes.</pattern>
            <that>Would you like some lemon slices with your tea?</that>
            <template>Okay. </template>
        </category>

        <category>
            <pattern>No. </pattern>
            <that>Would you like some lemon slices with your tea ?</that>
            <template>I like black tea.</template>
        </category>
    </aiml>

関連項目: `topic <#topic>`__

word
----------
[1.0]

AIML's the most basic pattern matching element.
The word element represents a word and is used inside the dialog engine and cannot be described in a scenario.
For English words, match processing is not case-sensitive.
For double-byte and single-byte characters, matches are performed in single-byte for alphanumeric characters and in double-byte for kana characters.

In the example below, matches any of HELLO, hello, Hello, or HeLlO.

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>HELLO</pattern>
            <template>
                Hello
            </template>
        </category>
    </aiml>

See also: `priority <#priority>`__

zeroormore
----------------
[1.0]

This element is a wildcard that matches at least zero arbitrary words.
If the wildcard is at the end in the pattern element, it matches up to the end of the user's utterance.
Also, if the wildcard is between other AIML pattern matching elements in the pattern element description,
match processing continues until the next pattern matching element.

| There is a priority in which the matching processes are applied between this wildcard matching process and the matching process of other AIML pattern matching elements.
| The wildcard "^" is matched before the AIML pattern matching elements ``set`` , ``iset`` , ``regex`` and ``bot``, but the wildcard "#" is matched after these AIML pattern matching elements.


In the example below, the grammar will match any sentence that is either 'HELLO' or a sentence that starts with 'HELLO' and one or more additional words.

For more information, see :doc:`Pattern Matching <AIML-Pattern-Matching>`.

* Use Case

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>Hello ^ </pattern>
            <template>
                Hello
            </template>
        </category>

        <category>
            <pattern>Hello #</pattern>
            <template>
                How are you?
            </template>
        </category>
    </aiml>

See also: `oneormore <#oneormore>`__ , :doc:`Pattern Matching <AIML-Pattern-Matching>`
