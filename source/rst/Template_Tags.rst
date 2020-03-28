==================================================
template element
==================================================

This chapter describes the AIML elements that can be described within the template element.

The following is a list of AIML elements that can be described within the template elements supported by the Dialog Platform.

-  `addtriple <#addtriple>`__
-  `authorise <#authorise>`__
-  `bot <#bot>`__
-  `button <#button>`__
-  `card <#card>`__
-  `carousel <#carousel>`__
-  `condition <#condition>`__

   -  :ref:`Block Condition<condition_type1>`

   -  :ref:`Single-predicate Condition <condition_type2>`

   -  :ref:`Multi-predicate Condition <condition_type3>`

   -  :ref:`Looping <condition_looping>`

-  `date <#date>`__
-  `delay <#delay>`__
-  `deletetriple <#deletetriple>`__
-  `denormalize <#denormalize>`__
-  `eval <#eval>`__
-  `explode <#explode>`__
-  `first <#first>`__
-  `extension <#extension>`__
-  `formal <#formal>`__
-  `gender <#gender>`__
-  `get <#get>`__
-  `id <#id>`__
-  `image <#image>`__
-  `implode <#implode>`__
-  `input <#input>`__
-  `interval <#interval>`__
-  `json <#json>`__
-  `learn <#learn>`__
-  `learnf <#learnf>`__
-  `li <#li>`__
-  `link <#link>`__
-  `list <#list>`__
-  `log <#log>`__
-  `lowercase <#lowercase>`__
-  `map <#map>`__
-  `nluintent <#nluintent>`__
-  `nluslot <#nluslot>`__
-  `normalize <#normalize>`__
-  `oob <#oob>`__
-  `olist <#olist>`__
-  `person <#person>`__
-  `person2 <#person2>`__
-  `program <#program>`__
-  `random <#random>`__
-  `reply <#reply>`__
-  `request <#request>`__
-  `resetlearn <#resetlearn>`__
-  `resetlearnf <#resetlearnf>`__
-  `response <#response>`__
-  `rest <#rest>`__
-  `set <#set>`__
-  `select <#select>`__
-  `sentence <#sentence>`__
-  `size <#size>`__
-  `space <#space>`__
-  `split <#split>`__
-  `sr <#sr>`__
-  `srai <#srai>`__
-  `sraix <#sraix>`__
-  `star <#star>`__
-  `system <#system>`__
-  `that <#that>`__
-  `thatstar <#thatstar>`__
-  `think <#think>`__
-  `topicstar <#topicstar>`__
-  `uniq <#uniq>`__
-  `uppercase <#uppercase>`__
-  `video <#video>`__
-  `vocabulary <#vocabulary>`__
-  `word <#word>`__
-  `xml <#xml>`__

Details
============
| This section provides detailed descriptions within the AIML template element.
| Most elements take additional data either as xml attributes or child elements.
| The [...] at the top of each element indicates the version of AIML in which the element was originally defined.

addtriple
---------------
[2.0]

The addtriple element adds the element (knowledge) to the RDF knowledge base.
The element has 3 components: subject, predicate, and object.
For more information about addtriple elements, see :doc:`RDF support<RDF_Support>`.

In the example below, for the user's utterance "My favorite food is fish", an element (knowledge) consisting of items subject = 'My favorite food', pred = 'is', object = 'fish' is registered in the RDF knowledge base.

* Use case

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>* favorite food is * </pattern>
            <template>
                <addtriple>
                    <subj><star /> favorite food</subj>
                    <pred>is</pred>
                    <obj><star index="2"/></obj>
                </addtriple>
                Registered the preferences
            </template>
        </category>
    </aiml>

| Input: My favorite food is fish
| Output: Registered  the preferences

See `uniq <#uniq>`__, `select <#select>`__ to check the results of the registration.

See also: `deletetriple <#deletetriple>`__, `select <#select>`__, `uniq <#uniq>`__, :doc:`RDF support<RDF_Support>`

.. _template_authorise:

authorise
---------------
[1.0]

The authorise element allows the user's role to toggle the execution of AIML elements described within the template element.
If the user's role differs from the role specified in the root attribute of the authorise element, the AIML element described in the authorise element is not executed.
See :doc:`Security <Security>` for more information.

* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "role","String","Yes","Role Name"
    "denied_srai","String","No","Srai destination on authentication failure"

* Use case

This use case can return the contents of a vocabulary only if the user's role is "root".

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>Number of vocabulary lists</pattern>
            <template>
                <authorise role="root">
                    <vocabulary />
                </authorise>
            </template>
        </category>
    </aiml>

In addition, you can specify the denied_srai attribute to determine the default behavior when the user's role differs from the specified role.

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>Number of vocabulary lists </pattern>
                <template>
                    <authorise role="root" denied_srai="ACCESS_DENIED">
                        <vocabulary />
                    </authorise>
                </template>
        </category>
    </aiml>

See also: :doc:`Security <Security>`

.. _template_bot:

bot
---------
[1.0]

Gets the properties specific to the bot.
This element is read-only.
These properties can be specified in properties.txt and read at startup to get as bot-specific information.

* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "name","String","Yes","Basically, any of name, birthdate, app_version, grammar_version is described (possible to change in properties.txt)."

* Use case

.. code:: xml

    <category>
       <pattern>Who are you? </pattern>
       <template>
           My name is <bot name = "name" />.
           I was born on <bot name = "birthdate" />.
           The application version is <bot name="app_version" />.
           The grammar version is <bot name="grammar_version" />.
       </template>
   </category>


You can use name as a child element of a bot to describe the same thing as the name attribute.

.. code:: xml

   <category>
       <pattern>Who are you? </pattern>
       <template>
           My name is <bot><name>name</name></bot>.
           I was born on <bot><name>birthdate</name></bot>.
           The application version is <bot><name>app_version</name></bot>.
           The grammar version is <bot><name>grammar_version</name></bot>.
       </template>
   </category>

See also: :ref:`File Management：properties<storage_entity>`

button
------------
[2.1]

The button element is a rich media element used to prompt the user to tap during a conversation.
Text used for the notation of button, postback for Bot, url when button is pressed can be described as child elements.

* Child element

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "text","String","Yes","Describes the display text for the button."
    "postback","String","No","Describes the operation when the button is pressed. This message is not shown to the user and is used to respond to a Bot or to process in an application."
    "url","String","No","Describes the URL when the button is pressed."

* Use case

.. code:: xml

   <category>
       <pattern>Transfer</pattern>
       <template>
            <button>
                <text>Do you want to search for transfer? </text>
                <postback>Transfer Guide </postback>
            </button>
       </template>
    </category>

   <category>
       <pattern>Search</pattern>
       <template>
            <button>
                <text>Do you want to search?</text>
                <url>https://searchsite.com</url>
            </button>
       </template>
    </category>


card
----------
[2.1]

A card is a card that uses several other elements, such as images, buttons, titles, and subtitles.
A menu containing all of these rich media elements appears.

* Child element

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "title","String","Yes","Describes the title of the card."
    "subtitle","String","No","Provide additional information for the card."
    "image","String","Yes","Describes the image URL etc. for the card."
    "button","String","Yes","Describes the button information for the card."


* Use case

.. code:: xml

    <category>
        <pattern>Search</pattern>
        <template>
            <card>
                <title>Card Menu</title>
                <subtitle>Card Menu Details</subtitle>
                <image>https://searchsite.com/image.png</image>
                <button>
                    <text>Do you want to search?</text>
                    <url>https://searchsite.com</url>
                </button>
            </card>
        </template>
    </category>

See also: `button <#button>`__, `image <#image>`__


carousel
--------------
[2.1]

The carousel element uses several card elements to display a tap through menu.
A menu containing all of these rich media elements appears.

* Child element

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "card","String","Yes","Specifies multiple cards. Display one card at a time and tap through to display another card."


* Use case

