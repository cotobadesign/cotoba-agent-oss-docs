Replace Config
===========================

Replacing a License Key
-------------------------

- Function to replace the setting value of license.keys
 When the configuration item of the config is specified as ``LICENSE_KEY:VALUE`` , it is a function to replace it with the value of license.keys.


Configuration Example

| Describe ``LICENSE_KEY:VALUE`` in the config file as shown below.
| ``LICENSE_KEY:`` is a fixed value, and ``VALUE`` specifies the key name as it describes in license.keys.

.. code:: yaml

   client:
     email:
       username: LICENSE_KEY:EMAIL_USERNAME
       password: LICENSE_KEY:EMAIL_PASSWORD

The following entry in license.keys replaces the LICENSE_KEY:VALUE above with the contents of license.keys at runtime.

.. code:: text

   EMAIL_USERNAME=sample@somewere.com
   EMAIL_PASSWORD=qwerty

If you use git, setting license.keys to .gitignore prevents you from unintentionally registering.


.. _config_subsitutions:

Command line options replacing
---------------------------------------

| When you start the dialog engine with the substitutions.txt file as a command line argument, you can specify parameters to be substituted as startup arguments.
| Replace with the file specified by the --subs command line option.

Configuration Example

| Describe $ +VALUE in the config file as shown below.
| VALUE specifies the key name as it describes in substitutions.txt.

.. code:: yaml

   client:
     email:
       host: $EMAIL_HOST
       port: $EMAIL_PORT

The following entry in substitutions.txt will replace the above $VALUE with the contents of substitutions.txt and work at runtime.

.. code:: text

   $EMAIL_HOST:prod_server.com
   $EMAIL_PORT:9999

This allows you to set ``--subs substitutions.txt``  and the replacement list at startup, without editing the config contents, and change the settings at runtime.
