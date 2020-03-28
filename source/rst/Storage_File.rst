================================
File Management
================================

| Describes files used on the Dialog Platform.
| In the following descriptions, the primary method of file management used by the Dialog Platform is local files.
| In addition to local files, file management methods can be also managed in the database, but the configuration is the same.

The definition file extension is aiml for the scenario file and txt for the non-scenario configuration file extension.

Directory Configuration
================================

 The directory structure of the scenario files is as follows.


.. code::

  storage Various File Directories
  ├── braintree AIML Extraction File Directory
  ├── categories Scenario File Directory
  │   └── *.aiml
  ├── conversations Dialog History Information Directory
  │   └── *.conv
  ├── debug Dialog History Information Directory
  │   ├── duplicates.txt
  │   ├── errors.txt
  │   └── *.log
  ├── lookups File Directory for Substitution Process
  │   ├── regex.txt
  │   ├── denormal.txt
  │   ├── gender.txt
  │   ├── normal.txt
  │   ├── person.txt
  │   └── person2.txt
  ├── maps map Element Usage File Directory
  │   └── *.txt
  ├── nodes map Element Usage File Directory
  │   ├── pattern_nodes.conf
  │   └── template_nodes.conf
  ├── properties File Directory for Property lists
  │   ├── nlu_servers.yaml
  │   ├── defaults.txt
  │   └── properties.txt
  ├── security File Directory for Security
  │   └── usergroups.yaml
  └── sets Set Usage File Directory
      └── *.txt


.. _storage_entity:

Entity
================================

| On the Dialog Platform, entities are defined by the type of data to be stored, and each entity manages its own files.
| The configuration of the file to be used is defined in the config file (config.yaml), and the data type to be used is specified by whether or not the following entities are set.
| There are two types of entities, one for common use and one for specific nodes, but they are defined and managed in the same way.
| Of the following entities, the data with "automatic generation" of "No" can be edited and used to expanded at startup.
| The data with "single file" of "Yes" can specify only one target file, and the file with "No" can specify multiple files by directory specification.

-  Common Entities

.. csv-table::
    :header: "Entity","Content","Single file","Automatic generation","Description"
    :widths: 18,40,10,5,65

    "binaries","Tree data","No","Yes","Stores binary data for the search tree expanding AIML."
    "braintree","Tree data","No","Yes","Stores XML formatted data for the search tree expanding AIML."
    "categories","dialog AIML","No","No","Stores AIML files used to control dialog."
    "conversations","dialog history information","No","Yes","The history information file of dialog processing contents (Contain the values of variables) including the past of each user is stored."
    "defaults","Property List","Yes","No","Stores definition files for global variables (name) that are expanded at initial startup."
    "duplicates","Duplicate specification information","Yes","Yes","(For debugging) Stores an error information file when expanding AIML files."
    "errors","Error Information","Yes","Yes","(For debugging) Stores an error information file when expanding AIML files."
    "license_keys","License Key","Yes","No","Stores license key files required for external connections."
    "pattern_nodes","Class definition list","Yes","No","Stores the processing class definition file for each node of pattern."
    "postprocessors","Class definition list","Yes","No","Stores the processing class definition file for editing response statements."
    "preprocessors","Class definition list","Yes","No","Stores the processing class definition file for editing a user utterance in a request."
    "properties","Property List","Yes","No","Stores the property definition files used by the :ref:`bot<pattern_bot>` of pattern and the :ref:`bot<template_bot>` node of the template."
    "spelling_corpus","Spelling Information","No","No","Stores the corpus file used for spelling checker."
    "template_nodes","Class definition list","Yes","No","Stores the processing class definition file for each node in template."

-  Entity for the Pattern node

.. csv-table::
    :header: "Entity","Stored Content","Single file","Automatic generation","Description"
    :widths: 18,40,10,5,65

    "regex_templates","Regular expression list","Yes","No","Stores the regular expression list file used in the template specification of the :ref:`regex<pattern_regex>` node."
    "sets","Target word list","No","No","Stores the word list file used by the :ref:`set<pattern_set>`  node."

