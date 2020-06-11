Security
============================

There are two levels of security definitions: authentication and authorisation.

Authentication
----------------------------

Authentication is implemented within the core code brain.
ask_question is called for each user inquiry.
One of its parameters is 'clientid', which is an ID for each user making utterance.
It is recommended that you perform the necessary authentication procedures separately.
In the following example, the 'clientid' is set to 'console' for the console client, but if multiple people are accessing the site, such as by REST, it is necessary to assign a unique ID.

.. code:: python

       def ask_question(self, bot, clientid, sentence) -> str:

           if self.authentication is not None:
               if self.authentication.authenticate(clientid) is False:
                   logging.error("[%s] failed authentication!")
                   return self.authentication.configuration.denied_srai

The authentication service is defined by a dynamically loaded class, and its base class is defined as follows。。

.. code:: python

   class Authenticator(object):

       def __init__(self, configuration: BrainSecurityConfiguration):
           self._configuration = configuration

       @property
       def configuration(self):
           return self._configuration

       def get_default_denied_srai(self):
           return self.configuration.denied_srai

       def authenticate(self, clientid: str):
           return False

This implementation is a simple one that matches IDs in the 'authorised' list. 
For more advanced authentication, an external service can be implemented, but it is recommended that additional authentication procedures be performed when using this program on the server.

.. code:: python

   class ClientIdAuthenticationService(Authenticator):

       def __init__(self, configuration: BrainSecurityConfiguration):
           Authenticator.__init__(self, configuration)
           self.authorised = [
               "console"
           ]

       # Its at this point that we would call a user auth service, and if that passes
       # return True, appending the user to the known authorised list of user
       # This is a very naive approach, and does not cater for users that log out, invalidate
       # their credentials, or have a TTL on their credentials
       # #Exercise for the reader......
       def _auth_clientid(self, clientid):
           authorised = False # call user_auth_service()
           if authorised is True:
               self.authorised.append(clientid)
           return authorised

       def authenticate(self, clientid: str):
           try:
               if clientid in self.authorised:
                   return True
               else:
                   if self._auth_clientid(clientid) is True:
                       return True

                   return False
           except Exception as excep:
               logging.error(str(excep))
               return False

To use the above features, the following items must be enabled in the Security section of config.yaml.

.. code:: yaml

   brain:
       security:
           authentication:
               classname: programy.security.authenticate.clientidauth.ClientIdAuthenticationService
               denied_srai: AUTHENTICATION_FAILED

.. csv-table::
    :header: "Parameter Name","Description"
    :widths: 30,70

    "classname","Defines the path to python which implements the base class 'Authenticator'."
    "denied_srai","If authentication fails, the interpreter can use the document defined in this configuration as a SRAI. (In the above example, 'AUTHENTICATION_FAILED' is set to SRAI.). The AIML file must contain this as a category pattern with appropriate text to indicate that access is denied."


Authorisation
----------------------------

Authorisation is defined by users, groups, and roles.

.. csv-table::
    :header: "Parameter Name","Description"
    :widths: 30,70

    "User","Define authorisation information for a single user. By including users in one or more groups, you can assign both specific and inherited roles."
    "Group","A group of users assigned one or more roles."
    "Role","An arbitrary authority string to be assigned to the user group."


The base authorisation class is defined as follows。

.. code:: python

   class Authoriser(object):

       def __init__(self, configuration: BrainSecurityConfiguration):
           self._configuration = configuration

       @property
       def configuration(self):
           return self._configuration

       def get_default_denied_srai(self):
           return self.configuration.denied_srai

       def authorise(self, userid, role):
           return False

The implementation of this base class for user, group, and role-based authorisation is as follows。

.. code:: python

   class BasicUserGroupAuthorisationService(Authoriser):

       def __init__(self, config: BrainSecurityConfiguration):
           Authoriser.__init__(self, config)
           self.load_users_and_groups()

       def load_users_and_groups(self):

           self._users = {}
           self._groups = {}

           if self.configuration.usergroups is not None:
               loader = UserGroupLoader()
               self._users, self._groups = loader.load_users_and_groups_from_file(self.configuration.usergroups)
           else:
               logging.warning("No user groups defined, authorisation tag will not work!")

       def authorise(self, clientid, role):
           if clientid not in self._users:
               raise AuthorisationException("User [%s] unknown to system!"%clientid)

           if clientid in self._users:
               user = self._users[clientid]
               return user.has_role(role)
           else:
               return False

To use the above features, the following items must be enabled in the Security section of config.yaml.

.. code:: yaml

       security:
           authorisation:
               classname: programy.security.authorise.usergroupsauthorisor.BasicUserGroupAuthorisationService
               denied_srai: AUTHORISATION_FAILED
               usergroups: ../storage/security/roles.yaml


.. csv-table::
    :header: "Parameter Name","Description"
    :widths: 30,70

    "classname","Define the path of python that implements the base class ‘Authenticator’."
    "denied_srai","If the authentication fails, the interpreter can use the text defined in the configuration as the SRAI. (In the above example, 'AUTHORISATION_FAILED' is set to SRAI.) The AIML file should be included as a category pattern with appropriate text to indicate that access is denied."
    "usergroups","Specify users, user groups, and role configuration files."

The format of the role file is as follows.

.. code:: yaml

   users:
     console:
       roles:
         user
       groups:
         sysadmin

   groups:
     sysadmin:
       roles:
         root, admin, system
       groups:
         user

     user:
       roles:
         ask

When using the AIML authorisation method, enclose the template in the 'authorise' tag.
If the input is ''ALLOW ACCESS'' and the user does not have the 'root' privilege associated with them, then SRAI is set to what is defined in denied_srai.

.. code:: xml

       <category>
           <pattern>ALLOW ACCESS</pattern>
           <template>
               <authorise role="root">
                   Access Allowed
               </authorise>
           </template>
       </category>