.. code:: xml

    <category>
        <pattern>Restaurant Search</pattern>
        <template>
            <carousel>
                <card>
                    <title>Italian</title>
                    <subtitle>Searching for Italian restaurants </subtitle>
                    <image>https://searchsite.com?q=italian</image>
                    <button>Italian Search </button>
                </card>
                <card>
                    <title>French</title>
                    <subtitle>Searching for French restaurants</subtitle>
                    <image>https://searchsite.com?q=french</image>
                    <button>French Search</button>
                </card>
            </carousel>
        </template>
    </category>

See also: `card <#card>`__, `button <#button>`__, `image <#image>`__


condition
---------------
[1.0]

| This would be used to describe conditional decisions in a template, and possible to describe the processing like switch-case.
| The branch is described by determining the variable specified in the condition attribute by the li attribute.
| Use the variables defined by get/set and Bot specific information as condition names.
| The variable type var is a local variable, name is a global variable, data is a global variable, and acts as a variable to hold until deleteVariable from the API specifies true.
| The following describes how conditions are described.


* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "name","variable name","No","Specifies the variable to be the branch condition."
    "var","variable name","No","Specifies the variable to be the branch condition."
    "data","variable name","No","Specifies the variable to be the branch condition."
    "bot","property name","No","Specifies the Bot specific information for the branch condition."
    "value","judgment value","No","Specifies the value for the branch condition."

* Child element

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "li","String","No","Describes the branch condition for the specified variable."

note : Each parameter of the attribute can also be specified as a child element.


.. _condition_type1:

Block Condition
~~~~~~~~~~~~~~~~~

| The first type of condition is a boolean statement that executes the included template tags if the value is true and does nothing if the value is false. 
| There are a number of ways the tag can be defined in XML, all 4 of the following statements are syntactically equal returning the value 'X' if the value of the predicate property is 'v'.

* Use case

.. code:: xml

   <condition name="property" value="v">X</condition>
   <condition name="property"><value>v</value>X</condition>
   <condition value="v"><name>property</name>X</condition>
   <condition><name>property</name><value>v</value>X</condition>



.. _condition_type2:

Single-predicate Condition
~~~~~~~~~~~~~~~~~~~~~~~~~~

| The second type of condition acts like a case or a switch statement in some programming languages.
| Whereby a single predicate is checked in turn for specific values. As soon as one of the conditions is true, the contained value in then returned. 
| If no value matches, then the parser checks if there is a default value and if so returns that, otherwise it returns no content.

In the example below, both conditions are syntactically equivalent and will return X if predicate called property has value a, and b if it has value Y, otherwise the statement will return the default value Z.

* Use case

.. code:: xml

   <condition name="property">
       <li value="a">X</li>
       <li value="b">Y</li>
       <li>Z</li>
   </condition>

   <condition>
       <name>property</name>
       <li value="a">X</li>
       <li value="b">Y</li>
       <li>Z</li>
   </condition>

.. _condition_type3:

Multi-predicate Condition
~~~~~~~~~~~~~~~~~~~~~~~~~

| The third form of a condition statement is very much like a nested set of if statements in most programming languages.
| Each condition to be checked defined as a <li> tag is checked in turn. Each statement may have different predicate names and values.
| As soon as one of the conditions is true further processing of the <li> items stops.



* Use case

.. code:: xml

   <condition>
       <li name="1" value="a">X</li>
       <li value="b"><name>1</name>Y</li>
       <li name="1"><value>b</value>Z</li>
       <li><name>1</name><value>b</value>Z</li>
       <li>Z<l/i>
   </condition>

.. _condition_looping:

Looping
~~~~~~~

| <loop>should be listed as one of the child elements of <li>.
| If the branch is made to the <li> with <loop>, after the processing of <li> is finished, the content of <condition>is reevaluated.

In the following example, the variable "topic" is evaluated to determine what to return. If the branch condition is not match, "chat" is set to "topic", <condition>is re-evaluated, and "chat" exits the loop.

* Use case

.. code:: xml

    <condition var="topic">
        <li value="flower">What flowers do you like ? </li>
        <li value="drink">Do you like coffee ? </li>
        <li value="chat">Did something good happen? </li>
        <li><think><set var="topic">chat </set></think><loop/></li>
    </condition>

See also: `li <#li>`__, `get <#get>`__, `set <#set>`__


date
----------
[1.0]

| Gets the date and time string. The locale/time specification in the API changes what is returned.
| The format attribute supports Python datetime string formatting. See `Python documentation (datetime) <https://docs.python.org/3.6/library/datetime.html>`__ for more information.


* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "format","String","No","Output format specification. If unspecified, %c."

* Child element

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "format","String","No","Output format specification. If unspecified, %c."

* Use case

.. code:: xml

   <category>
       <pattern>What's the date today? </pattern>
       <template>
           Today is <date format="%d/%m/%Y" />.
       </template>
   </category>

   <category>
       <pattern>What's the date today ?</pattern>
       <template>
           Today is <date><format>%d/%m/%Y</format></date>.
       </template>
   </category>

See also: `interval <#interval>`__

delay
-----------
[2.1]

The delay element is the delay factor.
It is used to define the wait time during text-to-speech playback and to specify the delay of the Bot's response to the user.

* Child element

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "seconds","String","Yes","Specifies the delay in seconds."

* Use case

.. code:: xml

   <category>
       <pattern>Wait * seconds</pattern>
       <template>
            <delay>
                <seconds><star/></seconds>
            </delay>
        </template>
    </category>


deletetriple
------------------
[2.0]

| Removes an element of knowledge from the RDF data store.
| The element will either have been loaded at startup, or added via the `addtriple <#addtriple>`__ element.
| See :doc:`RDFSupport<RDF_Support>` for more information.

* Use case

.. code:: xml

   <category>
       <pattern>Remove * is a * </pattern>
       <template>
           <deletetriple>
               <subj><star /></subj>
               <pred>isA</pred>
               <obj><star index="2"/></obj>
           </deletetriple>
       </template>
   </category>

See also: `addtriple <#addtriple>`__, `select <#select>`__, `uniq <#uniq>`__, :doc:`RDF Support<RDF_Support>`

.. _template_denormalize:

denormalize
-----------------
[1.0]

| During pre processing of the input text, the parser converts various characters and combinations of characters into distinct words.
| For example 'www.***.com' will be converted to 'www dot  _ _ _ dot com' by looking for matches in the mappings contained in dernormal.txt.
| If denormalize specifies that 'dot' should be transformed to '.' and '_' should be transformed to '*', normalize/denormalize restores to 'www.***.com'.

* Use case

.. code:: xml

   <category>
       <pattern>The URL is *.</pattern>
       <template>
           <think>
               <set var="url"><normalize><star /></normalize></set>
           </think>
           Restore <denormalize><get var = "url" /></denormalize>.
       </template>
   </category>


<denormalize/>is equivalent to <denormalize><star/></denormalize>.

* Use case

.. code:: xml

   <category>
       <pattern>URL is * </pattern>
       <template>
            Restore <denormalize />.
       </template>
   </category>

| Input: The URL is www.***.com.
| Output: Restore www.***.com.

See also: :ref:`File Management : denormal<storage_entity>`, `normalize <#normalize>`__

eval
----------
[1.0]

Typically used as part of `learn <#learn>`__ or `learnf <#learnf>`__ elements.
Eval evaluates the contained elements returning the textualized content.

In the example below, if a variable with name 'name' was set to TIMMY, and a variable 'animal' was set to dog, then evaluation of this learn node would then see a new category 'learnt' to match 'Who is TIMMY' with a response, 'Your DOG'.

* Use case

