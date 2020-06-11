Configuration
=====================================

.. _configuration_file:

Configuration file
-----------------------------

| The contents of the configuration file are used as parameters.
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
-  -–cformat [yaml|json|xml] Specify the format of the config file. If not specified, the extension of the configuration file is automatically determined.
-  -–logging [logging configuration] Specify the configuration file path for logging.
-  -–noloop This option does not execute a dialog loop. Read the config file and AIML, and terminate the dialog engine. Use it to check the start of the dialog engine.
-  -—subs Specify an alternate argument setting file. Replace the algebra in the config file with the content specified by subs. For more information, see :ref:`Command Line Option Substitutions<config_subsitutions>`.
| For this tutorial, we will focus on the Yaml version, but the names of values are the same. 
| An example of a basic configuration file using Yaml file format is below.


Config Section
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The configuration file is made upon a number of distinct sections. 
These sections creation a hierarchy of configuration which reflects how the client, bot and brain elements work together.

The basic configuration is client, which provides the implementation of interfaces such as REST, console, slack, and line. In the client, the following settings are mainly used.

-  :doc:`Client <Config_Client>`

   -  Bots
   -  Storage
 

The main configuration of the bot is as follows.

-  :doc:`Bot <Config_Bot>`

   -  conversations
   -  splitter/joiner
   -  spelling
   -  translation
   -  sentiment

The main structure of the brain is as follows.

-  :doc:`Brain <Config_Brain>`

   -  Overrides
   -  Defaults
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

