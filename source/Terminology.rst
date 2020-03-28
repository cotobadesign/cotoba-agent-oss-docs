========================
Glossary
========================

This chapter defines the terms used in the documentation.

Terms related to dialog processing
========================================

This section defines the dialog terms used in the documentation.

..  list-table::
    :widths: 40 60
    :header-rows: 1

    *
      + Term
      + Definition
    *
      + Dialog Scenario
      + A group of files describing processing programs and data related to the execution of dialog processing. Dialog programs are written in AIML(Artificial Intelligence Markup Language).
    *
      + developer
      + A person who describes dialog programs/data and develops dialog applications.
    *
      + Dialog Engine
      + An execution environment that registers dialog scenarios and performs dialog operations.
    *
      + bot
      + A dialog process in which the dialog scenario is registered with the dialog engine and dialog processing can be performed.
    *
      + user
      + A person who requests dialog processing from a bot.
    *
      + System
      + Another name for a bot. It is sometimes used in the context of comparison with the user.
    *
      + Request
      + The dialog request the user makes to the bot, or the contents thereof.
    *
      + Response
      + The dialog response that the bot returns to the user it interacts with, or its contents.
    *
      + Utterance
      + A string that is included in the request and sent to the bot by the user.
    *
      + Reply
      + A string that is included in the response and returned by the bot to the user.
    *
      + Intent Recognition
      + A function that infers the intent of a utterance using a machine learning model and extracts keywords (slots) associated with the intent from the utterance, or a module that executes the function.
    *
      + Dialog log
      + A log that records the operation process of dialog processing executed by the dialog engine (bot).
    *
      + SubAgent
      +  It is also called SA (SubAgent).
    *
      + MainAgent
      + The bot calling the SubAgent. It is also called MA (MainAgent).

cAIML
========================

This section defines terms related to cAIML (COTOBA AIML) Used in the documentation.

.. list-table::
    :widths: 40 60
    :header-rows: 1

    *
      + Term
      + Definition
    *
      + AIML
      + Abbreviation of Artificial Intelligence Markup Language. A language for writing dialog programs. AIML is written in XML format.
    *
      + cAIML
      + Abbreviation of COTOBA AIML. AIML with proprietary extensions by COTOBA DESIGN.
    *
      + tag
      + A tag (Start-tag '<...>' and End-tag '< /...>') defined in cAIML that specifies the basic structure of XML.
    *
      + Element
      + The main component of a tag. It is described in the notation of Start-tag <Element> and End-tag < /Element>.
    *
      + Attribute
      + The sub-components that make up the tag. It is described in the notation of the Start-tag <Element Attribute = "Value">. There may be more than one attribute, but the same attribute cannot be specified more than one. The attributes that can be used are specified for each element. You can think of an element as a function and an attributed as an argument of the function.
    *
      + Content
      + A string written between the Start-tag and End-tag. It is written in the notation '<Element attribute = "Value"> contents </ Element>'. The content can describe a plurality of normal strings and other elements. As a result, cAIML is a nested description format similar to XML.
    *
      + Element Name
      + The name of the element for which you want to specify a particular element.
    *
      + Attribute Name
      + The attribute element to specify a specific attribute.
    *
      + Attribute Value
      + The value to set for the attribute. When it is described as an attribute value in cAIML, it is stipulated by double quotation.
    *
      + block
      + Refers to an entire element (Description between the corresponding Start-tag and End-tag) that contains content.
    *
      + node
      + An element name of a tree structure corresponding to an element when an XML data structure of cAIML is viewed as a tree structure.
    *
      + NLU
      + Refers to the intent recognition function called from cAIML.