.. code:: xml

    <category>
        <pattern>Remember * is my pet * .</pattern>
        <template>
            Your pet is <star index = "2" /> <star />.
            <think>
                <set name="animal"><star /></set>
                <set name="name"><star index="2" /></set>
            </think>
            <learnf>
                <category>
                    <pattern>
                        <eval>
                            Who is
                            <get name="name"/>?
                        </eval>
                    </pattern>
                    <template>
                        Your
                        <eval>
                            <get name="animal"/>
                        </eval>
                        .
                   </template>
                </category>
            </learnf>
        </template>
    </category>

| Input: Remember TIMMY is my pet DOG.
| Output: Your pet is DOG TIMMY.
| Input: Who is TIMMY?
| Output: Your DOG.



See also: `learn <#learn>`__, `learnf <#learnf>`__

explode
-------------
[1.0]

Turns the contents of each word contained within the talk into a series single character words separated by spaces.
So for example 'FRED' would explode to 'F R E D', and 'Hello There' would explode to 'H e l l o T h e r e'.

* Use case

.. code:: xml

   <category>
       <pattern>EXPLODE *</pattern>
       <template>
           <explode><star /></explode>
       </template>
   </category>

<explode />is equivalent to <explode><star /></explode>.

.. code:: xml

   <category>
       <pattern>EXPLODE *</pattern>
       <template>
           <explode />
       </template>
   </category>

| Input: EXPLODE coffee
| Output: c o f f e e

See also: `implode <#implode>`__


image
-----------
[2.1]

Use the image element to return image information.
You can specify the image URL and file name.

.. code:: xml

    <category>
        <pattern>Image display </pattern>
        <template>
            <image>https://url.for.image</image>
        </template>
    </category>

first
-----------
[1.0]

| Give a list of words returns the first word. The opposite of rest which returns all but the first word. Usefully for iterating through a series of words in conjunction with the rest tag. Return NIL if there are no more words to return.
| If the obtainment fails, the value of "default-get" set in Config, etc. is returned just like `get <#get>`__.
| Retrieves the first data in the result list when applied to RDF knowledge base search results. See :doc:`RDF Support<RDF_Support>` for more information.

* Use case

.. code:: xml

   <category>
       <pattern>My name is * </pattern>
       <template>
           Your first name is <first><star /></first>.
       </template>
   </category>

<first />is equivalent to <first><star /></first>.

* Use case

.. code:: xml

   <category>
       <pattern>My name is * </pattern>
       <template>
          Your first name is <first />.
       </template>
   </category>

| Input: My name is Taro Yamada.
| Output: Your first name is Taro.


See also: `rest <#rest>`__

extension
---------------------
[custom]

It is an element that requires engine customization.
An extension provides the mechanism to call out to the underlying Python classes.
An extension is essentially made up of a full Python module path to a Python class which implements the ``programy.extensions.Extension`` interface.
See :doc:`Extensions<Extensions>` for more information.

* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "path","String","Yes","extension name used."

* Use case

.. code:: xml

   <category>
       <pattern>
           GEOCODE *
       </pattern>
       <template>
            <extension path="programy.extensions.goecode.geocode.GeoCodeExtension">
                <star />
            </extension>
       </template>
   </category>

See also: :doc:`Extensions<Extensions>`


formal
------------
[1.0]

The formal element will capitalise every distinct word contained between its elements.

* Use case

.. code:: xml

   <category>
       <pattern>My name is * *</pattern>
       <template>
        Hello Mr <formal><star /></formal> <formal><star index="2"/></formal>.
       </template>
   </category>

<formal />is equivalent to <formal><star /></formal>.

* Use case

.. code:: xml

   <category>
       <pattern>My name is * *</pattern>
       <template>
           Hello Mr <formal /><formal><star index="2"/></formal>
       </template>
   </category>


| Input: My name is george washington.
| Output: Hello Mr George Washington.

.. _template_gender:

gender
------------
[1.0]

| The gender element changes to a word of the opposite gender of a personal pronoun that represents the gender contained in the utterance sentence. The content of gender.txt is used for transformation.
| The transformation method is described in the before and after sets  and is only transformed if there is match within the gender set.


* Use case

.. code:: xml

   <category>
       <pattern>Does it belong to *?</pattern>
       <template>
           No, it belongs to <gender><star/></gender>.
       </template>
   </category>

| Input: Does it belong to him?
| Output: No, it belongs to her.


See also:  :ref:`File Management：gender<storage_entity>`


.. _template_get:

get
---------
[1.0]

| The get element is used to retrieve the value of a variable. If the retrieval fails, the value set by "default-get" in Config is returned.
| (If the definition of "default-get" was done in the property information of bots: properties.txt  , it takes precedences over Config definitions.)
| The values that can be retrieved by get are `set <#set>`__ during dialog.
| If you want the variable to be set to a value at startup, write it to defaults.txt and the variable can be used as a global one (name).
| There are three types of variable types: local and global variables have different retention periods.
| You can also retrieve RDF knowledge base elements by specifying child elements <tuples>. See :doc:`RDF Support<RDF_Support>`  for more information.


* Local Variables (var)

| By specifying the var attribute, it is treated as a local variable.
| Local variables are kept only in the category range where set/get is listed. Therefore, it is treated as a separate variable in the reference to srai.

* Persistent Global Variables (name)

| It is treated as a global variable by specifying the name attribute. Global variables can also refer to settings in different categories.
| In addition, the contents of global variables are maintained continuously, even when dialog processing is repeated.

* Retain Range Global Variable (data)

| By specifying the data attribute, it is treated as a global variable. The difference from name is that the variable defined in data is cleared when deleteVariable in the dialog API is set to true.


* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,65

    "name","Variable Name","Yes","var, name, or data must be set."
    "var","Variable Name","Yes","var, name, or data must be set."
    "data","Variable Name","Yes","var, name, or data must be set."

| If an AIML variable is specified as a value, it cannot be specified in the attribute, so it can also be specified as a child element.
| The behavior is the same as the attribute. If you specify the same attribute name and child element name, the child element setting takes precedence.

* Child element

.. csv-table::
    :header: "Parameter","","Type","Required","Description"
    :widths: 10,10,10,5,65

    "name","","Variable Name","Yes","var, name, or data must be set."
    "var","","Variable Name","Yes","var, name, or data must be set."
    "data","","Variable Name","Yes","var, name, or data must be set."


* Use case

.. code:: xml

    <!-- Access Global Variable -->
    <category>
        <pattern>Today is * .</pattern>
        <template>
            <think><set name="weather"><star/></set></think>
             Today's weather is <get name = "weather" />.
        </template>
    </category>

    <!-- Access Local Variable -->
    <category>
        <pattern>Tomorrow is * .</pattern>
        <template>
            <think><set var="weather"><star/></set></think>
             Today's weather is <get name="weather" />, tomorrow's weather is <get var = "weather" />.
        </template>
    </category>
    <category>
        <pattern>What's the weather? </pattern>
        <template>
             Today's weather is <get name = "weather" />, tomorrow's weather is <get var = "weather" />.
        </template>
    </category>


| Input: Today is sunny.
| Output: Today's weather is sunny.
| Input: Tomorrow is rainny.
| Output: Output: Today's weather is sunny, tomorrow's weather is rainny.
| Input: What's the weather?
| Output: Today's weather is sunny, tomorrow's weather is unknown.


See also: `set <#set>`__, :ref:`File Management：properties<storage_entity>`


id
--------
[1.0]

Returns the client name. The client name is specified by the client developer in Config.

* Use case

.. code:: xml

   <category>
       <pattern>What's your name? </pattern>
       <template>
           <id />
       </template>
   </category>

| Input: What's your name?
| Output: console


implode
-------------------
[custom]

Given a string of words, concatenates them all into a single string with no spaces.
If enable the implode, 'c o f f e e'  will be transformed to 'coffee'.

* Use case

.. code:: xml

   <category>
       <pattern>Implode *</pattern>
       <template>
           <implode><star /></implode>
       </template>
   </category>

<implode />is equivalent to <implode><star /></implode>.

* Use case