-  Entity for the Template node

.. csv-table::
    :header: "Entity","Stored Content","Single file","Automatic generation","Description"
    :widths: 18,40,10,5,65

    "denormal","Transformation dictionary","Yes","No","Stores the dictionary file used for transformations in the :ref:`denormalize<template_denormalize>` node."
    "gender","Transformation dictionary","Yes","No","Stores the dictionary file used for transformations in the :ref:`gender<template_gender>`  node."
    "learnf","Category list","No","YES","Stores the categories information created by process of the :ref:`learnf<template_learnf>` node on a per-user basis."
    "logs","Log Information","No","YES",":ref:`log<template_log>` information created by the processing of the node is stored for each user."
    "maps","Property List","No","No","Stores the dictionary file used for translation on the :ref:`map<template_map>`  node."
    "normal","Transformation dictionary","Yes","No","Stores the dictionary file used for transformations on the :ref:`normalize<template_normalize>`  node."
    "person","Transformation dictionary","Yes","No","Stores the dictionary file used for transformations on the :ref:`person<template_person>` node."
    "person2","Transformation dictionary","Yes","No","Stores the dictionary file used for transformations on the :ref:`person2<template_person2>` node."
    "rdfs","RDF Data List","No","No","Stores the definition file of the :doc:`RDF<RDF_Support>` data to be processed by the RDF related nodes."
    "rdf_updates","RDF Update Information","Yes","Yes","Stores change history information for RDF data."
    "usergroups","Security Information","Yes","No","Stores the role definition file used by the :ref:`authorise<template_authorise>` node."



Example of definition for using local files
==================================================

Entity definition
--------------------------------

| The following is example of defining an entity when using a client named ``console`` .
| In the client configuration, a section called  ``storage``  has entities as a subsection.
| Specifies the name of the I/O control method (Store) for each entity as the file management method.
| Here, the store method name: ``file`` is specified.

.. code:: yaml

   console:
     storage:
         entities:
             binaries: file
             braintree: file
             categories: file
             conversations: file
             defaults: file
             duplicates: file
             errors: file
             license_keys: file
             pattern_nodes: file
             postprocessors: file
             preprocessors: file
             properties: file
             spelling_corpus: file
             template_nodes: file
             regex_templates: file
             sets: file
             denormal: file
             gender: file
             learnf: file
             logs:   file
             maps: file
             normal: file
             person: file
             person2: file
             rdf: file
             rdf_updates: file
             usergroups: file

file storage engine definition
--------------------------------------------

| Subsection stores in the same storage section specifies the storage engine that performs the actual processing within the store.
| Here, for the store name: file, “type: file ” specifies to use the storage engine to I/O the local file.
| For local file I/O (file specification), the name of the actual storage engine is the ``entity name + '_ storage'``.
| The configuration of the storage engine for each entity is done in the 'config' subsection as follows:

.. code:: yaml

         stores:
             file:
                 type: file
                 config:
                   binaries_storage:
                     file: ./storage/braintree/braintree.bin
                   braintree_storage:
                     file: ./storage/braintree/braintree.xml
                   categories_storage:
                     dirs: ./storage/categories
                     subdirs: true
                     extension: aiml
                   conversations_storage:
                     dirs: ./storage/conversations
                   defaults_storage:
                     file: ./storage/properties/defaults.txt
                   duplicates_storage:
                     file: ./storage/debug/duplicates.txt
                   errors_storage:
                     file: ./storage/debug/errors.txt
                   license_keys_storage:
                     file: ./storage/licenses/license.keys
                   pattern_nodes_storage:
                     file: ./storage/nodes/pattern_nodes.conf
                   postprocessors_storage:
                     file: ./storage/processing/postprocessors.conf
                   preprocessors_storage:
                     file: ./storage/processing/preprocessors.conf
                   properties_storage:
                     file: ./storage/properties/properties.txt
                   spelling_corpus_storage:
                     file: ./storage/spelling/corpus.txt
                   template_nodes_storage:
                     file: ./storage/nodes/template_nodes.conf
                   regex_templates_storage:
                     file: ./storage/lookups/regex.txt
                   sets_storage:
                     dirs: ./storage/sets
                     extension: txt
                   denormal_storage:
                     file: ./storage/lookups/denormal.txt
                   gender_storage:
                     file: ./storage/lookups/gender.txt
                   learnf_storage:
                     dirs: ./storage/learnf
                   logs_storage:
                     dirs: ./storage/debug
                   maps_storage:
                     dirs: ./storage/maps
                     extension: txt
                   normal_storage:
                     file: ./storage/lookups/normal.txt
                   person_storage:
                     file: ./storage/lookups/person.txt
                   person2_storage:
                     file: ./storage/lookups/person2.txt
                   rdfs_storage:
                     dirs: ./storage/rdfs
                     subdirs: true
                     extension: txt
                   rdf_updates_storage:
                     dirs: ./storage/rdf_updates
                   usergroups_storage:
                     file: ./storage/security/usergroups.yaml

The definition in the 'config' subsection is a description method indicating the use of a single file only or multiple files depending on the entity involved.

For entities in a single file
------------------------------------------------

For entities that specify a single file, the 'file' attribute specifies the file path.

.. code:: yaml

                   usergroups_storage:
                       file: ./storage/security/usergroups.yaml

For entities that can use multiple files
------------------------------------------------------

In the case of an entity that can use multiple files, specify the following three attributes.
However, for automatically generated entities, only the directory path is specified.

- dirs: Specifies the target file directory path.
- subdirs:  true/false set whether to search subdirectories under the target file directory.
- extension: The extension of the file type to load.

.. code:: yaml

                   categories_storage:
                     dirs: ./storage/categories
                     subdirs: true
                     extension: aiml

                   conversations_storage:
                     dirs: ./storage/conversations

There are following four storage engines that are not automatically generated and can specify multiple files.

- categories_storage
- sets_storage
- maps_storage
- rdfs_storage


Example of definition for using a database
==================================================

An example of using Redis for database management is shown below.

Entity Definition
--------------------------------

For the entity managed by Redis, store method name: ``redis``  is specified in the entities subsection of storage.

.. code:: yaml

   console:
     storage:
         entities:
             binaries: file
             braintree: file
             categories: redis
             ：

The entities that can input and output in Redis are as follows.

- binaries ： Stores binary data for the AIML expanded search tree.
- braintree ： Stores XML format data for the AIML expanded search tree.
- conversations ： History information of dialog processing contents (Contain the values of variables) including the past of each user is stored.
- duplicates ： (For debugging) Stores category duplicate information when extracting AIML files.
- errors ： (For debugging) Stores error information when extracting AIML files.
- learnf ：Stores categories information created by processing the :ref:`learnf<template_learnf>` node for each user.
- logs ：Log information created by processing the :ref:`log<template_log>` node is stored for each user.


Redis Storage Engine Definition Example
--------------------------------------------

| In the storage section, the stores subsection specifies that I/O is to be made to Redis by "type: redis" for the store name: redis.
| In the 'config' subsection, specify the common parameters required for using Redis, and the actual I/O are performed by the storage engine for each entity, including the key setting.

.. code:: yaml

         stores:
            redis:
                type: redis
                config:
                    host: localhost
                    port: 6379
                    password: xxxx
                    db: 0
                    prefix: programy
                    drop_all_first: false

How to describe a definition file
===================================

Describes the definition files that are commonly used outside of AIML in editable files.

Property List File
--------------------------------

The file specified by the following entities is described in the form of 'Name: Value' for the purpose of setting the value of variables, etc.

