metadata
=======================================

Summary
----------------------------------------
| The metadata information set in the body of the dialog API request can be used as variable data (Text or JSON) for the scenario.
| It can be also set to metadata information in the body of a dialog API response as variable data for a scenario.

Using metadata set in the dialog API
----------------------------------------

| Retains metadata information specified as JSON elements in the dialog API request expanded into variables available in the dialog scenario.
| The ``metadata`` element in JSON passed from the API expands to the variable name ``__USER_METADATA__``  .
| If the metadata element is in JSON format, you can get the element in __USER_METADATA__ by using the json :ref:`json<template_json>`  tag.
| The contents of __USER_METADATA__ remain in effect until the dialog API response is returned. To use the contents of __USER_METADATA__ continuously, assign it to a variable separately.


How metadata is expanded into variables
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| Expands the data passed as metadata from the dialog API into the variable __USER_METADATA__.
| You can set up metadata with text data and JSON data. How to handle each scenario is explained in the following.

| In the following description, information is obtained using the myService subagent, and the following results are returned as JSON data.
| The returned data is expanded into the variable ``__SUBAGENT__.myService``. For more information about subagent federation, see :doc:`SubAgent <SubAgent>` .

.. code:: json

    {
        "transportation": {
            "station": {
                "departure": "Tokyo",
                "arrival": "Kyoto"
            },
            "time": {
                "departure": "11/1/2018 11:00",
                "arrival": "11/1/2018 13:30"
            },
            "facility": [" Rokuon-ji Temple "," Kiyomizu-dera Temple "," Fushimi Inari-taisha Shrine "]
        }
    }



How to Treat Text Data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For text data, you can get the contents with the :ref:`get<template_get>`  tag for the variable __USER_METADATA__.

Description for the case where a string is given as metadata in a dialog API call is shown below.

..  code:: json

    {
        "locale": "en-US",
        "time": "2018-07-01T12:18:45+09:00",
        "topic": "*",
        "utterance": "subagent Hello",
        "metadata": "Metadata Test"
    }

| The scenario is described as follows.
| You can pass the contents of __USER_METADATA__ to the myService subagent by getting the contents with the  :ref:`get<template_get>`  tag and setting it as the contents of the  :ref:`sraix<template_sraix>`  tag.
| The subagent's return data is held in  ``__SUBAGENT__.myService``  (See  :doc:`SubAgent <SubAgent>`  for more details), which gets the value in JSON by specifying the element's key.

.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <think>
                    <sraix service="myService">
                        <star />
                        <get var="__USER_METADATA__" />
                    </sraix>
                    <set name="departure"><json var="__SUBAGENT__.myService.transportation.station.departure" /></set>
                    <set name="arrival"><json var="__SUBAGENT__.myService.transportation.station.arrival" /></set>
                </think>
                Searches for the route from<get name="departure" /> to <get name="arrival" />.
            </template>
        </category>
    </aiml>

If the user utterance is "subagent Hello", the data developed below is passed to the subagent.

.. csv-table::
    :header: "argument number","contents passed to the subagent"
    :widths: 20,80

    "The first argument","Hello"
    "The second argument","Metadata Test"


How to treat it as JSON data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following describes the case where JSON data is provided as metadata in a dialog API call.

..  code:: json

    {
        "locale": "en-US",
        "time": "2018-07-01T12:18:45+09:00",
        "topic": "*",
        "utterance": "subagent Hello",
        "metadata": {"arg1": "value1", "arg2": "value2", "arg3": "value3"}
    }

| If metadata is JSON, it can be handled as JSON format data by using :ref:`json<template_json>` tag.
| If you want to get a JSON value and set it individually, you can pass it to the subagent by keying it in the :ref:`json<template_json>` tag and setting it as the content of the  :ref:`sraix tag<template_sraix>`.

