OOB
============================

For each ``OOB(Out of Band)``  node to be used, it is necessary to implement a Python class and associate it with config.
The OOB implementation base class is defined as follows.

.. csv-table::
    :header: "Parameter Name","Description"
    :widths: 20,20,70

    "default","",""
    "","classname","Defines the python path for the default OOB."
    "Name of the OOB","","The name set in this item is the OOB name specified in AIML. In the example, 'email' is an OOB tag."
    "","classname","Defines the path to python for the OOB to implement. The child elements to be implemented in OOB are defined in the implementation class and need not be specified in config."



.. code:: python

   import xml.etree.ElementTree as ET

   class OutOfBandProcessor(object):

       def __init__(self):
           return

       # Override this method to extract the data for your command
       # See actual implementations for details of how to do this
       def parse_oob_xml(self, oob: ET.Element):
           return

       # Override this method in your own class to do something
       # useful with the command data
       def execute_oob_command(self, bot, clientid):
           return ""

       def process_out_of_bounds(self, bot, clientid, oob):
           if self.parse_oob_xml(oob) is True:
               return self.execute_oob_command(bot, clientid)
           else:
               return ""

If there is an OOB function to send e-mail, AIML using OOB is described as follows.

.. code:: xml

   <oob>
       <email>
           <to>Destination </to>
           <subject>Subject </subject>
           <body>Articles </body>
       </email>
   </oob>

The implementation class looks like the following.
Get and hold child elements with parse_oob_xml() method, and implement the process to actually send mail by send_oob_command () method.

.. code:: python

   class EmailOutOfBandProcessor(OutOfBandProcessor):

       def __init__(self):
           OutOfBandProcessor.__init__(self)
           self._to = None
           self._subject = None
           self._body = None

       def parse_oob_xml(self, oob: ET.Element):
           for child in oob:
               if child.tag == 'to':
                   self._to = child.text
               elif child.tag == 'subject':
                   self._subject = child.text
               elif child.tag == 'body':
                   self._body = child.text
               else:
                   logging.error ("Unknown child element [%s] in email oob"%(child.tag))

               if self._to is not None and \
                   self._subject is not None and \
                   self.body is not None:
                   return True

               logging.error("Invalid email oob command")
               return False

       def execute_oob_command(self, bot, clientid):
           logging.info("EmailOutOfBandProcessor: Emailing=%s", self._to)
           return ""



For OOB settings, configure the following settings in config.yaml.

.. code:: yaml

    oob:
        default:
            classname: programy.oob.default.DefaultOutOfBandProcessor
        email:
            classname: programy.oob.email.EmailOutOfBandProcessor



For more information on OOB settings, see :doc:`OOB settings<config/Config_Brain_OOB>`.
