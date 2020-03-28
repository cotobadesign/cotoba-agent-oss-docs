
COTOBA Agent dialog engine API
===================================

Dialog API
===============================
This API is used when the dialog engine is launched using the REST API class (programy.clients.restful.yadlan.sanic.client).
Send a request to the bot that includes the user's spoken text and get a response from the bot that includes the response text.


.. csv-table::
    :header: "Item","Content"
    :widths: 20,80

    "Protocol","HTTP"
    "Method","POST"

Request
-------------------------------

The contents of the dialog API request are as follows.



* Request Header

..  csv-table::
    :header: "Field Name","Value","Description"
    :widths: 20,30,50

    "Content-Type","application/json;charset=UTF-8","Specifies JSON as the content type and UTF8 as the character code."


* Request Body

.. csv-table::
    :header: "Item Name","Key","Type","Required","Description"
    :widths: 20,20,10,10,40

    "Locale","locale","string","No","Language specification: Specifies a hyphenated combination of country code ISO-3166 and language code ISO-639 . Examples: ja-JP, en-US, zh-CN. If not specified, it will operate in the language specified on the bot side."
    "Time Information","time","string","No","Requestor time. It is specified in ISO8601(RFC3339) format. Example: 2018 - 07 - 01T 12:18:45 + 09: 00. If not set, it operates at server time."
    "User ID","userId","string","Yes","Specifies a unique ID for each user."
    "Topic ID","topic","string","No","Specifies the scenario ID from the dialog scenario. If not specified, it works as topic owned by dialog engine."
    "User Utterance","utterance","string","Yes","The user's utterance."
    "Task Variables Deleting","deleteVariable","boolean","No","When set to true, the variables set by ``data`` attribute of the set/get in the AIML description will be deleted all at once. The variables set by name and var are not deleted."
    "Metadata","metadata","string","No","You can set either JSON format metadata or strings. If the data you set up is to be used in a dialog scenario, the dialog scenario must be written to use this metadata. See Using Variables in :doc:`Dialog API Data for more information<rst/API_Variables>`."
    "Config","config","","No","Settings for dialog engine operating conditions"
    "Log Level","logLevel","string","No","Sets the dialog log output level for the dialog engine. The setting values are as follows. none: Do not output the dialog log, error: Output a dialog log that is greater than or equal to the error level, warning: Output a dialog log that is greater than or equal to the warning level, info: Output a dialog log that is greater than or equal to the operation status, debug: Output a dialog log that is greater than or equal to the debug dialog log. The large/small relation of the dialog log output is none<error<warning<info<debug. If unspecified, the dialog log output level remains unchanged."

* Example of Request

..  code:: http

    POST /v1.0/ask HTTP/1.1
    Host: www.***.com
    Accept: */*
    Content-Type: application/json;charset=UTF-8
    
    {
        "locale": "en-US",
        "time": "2018-07-01T12:18:45+09:00",
        "userId": "E8BDF659B007ADA2C4841EA364E8A70308E03A71",
        "topic": "greeting",
        "utterance": "Hello.",
        "deleteVariable": false,
        "metadata": {"arg1":"value1","arg2":"value2"},
        "config": {"logLevel":"debug"}
    }

Response
-------------------------------
The body of the response to the dialog API request is in JSON format.
The response codes for dialog API requests are listed below.
It may also return response codes (HTTP status codes other than those listed below) that are not part of the dialog engine.
In this case, the contents of the response body are undefined.

* Response Code

.. csv-table::
    :header: "Code","Description"
    :widths: 20,80

    "200","Request successful."
    "400","Parameter error. The content of the request needs to be reviewed."
    "403","Permission error. The API key needs to be reviewed."
    "404","Specified bot-id does not exist."

* Response Header

..  csv-table::
    :header: "Field Name","Value","Description"
    :widths: 20,50,30

    "Content-Type","application/json;charset=UTF-8","Specify JSON as the content type and UTF8 as the character code."

* Response Body

.. csv-table::
    :header: "Item Name","Key","Type","Required","Description"
    :widths: 20,20,10,10,40

    "User Utterance","utterance","string","Yes","A user utterance that is processed inside the dialog engine. Returns the result of internal processing such as the character normalized from Full-width alphanumeric to Half-width alphanumeric and normalizing Half-width kana to Full-width Kana."
    "User ID","userId","string","Yes","Specifies a unique ID for each user. Same as the userId of the request."
    "Response","response","string","Yes","The response from the dialog engine. Returns a UTF8 string."
    "Topic Name","topic","string","Yes","The name of the current topic."
    "latency","latency","number","Yes","In-engine processing time. The processing time from the receipt of the request to the return of the response, in seconds. It is the processing time including pattern match processing, intent recognition processing, and SubAgent processing registered in the scenario."
    "Metadata","metadata","string","No","Either JSON format metadata or a string is set. The content of the metadata is specified by the description in the dialog scenario."

* Example of Response

..  code:: http

    HTTP/1.1 200 Ok
    Content-Type: application/json;charset=UTF-8

    {
        "response": "Hello, the weather is nice today, too.",
        "userId": "E8BDF659B007ADA2C4841EA364E8A70308E03A71",
        "topic": "greeting",
        "utterance": "Hello."
    }


An example in which the user states "Play next song" in the dialog scenario corresponding to music playback,
and the dialog scenario is written to set the playback instruction information in metadata.

..  code:: http

    HTTP/1.1 200 Ok
    Content-Type: application/json;charset=UTF-8

    {
        "response": "I will play the next song.",
        "userId": "E8BDF659B007ADA2C4841EA364E8A70308E03A71",
        "topic": "music_play"
        "utterance": "Hello."
        "metadata": {"play":"next"},
    }

.. _debug_api:

Debug API
================================
The debug API is an API for retrieving error information and dialog history information that occurred when registering an uploaded zipped archive dialog scenario file with the dialog engine.
You can get the dialog state including the past dialog.
You can also set (Change) the value of a global variable for use during dialog.

.. csv-table::
    :header: "Item","Content"
    :widths: 20,80

    "Protocol","HTTP"
    "Method","POST"

Request
-------------------------------
This is the content set in the debug API request.
Only pre-registered users can access  the debug API endpoint.

* Request Header

..  csv-table::
    :header: "Field Name","Value","Description"
    :widths: 20,50,30

    "x-dev-key","yyyyyyyyyyyyyyyyy","Specify the API key obtained by x-dev-key in `user-information <#user-information>`__ ."
    "Content-Type","application/json;charset=UTF-8","Specify JSON as the content type and UTF8 as the character code."

* Request Body

.. csv-table::
    :header: "Item Name","Key","Type","Required","Description"
    :widths: 20,20,10,10,40

    "User ID","userId","string","No","Specifies a unique ID for each user. If the user is unspecified or does not exist, the conversation and logs are not retrieved, only duplicates and errors are."
    "Variable List","variables","","No","Specifies information about a variable set a value in a list format. If the user ID is not specified, the variable list specification is disabled. If the user does not exist, the conversation information including the updated variable information can be obtained, but there is no dialog history."
    "Variable Type","type","string","No","Specifies the variable type. The type that can be specified is 'name' or 'data'. (Specify with key, value.)"
    "Variable Name","key","string","No","Specify the variable name to set the value. (Specify with type, value.)"
    "Value","value","string","No","Describe the value to be changed. (Specify with type, value.)"

* Example of Request

..  code:: http

    POST / HTTP/1.1
    Host: www.***.com
    Accept: */*
    x-dev-key: yyyyyyyyyyyyyyyyy

    Content-Type: application/json;charset=UTF-8

    {
        "userId": "E8BDF659B007ADA2C4841EA364E8A70308000000",
        "variables": [
            {
                "type": "name",
                "key": "name_variable",
                "value": "0"
            },
            {
                "type": "data",
                "key": "data_variable",
                "value": "1"
            },
            ：
            }
        ]
    }