.. code:: xml

   <category>
       <pattern>Implode the acronym *</pattern>
       <template>
           <implode />
       </template>
   </category>


| Input: Implode c o f f e e
| Output: coffee

See also: `explode <#explode>`__

input
-----------
[1.0]

Returns the entire pattern sentences.
This is different to  the wildcard ``<star/>``  tag which only returns those values that match one of the wildcards in the pattern.

* Use case

.. code:: xml

   <category>
       <pattern>What was my question ?</pattern>
       <template>
           Your question was "<input />".
       </template>
   </category>

| Input: What was my question?
| Output: Your question was "What was my question?".


interval
--------------
[1.0]

| Calculates the difference between 2 time entities.
| The format attribute supports Python formatting of date and time strings. See `Python documentation <https://docs.python.jp/3.6/library/datetime.html>`__ for details.

* Child element

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "from","String","Yes","Describes the starting time of the calculation."
    "to","String","Yes","Describes the final starting time of the calculation."
    "style","String","Yes","The unit to return in interval. One of years, months, days, or seconds."


* Use case

.. code:: xml

   <category>
       <pattern>How old are you? </pattern>
       <template>
            <interval format="%B %d, %Y">
                <style>years</style>
                <from><bot name="birthdate"/></from>
                <to><date format="%B %d, %Y" /></to>
            </interval>
            years old.
       </template>
   </category>

| Input: How old are you?
| Output: 5 years old.

See also: `date <#date>`__



.. _template_json:

json
---------
[custom]

| This is a function for using JSON in AIML.
| Use to leverage JSON data for use with :doc:`SubAgent<SubAgent>`, :doc:`Metadata<Metadata>`, :doc:`NLU<NLU>`  (intent recognition),etc.
| See :doc:`JSON <JSON>` for more information.

| The variable name specified in name/var/data of the attribute/child element is the variable name defined in get/set.
| The variable type var is a local variable, name is a global variable, data is a global variable, and acts as a variable to hold until deleteVariable from the API specifies true.
| You can also use system fixed variable names, such as metadata variables and subagent return values.

* Attribute

.. csv-table::
    :header: "Parameter","Set value","Type","Required","Description"
    :widths: 10,10,10,5,65

    "name","","JSON Name","Yes","Specify the JSON to parse. var, name, or data must be set."
    "var","","JSON Name","Yes","Specify the JSON to parse. var, name, or data must be set."
    "data","","JSON Name","Yes","Specify the JSON to parse. var, name, or data must be set."
    "function","","Function Name","No","Describes the processing for JSON."
    "","len","Function Name","No","If the JSON property is an array, gets array length. For JSON objects, get the number of elements in the JSON object."
    "","delete","Function Name","No","Deletes the target property. If index is specified in the array, the target element is deleted."
    "","insert","Function Name","No","Specifies the addition of a value to a JSON array. It is specified with an array number (index)."
    "index","","Index","No","Specifies the index when obtaining JSON data. If the target is an array, it is specified the array number. In a JSON object, it specifies the key count from the head. When setting or modifying JSON data, it can be specified only arrays."
    "item","","Get key name","No","Used to get the key from JSON data. Specify this attribute to get keys instead of values."
    "key","","Key specification","No","Specifies the key to manipulate JSON data."

* Child element

| If an AIML variable is specified as a value, it cannot be specified in the attribute, so it can also be specified as a child element.
| The behavior is the same as the attribute. If you specify the same attribute name and child element name, the child element setting takes precedence.

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "function","Function name","No","Describes the processing for JSON. See the attribute's function for its contents."
    "index","Index","No","You can specify it for a JSON objects and an array. For an array, it specifies the array index and for a JSON objects, it specifies the key count from the beginning. When setting or modifying JSON data, you can only specify arrays."
    "item","Get key name","No","Used to get the key from JSON data. Specify this attribute to get keys instead of values."
    "key","Key specification","No","Specifies the key to manipulate the JSON data."


* Use case

| This section describes how to get JSON data when a response is returned from a SubAgent called "transit" and use it as a response.
| When the following json data is returned from the SubAgent, "__SUBAGENT__.transit" becomes the storage variable name of the response data from the SubAgent.
| When obtaining JSON data, specifies the target JSON name in the attribute. In this case, "__SUBAGENT__.transit" is the target JSON name.
| When obtaining child elements of JSON data, the json name should be a property of the per-element key name concatenated with ".".

.. code:: json

        {
            "transportation":{
                "station":{
                    "departure":"Tokyo",
                    "arrival":"Kyoto"
                },
                "time":{
                    "departure":"11/1/2018 11:00",
                    "arrival":"11/1/2018 13:30"
                }
            }
        }

As in the example above, if you want to return the transportation.station.separate, you can use the following:

.. code:: xml

    <category>
        <pattern>From Tokyo to Kyoto.</pattern>
        <template>
            Departure from <json var = "__SUBAGENT__.transit.transportation.station.departure" />.
        </template>
    </category>

| Input: From Tokyo to Kyoto.
| Output: Departure from Tokyo.

See also: :doc:`JSON <JSON>`, :doc:`SubAgent<SubAgent>`


learn
-----------
[2.0]

| The learn element enables the new category depending on the dialog condition.
| This new category is held in memory and only takes effect when accessed by the same client for the duration of context.

Because learnf is file preserving, it retains its state when the bot is restarted, but learn is initialized when the bot is restarted.

* Use case

.. code:: xml

   <category>
        <pattern>Remember * is my pet *.</pattern>
        <template>
            <think>
                <set name="name"><star /></set>
                <set name="animal"><star index="2" /></set>
            </think>
            <learn>
                <category>
                    <pattern>
                        Who is
                        <eval>
                            <get name="name"/>
                        </eval>
                        ?
                    </pattern>
                    <template>
                        Your
                        <eval>
                            <get name="animal"/>
                        </eval>.
                    </template>
                </category>
            </learn>
        </template>
    </category>

| Input: Remember TIMMY is my pet DOG.
| Input: Who is TIMMY?
| Output: Your DOG.

See also: `eval <#eval>`__, `learnf <#learnf>`__

.. _template_learnf:

learnf
------------
[2.0]

| The learnf element enables the new category depending on the dialog condition.
| This new category is kept in the file, and when enabled, its contents are kept. It is only enabled when accessed by the same client.

Since learnf is a file retention, it is reloaded when the bot is restarted.


* Use case

.. code:: xml

   <category>
        <pattern>Remember * is my pet *.</pattern>
        <template>
            <think>
                <set name="name"><star /></set>
                <set name="animal"><star index="2" /></set>
            </think>
            <learnf>
                <category>
                    <pattern>
                        Who is
                        <eval>
                            <get name="name"/>
                        </eval>
                        ?
                    </pattern>
                    <template>
                        Your
                        <eval>
                            <get name="animal"/>
                        </eval>
                        .
                    </template>
                </category>
            </learnf>
        </template>
    </category>

| Input: Remeber TIMMY is my pet DOG.
| Input: Who is TIMMY?
| Output: Your DOG.

See also: `eval <#eval>`__, `learn <#learn>`__

li
---------------
[1.0]

The li element describes the branch condition specified in <condition>. 
For detailed usage, see `condition <#condition>`__ for more information.

* Child element

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "think","String","No","The definition which does not affect the action is described."
    "set","String","No","Set variables."
    "get","String","No","Gets the value of a variable."
    "loop","String","No","Specifies a loop for <condition>."
    "star","String","No","Reuse the input wildcards."

See also : `condition <#condition>`__, :ref:`loop <condition_looping>`,  `think <#think>`__, `set <#set>`__, `get <#get>`__, `star <#star>`__


link
----------
[2.1]

The link element is a rich media element used for purposes such as URLs to display to the user during a dialog.
Child elements can describe the text used to display or read, and the destination url.

* Child element

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "text","String","Yes","Describes the display text to the button."
    "url","String","No","Describes the URL when the button is pressed."

