NLU
============================

| The definition of the pattern element to perform intent recognition processing.
| If ``nlu`` is defined as a child element of pattern, it is evaluated for matching the intent of the intent recognition.
| In the dialog control, the rule base intention interpretation is carried out according to the description of the scenario, and the response is returned according to the result of the evaluation by pattern matching. If there is no matching pattern, it performs dialog control using the intent result of advanced intent interpretation.
| This is to give priority to what the scenario developer describes rather than the result of intent recognition.
| As an exception, if there is a category in which only wildcards are described as patterns, the matching process is performed after not matching both the scenario description matching and the intent recognition matching.
| Even if you define nlu for a child element, the contents of the pattern element do the usual pattern evaluation.
| For nlu element attributes, see :ref:`nlu<pattarn_nlu>` .

.. _nlu_json_example:

Basic usage
-----------------------------

The following example shows the return of intent and slot information in the following format from the intent recognition.

.. code:: json

    {
        "intents": [
            {"intent": "transportation", "score": 0.9 },
            {"intent": "aroundsearch", "score": 0.8 }
        ],
        "slots": [
            {"slot": "departure", "entity": "Tokyo", "score": 0.85, "startOffset": 3, "endOffset": 5 },
            {"slot": "arrival", "entity": "Kyoto", "score": 0.86, "startOffset": 8, "endOffset": 10 },
            {"slot": "departure_time", "entity": "2018/11/1 19:00", "score": 0.87, "startOffset": 12, "endOffset": 14 },
            {"slot": "arrival_time", "entity": "2018/11/1 11:00", "score": 0.88, "startOffset": 13, "endOffset": 18 }
        ]
    }

Intent Matching
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| Describes nlu as a child element of pattern in AIML.
| In the example below, the intent returned from the intent recognition is 'transportation', it will match and return a response.

.. code:: xml

    <category>
        <pattern>
            <nlu intent="transportation" />
        </pattern>
        <template>
            Is it transfer information?
        </template>
    </category>

| Input: I want to go from Tokyo to Kyoto.
| Output: Is it transfer information?


Pattern that is a candidate intent but does not match
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| In the following example, the intent for intent recognition is 'aroundsearch'.
| 'aroundsearch' is a candidate intent, but it is not a maximum likelihood candidate, so it does not match.

.. code:: xml

    <category>
        <pattern>
            <nlu intent="aroundsearch" />
        </pattern>
        <template>
            Around search?
        </template>
    </category>

| Input:  I want to go from Tokyo to Kyoto.
| Output: NO_MATCH


Matching when it is not a maximum likelihood candidate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| Set the attribute  ``maxLikelihood``  to  ``false``  if you want to match the intent including 'aroundsearch' even if 'aroundsearch' is not a maximum likelihood candidate.
| If ``maxLikelihood``  is not specified, the behavior is the same as specifying ``true`` .

.. code:: xml

    <category>
        <pattern>
            <nlu intent="aroundsearch" maxLikelihood="false" />
        </pattern>
        <template>
            Around search?
        </template>
    </category>

| Input:  I want to go from Tokyo to Kyoto.
| Output:  Around search?

Match with score specification
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| Describes the match condition by the score value of the intent.
| As an attribute, five conditions of scoreGt, scoreGe, score, scoreLe and scoreLt can be specified, and they have the following meanings.
| Also, if this attribute is specified, ``maxLikelihood`` is treated as ``false`` for reliability comparison matching.

.. csv-table::
    :header: "Parameter Name","Meaning","Description"
    :widths: 10,10,75

    "scoreGt",">","Matches if the confidence level of the target intent is greater than the specified value."
    "scoreGe",">=","Matches if the confidence level of the target intent is greater than or equal to the specified value."
    "score","=","Matches if the confidence level of the target intent is equal to the specified value."
    "scoreLe","<=","Matches if the confidence level of the target intent is less than or equal to the specified value."
    "scoreLt","<","Matches if the confidence level of the target intent is less than the specified value."

The operation when scoreXx is specified is the following matching.

.. code:: xml

    <nlu intent="transportation" scoreGt="0.9"/>  Do not match transportation.
    <nlu intent="transportation" scoreGe="0.9"/>  Matching transportation.
    <nlu intent="transportation" score="0.9"/>    Matching transportation.
    <nlu intent="aroundsearch" scoreLe="0.8"/>  Matching aroundsearch.
    <nlu intent="aroundsearch" scoreLt="0.8"/>  Do not match aroundsearch.

| In addition, as shown in the following example, a description in which multiple conditions are satisfied depending on the result of intent recognition is possible, but a description in which category is described in the earlier order in the AIML file is applied.
| When multiple AIML files are used, the AIML expansion process is performed in ascending order by directory name and file name, so the order must be kept in mind.
| (If a subdirectory is used, the files in the upper directory are processed before moving to the file processing under the subdirectory.)

.. code:: xml

    <category>
        <pattern><nlu intent="transportation" scoreGe="0.8"/></pattern>
        <template> Is it transfer information?</template>
    </category>

    <category>
        <pattern><nlu intent="aroundsearch" scoreGe="0.8"/></pattern>
        <template> Around search?</template>
    </category>


Intent matches and wildcards
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following is an example of calling a chat subagent if it does not match the rule base or intent recognition result.
If there is a category with only wildcards as pattern, it will match after matching both the scenario description and the intent recognition.

