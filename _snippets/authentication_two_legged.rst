

This task produces a two-legged access token with a scope sufficient to authenticate the remaining tasks in this tutorial.

**********************
Key learnings
**********************

By the end of this task, you will know how to obtain a two-legged access token when the Client ID and Client Secret is known.

****************************
Endpoints used in this task
****************************

You use the following operation for this task:


..  list-table::
    :widths: 15 25 60
    :header-rows: 1

    * - Operation
      - HTTP Request
      - Description
    * - **Get Access Token**
      - `POST /token`_
      - Returns an access token for the specified Client ID and Client Secret by way of a two-legged OAuth flow.

.. _POST /token: /en/docs/oauth/v2/reference/http/gettoken-POST/

***************************
Step 1 - Register an App
***************************

Follow the instructions in the tutorial `Create an App </en/docs/oauth/v2/tutorials/create-app/>`_ . When specifying details of the app, make sure that the  "Model Derivative API" and "Data Management API" are selected.

***************************************************************
Step 2: Convert Client ID and Secret to Base64 encoded string
***************************************************************

You must concatenate your Client ID with the Client Secret and convert it to a Base64 encoded string before you can request a two-legged OAuth access token.

1. Combine your Client ID with your Client Secret as shown below.

   .. code-block:: shell

    <CLIENT_ID>:<CLIENT_SECRET>

2. Use the appropriate function or method in your preferred programming language to encode the combined string to the Base64 format. Examples:

   ===========================  ===============================================
   Programming Language         Method/Function
   ===========================  ===============================================
   JavaScript                   ``btoa()`` function
   Python                       ``b64encode()`` function from the ``base64`` module
   C#                           ``Convert.ToBase64String()`` method
   ===========================  ===============================================

   **Note:** There are online tools that you can use to convert the combined string to a Base64 encoded string.  However, we don't recommend that you use such tools. Exposing your Client ID and Client Secret to an online tool can pose a security threat.
   
   You should end up with a string that looks like ``RjZEbjh5cGVtMWo4UDZzVXo4SVgzcG1Tc09BOTlHVVQ6QVNOa3c4S3F6MXQwV1hISw==``.


***************************************************************
Step 3: Use encoded string to obtain an Access Token
***************************************************************

Call the `POST token </en/docs/oauth/v2/reference/http/gettoken-POST>`_ endpoint:

The Base64 encoded Client ID + Client Secret are passed through the ``Authorization`` header. The ``grant_type`` and ``scope`` are specified as form fields in the request body.

Request
=======

.. code-block:: shell

    curl -v 'https://developer.api.autodesk.com/authentication/v2/token' \
       -X 'POST' \
       -H 'Content-Type: application/x-www-form-urlencoded' \
       -H 'Accept: application/json' \
       -H 'Authorization: Basic <BASE64_ENCODED_STRING_FROM_STEP_1>' \
       -d 'grant_type=client_credentials' \
       -d 'scope=data:write data:read bucket:create bucket:delete'



Response
========

.. code-block:: json
  :emphasize-lines: 4

  {
    "token_type": "Bearer",
    "expires_in": 1799,
    "access_token": "<YOUR_ACCESS_TOKEN>"

  }

**Notes:**

- Copy the access token (indicated by ``<YOUR_ACCESS_TOKEN>`` in the preceding example) in the response. You use this value for all subsequent requests in this tutorial. The token remains valid for an hour.
- The access token expires in the number of seconds specified by the ``expires_in`` attribute.
- Although the scope specified in the request is ``data:write data:read bucket:create bucket:delete``, Model Derivative requires only the scopes ``data:write`` and  ``data:read``. The scopes ``bucket:create bucket:delete`` are for HTTP requests to the Forge Data Management API.
