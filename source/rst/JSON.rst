JSON
============================

This is a function for using JSON in AIML.
Used to leverage JSON data in AIML, including SubAgent, metadata, and intent recognition results.

The variable name specified in name/var/data is the variable name defined in get/set.
get/set var is a local variable, get/set name is a global variable, and get/set data is a global variable that will be hold until deleteVariable is set true.
In addition, system fixed variable names such as metadata and subagent return values can be used as name.


* Attribute

.. csv-table::
    :header: "Parameter","Setting Value","Type","Required","Description"
    :widths: 10,10,10,5,65

    "name","","JSON name","Yes","Specifies the JSON that performs parse. var, name, or data must be set."
    "var","","JSON name","Yes","Specifies the JSON that performs parse. var, name, or data must be set."
    "data","","JSON name","Yes","Specifies the JSON that performs parse. var, name, or data must be set."
    "key","","Key specification","No","Specifies the key to manipulate the JSON data."
    "item","","Get key name ","No","Used to get keys from the JSON data. Specify this attribute to get keys instead of values."
    "function","","Function name","No","Describes the processing for JSON."
    "","len","Function name","No","If the JSON property is an array, get the array length. For JSON objects, get the number of elements in the JSON object."
    "","delete","Function name","No","Deletes the target property. If index is specified in the array, the element is deleted."
    "","insert","Function name","No","Specifies the addition of a value to the JSON array."
    "index","","Index","No","Specifies the index when getting the JSON data. If the target is an array, it is the array number. In a JSON object, the keys are counted from the beginning. When setting or modifying JSON data, only specify the arrays."
    "type","","Configuration Type","No","Sets a numeric value, a boolean character, or null as a string in a data update."


* Child element

If an AIML variable is specified as a value, it cannot be specified in the attribute, so it can also be specified as a child element.
The behavior is the same as the attribute. If the same attribute name and child element name can be specified, the child element setting takes precedence.
However, only when key is used in a child element, name and var are distinguished and treated separately.

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "function","Function name","No","Describes the processing for the JSON. See the attribute for the function."
    "index","Index","No","Specifies the index when getting the JSON data. If the target is an array, it is the array number. In a JSON object, the keys are counted from the beginning. When setting or modifying JSON data, only specify arrays."
    "item","Get Key name","No","Used to get keys from JSON data. Specify this attribute to get keys instead of values."
    "key","Key specification","No","Specifies the key to manipulate the JSON data."


Basic usage
-----------------------------

The name, data, or var attribute of the JSON element specifies the target json data. 
When the contents of a JSON element is set,  the value of the target JSON data is set and updated.
If the contents of the JSON element was not set, gets the value of that key, unless delete it. If the retrieval fails, the value of "default-get" set in Config, etc. is returned like the :ref:`get<template_get>` .
Describes how to get and set up data for JSON as follows. 
This is an example of using the subagent return value. The subagent return value is assumed to be held in the variable ``__SUBAGENT __.profile`` .


.. code:: json

    {
        "profile": {
            "name": "Ichiro",
            "birthday": "01011970",
            "age": 48,
            "sex": "male",
            "home": { "address": "Minato-ku, Tokyo","coordinates": "x,y" },
            "family": {
                "father": "Taro",
                "mother": "Hanako",
                "brother": [ "Jiro", "Kikuko" ],
                "children": [ "Saburo", "Momoko" ]
            },
            "relative": {
                "grandfather": [ "Shiro", "Goro" ],
                "grandmother": [ "Kuriko", "Umeko" ],
                "cousin": ["Kotaro" , "Sakiko"],
                "grandchildren": [ "Aki" ]
            },
            "friend": [ "Kazuhiro", "Kyoko" ],
            "hobby": [ "golf", "baseball" ],
            "medical history": [ "high blood pressure", "hay fever" ],
            "allergy": [ "milk", "egg" ]
        }
    }

How to specify attributes/child elements when getting the JSON data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Describes how to specify attributes and child elements.
Although they are described differently, the results are the same.

Getting Key Specification Values
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If this gets the value of "father", the following description is given. 
For attributes, specify the keys you want to get separated by  ``.``.
For child elements, enter the key you want to get in the contents of ``<key>``.

.. code:: xml

    <json var="__USER_METADATA__.profile.family.father" />
    <!-- <json var="__USER_METADATA__.profile"><key>family.father </key> </json> The same as above -->


