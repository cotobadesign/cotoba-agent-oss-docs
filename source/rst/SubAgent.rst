SubAgent
=======================================

Summary
----------------------------------------

The subagent (SubAgent) is an external service invoked using the sraix element.
Describes how to invoke the external service specified by sraix.

There are three ways to call external services.

* Generic REST interface
    If "botId"/"service" is not specified in the attribute, the external service is invoked as a REST-API using the child elements of host, header, query, body, etc.
* Public bot calls on the dialog platform
    If the attribute is "botId", it calls the bot exposed on the dialog platform.
* Custom External Service Implementation
    If the attribute is "service", the custom implementation process is used to invoke external services.


* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "`Unspecified <#rest>`__","","No","Generic REST API calls with child elements."
    ":ref:`service<subagent_custom>`","String","No","The service name of the custom external service."
    "`botId <#cotoba-designbot>`__","String","No","The bot ID exposed on the dialog platform."
    "`host <#cotoba-designbot>`__","String","No","Dialog platform or host name."
    "`default <#default>`__","String","No","Response statement for external call failure."

| The body of the response from the SubAgent expands to the local variables (var) available in the scenario (AIML). Because AIML can only process text, it does not support binary data return values.
| If the response body contains JSON, the json tag can get the parameters inside JSON.
| The content expanded to the local variable (var) is valid only in the range of the AIML category, so it should be assigned to the global variable (name/data) separately when it is used in other categories (contains srai) continuously.
| Also, if the subagent is called multiple times within a category, the return value from the subagent may be overwritten, so assign the required response contents to variables individually.

| The "default" attribute can be specified in any of three ways to call an external service, and in the event where the call fails, returns the set contents as the return value.
| The "host" attribute is only available on the dialog platform and is used differently from the "host" of the child element.


Generic REST interface
----------------------------------------

If the attribute "botId"/"service" of sraix is unspecified, the external service is invoked as a REST-API using the child elements of host, header, query, body, etc.


Send
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Child element

.. csv-table::
    :header: "Tag","Required","Description"
    :widths: 10,5,75

    "host","Yes","The URL to connect to."
    "method","No","HTTP method. When not specified, GET is used. POST/PUT/DELETE are also supported."
    "query","No","Specify parameters for query with an associative array."
    "header","No","Specify keys and values for header with an associative array."
    "body","No","Specifies what is set for the body."


At the time of request, specify as follows.
Set a character string to body.

.. code:: xml

    <category>
        <pattern>XXX</pattern>
        <template>
            <think>
                <sraix>
                    <host>http://www.***.com/ask</host>
                    <method>POST</method>
                    <query>"userid":"1234567890","q":"question"</query>
                    <header>"Authorization":"yyyyyyyyyyyyyyyyy","Content-Type":"application/json;charset=UTF-8"</header>
                    <body>{"question":"Ask this question"}</body>
                </sraix>
            </think>
        </template>
    </category>

Sent

..  code:: json

    POST /ask?userid=1234567890&q=question HTTP/1.1
    Host: www.***.com
    Content-Type: application/json;charset=UTF-8
    Authorization: yyyyyyyyyyyyyyyyy

    {
        "question":"Ask this question"
    }

When specifying the metadata specified by the dialog API in the body, get __USER_METADATA__ in the json tag and set it in the child element "body".

.. code:: xml

    <category>
        <pattern>XXX</pattern>
        <template>
            <think>
                <sraix>
                    <host>http://somehost.com</host>
                    <method>POST</method>
                    <query>"userid":"1234567890","q":"question"</query>
                    <header>"Authorization":"yyyyyyyyyyyyyyyyy","Content-Type":"application/json;charset=UTF-8"</header>
                    <body><json var="__USER_METADATA__" /></body>
                </sraix>
            </think>
        </template>
    </category>


Receive
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| Returns the body contents of the receive result as the result of the sraix.
| Because AIML can only handle text, it does not support binary bodies.
| The receive results are also expanded to the local variable (var):  ``__SUBAGENT_BODY__`` .By specifying <get var = "__ SUBAGENT_BODY __"> in get, the string of the body can be obtained.
| The contents of local variables (var) are held in category units, so it should be assigned to the global variable (name/data) separately when you use the contents of responses continuously.
| Also, if the generic REST interface is called more than once within a category, the ``__SUBAGENT_BODY__`` is overwritten, so assign the required response to a variable.

If the body content is JSON, the :ref:`json<template_json>` tag can get parameters inside JSON.
The contents of the body is,

..  code:: json

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
            "facility": ["Rokuon-ji Temple", "Kiyomizu-dera Temple", "Fushimi Inari Taisha Shrine"]
        }
    }

