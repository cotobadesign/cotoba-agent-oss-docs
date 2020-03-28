Brain Configuration
==========================

For brain configuration, brain sets dialog processing and individual settings for each element. 
In addition, it consists of the following subsections depending on the processing contents in the brain.

The configuration section is as follows:

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

Configuration Example

.. code:: yaml

   brain:

      overrides:
        allow_system_aiml: true
        allow_learn_aiml: true
        allow_learnf_aiml: true

      defaults:
        default-get: unknown
        default-property: unknown
        default-map: unknown
        learnf-path: file

      binaries:
        save_binary: true
        load_binary: true
        load_aiml_on_binary_fail: true

      braintree:
        create: true

      services:
          REST:
              classname: programy.services.rest.GenericRESTService
              method: GET
              host: 0.0.0.0
              port: 8080
          Pannous:
              classname: programy.services.pannous.PannousService
              url: http://weannie.pannous.com/api

      security:
          authentication:
              classname: programy.security.authenticate.passthrough.BasicPassThroughAuthenticationService
              denied_srai: AUTHENTICATION_FAILED
          authorisation:
              classname: programy.security.authorise.usergroupsauthorisor.BasicUserGroupAuthorisationService
              denied_srai: AUTHORISATION_FAILED
              usergroups:
                storage: file

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

      dynamic:
          variables:
              gettime: programy.dynamic.variables.datetime.GetTime
          sets:
              numeric: programy.dynamic.sets.numeric.IsNumeric
              roman:   programy.dynamic.sets.roman.IsRomanNumeral
          maps:
              romantodec: programy.dynamic.maps.roman.MapRomanToDecimal
              dectoroman: programy.dynamic.maps.roman.MapDecimalToRoman