.. code:: xml

    <category>
        <pattern>Search</pattern>
        <template>
            <link>
                <text>Search site</text>
                <url>searchsite.com</url>
            </link>
        </template>
    </category>


list
----------
[2.1]

The link element is a rich media element that returns the elements described in item in list format.
It can describe the contents of the list to the item of the child element.
Also it is possible to nest the item with list.

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "item","String","Yes","Describes the contents of the list."

.. code:: xml

    <category>
        <pattern>list</pattern>
        <template>
            <list>
                <item>
                    <list>
                        <item>list item 1.1 </item>
                        <item>list item 1.2 </item>
                    </list>
                </item>
                <item>list item 2.1 </item>
                <item>list item 3.1 </item>
            </list>
        </template>
    </category>

.. _template_log:

log
---------------
[custom]

| Allows a developer to embed logging elements into the itself. These elements are then output into the bot log file.
| Logging levels are as follows, and are equivalent to `Python Logging <https://docs.python.jp/3.6/library/logging.html>`__ .


* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "level","Variable Name","No","Specify error, warning, debug, info. The default output is info."

See :ref:`Log Settings <config_logging>` for more information.

* Use case

.. code:: xml

    <category>
        <pattern>Hello</pattern>
        <template>
            Hello.
            <log>Greetings</log>
        </template>
    </category>

    <category>
        <pattern>Goodbye</pattern>
        <template>
            Goodbye
            <log level="error">Greetings</log>
        </template>
    </category>

| Input: Hello
| Output: Hello             NOTE: The log would be output "Greetings" at the info level.
| Input: Goodbye
| Output: Goodbye           NOTE: The log would be output "Greetings" at the error level.

See also: :ref:`Log Settings <config_logging>`

lowercase
---------------
[1.0]

Changes half-width alphabetic characters to lowercase.

* Use case

.. code:: xml

   <category>
       <pattern>HELLO * </pattern>
       <template>
           HELLO <lowercase><star /></lowercase>
       </template>
   </category>


<lowercase />is equivalent to <lowercase><star /></lowercase>.

* Use case

.. code:: xml

   <category>
       <pattern>HELLO *</pattern>
       <template>
           HELLO <lowercase><star /></lowercase>
       </template>
   </category>

| Input: HELLO GEORGE WASHINGTON
| Output: HELLO george washington

See also: `uppercase <#uppercase>`__

.. _template_map:

map
---------
[1.0]

| At startup, reference a map file listing 'keys: value' and return the value matching the key. If the key does not match, the value set in Config "default-get" is returned.
| As a map file, reference the file stored in the directory specified by config.


* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "name","Variable Name","Yes","Specifies the map file name."


* Use case

.. code:: xml

   <category>
       <pattern>Where is the prefectural capital of  *? </pattern>
       <template>
          <map name="prefectural_office"><star/></map>です。
       </template>
   </category>


| Input:  Where is the prefectural capital of Kanagawa Prefecture?
| Output: Yokohama.

See also: :ref:`File Management：maps<storage_entity>`


.. _template_nluintent:

nluintent
---------
[custom]

| This function is used to obtain intent information for NLU results.
| Values are returned only if there is an NLU result. Therefore, it is basically used in template when the category that specified  :ref:`nlu tag<pattarn_nlu>` in pattern matches.
| See :doc:`NLU <NLU>`  for more information.

* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,30,5,55

    "name","Intent Name","Yes","Specify the intent name to get. ``*`` is treated as a wildcard. When a wildcard is specified, the index specifies what to get."
    "item","Get Item Name","Yes","Gets information about the specified intent. The ``intent`` , ``score`` and  ``count`` can be specified. 
    If intent is specified, the intent name can be obtained. When score is specified, the confidence level (0.0 ~ 1.0) is obtained. The count returns the number of intent names."
    "index","Index","No","Specifies the index number of the intent to get. The index is for the list that only includes the intents matching with intent name specified by name."

| If an AIML variable is specified as a value, it cannot be specified in the attribute, so it can also be specified as a child element.
| The behavior is the same as the attribute. If the same attribute name and child element name would be specified, the child element setting takes precedence.

* Child element

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,30,5,55

    "name","Intent Name","Yes","Specifies the name of the intent to get. See the attribute's name for its contents."
    "item","Get Item Name","Yes","Gets information about the specified intent. See the attribute item for its contents."
    "index","Index","No","Specifies the index number of the intent to get. See the index of the attribute for its contents."



* Use case

Get information from NLU processing results.
In the following example, the intent is obtained from the NLU processing result.

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

To get the intent processed by the NLU. Describe as follows.

.. code:: xml

    <category>
        <pattern>
            <nlu intent="restaurantsearch"/>
        </pattern>
        <template>
            <nluintent name="restaurantsearch" item="score"/>
        </template>
    </category>

| Input: Look for Italian, French or Chinese.
| Output: 0.9

See also: :doc:`NLU <NLU>` 、 :ref:`get the NLU Intent<nlu_intent_example>`

.. _template_nluslot:

nluslot
---------
[custom]

| This function is used to obtain slot information of NLU results.
| Values are returned only if there is an NLU result. Therefore, it is basically used in template when a category with nlu tag specified in pattern matches.
| See :doc:`NLU <NLU>` for more information.

* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,30,5,55

    "name","Slot Name","Yes","Specifies the slot name to get. ``*`` is a wildcard. When a wildcard is specified, index specifies what to get."
    "item","Get Item Name","Yes","Gets information about the specified slot. It can be specified ``slot`` , ``entity`` , ``score`` ,``startOffset`` , ``endOffset`` and ``count`` .
    When slot is specified, gets the slot name. If entity is specified, gets the extracted string of the slot, if score is specified, gets the confidence level (0.0 ~ 1.0), if startOffset is specified, gets the start character position of the extracted string, and if endOffset is specified, gets the end character position of the extracted string.
    The count returns the number of identical slot names."
    "index","Index","No","Specifies the index number of the slot to gets. Specifies the index number in the list that matches the slot name specified by name."

If an AIML variable is specified as a value, it cannot be specified in the attribute, so it can also be specified as a child element. 
The behavior is the same as the attribute. If it specifies the same attribute name and child element name, the child element setting takes precedence.


* Child element

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,30,5,55

    "name","Slot Name","Yes","Specifies the slot name to get. See the attribute's name for its contents."
    "item","Get Item Name","Yes","Gets information about the specified slot. See the attribute item for its contents."
    "index","Index","No","Specifies the index number of the slot to get. See the index of the attribute for its contents."



* Use case

Get slot information from NLU processing result.
This section explains how to obtain a slot from the NLU processing result in the following example.

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

If you want to get  the slots from NLU processing result, describe as follows.

.. code:: xml

    <category>
        <pattern>
            <nlu intent="restaurantsearch"/>
        </pattern>
        <template>
            <nluslot name="genre" item="count" />
            <nluslot name="genre" item="entity" index="0"/>
            <nluslot name="genre" item="entity" index="1"/>
            <nluslot name="genre" item="entity" index="2"/>
        </template>
    </category>

| Input: Search for Italian, French or Chinese.
| Output: 3 Italian French Chinese

See also: :doc:`NLU <NLU>` 、 :ref:`Get NLU Slot<nlu_slot_example>`

.. _template_normalize:

normalize
---------------
[1.0]

Transforms symbols in the target string or abbreviated string to the specified word. The transformation contents are specified in normal.txt. 
For example, if '.' is transformed to' dot 'and' * 'is transformed to' _ ', then ' www.***.com' will be transformed to 'www dot _ _ _ dot com'.

* Use case

.. code:: xml

   <category>
       <pattern>URL is * </pattern>
       <template>
           Displays <normalize><star /></normalize>.
       </template>
   </category>

<normalize />is equivalent to <normalize><star /></normalize>.

* Use case

.. code:: xml

   <category>
       <pattern>URL is * </pattern>
       <template>
            Displays <normalize />.
       </template>
   </category>