- defaults ： Defines the initial values of global variables (name).
- properties :  Defines bot properties used in pattern :ref:`bot<pattern_bot>` and template :ref:`bot<template_bot>` nodes.
- maps ： Defines the key/value relationship used by the :ref:`map<template_map>` node.
- nlu_servers : Configure the NLU server.

defaults
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| The ``defaults``  entity enables you to set the value of global variables (name) at initial startup for use in scenarios.
| However, if there is a dialog information history for each user, this setting is disabled because the latest value on the history is reflected.
| That is, it is only valid for new users.
| The following example specifies that name variable: initial_variable should be set to value: "initial value".

.. code::

  initial_variable: initial value


.. _storage_file_properties:

properties
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``properties`` entity sets parameters that determine the default behavior of the bot, along with information references at the bot node.

.. csv-table::
    :header: "Entity","Content","Description","Default Value"
    :widths: 10,30,30,30

    "name","bot name","The value obtained when the :ref:`bot<template_bot>` attribute name is specified as name.","(If unspecified, the value of default-get)"
    "birthdate","bot creation date","The value obtained when the :ref:`bot<template_bot>` attribute name is specified as name.","(If unspecified, the value of default-get)"
    "grammar_version","grammar version","The value obtained when specifying grammar_version for the :ref:`bot<template_bot>`  attribute name.","(If unspecified, the value of default-get)"
    "app_version","app version","The value obtained when app_version is specified for the :ref:`bot<template_bot>` attribute name.","(If unspecified, the value of default-get)"
    "default-response","default response","Return if no pattern matches.","unknown"
    "default-get","default get","A string that can be retrieved by a get on an undefined variable.","unknown"
    "joiner_terminator","end-of-sentence character","Specifies the character string to which the ending phrase of the response sentence is added. If not specified, nothing is given.","."
    "joiner_join_chars","excluded end-of-sentence character","Specifies a string that is used to combine and exclude the joiner_terminator character when automatically appending the statement terminator with the joiner_terminator specification. If not specified, the character specified by joiner_terminator is appended.",".?!"
    "splitter_split_chars","sentence separator","Specifies the character used internally to devide a sentence. If the specified string is included in the statement, it is treated as a multiple statement, and response returns a string created by combining multiple response statements. However, metadata only returns what you set in the final statement. If not specified, the utterance is treated as one sentence.","."
    "punctation_chars","delimiter","Specifies the character to be treated as a delimiter. Delimiters are excluded from matching, and the matching process is performed to the form created by excluding delimiters from utterance sentences and reponse sentences.","(None)"

* Configuration Example

.. code::

  name:Basic Response
  birthdate:March 01, 2019

  grammar_version:0.0.1
  app_version: 0.0.1

  default-response: Sorry, I didn't understand.
  default-get:  I don't know.
  version: v0.0.1

  joiner_terminator: .
  joiner_join_chars: .?!
  splitter_split_chars: .
  punctation_chars: ;'",!()[]：’”；


joiner_terminator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Specifies the character string to which the ending phrase of the response sentence is automatically added.
If you specify "Hello" for template,

* Configuration Example

.. code::

  joiner_terminator: .

.. code:: xml

    <category>
        <pattern> Hello </pattern>
        <template>Let's have a great day. </template>
    </category>


| Input: Hello
| Output: Let's have a great day.


The response of the dialog API response: To prevent a punctuation mark from being added at the end of the response, define the following.
(You can disable it by leaving after ":" blank.)

.. code::

  joiner_terminator:

If unspecified, no punctuation is added to the response.

| Input: Hello
| Output: Let's have a great day


joiner_join_chars
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| Specifies a string that is used to combine and exclude the joiner_terminator character when automatically appending the statement terminator with the joiner_terminator specification.
| In the case where joiner_join_chars is not specified, if you include a response such as "Let's have a great day." or "I feel great!" and combine the response with the characters specified by the joiner_terminator, it adds to the response the ending character, e.g., punctuation mark, specified by joiner_terminator even if the response already has the ending character and returns a sentence such as "Let's have a great day.." or "I feel great!.".


