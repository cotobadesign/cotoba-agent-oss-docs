pre/post processor
============================

| There are two types of processors in the dialog engine: pre-processor and post-processor.
| It is used when the utterance sentence needs to be processed before the dialog processing in the dialog engine, and when the response sentence from the dialog engine needs to be processed before the return.

-  `Pre Processors <#pre-processors>`__  are preprocessors that process utterance sentences before they are processed in the engine.
-  `Post Processors <#post-processors>`__ are post-processing processors for response sentences before they are returned to the end user.


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