Getting Arrays
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When you get the value of "brother" which is an array, as with the getting value, 
you get the specified array by describing the key you want to get by separating ``.`` or the child element ``<key>``.
Get ["Jiro" , "Kikuko"] as the result of the execution.

.. code:: xml

    <json var="__USER_METADATA__.profile.family.brother" />
    <!-- <json var="__USER_METADATA__.profile.family"> <key> brother</key> </json> The same as above -->


Getting Array Length
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set function to "len" to get the array length.
If it is "brother", returns ``2``.

.. code:: xml

    <json var="__USER_METADATA__.profile.family.brother" function="len"/>
    <!-- <json var="__USER_METADATA__.profile.family.brother"><function>len</function></json>   The same as above -->

Getting Array Values
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When retrieving the contents of an array, specify the array index.
Gets the 0 th value for "brother" ``Jiro``.

.. code:: xml

    <json var="__USER_METADATA__.profile.family.brother" index="0"/>
    <!-- <json var="__USER_METADATA__.profile.family.brother"><index>0</index></json>  The same as above -->



Getting JSON Object Element Count
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When you want to get the number of elements in a JSON object, specify the ``len`` for the function.
Gets ``11`` elements if specified the profile.

.. code:: xml

    <json var="__USER_METADATA__.profile" function="len"/>
    <!-- <json var="__USER_METADATA__.profile"><function>len</function></json>  The same as above -->


Getting a key for a JSON object
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When gets the key of a JSON object, specify a ``key`` for item.
The fifth profile key gets the  ``family`` .

.. code:: xml

    <json var="__USER_METADATA__.profile" item="key" index="5"/>
    <!-- <json var="__USER_METADATA__.profile"><item>key</item><index>5</index></json>  The same as above -->



Update JSON Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When changing the value of a key already in the JSON data, describe the value in the contents of the JSON element. 
Update "Address" to "Shin-Yokohama" by describing the contents.
If empty, specify ``""`` .

.. code:: xml

    <json var="__USER_METADATA__.profile.home.address">Shin-Yokohama</json>
    <json var="__USER_METADATA__.profile.home.coordinates">""</json>


Before Update

.. code:: json

    "home": {"address" : "Minato-ku, Tokyo" , "coordinates" : "x,y"},

After Update

.. code:: json

    "home": {"address" : "Shin-Yokohama" , "coordinates" : ""},


Append to JSON Data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To add a new key, add it by specifying a value for the new key.

.. code:: xml

    <json var="__USER_METADATA__.profile.zip-code">222-0033</json>
    <!-- <json var="__USER_METADATA__.profile"><key>zip-code</key>222-0033</json>  The same as above -->

Before Update

.. code:: json

    {
        "profile":{
            "medical history": ["high blood pressure" , "hay fever"],
            "allergy" : ["milk" , "egg"]
        }
    }

After Update

.. code:: json

    {
        "profile":{
            "medical history": ["high blood pressure" , "hay fever"],
            "allergy" : ["milk" , "egg"],
            "zip-code" : "222-0033"
        }
    }

Changing the contents of an array
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To change the contents of an array, specify the array index.
If change the 0 value of "hobby", get as follows.

.. code:: xml

    <json var="__USER_METADATA__.profile.hobby" index="0">soccer </json>
    <!-- <json var="__USER_METADATA__.profile"><key>hobby</key><index>0</index>soccer</json>  The same as above-->

Before Update

.. code:: json

    {
        "profile":{
            "hobby": ["golf" , "baseball"]
        }
    }

After Update

.. code:: json

    {
        "profile":{
            "hobby": ["soccer" , "baseball"]
        }
    }


Modifying Arrays
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To modify all the elements of "hobby" in the array,
enclose individual elements in double quotes and separate them with commas.

.. code:: xml

    <json var="__USER_METADATA__.profile.hobby">"soccer","fishing","movie watching"</json>
    <!-- <json var="__USER_METADATA__.profile"><key>hobby</key>"soccer","fishing","movie watching"</json>  The same as above -->

is specified,

Before Update

.. code:: json

            "hobby": ["golf","baseball"],

After Update

.. code:: json

            "hobby": ["soccer","fishing","movie watching"],

Adding Elements to Arrays
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To add an element to the array, specifies the function to insert and index to set the insertion point.
To add a value at the beginning, specify 0 for index.
A minus index represents the trailing index value, e.g., index = -1 appends the value to the end of the array.
Enclose individual elements in double quotes and separate them with commas.