| Input: URL is www.***.com
| Output: Displays www dot _ _ _ dot com.


See also: :ref:`File Management：normal<storage_entity>` , `denormalize <#denormalize>`__

olist
-----------
[2.1]

The olist (ordered list) element is a rich media element that returns the elements listed in item.
The item of the child element can contain the contents of the list. You can also nest item with list.


.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "item","String","Yes","Describe the contents of the list."

.. code:: xml

   <category>
       <pattern>Display the list</pattern>
       <template>
            <olist>
               <item>
                    <card>
                        <image>https://searchsite.com/image0.png</image>
                        <title>Image No.1</title>
                        <subtitle>Tag olist No.1</subtitle>
                        <button>
                            <text>Yes</text>
                            <url>https://searchsite.com:?q=yes</url>
                        </button>
                    </card>
                </item>
                <item>
                    <card>
                        <image>https://searchsite.com/image1.png</image>
                        <title>Image No.2</title>
                        <subtitle>Tag olist No.2</subtitle>
                        <button>
                            <text>No</text>
                            <url>https://searchsite.com:?q=no</url>
                        </button>
                    </card>
                </item>
            </olist>
       </template>
    </category>

See also: `card <#card>`__


oob
---------
[1.0]

OOB stands for "Out of Band" and when the oob element is evaluated, the corresponding internal module performs processing and returns the processing result to the client.
The processing in the internal module is actually intended for equipment operation and is intended for use in embedded devices.
The internal modules that handle OOB are designed and implemented by system developers. See  :doc:`OOB <OOB>`  for more information.

* Use case

.. code:: xml

   <category>
       <pattern>DIAL *</pattern>
       <template>
            <oob><dial><star /></dial></oob>
       </template>
   </category>

| Input: DIAL 0123-456-7890
| Output: (DIAL) (The returned contents depend on the implementation of the internal module.)


See also: `xml <#xml>`__ 、 :doc:`OOB <OOB>`

.. _template_person:

person
------------
[1.0]

| The person element converts between the first-person pronouns and second-person pronouns in the utterance sentence. Use the contents of person.txt for transformation.
| The transformation method is given in the before and after sets and is only transformed if there is a match in the person set.

* Use case

.. code:: xml

   <category>
       <pattern>I am waiting for * .</pattern>
       <template>
           You are waiting for <person><star /></person>.
       </template>
   </category>

<person />is equivalent to <person><star /></person>.

* Use case

.. code:: xml

   <category>
       <pattern>I am waiting for * .</pattern>
       <template>
           You are waiting for <person />.
       </template>
   </category>

| Input:  I am waiting for you.
| Output: You are waiting for me.


See also: :ref:`File Management：person<storage_entity>` , `person2 <#person2>`__

.. _template_person2:

person2
-------------
[1.0]

| The person2 element converts between the pronouns of the first person and the pronouns of the third person in the utterance sentence. Use the contents of person2.txt for transformation.
| The transformation method is specified in the before and after sets and is only transformed if there is a match in the set person2.

* Use case

.. code:: xml

   <category>
       <pattern>Please tell * *. </pattern>
       <template>
           I tell <person2><star/></person2> <star index="2" />.
       </template>
   </category>

<person2 />is equivalent to <person 2><star /></person 2>.

* Use case

.. code:: xml

   <category>
       <pattern>Please tell *  *. </pattern>
       <template>
           I tell <person2 /> <star index="2" />.
       </template>
   </category>

| Input: Please tell me how to get there.
| Output:  I tell you how to get there.


See also: :ref:`File Management：person2<storage_entity>` , `person <#person>`__

program
-------------
[1.0]

Returns the program version of the Bot specified in Config.

* Use case

.. code:: xml

   <category>
       <pattern>version</pattern>
       <template>
           <program />
       </template>
   </category>

| Input: version
| Output: AIML bot version X


random
------------
[1.0]

Randomly selects the <li> elements used by <condition>.

* Use case

.. code:: xml

   <category>
       <pattern>Hello</pattern>
       <template>
           <random>
                <li>Hello</li>
                <li>How are you today?</li>
                <li>Shall I check today's schedule?</li>
           </random>
       </template>
   </category>

| Input: Hello
| Output: Shall I check today's schedule?
| Input: Hello
| Output: How are you today?

See also: `li <#li>`__、`condition <#condition>`__

reply
-----------
[2.1]

The reply element is a rich media element similar to the button element.
As a child element,  you can write the text you want to use for reading and the postback to the Bot.
The difference between reply and button is that reply is intended to be used for voice interaction instead of GUI.

* Child element

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "text","String","Yes","Describe text-to-speech text."
    "postback","String","No","Describe the operation. This message is not shown to the user and is used to respond to a Bot or to make some process in an application."

* Use case

.. code:: xml

   <category>
       <pattern>Transfer </pattern>
       <template>
            <reply>
                <text>Do you want to do a transfer search?</text>
                <postback>Transfer Guide </postback>
            </reply>
       </template>
    </category>


request
-------------
[1.0]

Returns the input history. The index attribute specifies the history number.
0 is the current input, and the higher the number, the more past history.
This returns all sentences in an input if the input has multiple sentences.

* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "index","String","No","Index number. 0 is the current index number, an alternative form with no attributes implies the use of index='1'."

* Use case

.. code:: xml

   <category>
       <pattern>What did I say?</pattern>
       <template>
             You said <request index="1" /> and before that
             you said <request index="2" /> and you just said
             <request index="0" />.
       </template>
   </category>

| Input: Hello
| Output: Hello
| Input:  It's already night.
| Output: Good evening.
| Input:  What did I say?
| Output: You said  "It's already night." and before that you said "Hello" and you just said "What did I say?".


<request />is equivalent to <request index="1" />.

* Use case

.. code:: xml

   <category>
       <pattern>What did I say?</pattern>
       <template>
             You said <request />.
       </template>
   </category>

| Input: Hello
| Output: Hello
| Input: What did I say?
| Output: You said Hello.

See also: `response <#response>`__

resetlearn
----------------
[2.x]

Clears all categories enabled in the ``<learn>`` ``<learnf>`` elements.

* Use case

.. code:: xml

   <category>
       <pattern>Forget what I said.</pattern>
       <template>
            <think><resetlearn /></think>
            OK, I have forgotten what you taught me.
       </template>
   </category>

resetlearnf
-----------------
[2.x]

Clears all categories enabled in the ``<learn>`` ``<learnf>`` elements.
This differs from resetlearn in that it deletes all the files that the learnf element created.

* Use case

.. code:: xml

   <category>
       <pattern>Forget what I said. </pattern>
       <template>
            <think><resetlearnf /></think>
            OK, I have forgotten what you taught me.
       </template>
   </category>

response
--------------
[1.0]

Returns the output history. The index attribute specifies the history number. 
The higher the number, the more past history.
This returns all sentences if a multi sentence question is asked.

* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "index","String","No","Index number. An alternative form with no attributes implies the use of index='1'."

* Use case

.. code:: xml

   <category>
       <pattern>What did you just say?</pattern>
       <template>
             I said <response index="1" />and before that
             I said <response index="2" />.
       </template>
   </category>

| Input: Hello
| Output: Hello
| Input: It's already night.
| Output: Good evening.
| Input: What did you just say?
| Output: I said "Good evening" and before that  I said "Hello".

<response />is equivalent to <response index = "1" />.

* Use case

.. code:: xml

   <category>
       <pattern>What did you just say? </pattern>
       <template>
             I said <response/>.
       </template>
   </category>

| Input: Hello
| Output: Hello
| Input: What did you just say?
| Output: I said Hello.


See also: `request <#request>`__

rest
----------
[2.0]

