========================
About COTOBA AIML
========================
COTOBA AIML is a dialog language used to describe dialog scenarios in COTOBA Agent, the dialog platform of COTOBA DESIGN, Inc. Based on AIML (Artificial Intelligence Markup Language), COTOBA DESIGN, Inc. has developed its own extensions to enable dialog control using information other than COTOBA Agent, such as JSON handling, metadata processing from APIs, and dialog control by calling REST API and referencing return values.


How to describe the COTOBA AIML
-------------------------------

This section explains how to describe a dialog scenario. Here is an example of a basic dialog scenario in which we describe the response of the dialog platform to the content of the user's speech.

By combining the contents of the COTOBA AIML reference, you can create a variety of dialog scenarios for holding variables, using JSON, and calling REST API.


Basic Dialog Scenario
-------------------------------
Depending on the content of the user's utterance, the response from the dialog platform is defined and the dialog scenario is described.

The category element is the basic unit of the dialog rule of AIML. The category element contains a single dialog rule, such as pattern and template.

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">

        <category>
            <pattern>Good morning.</pattern>
            <template>Good morning. Let's do our best today!</template>
        </category>

    </aiml>

| Input: Good morning.
| Output: Good morning. Let's do our best today!


Branching of the Response
-------------------------------
When creating a dialog scenario, it is often necessary to modify the response text according to the user's previous utterance.

For example, there is a scene where you respond to a query from a dialog platform with a "yes/no" response. Although "yes/no" is used in the pattern, common phrases such as "yes/no" are used in other dialogs and need to be distinguished.

An example of a response branch in this case is that.

In the example below, the response is changed according to the results of "Yes" and "No". However, user utterances of "Yes" and "No" can occur in other cases as well, but this response matches the previous utterance of the Bot with that, so that the response of the Bot matches only when "Would you like sugar and milk in your coffee?".

.. code:: xml

    <category>
        <pattern>I like coffee.</pattern>
        <template>Do you put sugar and milk in your coffee?</template>
    </category>

    <category>
        <pattern>Yes</pattern>
        <that>Do you put sugar and milk in your coffee?</that>
        <template>Okay.</template>
    </category>

    <category>
        <pattern>No</pattern>
        <that>Do you put sugar and milk in your coffee?</that>
        <template>Black coffee.</template>
    </category>

| Input: I like coffee.
| Output: Do you put sugar and milk in your coffee?
| Input: Yes
| Output: Okay.

Extracting Dialog Content and Using Variables
-------------------------------
To extract the content of the user's speech, use "*" and star. In addition, get and set are used to retain the content of the user's speech.

Hold the type of pet in the first utterance. In this case, "*" and wildcard are used for the pet type of the user's speech pattern. If you want to use the wildcard part in template, use <star/>. star will be the string of the wildcard range defined by pattern.

The use of variables uses set/get, which holds the contents of the set in the name petcategory specified by the set attribute.

In the following utterances, hold the name of the user's pet Similarly, we use set, but with a different variable name, petname.

In the following utterances, the content of the variable is included in the selection of the response from the dialog platform and the response content using the retained content.

To branch by the content of a variable, you can use condition, which is an element that compares strings with the target variable, and you can describe the process like switch-case.

In the following example, the pet type petcategory "dog" or "cat" is split by the case li of the switch-case statement. If neither is found, the unevaluated result is returned.

It also returns the content of the response, which is held by the petname.

.. code:: xml

    <category>
        <pattern>My pet is *.</pattern>
        <template>
            <think><set name="petcategory"><star/></set></think>
            I guess you like <star/>.
        </template>
    </category>

    <category>
        <pattern>My pet's name is *.</pattern>
        <template>
            <think><set name="petname"><star/></set></think>
            That's a good name.
        </template>
    </category>

    <category>
        <pattern>Do you remember my pet?</pattern>
        <template>
            <condition name="petcategory">
                <li value="dog">Your pet is a dog <get name="petname"/>.</li>
                <li value="cat">Your pet is a cat <get name="petname"/>.</li>
                <li>I don't think you have a pet.</li>
            </condition>
        </template>
    </category>

| Input: My pet is a dog.
| Output: I guess you like a dog.
| Input: My pet's name is Maron.
| Output: That's a good name.
| Input: Do you remember my pet?
| Output: Your pet is a dog Maron.


BOT Collaboration
-------------------------------
Multiple BOTs can be created and the results of each can be linked and operated. It uses the external REST API call of the sraix element for coordination. As shown below, specify the bot ID you have already created as the host name caller and set the necessary information in the body.

The return value from BOT is contained in var:__SUBAGENT_BODY__ and can be retrieved by the json element.


.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>

    <aiml version="2.0">
        <category>
            <pattern>Subagent *</pattern>
            <template>
                <think>
                    <json var="body.utterance"><star/></json>
                    <json var="body.userId"><get var="__USER_USERID__"/></json>
                    <set var="__SYSTEM_METADATA__"><json var="body"/></set>
                    <sraix>
                        <host>https://HOSTNAME/bots/BOT_ID/ask</host>
                    
                        <method>POST</method>
                        <header>"Content-Type":"application/json;charset=UTF-8"</header>
                        <body><json var="body"/></body>
                    </sraix>
                    
                </think>
                <json var="__SUBAGENT_BODY__.response"/>
            </template>
        </category>

    </aiml>


Intent Recognition Engine Linkage
-------------------------------

When using the model created by the intent recognition engine, the inference endpoint is set at the time of bot creation.

Use NLU elements to create a pattern branching scenario with intent of the intent recognition  engine. In this case, the intent and slot for the intent recognition engine can be obtained by using the nluintent and nluslot element.

In addition, the dialog platform performs a rule-based intent recognition according to the description of the scenario and returns a response according to the results evaluated by pattern matching, but if there is no matching pattern, the dialog control is performed using the intent results of the intent recognition. This is to give priority to what the scenario author describes over the consequences of the intent recognition. As an exception, if there is a category with only a wildcard as a pattern, the match is processed after both the scenario description match and the intent recognition match fail to match. Even if you define a child element nlu, the contents of the pattern element will still result in the usual pattern evaluation; see :ref:`nlu<pattern_nlu>`  for the attributes of nlu elements.

In the following example, if the result of the intent recognition engine is "Restaurant Search", it matches the pattern and returns the intent list and slot list of the intent recognition engine.

.. code:: xml

    <aiml version="2.0">
        <category>
            <pattern>
                <nlu intent="Restaurant Search"/>
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
                        <!-- score:<nluslot name="*" item="score"><index><get var="count" /></index></nluslot> -->
                        <think>
                            <set var="count"><map name="upcount"><get var="count" /></map></set>
                        </think>
                        <loop/>
                    </li>
                </condition>
            </template>
        </category>
    </aiml>