In the example below, index = "0" is specified for "hobby" that is an array, and the value is added to the beginning of the array.

.. code:: xml

    <json var="__USER_METADATA__.profile.hobby" function="insert" index="0">"soccer" , "fishing" , "movie watching" , "travel (overseas, domestic)" </ json>
    <!-- <json var="__USER_METADATA__.profile"><key>hobby</key><function>insert</function><index>0</index>"soccer","fishing", "movie watching", "travel (overseas, domestic)"</json>   The same as above -->


Before Update

.. code:: json

            "hobby": ["golf","baseball"],

After Update

.. code:: json

            "hobby": ["soccer" , "fishing" , "movie watching" , "travel (overseas, domestic)" , "golf" , "baseball"],


In the example below, the value is added to the end of the array by specifying index = "2" that is the number of the elements in array "hobby".

.. code:: xml

    <json var="__USER_METADATA__.profile.hobby" function="insert" index="2">"soccer , "fishing" , "movie watching" , "travel (overseas,domestic)"</json>
    <!-- <json var="__USER_METADATA__.profile"><key>hobby</key><function>insert</function><index>2</index>"soccer , "fishing" , "movie watching" , "travel (overseas,domestic)"</json>   The same operation as above -->

Before Update

.. code:: json

            "hobby": ["golf" , "baseball"],

After Update

.. code:: json

            "hobby": ["golf" , "baseball" , "soccer" , "fishing" , "movie watching" , "travel (overseas, domestic)"],

Similarly, index = "-1" adds a value to the end of the array.

.. code:: xml

    <json var="__USER_METADATA__.profile.hobby" function="insert" index="-1">"soccer , "fishing" , "movie watching" , "travel (overseas,domestic)"</json>
    <!-- <json var="__USER_METADATA__.profile"><key>hobby</key><function>insert</function><index>-1</index>"soccer , "fishing" , "movie watching" , "travel (overseas,domestic)"</json>  The same as above -->

Before Update

.. code:: json

            "hobby": ["golf" , "baseball"],

After Update

.. code:: json

            "hobby": ["golf" , "baseball" , "soccer" , "fishing" , "movie watching" , " travel (oversea, domestic)"],

Creating Arrays
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To create an array, set elements separated by commas or specify the function with insert and set index to 0 or -1. (Not created if index is not 0 or -1)

The following example creates a "education" as a new array element.


.. code:: xml

    <json var="__USER_METADATA__.profile.education"> "A Elementary School" , "B Junior High School" , "C high school" , "D University" </json>
    <!-- <json var="__USER_METADATA__.profile.education" function="insert" index="0"> "A Elementary School", "B Junior High School", "C high school", "D University" </json> The same as above -->
    <!-- <json var="__USER_METADATA__.profile.education" function="insert" index="-1"> "A Elementary School", "B Junior High School", "C high school", "D University" </json> The same as above -->
    <!-- <json var="__USER_METADATA__.profile"><key>education</key><function>insert</function><index>0</index> "A Elementary School", "B Junior High School", "C high school", "D University" </json> The same as above -->

After creation

.. code:: json

            "education":["A Elementary School","B Junior High School","C high school","D University"]

In the case of an array with only one element, if insert is not specified for function, a JSON object is created, so insert is required for the function.
(It is not possible to change a JSON object to an array by specifying function with insert. It must first be created as an array element.)

.. code:: xml

    <!-- <json var="__USER_METADATA__.profile.education" function = "insert" index = "0"> "D University" </json> The same as above -->
    <!-- <json var="__USER_METADATA__.profile.education" function = "insert" index = "-1"> "D University" </json> The same as above -->
    <!-- <json var="__USER_METADATA__.profile"> <key> education </key> <function> insert </function> <index> 0 </index> "D University" </json> The same as above -->

After the update, a one-element array is created.

.. code:: json

            "education" : ["D University"]


If insert is not specified for function,

.. code:: xml

    <json var="__USER_METADATA__.profile.education"> "D University"</json>

After update, a JSON object is created.

.. code:: json

            "education" : "D University"


Deleting JSON Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Deleting Array Elements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To delete an array element, set function to delete and index to the index of the target value.
Deletes the value at the specified index. 
A negative index represents a trailing index value, and index = -1 deletes the last value in the array.

