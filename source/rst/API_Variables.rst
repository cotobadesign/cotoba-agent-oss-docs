Using Variables in dialog API Data
=======================================

Summary
----------------------------------------
Describes how to use the data specified in the :ref:`dialog API<coversation_api>`

Using Variables in dialog API Data
----------------------------------------

The data provided by the dialog API is kept expanded to variables that can be used in the dialog scenario.
The retention period is until the response is returned to the client.To keep the content continously, assign a variable separately.
If the variable is unset during a dialog API request, 'None' is returned.

The variable names that hold the dialog API data are as follows.

.. csv-table::
    :header: "Item Name","Variable Name","Type","Description"
    :widths: 30,20,10,40

    "Locale","__USER_LOCALE__","string","The language code specified in the dialog API ``locale``. "
    "Time Information","__USER_TIME__","string","The time information specified in the dialog API ``time``. "
    "User ID","__USER_USERID__","string","The user ID specified in the dialog API ``userId`` ."
    "User Utterance","__USER_UTTERANCE__","string","The user utterance specified in the dialog API ``utterance`` ."
    "Metadata","__USER_METADATA__","string","Metadata specified in the dialog API  ``metadata`` . It is held as a string as a variable. Used as JSON data when handling with :doc:`json <JSON>` tag."


Usage Examples
----------------------------------------

If the dialog API data is given in the dialog API from the client,

..  code:: json

    {
        "locale": "en-US",
        "time": "2018-07-01T12:18:45+09:00",
        "userId": "E8BDF659B007ADA2C4841EA364E8A70308E03A71",
        "topic": "*",
        "utterance": "Hello",
        "metadata": {"arg1": "value1", "arg2": "value2"}
    }

The handling in the scenario is as follows.

.. code:: xml

    <aiml>
        <category>
            <pattern> Hello </pattern>
            <template>
                <get var="__USER_LOCALE__" /> ,
                <get var="__USER_TIME__" /> ,
                <get var="__USER_USERID__" /> ,
                <get var="__USER_UTTERANCE__" />
            </template>
        </category>
    </aiml>

| Input: Hello
| Output: en-US,2018-07-01T12:18:45+09:00,E8BDF659B007ADA2C4841EA364E8A70308E03A71,Hello

For information about metadata, see :doc:`metadata <Metadata>` .