.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <think>
                    <sraix service="myService">
                        <star />
                        <json var="__USER_METADATA__.arg1" />
                        <json var="__USER_METADATA__.arg2" />
                        <json var="__USER_METADATA__.arg3" />
                    </sraix>
                    <set name="departure"><json var="__SUBAGENT__.myService.transportation.station.departure" /></set>
                    <set name="arrival"><json var="__SUBAGENT__.myService.transportation.station.arrival" /></set>
                </think>
                Searches for the route from <get name="departure" /> to <get name="arrival" />.
            </template>
        </category>
    </aiml>


If the user utterance is "subagent Hello", the data developed below is passed to the subagent.

.. csv-table::
    :header: "Argument number","Contents passed to the subagent"
    :widths: 20,80

    "The first argument","Hello"
    "The second argument","value1"
    "The third argument","value2"
    "The fourth argument","value3"


How to Pass All Metadata to Subagent
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| JSON data passed as metadata from the dialog API can be passed directly to the subagent as JSON.
| By specifying __USER_METADATA__ for the attribute of the :ref:`json<template_json>` tag, all data set to metadata is got and passed to the subagent.


.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <think>
                    <sraix service="myService">
                        <star />
                        <json var="__USER_METADATA__" />
                    </sraix>
                    <set name=departure><json var="__SUBAGENT__.myService.transportation.station.departure" /></set>
                    <set name=arrival><json var="__SUBAGENT__.myService.transportation.station.arrival" /></set>
                </think>
                Searches for the route from <get name = 'departure'> to <get name = 'arrival'>.
            </template>
        </category>
    </aiml>


If the user utterance is "subagent hello", the JSON specified in the second argument to the myService subagent is passed as is.

.. csv-table::
    :header: "Argument number","Contents passed to the subagent"
    :widths: 20,80

    "Argument number","Hello"
    "The second argument","{'arg1': 'value1', 'arg2': 'value2', 'arg3': 'value3'}"



Setting Metadata to Return to the dialog API
-----------------------------------------------

| Specify the metadata element to be set in the dialog API response by setting the data in the scenario return metadata variable __SYSTEM_METADATA__.
| The metadata element of the response can be set either text data orJSON data, and in the following how to handle it in each scenario is shown.


Handling as text data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| In the example below, the text "departure" in the data obtained from the myService subagent is set to the return metadata variable.
| From the JSON returned by the subagent, get the element (Text) of  Departure: "station.departure" and set it to __SYSTEM_METADATA__.
| This returns the text data as a metadata element in the dialog API response.

.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <think>
                    <sraix service="myService">
                        <star />
                    </sraix>
                    <set var="__SYSTEM_METADATA__"><json var="__SUBAGENT__.myService.transportation.station.departure" /></set>
                </think>
                The departure in the metadata is set.
            </template>
        </category>
    </aiml>

| Input: subagent Tokyo
| Output: The departure in the metadata is set.
| Metadata content: "Tokyo"



Handling JSON Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| In the example below, JSON data obtained from the myService subagent is set to the returned metadata variable.
| The :ref:`json<template_json>`  tag sets the entire JSON data returned from the subagent to __SYSTEM_METADATA__.
| This returns the JSON data as a metadata element in the dialog API response.


.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <think>
                    <sraix service="myService">
                        <star />
                    </sraix>
                    <set var="__SYSTEM_METADATA__"><json var="__SUBAGENT__.myService" /></set>
                </think>
                The sub agent processing result is set to the metadata.
            </template>
        </category>
    </aiml>

| Input: subagent Tokyo
| Output: The sub agent processing result is set to the metadata.
| metadata  content:
        {"transportation":
            {"station":
                {"departure": "Tokyo",
                 "arrival": "Kyoto"},
            "time":
                {"departure": "2018/11/1 11:00",
                 "arrival": "2018/11/1 13:30"},
            "facility: ["Rokuon-ji Temple", "Kiyomizu-dera Temple", " Fushimi Inari-taisha Shrine"]}}

See also: :doc:`dialogAPI <../Api>`, :doc:`dialog API data variable usage <API_Variables>`, :doc:`JSON <JSON>`, :doc:`SubAgent <SubAgent>`
