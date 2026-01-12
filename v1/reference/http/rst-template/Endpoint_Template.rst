Action

###############################
METHOD endpoint
###############################

.. code-block:: shell

  Use this page as a template if you are directly writing the API Reference in RST.


<*Briefly describe what this does. For example, Returns a time-limited link to an output file of the specified format.*>


  <*If you are writing the API reference as a Swagger/OAS yml file, see* https://wiki.autodesk.com/x/GtCNLg *for guidelines*>

  <*If you are directly writing the API reference in RST, see* https://wiki.autodesk.com/x/LKolFw *for guidelines.*>


********************
Resource Information
********************

======================   ==================================================================================
Method and URI           DELETE **\https://developer.api.autodesk.com/photo-to-3d/v1/photoscene/:photosceneid**
Authentication Context   **App only  User context required  (Delete inapplicable)**
Required OAuth Scopes    ``data:write`` ``data write`` (Delete inapplicable)
Data Format              **JSON**
======================   ==================================================================================

Request
=======

*******
Headers
*******

=============   ========    ==========   ==================================================================
Name            Required    Value Type   Description
=============   ========    ==========   ==================================================================
Authorization   yes         string       Must be ``Bearer <token>``, where ``<token>`` is obtained via `OAuth </en/docs/oauth/v1/reference/http/authenticate-POST>`_.
Content-Type    yes         string       Must be ``application/json``.
=============   ========    ==========   ==================================================================

Request
=======

**************
URI Parameters
**************

============   ========    ==========   =====================================================
Name           Required    Value Type   Description
============   ========    ==========   =====================================================
photosceneid   yes         string       Specifies the ID of the photoscene to delete.
============   ========    ==========   =====================================================

Request
=======

***********************
Query String Parameters
***********************

=============   ========   ==========   =============================================================================================================
Name            Required   Value Type   Description
=============   ========   ==========   =============================================================================================================
client_id       yes        string       Client ID of the app.
response_type   yes        string       The value MUST be either ``code`` for authorization code grant flow or
                                        ``token`` for implicit grant flow.
redirect_uri    yes        string       URL-encoded callback URL that the end user will be redirected to after completing the authorization flow,
                                        which can include query parameters and any other valid URL construct.

                                        **Note:** This must match the pattern of the callback URL field of the app's registration in the
                                        `My Apps </myapps>`_ section. The pattern may include wildcards after the hostname, allowing
                                        different ``redirect_uri`` values to be specified in different parts of your app.
scope           no         string       A URL-encoded, space-separated list of requested scopes.
                                        *Note: A URL-encoded space is* ``%20``.

                                        **Note:** See the `Scopes </en/docs/oauth/v1/overview/scopes>`_
                                        page for more information on when scopes are required.*
state           no         string       A URL-encoded payload containing arbitrary data that the authentication flow will pass back verbatim in
                                        a ``state`` query parameter to the callback URL.
=============   ========   ==========   =============================================================================================================

Request
=======

****************
Form Parameters
****************

=============   ========   ==========   ==================================================================
Name            Required   Value Type   Description
=============   ========   ==========   ==================================================================
client_id       yes        string       Client ID of the app
client_secret   yes        string       Client secret of the app
grant_type      yes        string       Must be ``client_credentials``
scope           no         string       Space-separated list of required scopes
                                        **Note:** A URL-encoded space is* ``%20``.

                                        *See the* `Scopes </en/docs/oauth/v1/overview/scopes>`_
                                        *page for more information on when scopes are required.*
=============   ========   ==========   ==================================================================

Request
=======

**************
Body Structure
**************