* Configuration Example

.. code::

  joiner_terminator: .
  joiner_join_chars: .?!

.. code:: xml

    <category>
        <pattern> Hello </pattern>
        <template>Let's have a great day.</template>
    </category>
    <category>
        <pattern>It's nice weather today, too.</pattern>
        <template> I feel great! </template>
    </category>

| Input: Hello
| Output: Let's have a great day.
| Input: It's nice weather today, too
| Output: I feel great!


If joiner_join_chars is unspecified, the characters specified in joiner_terminator are always combined.

.. code::

  joiner_terminator: .
  joiner_join_chars:

| Input: Hello
| Output: Let's have a great day..
| Input: It's nice weather today, too
| Output: I feel great!.


splitter_split_chars
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Specifies the character used intrernally to divide a sentence.
If the specified string is included in the statement, it is treated as a multiple statement, and returns a string created by combining multiple response statements.
For the "Hello. It's nice weather today, too." utterance, if you specify ". " for splitter_split_chars,
it will process 2 statements: "Hello." and "It's nice weather today, too.".

* Configuration Example

.. code::

  joiner_terminator: .
  splitter_split_chars:  .

.. code:: xml

    <category>
        <pattern>Hello. </pattern>
        <template>It's nice weather today, too.</template>
    </category>
    <category>
        <pattern>Let's have a great day. </pattern>
        <template>I feel great.</template>
    </category>

| Input: Hello. It's nice weather today, too.
| Output:  Let's have a great day. I feel great.

If splitter_split_chars is not specified, the utterance is not split, so matching with "Hello. It's nice weather today, too." as a single sentence is performed, and no response is given because there is no matching utterance in AIML described above.

.. code::

  splitter_split_chars:


| Input: Hello. It's nice weather today, too.
| Output: Sorry, I didn't understand.


punctation_chars
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Specifies the character to be treated as a delimiter.Delimiters are excluded from the matching process and are excluded from utterance sentences.
If you have the input "Hello." and "Hello", the characters in punctation_chars are ignored and treated as the same utterance.

* Configuration Example

.. code:: 

  punctation_chars: ;'",!()[]：’”；

.. code:: xml

    <category>
        <pattern>Hello</pattern>
        <template>Let's have a great day</template>
    </category>

| Input: Hello.
| Output:  Let's have a great day.
| Input: Hello
| Output: Let's have a great day.


If punctation_chars is not specified, "." is also matched, so "Hello." and "Hello" are treated as the different utterances.

.. code::

  punctation_chars:

| Input: Hello
| Output: Let's have a great day.
| Input: Hello.
| Output: Sorry, I didn't understand.



maps
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| In the ``maps``  entity, information references in the map node are done by file names (exclude extension), so you can separate files by type of information.
| The following example is from prefectural_office.txt, which lists the relationships between prefectures and prefectural capitals.

.. code::

  Tokyo Metropolis: Tokyo
  Tokyo: Tokyo
  Kanagawa Prefecture: Yokohama City
  Kanagawa: Yokohama City
  Osaka Prefecture: Osaka City
  Osaka: Osaka City
   ：