.. code:: xml

    <aiml>
        <category>
            <pattern>Hello </pattern>
            <template>Hello </template>
        </category>

        <category>
            <pattern><nlu intent="aroundsearch" /></pattern>
            <template>
                Around search.
            </template>
        </category>

        <category>
            <pattern>
                *
            </pattern>
            <template>
                <sraix service="chatting"><get var="__USER_UTTERANCE__" /></sraix>
            </template>
        </category>
    </aiml>

| Input: Hello
| Output: Hello
| Input: Convenience stores around here
| Output: Around search.
| Input: chatting
| Output:  It's the result of a chatting.


Getting NLU Data
-----------------------------

The NLU data is expanded into the variable ``__SYSTEM_NLUDATA__`` .
Here is an example of using a JSON element to get data for  :ref:`an example of a intent recognition result<nlu_json_example>` .

.. code:: xml

    <category>
        <pattern>
            <nlu intent="transportation" />
        </pattern>
        <template>
            <think>
                    <set var="slot"><json var="__SYSTEM_NLUDATA__.slots"><index>1</index></json></set>
                    <set var="entity"><json var="slot.entity" /></set>
                    <set var="score"><json var="slot.score" /></set>
            </think>
            <get var="entity"/> has a score of <get var="score" />.
        </template>
    </category>

| Input:  I want to go from Tokyo to Kyoto.
| Output: Tokyo has a score of 0.85.

See also: :ref:`nlu<pattarn_nlu>`、 :doc:`JSON element <JSON>`


.. _nlu_intent_example:

Getting NLU Intent
-----------------------------

Use :ref:`nluintent<template_nluintent>` to get the contents of an intent with template. 
The NLU processing result explains how to get intent information on the assumption that the following result is obtained.

.. code:: json

    {
        "intents": [
            {"intent": "restaurantsearch", "score": 0.9 },
            {"intent": "aroundsearch", "score": 0.4 }
        ],
        "slots": [
            {"slot": "genre", "entity": "Italian", "score": 0.95, "startOffset": 0, "endOffset": 5 },
            {"slot": "genre", "entity": "French", "score": 0.86, "startOffset": 7, "endOffset": 10 },
            {"slot": "genre", "entity": "Chinese", "score": 0.75, "startOffset": 12, "endOffset": 14 }
        ]
    }

This example gets the intent information processed by the NLU. The map is supposed to be defined to count up values.
Keep the intent count in the intentCount and get the intent name and score for each slot until variable count equals the intentCount.


.. code:: xml

    <category>
        <pattern>
            <nlu intent="restaurantsearch"/>
        </pattern>
        <template>
            <think>
              <set var="count">0</set>
              <set var="intentCount"><nluintent name="*" item="count" /></set>
            </think>
            <condition>
                <li var="count"><value><get var="intentCount" /></value></li>
                <li>
                    intent:<nluintent name="*" item="intent"><index><get var="count" /></index></nluintent>
                    score:<nluintent name="*" item="score"><index><get var="count" /></index></nluintent>
                    <think>
                        <set var="count"><map name="upcount"><get var="count" /></map></set>
                    </think>
                    <loop/>
                </li>
            </condition>
        </template>
    </category>

| Input: Look for Italian, French or Chinese.
| Output: intent:restaurantsearch score:0.9 intent:aroundsearch score:0.4


See also: :ref:`nluintent<template_nluintent>`


.. _nlu_slot_example:

Getting NLU Slots
-----------------------------

Use :ref:`nluslot<template_nluslot>` to get the contents of the slot resulting from NLU processing with template.
The NLU processing result explains how to get slot information on the assumption that the following result is obtained.

.. code:: json

    {
        "intents": [
            {"intent": "restaurantsearch", "score": 0.9 },
            {"intent": "aroundsearch", "score": 0.4 }
        ],
        "slots": [
            {"slot": "genre", "entity": "Italian", "score": 0.95, "startOffset": 0, "endOffset": 5 },
            {"slot": "genre", "entity": "French", "score": 0.86, "startOffset": 7, "endOffset": 10 },
            {"slot": "genre", "entity": "Chinese", "score": 0.75, "startOffset": 12, "endOffset": 14 }
        ]
    }

This example gets the slot information processed by the NLU. The map is supposed to be defined to count up values.
Keep the slot count in the slotCount and get the slot name, entity and score for each slot until variable count equals the slotCount.

.. code:: xml

    <category>
        <pattern>
            <nlu intent="restaurantsearch" />
        </pattern>
        <template>
            <think>
              <set var="count">0</set>
              <set var="slotCount"><nluslot name="*" item="count" /></set>
            </think>
            <condition>
                <li var="count"><value><get var="slotCount" /></value></li>
                <li>
                    slot:<nluslot name="*" item="slot"><index><get var="count" /></index></nluslot>
                    entity:<nluslot name="*" item="entity"><index><get var="count" /></index></nluslot>
                    score:<nluslot name="*" item="score"><index><get var="count" /></index></nluslot>
                    <think>
                        <set var="count"><map name="upcount"><get var="count" /></map></set>
                    </think>
                    <loop/>
                </li>
            </condition>
        </template>
    </category>

| Input: Look for Italian, French or Chinese.
| Output: slot:genre entity:Italian  score:0.95 slot:genre entity:French  score:0.86 slot:genre entity:Chinese  score:0.75 

See also: :ref:`nluslot<template_nluslot>`