============================================================  ========   =============    ==============================================================================================================================
Attribute                                                     Required   Value Type       Description
============================================================  ========   =============    ==============================================================================================================================
Id                                                            yes        string           Name of the Activity.
Instruction                                                   yes        object           Describes the actions to be taken.
Instruction.Script                                            yes        String           Content that the service worker uses to create an AutoCAD script file locally for the AutoCAD core engine to execute.
Instruction.CommandLineParameters                             no         string           Additional command line parameters that are passed from the service worker to the new session of the AutoCAD core engine.
AppPackages                                                   yes        array: string    IDs of the AppPackages to reference.
RequiredEngineVersion                                         yes        enum: string     Version of the AutoCAD core engine to execute the Activity. Possible values: ``20.1``, ``21.0``, ``22.0``.
Parameters                                                    yes        object           Specifies an object that contains the names of the drawings specified in the ``Instruction`` object
Parameters.InputParameters                                    yes        array: object    An array of objects, where each element contains an object that represents the names of the actual drawings specified in the ``Instruction`` object.
Parameters.InputParameters.InputParameters[i]                 yes        object           An object that represents a drawing specified in the ``Instruction`` object.
Parameters.InputParameters.InputParameters[i].Name            yes        string           Name of an input Argument object; must be unique within this InputParameters array
Parameters.InputParameters.InputParameters[i].LocalFileName   yes        string           File name for a locally saved WorkItem input Argument after the resource has been downloaded.
AllowedChildProcesses                                         yes        array: object    Child processes that will be spawned by the activity
Version                                                       yes        int              Version number of the Activity
Description                                                   no         string           Additional details about the Activity
HostApplication                                               no         string           Name of the application to execute a specified WorkItem
IsPublic                                                      yes        bool             Specifies whether the Activity can be publicly targeted
============================================================  ========   =============    ==============================================================================================================================

Response
========

************************
HTTP Status Code Summary
************************

================  ======================  ===============================================================================================================
HTTP Status Code  Message                 Meaning
================  ======================  ===============================================================================================================
200               OK                      Specified action was successfully completed
400               BAD REQUEST             The request was invalid. Check the response body for details
401               UNAUTHORIZED            The access token is invalid
403               FORBIDDEN               The request was refused. Check the response body for details
415               UNSUPPORTED MEDIA TYPE  The ``Content-Type`` header is missing or specifies an unsupported value
429               TOO MANY REQUESTS       The rate limit has been exceeded; wait some time before retrying
500               INTERNAL SERVER ERROR   An unexpected condition was encountered
================  ======================  ===============================================================================================================

Response
========

********************
Body Structure (200)
********************

==================   ==========   ==========================================================================
Name                 Value Type   Description
==================   ==========   ==========================================================================
photoscene           object       A JSON object containing details of the assets that were deleted
photoscene.deleted   int          The number of assets that were deleted
==================   ==========   ==========================================================================

Response
========

********************
Body Structure (400)
********************

==========   ==========   =====================================
Name         Value Type   Description
==========   ==========   =====================================
error        object       A JSON object containing details of the error that occurred.
error.code   int          A code that identifies the error type. See Error List for possible values.
error.msg    string       A short, human readable explanation of the error.
==========   ==========   =====================================

**Note:** See `Error Handling </en/docs/oas_example_single_project/v1/developers_guide/error_handling>`_ for a full list of errors returned by this service.


**********
Example 1
**********
This example demonstrates successfully deleting the photoscene bearing photosceneid AtAuFsedTdqWdhF9VzHepp5oM9PITiuizI4xdMbz.

Request
=======
.. code-block:: shell

  curl -v 'https://developer.api.autodesk.com/photo-to-3d/v1/photoscene/AtAuFsedTdqWdhF9VzHepp5oM9PITiuizI4xdMbz' \
    -X 'DELETE' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer eyjhbGCIOIjIuzI1NiISimtpZCI6...'

Response
========
.. code-block:: json

  {
    "photoscene": {
      "deleted": "7"
    }
  }

**********
Example 2
**********
This example demonstrates an attempt to delete a non-existent photoscene.

Request
=======
.. code-block:: shell

  curl -v 'https://developer.api.autodesk.com/photo-to-3d/v1/photoscene/BtAuFsedTdqWdhF9VzHepp5oM9PITiuizI4xdMbz' \
    -X 'DELETE' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer eyjhbGCIOIjIuzI1NiISimtpZCI6...'

Response
========
.. code-block:: json

  {
    "error": {
      "code": "19",
      "msg": "Specified photoscene ID doesn't exist in the database"
    }
  }
