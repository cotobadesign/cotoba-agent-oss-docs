Configuration
=====================================

.. _configuration_file:

Configuration file
-----------------------------

| The contents described in the configuration file are used as various parameters.
| When the dialog engine is started, specify the configuration file with the --config option.
| The format that can be specified in the config file is any of .yaml, .json, .xml.
| The format is determined by specifying the format with the -–cformat option or the extension of the configuration file specified with the ––config option.

Configuration Example

::

   python3 ../../src/programy/clients/console.py --config ./config.yaml --cformat yaml --logging ./logging.yaml 


.. _configuration_command_line_subsitutions:

Command Line Options
-----------------------------

| There are command-line options that are used to control startup.
| This command line option can specify the same option for all clients.

-  -–config [config file path] Specify the path of the config file to be used when executing the client.
-  -–cformat [yaml|json|xml] Specify the format of the config file. If specified and not specified, it is automatically determined from the extension of the configuration file.
-  -–logging [logging configuration] Specify the configuration file path for logging.
-  -–noloop This option does not execute a dialog loop. Read the config file and AIML, and terminate the dialog engine. Use it to check the start of the dialog engine.
-  -—subs Alternate argument setting file specification. Replaces the algebra described in the config file with the contents specified by subs. See  :ref:`Command Line Option Substitution<config_subsitutions>` for more information.

| For this tutorial, we will focus on the Yaml version, but the names of values are the same. 
| An example of a basic configuration file using Yaml file format is below.


Config Section
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The configuration file is made upon a number of distinct sections. 
These sections creation a hierarchy of configuration which reflects how the client, bot and brain elements work together.

The basic configuration is client.
They provide implementations of various interfaces such as REST, console, slack, line, etc. 
The main settings for client are the following.

-  :doc:`Client <Config_Client>`

   -  Bots
   -  Storage
   -  Renderer
   -  Scheduler
   -  Email

The main configuration of the bot is as follows.

-  :doc:`Bot <Config_Bot>`

   -  Brains
   -  Conversations
   -  Sentence Splitting/Joining
   -  Spelling
   -  Translations
   -  Sentiment Analysis

The main structure of the brain is as follows.

-  :doc:`Brain <Config_Brain>`

   -  Overrides
   -  Default Responses
   -  Initial Questions
   -  Binaries
   -  Braintree
   -  Services
   -  Security
   -  OOB
   -  Dynamic Maps and Sets
   -  Tokenizers
   -  Debug Files

The log structure is specified in the following file.

-  :ref:`Logging <config_logging>`