, then

.. code:: xml

    <json var="__SUBAGENT_BODY__.transportation.station.departure" />
    <json var="__SUBAGENT_BODY__.facility" function="len" />
    <json var="__SUBAGENT_BODY__.facility"><index>1</index></json>

With the description, the internal information of the body can be obtained by JSON tag.


Response for communication failure (default)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In case of communication failure, the string specified by the "default" attribute is returned as the return value of sraix. 
The same is true for access to both the :ref:`custom external service implementations<subagent_custom>`.
The following is an example of using a bot call exposed on a dialog platform.

.. code:: xml

   <category>
       <pattern> bot status check * </pattern>
       <template>
           The status of <star /> is <sraix service = "sameBot" default = "communication failed"> <star /> </sraix>.
        </template>
   </category>

| Input:  bot status check public bot
| Output: The status of public bot is communication failed.


.. _subagent_cotoba_design_pf:

Public bot calls on dialog platforms
--------------------------------------------

When "botId" is specified in the attribute of sraix, bot (public Bot) published on the dialog platform is called.
The "botId" is the ID of the bot and specified by the dialog platform, and the content of sraix is sent as an input sentence (utterance sentence) to the public bot.
The content returned from the public bot is in JSON format specified in the received data of the :ref:`dialog API<coversation_api>` , and the return value of sraix returns the response element within it.


Send
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following example is for a public bot that returns "Ok" as a response.

.. code:: xml

   <category>
       <pattern> bot status check * </ pattern>
       <template>
           The status of <star /> is <sraix botId="sameBot"> <star /> </sraix>.
        </template>
   </category>

| Input: bot status check public bot
| Output: Public bot status is OK.


| Describe the parameters as chile elements when using the public bot The content of the child element is sent as the content of the body of the :ref:`dialog API<coversation_api>` .
| See :ref:`dialog API<coversation_api>` , about the meaning of child elements.
| If not specified, some elements inherit what is specified in the dialog API. If the element does not need to inherit anything, child elements must be configured (null string, etc.).
| (For sraix, if no user ID is specified, uses another ID generated from the user ID specified in the dialog API.)
| When using a public bot, sraix has no child elements to configure user utterances, and is treated as a utterance that informs the public bot of the contents of sraix.

* Child element

.. csv-table::
    :header: "Item","Tag Name","Type","Required","Inheritation from the dialog API"
    :widths: 30,30,20,20,60

    "Locale","locale","string","No","Yes"
    "Time information","time","string","No","Yes"
    "User ID","userId","string","Yes","No (Generate a different user ID)"
    "Topic ID","topic","string","No","No"
    "Delete Task Variable","deleteVariable","boolean","No","No"
    "Metadata","metadata","string","No","Yes"
    "Configure","config","","No","No"
    "","logLevel","string","No","No"


In the following example, topic, deleteVariable, metadata, and config are specified in a scenario and locale and time are specified by inheriting the contents of the coller's request.

.. code:: xml

    <category>
        <pattern> bot status check * </pattern>
        <template>
            <think>
                <json var="askSubagent.zip">222-0033</json>
                <json var="config.logLevel">debug</json>
            </think>
            <sraix botId="sameBot">
                <star/>
                <topic>*</topic>
                <deleteVariable>true</deleteVariable>
                <metadata><json var="askSubagent"/></metadata>
                <config><json var="config"/></config>
            </sraix>
        </template>
    </category>

| Input: bot status check zip search
| Output: Shin-Yokohama


Receive
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The "response" element in the body (JSON format) received from the public Bot will be set as the return value of sraix. 
In the following example, if the received data from sameBot is 

..  code:: json

    HTTP/1.1 200 Ok
    Content-Type: application/json;charset=UTF-8

    {
        "response": "Hello, it's nice weather today, too.",
        "topic": "greeting"
    }

then the following AIML results are,

.. code:: xml

   <category>
        <pattern>*</pattern>
        <template>
           <sraix botId="sameBot"><star/></sraix>
        </template>
   </category>

| Input: Hello
| Output: Hello, it's nice weather today, too.




Reception from a public Bot is expanded to the local variable (var) ``__SUBAGENT_EXTBOT__.botID`` and can be obtained with the get.
In addition, the variable is held in category units, so it must be assigned to the global variable (name/data) to use it continuously.

.. code:: xml

    <json var="__SUBAGENT_EXTBOT__.sameBot" />

Because the body content from the public bot is JSON, you can get the parameters inside JSON with :ref:`json<template_json>` tag. 
 If the ``metadata`` content is JSON, the JSON tag can also retrieve the parameters in  ``metadata`` .