| Given a series of words returns all but the first word. Provides the opposite functionality to the first. If the retrieval fails, the value of "default-get" set in Config, etc. is returned like the  `get <#get>`__ .
| For example, "Taro Yamada" returns "Yamada".
| When applied to RDF Knowledgebase search results, it gets non-head data in the results list. See  :doc:`RDF Support <RDF_Support>` for more information.

* Use case

.. code:: xml

   <category>
       <pattern>My name is * .</pattern>
       <template>
           Your name is <rest><star /></rest>.
       </template>
   </category>

| Input: My name is Taro Yamada.
| Output: Your name is Yamada.


See also: `first <#first>`__


.. _template_set:

set
---------
[1.0]

The set elements in the template can set global and local variables.
For differences in the variable type: name/var/data, see `get <#get>`__.


* Use case

.. code:: xml

   <!-- global variables -->
   <category>
       <pattern>MY NAME IS *</pattern>
       <template>
           <set name="myname"><star /></set>
       </template>
   </category>

   <!-- local variables -->
   <category>
       <pattern>MY NAME IS *</pattern>
       <template>
           <set var="myname"><star /></set>
       </template>
   </category>

See also: `get <#get>`__

select
------------
[2.0]

| The contents of the RDF file referenced at startup and the RDF knowledgebase added by addtriple would search,  and to get the appropriate information.
| As an RDF file, it refers to the file stored in the directory specified by config.
| See  :doc:`RDF Support <RDF_Support>` for more information.

* Use case

.. code:: xml

   <category>
       <pattern>* legs animals?</pattern>
       <template>
           <select>
                <vars>?name</vars>
                <q><subj>?name</subj><pred>legs</pred><obj><star/></obj></q>
           </select>
       </template>
   </category>

| Input:  4 legs animals?
| Output: [[["?name", "ZEBRA"]], [["?name", "LION"]], [["?name", "ELEPHANT"]]]

.. code:: xml

   <category>
       <pattern>How many legs animals have?</pattern>
       <template>
        <select>
            <vars>?name ?number</vars>
            <q><subj>?name</subj><pred>legs</pred><obj>?number</obj></q>
        </select>
       </template>
   </category>

| Input: How many legs animals have?
| Output: [[["?name", "ANT"], ["?number", "6"]], [["?name", "BAT"], ["?number", "2"]], [["?name", "LION"], ["?number", "4"]], [["?name", "PIG"], ["?number", "4"]], [["?name", "ELEPHANT"], ["?number", "4"]], [["?name", "PERSON"], ["?number", "2"]], [["?name", "BEE"], ["?number", "6"]], [["?name", "BUFFALO"], ["?number", "4"]], [["?name", "ANIMAL"], ["?number", "Legs"]], [["?name", "FROG"], ["?number", "4"]], [["?name", "PENGUIN"], ["?number", "2"]], [["?name", "DUCK"], ["?number", "2"]], [["?name", "BIRD"], ["?number", "2"]], [["?name", "MONKEY"], ["?number", "4"]], [["?name", "GOOSE"], ["?number", "2"]], [["?name", "FOX"], ["?number", "4"]], [["?name", "KANGAROO"], ["?number", "2"]], [["?name", "DOG"], ["?number", "4"]], [["?name", "COW"], ["?number", "4"]], [["?name", "SHEEP"], ["?number", "4"]], [["?name", "FISH"], ["?number", "0"]], [["?name", "OX"], ["?number", "4"]], [["?name", "DOLPHIN"], ["?number", "0"]], [["?name", "BEAR"], ["?number", "4"]], [["?name", "WOLF"], ["?number", "4"]], [["?name", "ZEBRA"], ["?number", "4"]], [["?name", "CAT"], ["?number", "4"]], [["?name", "WHALE"], ["?number", "0"]], [["?name", "CHICKEN"], ["?number", "2"]], [["?name", "TIGER"], ["?number", "4"]], [["?name", "HORSE"], ["?number", "4"]], [["?name", "OWL"], ["?number", "2"]], [["?name", "GOAT"], ["?number", "4"]], [["?name", "RABBIT"], ["?number", "4"]]]

See also: `addtriple <#addtriple>`__, `deletetriple <#deletetriple>`__, `uniq <#uniq>`__, :doc:`RDF Support<RDF_Support>`, :ref:`File Management：rdfs<storage_entity>`

sentence
--------------
[1.0]

Capitalises the first word of the sentence and sets all other words to lowercase.

* Use case

.. code:: xml

   <category>
       <pattern>Create a sentence with the word *</pattern>
       <template>
           <sentence>HAVE you Heard ABouT <star/></sentence>
       </template>
   </category>

| Input: Create a sentence with the word AnImAl
| Output: Have you heard about animal

<sentence />is equivalent to <sentence><star /></sentence>.

* Use case

.. code:: xml

   <category>
       <pattern>CORRECT THIS *</pattern>
       <template>
           <sentence />
       </template>
   </category>

| Input: CORRECT THIS PleAse tEll Us The WeSthEr ToDay.
| Output: Please tell us the weather today.


size
----------
[1.0]

Returns the number of categories in the Bot.

* Use case

.. code:: xml

   <category>
       <pattern>How many categories do you understand? </pattern>
       <template>
           <size />.
       </template>
   </category>

| Input: How many categories do you understand?
| Output: 5000.



space
-----------
[custom]

The space element inserts a halfwidth space when the sentence is created.

.. code:: xml

   <category>
       <pattern>Good morning.</pattern>
       <template>
            <think>
                <set var="french">French</set>
                <set var="italian">Italian</set>
                <set var="chinese">Chinese</set>
            </think>
            Search <get var="french"/>,<get var="italian"/>,<get var="chinese"/>.
            Search <get var="french"/><space/><get var="italian"/><space/><get var="chinese"/>.
       </template>
   </category>

| Input: Good morning
| Output: Search French,Italian,Chinese. Search French Italian Chinese.



split
-----------
[2.1]

The split element is used to split the Bot's response into multiple parts.
Split messages are treated as separate messages.

.. code:: xml

   <category>
       <pattern>Good morning.</pattern>
       <template>
            The weather is nice today.
            <split/>
            I hope it will be fine again tomorrow.
       </template>
   </category>

| Input: Good morning.
| Output: The weather is nice today.
| Output: I hope it will be fine again tomorrow.

sr
--------
[1.0]

Shorthand for ``<srai><star/></srai>`` .

* Use case

.. code:: xml

   <category>
       <pattern>My question is * </pattern>
       <template>
           <sr />
       </template>
   </category>

<sr />is equivalent to <srai><star /></srai>.

.. code:: xml

   <category>
       <pattern>My question is * </pattern>
       <template>
           <srai><star/></srai>
       </template>
   </category>

See also: `star <#star>`__, `srai <#srai>`__

.. _srai:

srai
----------
[1.0]

The srai element allows your bot to recursively call categories after transforming the user’s input. 
So you can define a template that calls another category.
The acronym “srai” has no official meaning, but is sometimes defined as symbolic reduction or symbolic recursion.

* Use case

.. code:: xml

    <category>
        <pattern>Hello</pattern>
        <template><srai>HI</srai></template>
    </category>

    <category>
        <pattern>Ciao</pattern>
        <template><srai>HI</srai></template>
    </category>

    <category>
        <pattern>Hola</pattern>
        <template><srai>HI</srai></template>
    </category>

    <category>
        <pattern>HI</pattern>
        <template>Hello</template>
    </category>

| Input: Hola
| Output: Hello

See also: `star <#star>`__, `sr <#sr>`__, `sraix <#sraix>`__


.. _template_sraix:

sraix
-----------
[2.0]

Call any external REST API. Used for SubAgent calls.
See :doc:`SubAgent<SubAgent>` for more information on using sraix.

* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "service","String","No","The service name of the custom external service."
    "botid","String","No","The bot name published on the Dialog Platform."

* Use case

.. code:: xml

   <category>
       <pattern>Transfer information from * to *.</pattern>
       <template>
           <sraix service="myService">
               <star/>
               <star index="2"/>
           </sraix>
       </template>
   </category>

