Log setting
=====================

.. _config_logging:


Perform logging using the Python logging function.
See the `Python logging documentation <https://docs.python.jp/3/library/logging.config.html#user-defined-objects>`__  for details on how to specify log configuration options.

This is a log setting example to output to /tmp/y-bot.log.

.. code:: yaml

       version: 1
       disable_existing_loggers: False

       formatters:
         simple:
           format: '%(asctime)s  %(name)-10s %(levelname)-7s %(message)s'

       handlers:
         file:
           class: logging.handlers.RotatingFileHandler
           formatter: simple
           filename: /tmp/y-bot.log

       root:
         level: DEBUG
         handlers:
             - file
