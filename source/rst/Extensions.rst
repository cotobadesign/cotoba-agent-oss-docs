Extensions
=======================================

Summary
----------------------------------------

Extensions is a feature that allows you to implement extensions separately.
It provides a mechanism to call additional functionality by writing the full Python path in the path of the extension element.

Available extensions
----------------------------------------

TBD

Implementing Custom Extensions
----------------------------------------

The extension must inherit from the base class.

.. code:: python

       programy.extensions.Extension

The following is an example of implementation. The method that executes the contents of the extension element is execute, and implements the execute method with bot, clientid, and data as arguments.
 In data, the content of the extension element is set with a space delimiter.

.. code:: python

    from programy.extensions.base import Extension

    class GeoCodeExtension(Extension):
        __metaclass__ = ABCMeta

        @abstractmethod
        def execute(self, context, data):
            latlng = getGeoCode(data)
            if latlng is not None:
                return "%s,%s"%(
                    latlng.latitude, latlng.longitude
                )

            return None

When using a custom extension using extension in AIML, describe the extension element as shown in the example below.
As the node processing of the AIML extension tag, the class defined by the attribute path is loaded and execute() is called. The return value of execute() is the result of template.
The return value of execute() is the result of template.

.. code:: xml

   <category>
       <pattern>
           What are the coordinates of * ?
       </pattern>
       <template>
            Latitude and longitude are,
            <extension path="programy.extensions.geocode.geocode.GeoCodeExtension">
                <star />
            </extension>
            .
       </template>
   </category>

Input: What are the coordinates of Tokyo Station?
Output: The latitude and the longitude are 35.681236, 139.767125.