| Input: Transfer information from Tokyo to Osaka.
| Output: The Nozomi departing at 10:00 a.m. is the first to arrive.

See also: `star <#star>`__, `sr <#sr>`__, :doc:`SubAgent<SubAgent>`

star
----------
[1.0]

| The star element is a description that uses user input from wildcards.
| The index attribute provides the ability to access each individual match in the sentence. Wildcards include the one or more characters  ``*`` and  ``_`` the zero or more characters ``^`` and ``#`` .
| The Index also includes strings that correspond to ``set`` , ``iset`` , ``regex`` , ``bot`` and bot elements of pattern elements.
| If no such information exists, an empty string is returned.

* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "index","String","No","Input number. An alternative form with no attributes implies the use of index='1'."


* Use case

.. code:: xml

   <category>
       <pattern>I like * and *　.</pattern>
       <template>
           You like <star /> and <star index = "2" />.
       </template>
   </category>


| Input: I like flowers and cats.
| Output: You like flowers and cats.


See also: `sr <#sr>`__, `srai <#srai>`__

system
------------
[1.0]

The system element allows you to make underlying system calls. 
This is obviously a security concern allowing users unfettered access to the underlying system if they know the operating system and can inject shell scripts. 
 By default this is disabled, but can be turned on by setting the configuration option  ``allow_system_aiml`` to True.


* Use case

.. code:: xml

   <category>
       <pattern>LIST ALL AIML FILES</pattern>
       <template>
           <system>ls -l *.aiml</system>
       </template>
   </category>

.. _template_that:

that
----------
[1.0]

The that element is also defined as a child element of  ``category`` , and is used to match the bot's previous response, 
but if specified as a template element, that acts as an element ``that`` retrieves past response statements from the bot.

* Attribute

.. csv-table::
    :header: "Parameter","Type","Required","Description"
    :widths: 10,10,5,75

    "index","String","No","Input number. An alternative form with no attributes implies the use of index='1'."


* Use case

.. code:: xml

   <category>
       <pattern>Hello.</pattern>
       <template>
            Hello.
       </template>
   </category>

   <category>
       <pattern>Pardon?</pattern>
       <template>
            I said <that/>
       </template>
   </category>

| Input: Hello.
| Output: Hello.
| Input: Pardon?
| Output:  I said Hello.

See also: :ref:`that(pattern)<pattern_that>`, `thatstar <#thatstar>`__, `topicstar <#topicstar>`__


thatstar
--------------
[1.0]

| Use thatstar as a wildcard specification for the that.
| The that element can use the full set of wildcard matching that is available in the  ``pattern``　element. These matches are accessed in the same way as using  ``<star />`` , but for that element, we use  ``<thatstar />`` .
| If retrieval fails, an empty string is returned.

* Use case

.. code:: xml

   <category>
       <pattern>...</pattern>
       <template>
            Do you like coffee?
       </template>
   </category>
   <category>
       <pattern>Yes.</pattern>
       <that>Do you like * ? </that>
       <template>
           I like <thatstar />, too.
       </template>
   </category>

| Input: ...
| Output: Do you like coffee?
| Input: Yes.
| Output: I like coffee, too.

See also: :ref:`that(pattern)<pattern_that>`, `that <#that>`__, `topicstar <#topicstar>`__

.. _template_think:

think
-----------
[1.0]

The think element allows your bot to set predicates without actually displaying the contents of a set element to the user.
This is sometimes referred to as “silently” setting a predicate.

* Use case

.. code:: xml

   <category>
       <pattern>My name is * </pattern>
       <template>
          <think><set name="name"><star /></set></think>
          I remembered your name.
       </template>
   </category>


topicstar
---------------
[1.0]

| Use the topicstar element as a wildcard for the ``topic`` .
| The topic can use the full set of wildcard matching that is available in the pattern. These matches are accessed in the same way as using  ``<star />`` , but for the topic, we use  ``<topicstar />`` .
| It is also possible to specify "index" for the attribute. If retrieval fails, an empty string is returned.

* Use case

.. code:: xml

    <category>
        <pattern>I like coffee. </pattern>
        <template>
            <think><set name="topic">beverages coffee </set></think>
            OK.
        </template>
    </category>

   <topic name="beverages *">
   <category>
       <pattern>What's my favorite drink?</pattern>
           <template><topicstar/>. </template>
       </category>
   </topic>

| Input: I like coffee.
| Output: OK.
| Input:  What's my favorite drink?
| Output: Coffee.



See also: :ref:`topic(pattern)<pattern_topic>`, `thatstar <#thatstar>`__

uniq
----------
[2.0]

| Uniq is used in conjunction with  `select <#select>`__.  Uniq searches the contents of the RDF file it references at startup and the RDF knowledge base added with addtriple to get the appropriate information.
| As an RDF file, it refers to the file stored in the directory specified by the config.
| The difference between select and uniq is that select returns the exact result of multiple matches, while uniq returns the result where duplicate matches are excluded.
| See :doc:`RDF Support <RDF_Support>` for more information.

* Use case

.. code:: xml

    <category>
        <pattern>* are * </pattern>
        <template>
            <addtriple>
                <subj><star /></subj>
                <pred>are </pred>
                <obj><star index="2"/></obj>
            </addtriple>
            Registered
        </template>
    </category>
    <category>
        <pattern>Search * * * </pattern>
        <template>
            <uniq>
                <subj><star /></subj>
                <pred><star index="2"/></pred>
                <obj><star index="3"/></obj>
            </uniq>
        </template>
    </category>

| Input: Cherry blossoms are Rosaceae
| Output: Registered
| Input: Strawberries are Rosaceae
| Output: Registered
| Input: Search Cerry blossoms?
| Output: Rosaceae
| Input: Search Rosaceae?
| Output: Cherry blossom Strawberry

See also: `addtriple <#addtriple>`__, `deletetriple <#deletetriple>`__, `select <#select>`__, :doc:`RDF Support<RDF_Support>`, :ref:`File Management：rdfs<storage_entity>`

uppercase
---------------
[1.0]

Capitalises half-width alphabetic characters.

* Use case

.. code:: xml

   <category>
       <pattern>Hello * </pattern>
       <template>
           Hello <uppercase><star /></uppercase>
       </template>
   </category>

<uppercase />is equivalent to <uppercase><star /></uppercase>.

* Use case

.. code:: xml

   <category>
       <pattern>Hello * </pattern>
       <template>
           Hello <uppercase />
       </template>
   </category>


| Input: Hello george washington
| Output: Hello GEORGE WASHINGTON



See also: `lowercase <#lowercase>`__

vocabulary
----------------
[1.0]

Returns the number of distinct words known by the Bot.

* Use case

.. code:: xml

   <category>
       <pattern>How many words do you know? </pattern>
       <template>
           <vocabulary />.
       </template>
   </category>

| Input: How many words do you know?
| Output: 10000.

See also: `size <#size>`__

video
-----------
[2.1]

If this uses the video element, returns the video information. 
You can specify a URL or a file name for the video.

* Use case

.. code:: xml

    <category>
        <pattern>Video Display </pattern>
        <template>
            <video>https://url.for.video</video>
        </template>
    </category>

word
----------
[1.0]

A ``<word>`` tag as such does not exist, but each individual text word in a sentence is converted into a word node.

* Use case

.. code:: xml

   <category>
       <pattern>HELLO</pattern>
       <template>Hi there!</template>
   </category>

In this use case, HELLO is expanded to the word node.

xml
---------
[1.0]

Like a word node, there is not  ``<XML>`` tag as such however given that AIML is an XML dialect, and if the parser comes across an xml element it does not undertand, it stores it as pure XML.

* Use case

.. code:: xml

   <category>
       <pattern>HIGHLIGHT * IN BOLD</pattern>
       <template>
           <bold><star /></bold>
       </template>
   </category>
