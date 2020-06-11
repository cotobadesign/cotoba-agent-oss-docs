Pre/Post Processors
============================

| There are two types of processors in the dialog engine: Pre Processor and Post Processor.
| It is used when the utterance sentence needs to be processed before the dialog processing in the dialog engine, and when the response sentence from the dialog engine needs to be processed before the return.

- The `Pre Processors <#pre-processors>`__  are processors that pre-process strings before processing them internally in the dialogue engine.
- The `Post Processors <#post-processors>`__ are processors that post-process strings before they are returned by the API for the response sentences that have been interacted with inside the dialog engine.
Pre Processors
-----------------------------

Pre Processors inherit from the following abstract base class.

.. code:: python

       programy.processors.processing.PreProcessor


This class has a single method, process, which preprocesses the input string and returns the processed string. 
The returned string is used within the dialog engine to proceed with the dialog.

.. code:: python

       class PreProcessor(Processor):

           def __init__(self):
               Processor.__init__(self)

           @abstractmethod
           def process(self, bot, clientid, string):
               pass

Post Processors
-----------------------------

Post Processors inherit from the following abstract base class.

.. code:: python

       programy.processors.processing.PostProcessor

This class has a single method, process, which preprocesses the input string and returns the processed string. 
Use the returned string as a response to the end user.

.. code:: python

       class PostProcessor(Processor):
           def __init__(self):
               Processor.__init__(self)

           @abstractmethod
           def process(self, bot, clientid, string):
               pass