.. code:: XML

    <json var="__USER_METADATA__.profile.hobby index="0" function="delete" />
    <!-- <json var="__USER_METADATA__.profile.hobby"><index>0</index><function>delete</function></json>  The same as above -->
    <json var="__USER_METADATA__.profile.hobby index="-1" function="delete" />
    <!-- <json var="__USER_METADATA__.profile.hobby"><index>-1</index><function>delete</function></json>  The same as above -->

Before Update

.. code:: json

            "hobby": ["golf" , "baseball" , "reading"],

After Update

.. code:: json

            "hobby": ["baseball"],


Deleting Keys
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To delete keys, set function to delete.
Deletes the specified key and value.

Delete the "hobby" key and value by specifying "delete" for "function".

.. code:: xml

    <json var="__USER_METADATA__.profile.hobby" function="delete" />
    <!-- <json var="__USER_METADATA__.profile.hobby"><function>delete</function><json>  The same as above -->

Before Update

.. code:: json

            "friend": ["Kazuhiro" , "Kyoko"],
            "hobby": ["golf" , "baseball"],
            "medical history": ["high blood pressure" , "hay fever"],

After Update

.. code:: json

            "friend": ["Kazuhiro" , "Kyoko"],
            "medical history": ["high blood pressure" , "hay fever"],


Specify JSON format data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To set the element to JSON format data, specify the JSON string format enclosed in curly braces: {}.
Although it can be specified by array operation, it is necessary to specify one element at a time because the description in JSON string format cannot be specified in the list.

.. code:: XML

    <json var="__USER_METADATA__.profile.family.father">{"name": "Taro", "age": 80}</json>

Before Update

.. code:: json

            "father":"Taro",

After Update

.. code:: json

            "father": {
                   "name": "Taro",
                   "age": 80
                   },


Note that this function sets the contents of the JSON key, and it is not possible to set the data in JSON format directly to the variable as shown below.
To :ref:`set<template_set>` a variable to JSON formatted content, use the set element.

.. code:: XML

    <!-- Not Configurable --><json var = "__USER_METADATA__"> {"profile" : {"family" : {"father" : {"name": "Taro", "age": 80}}}} </json>

    <!-- Configurable --> <set var="__USER_METADATA__">{"profile": {"family": {"father": {"name": "Taro", "age": 80}}}}</set>


See: :doc:`SubAgent<SubAgent>` ,:doc:`Metadata<Metadata>` ,:doc:`intent recognition<NLU>` 


Handling of Numeric, Boolean, and null
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

AIML only handles strings, not JSON numbers, truth values, or nulls directly. 
The operation for setting and getting these contents in the JSON element is explained.


Settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| If only a numerical value is specified at the time of setting, it will be treated internally as a numerical value.
| If a Non-numeric string is contained, it will be treated as strings.
| When "true" or "false" indicating boolean value is set, it will be registered in JSON as boolean value.
| If the value is set to null, set null to JSON.
| If set a numeric value, true, false, or null as a character string, specify ``string`` for type.

Example:

.. code:: xml

    <json var="__USER_METADATA__.profile.age">30</json>
    <json var="__USER_METADATA__.profile.fullage">31 years old</json>
    <json var="__USER_METADATA__.profile.birthday" type="string">01011970</json>
    <json var="__USER_METADATA__.profile.self-Introduction">null</json>
    <json var="__USER_METADATA__.profile.telephone-authentication">true</json>
    <json var="__USER_METADATA__.profile.mail-authentication" type="string">false</json>

The setting result of the example is the following JSON.

.. code:: json

    {
        "profile": {
            "age": 30,
            "fullage": ”31 years old",
            "birthday": "01011970",
            "self-introduction": null,
            "telephone-authentication": true,
            "mail-authentication": "false"
        }
    }

Getting
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When getting numeric value, boolean value, and null in JSON element, these are obtained as character strings.
In the case of a numeric value, it is obtained as a numeric character string, in the case of a boolean value, it is obtained as a character string of "true", "false", and in the case of null, it is obtained as a character string of "null".

Example:

.. code:: XML

    <json var="__USER_METADATA__.profile.age"/>
    <json var="__USER_METADATA__.profile.fullage"/>
    <json var="__USER_METADATA__.profile.birthday"/>
    <json var="__USER_METADATA__.profile.telephone-authentication"/>
    <json var="__USER_METADATA__.profile.mail-authentication"/>
    <json var="__USER_METADATA__.profile.self-introduction"/>


The scenario designer should be aware of the source data type because each retrieval value is obtained as a string, as shown below.

.. code::

    30
    31 years old
    01011970
    true
    false
    null