nlu_servers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``nlu_servers`` entity sets URL for access (endpoint) and API key within the nlu node.
| Please contact COTOBA DESIGN for endpoints and API keys to use within the nlu node. (https://www.cotoba.net)
| The following example shows how to set two URLs: the first URL has no API key set, and the second URL has an API key set.

.. code:: yaml

  nlu:
    - url: http://localhost:5201/run
    - url: http://localhost:3000/run
      apikey: test_key



Word list file
--------------------------------

The file specified by the following entities lists the words and strings to be processed.

- sets ： Defines a list of words to be matched for the :ref:`set<pattern_set>` node.

| The ``sets``  entity is able to separate files for each type of information because the set node references the information by file name (exclude extension).
| In the case of Japanese, a match may not be made depending on the result of word division performed during the match processing, so the match processing is performed as a character string instead of a word.
| The following is an example of prefecture.txt listing the state/province names:.

.. code::

  Tokyo Metropolis
  Tokyo
  Kanagawa Prefecture
  Kanagawa
  Osaka Prefecture
  Osaka
   ：

Regular expression list file
--------------------------------

The file specified by the following entity should be of the form 'regular expression name: regular expression string'.

- regex_templates ：Define regular expression list to be used in template specification of :ref:`regex<pattern_regex>` node.

| The ``regex_templates`` entity is used to commonly define a regular expression string in a file for use in matching operations performed on regex nodes.
| On the regex node side, the template attribute specifies the regular expression name. Regular expression descriptions must be specified on a word basis.
| The description example is as follows.

.. code::

  realize:REALI[Z|S]E
  elevator: elevator | lift
    ：

Conversion dictionary file
--------------------------------
The file specified by the following entity is of the form "Value to be converted", "Converted value" for the purpose of creating a table for translation.

- normal ：Defines a list of conversion tables for the :ref:`normalize<template_normalize>` node.
- denormal ：Defines a list of conversion tables for the  :ref:`denormalize<template_denormalize>` node.
- gender ：Defines a list of conversion tables for the :ref:`gender<template_gender>` node.
- person ：Defines a list of conversion tables for a :ref:`person<template_person>` node.
- person2 ：Defines a list of conversion tables for the :ref:`person2<template_person2>` node.

normal
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| In ``normal`` entities, symbols etc. in strings are converted into independent words by defining them as follows.
| For alphabetic characters, if the 1st character of "Value to be converted" is not '' (Blank), all matches in the subject string are converted (character substitution), assuming symbol conversion.
| On the other hand, if the 1st character is ' '(Blank), the conversion is done by word (word substitution).
| The normalize conversion is done in the following order: character conversion, word conversion. In both cases, blank is inserted before and after the converted text.
| As an example of combining the 2 transforms, you could convert "Mr." to " mister dot " by converting "." to "dot" and "Mr" to "mister".
| Japanese words are converted to units.

.. code::

  ".","dot"
  "/","slash"
  ":","colon"
  "*","_"
  " Mr","mister"
  " can t","can not"


denormal
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| A ``denormal`` entity, which is paired with a ``normal`` entity conversion, is defined as follows and convert words to character strings such as symbols.
| For alphabetic characters, "Value to be converted" is a word, and "Converted value" includes the need for ' '(Blank) when concatenating with the surrounding string. If there is no space, it is concatenated with the surrounding string.
| As the opposite of normalize, if you want to change "mister dot" back to "Mr.", you can also specify "Dot" as "." and "mister "in the" Mr" as 2 separate specifications.

.. code::

  "dot","."
  "slash","/"
  "colon",":"
  "_","*"
  "mister dot"," Mr."
  "can not"," can't "
 

gender,person,person2
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The entities ``gender`` , ``person`` and ``person2`` specify word-by-word conversions; for example, gender defines:.

.. code::

  "he","she"
  "his","her"
  "him","her"
  "her","him"
  "she","he"



Other definition files
--------------------------------

See :doc:`RDF Support<RDF_Support>` for the file format specified by the ``rdfs`` entity.

See :doc:`Security <Security>` for the file format specified by the ``usergroups`` entity.

The following entities contain implementation-dependent content (Class definitions, etc.) and therefore do not need to be described:.

- license_keys ： The license key files required for external connections, etc.
- pattern_nodes ： Contains the processing class definition files for each node of the pattern.
- postprocessors ： The processing class definition file for editing response statements.
- preprocessors ： A processing class definition file for editing user utterances in a request.
- spelling_corpus ： The corpus file from which the spelling checker is based.
- template_nodes ： The processing class definition file for each node of template.