Response
-------------------------------
The body of the response to the debug API request is in JSON format.
The response codes for debug API requests are listed below.
It may also return response codes (HTTP status codes other than those listed below) that are not part of the dialog engine.
In this case, the contents of the response body are undefined.

If variable list: variables is specified at the time of sending, information reflecting the variable setting is returned in the received data.

* Response Code

.. csv-table:: 
    :header: "Code","Description"
    :widths: 20,80

    "200","Request successful."
    "400","Parameter error. The request needs to be reviewed."
    "403","Permission error. The API key needs to be reviewed."
    "404","Specified bot-id does not exist."

* Response Header

..  csv-table::
    :header: "Field Name","Value","Description"
    :widths: 20,50,30

    "Content-Type","application/json;charset=UTF-8","Specify JSON as the content type and UTF8 as the character code."

* Response body

.. csv-table::
    :header: "Item Name","Key","Type","Required","Description"
    :widths: 20,20,10,10,40

    "Speech Content","conversations","json","Yes","Acquires the dialog history of the specified user."
    "Scenario error information","errors","json","Yes","Acquires the error details when registering the dialog scenario."
    "Scenario duplicate information","duplicates","json","Yes","Acquires pattern duplication when registering a dialog scenario."
    "Log Information","logs","json","Yes","Acquires the dialog log contents output by the log tag in the template tag during the most recent dialog processing."

* Example of Response

..  code::

    HTTP/1.1 200 Ok
    Content-Type: application/json;charset=UTF-8

    {
        "conversations": {
            "categories": 1251
            "client_context": {
                "botid": "bot",
                "brainid": "brain",
                "clientid": "yadlan",
                "depth": 0,
                "userid": "E8BDF659B007ADA2C4841EA364E8A70308E03A71"
            },
            "data_properties": {
                "data_variable": "1"
            },
            "exception": null,
            "max_histories": 100,
            "properties": {
                "topic": "daytime",
                "name_variable": "0"
            },
            "questions": [
                {
                    "data_properties": {},
                    "exception": null,
                    "name_properties": {
                        "topic": "daytime"
                    },
                    "sentences": [
                        {
                            "matched_node": {
                                "end_line": "92",
                                "file_name": "../storage/categories/basic.aiml",
                                "start_line": "78"
                    :
                :
            ]
        },
        "duplicates": [
            {
                "category": {
                    "end": "35",
                    "start": "21"
                },
                "description": "Dupicate grammar tree found [Hello]",
                "file": "../storage/categories/basic.aiml",
                "node": {
                    "column": "9",
                    "raw": "22"
                }
            }
        ],
        "errors": [
            {
                "category": {
                    "end": "None",
                    "start": "None"
                },
                "description": "Failed to load contents of AIML file : XML-Parser Exception [mismatched tag: line 238, column 25]",
                "file": "../storage/categories/ng.aiml",
                "node": {
                    "column": "0",
                    "raw": "0"
                },
                "node_name": null
            }
        ]
        "logs": [
            {
                "info": "(templete log-node) log message"
            }
        ]
    }
