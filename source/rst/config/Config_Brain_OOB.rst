OOB Settings
=====================================

OOB (Out of Band) processing requires a Python class for each OOB child element used.
This class processes OOB commands and executes the necessary commands on the client side.

The following is the default implementation class. In the execution function, only argument check is performed, and no specific action is performed.
In practice, each system requires its own implementation.

.. code:: yaml

    oob:
      default:
        classname: programy.oob.defaults.default.DefaultOutOfBandProcessor
      alarm:
        classname: programy.oob.defaults.alarm.AlarmOutOfBandProcessor
      camera:
        classname: programy.oob.defaults.camera.CameraOutOfBandProcessor
      clear:
        classname: programy.oob.defaults.clear.ClearOutOfBandProcessor
      dial:
        classname: programy.oob.defaults.dial.DialOutOfBandProcessor
      dialog:
        classname: programy.oob.defaults.dialog.DialogOutOfBandProcessor
      email:
        classname: programy.oob.defaults.email.EmailOutOfBandProcessor
      geomap:
        classname: programy.oob.defaults.map.MapOutOfBandProcessor
      schedule:
        classname: programy.oob.defaults.schedule.ScheduleOutOfBandProcessor
      search:
        classname: programy.oob.defaults.search.SearchOutOfBandProcessor
      sms:
        classname: programy.oob.defaults.sms.SMSOutOfBandProcessor
      url:
        classname: programy.oob.defaults.url.URLOutOfBandProcessor
      wifi:
        classname: programy.oob.defaults.wifi.WifiOutOfBandProcessor


.. csv-table::
    :header: "Parameter","Description","Example","Default"
    :widths: 10,20,60,10

    "classname","Full Python classpath for OOB implementation","programy.oob.defaults.alarm.AlarmOutOfBandProcessor","None"

See :doc:`OOB<../OOB>` for OOB content.