If the metadata content is

.. code:: json

        "metadata":{"broadcaster":"OBS","title":"afternoon news"}

, then

.. code:: xml

    <json var="__SUBAGENT_EXTBOT__.sameBot.response" />
    <json var="__SUBAGENT_EXTBOT__.sameBot.utterance" />
    <json var="__SUBAGENT_EXTBOT__.sameBot.topic" />
    <json var="__SUBAGENT_EXTBOT__.sameBot.metadata" />
    <json var="__SUBAGENT_EXTBOT__.sameBot.metadata.broadcaster" />
    <json var="__SUBAGENT_EXTBOT__.sameBot.metadata.title" />

can get the return value from the public bot and the metadata information.


.. _subagent_custom:

Custom External Service Implementation
----------------------------------------

If the attribute is set to "service", then the custom implementation can be used to call external services. 
The custom external service inherits the following base class and is implemented individually for the calling method that requires implementation for each service (SubAgent) used.

.. code:: python

    programy.services.service.Service

The implementation of the processing class creates a class that inherits from the base class and implements a process that returns a result string as the ask_question() function, using the "question" argument corresponding to the utterance data. 
When linking with external service, REST communication function will be implemented in ask_question().

.. code:: python

    from programy.services.service import Service

    class StatusCheck(Service):
       __metaclass__ = ABCMeta

       def __init__(self, config: BrainServiceConfiguration):
           self._config = config

       @property
       def configuration(self):
           return self._config

       def load_additional_config(self, service_config):
           pass

       @abstractmethod
       def def ask_question(self, client_context, question: str):
           return "OK"


Then add the entry for the custom external service to the ``services`` section of the configuration definition: config.yaml so that it can be used as a service name for sraix.

.. code:: yaml

           myService:
               classname: programy.services.myService.StatusCheck
               url: http://myService.com/api/statuscheck


For use with AIML, specify the entry name of the custom external service in the sraix attribute "service", as in the following example. 
As the processing of the custom external service for sraix, load the class defined by the classname of the entry of the custom external service, and call the function: ask_question(). 
The return value of the function: ask_question() is the result of sraix.

.. code:: xml

   <category>
       <pattern>Status Check *</pattern>
       <template>
           The status of <star /> is <sraix service="myService"><star/></sraix>.
       </template>
   </category>

| Input: Status Check custom
| Output: The status of custom is OK.

Arguments and Return Values to Custom External Services
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Arguments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The sraix service="myService" is a custom external service call that treats the inside of a sraix element as an argument.
Argument definitions depend on the argument I/F of each custom external service and must be implemented for each service.
The following example uses an external service called myService and assumes four arguments.

.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <set var="text">
                    <sraix service="myService">
                        <star/>
                        <json var="__USER_METADATA__.arg1" />
                        <json var="__USER_METADATA__.arg2" />
                        <json var="__USER_METADATA__.arg3" />
                    </sraix>
                </set>
                <think>
                    <set name="departure"><json var="__SUBAGENT__.myService.transportation.station.departure" /></set>
                    <set name="arrival"><json var="__SUBAGENT__.myService.transportation.station.arrival" /></set>
                </think>
                Searches for <get name="departure"> through <get name="arrival">.
            </template>
        </category>
    </aiml>


Return Values
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The return value of the custom external service, that is, the return value of ask_question() function of the individual implementation, expands to the local variable (var)  ``__SUBAGENT__.service name `` . 
These variables are held in category units, so they must be assigned to global variables (name/data) when it is used continuously.

The format stored in the variable can be text or JSON, and the custom implementation cannot use binaries.

In the following example, the return value of the operation on myService is expanded to  ``__SUBAGENT__.myService`` , but its contents are in JSON format,

..  code:: json

    {
        "transportation": {
            "station": {
                "departure" :"Tokyo",
                "arrival: "Kyoto"
            },
            "time": {
                "departure": "2018/11/1 11:00",
                "arrival": "2018/11/1 13:30"
            },
            "facility: ["Rokuon-ji Temple", "Kiyomizu-dera Temple", "Fushimi Inari-taisha Shrine"]
        }
    }

,then

.. code:: xml

    <json var="__SUBAGENT__.myService.transportation.station.departure" />
    <json var="__SUBAGENT__.myService.transportation.station.arrival" />

As a, you can use the json tag to get internal information about the body.
``__SUBAGENT__.myService`` is text, it will be retrieved with the get tag.


See Also: :doc:`Metadata <Metadata>`, :doc:`Dialog API <../Api>`, :doc:`JSON <JSON>`
