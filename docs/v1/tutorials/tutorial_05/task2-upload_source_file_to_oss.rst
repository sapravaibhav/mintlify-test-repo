########################################################################
Task 2 – Upload Source File to OSS
########################################################################

.. code-block:: shell

  Use this as a template is for a Task topic in a multi-topic tutorial.
  It is newer and is meant to be accompanied by a Postman Collection

  Guidelines are available at https://wiki.autodesk.com/x/a-ODGQ


<*Guidelines are available at* https://wiki.autodesk.com/x/a-ODGQ >

The Object Storage Service (OSS) is a generic Cloud Storage Service that is part of the Data Management API. In this task, you upload a Revit model to OSS.
While you can use any model, for this tutorial we recommend that you use the Revit model we provide ( *rac_basic_sample_project.rvt* ). You can get this file from the *tutorial_data* folder of the GitHub repository containing the Postman Collection for this tutorial.

***********************
Key learnings
***********************

By the end of this task you will be able to:

- Create a Bucket to store the files.
- Obtain a signed URL to upload a file to the bucket.
- Upload a file to the Bucket.
- Obtain the URN of the uploaded file.
- Convert the URN to a Base64-encoded URN.

****************************
Endpoints used in this task
****************************

You use the following operations in this task:

..  list-table::
    :widths: 15 25 60
    :header-rows: 1

    * - Operation
      - HTTP Request
      - Description
    * - **Create a Bucket**
      - `POST /buckets`_
      - Creates a new OSS Bucket.
    * - **Get a Signed Upload URL**
      - `GET /buckets/:bucketKey/objects/:objectKey/signeds3upload`_
      - Requests an S3 signed URL (or an array of URLs, for uploading in chunks) to upload an object to an OSS bucket.
    * - **Finalize Upload**
      - `POST /buckets/:bucketKey/objects/:objectKey/signeds3upload`_
      - Instructs OSS that all data for the object has been uploaded and object creation can start.


.. _POST /buckets: /en/docs/data/v2/reference/http/buckets-POST/
.. _GET /buckets/:bucketKey/objects/:objectKey/signeds3upload: /en/docs/data/v2/reference/http/buckets-:bucketKey-objects-:objectKey-signeds3upload-POST/
.. _POST /buckets/:bucketKey/objects/:objectKey/signeds3upload: /en/docs/data/v2/reference/http/buckets-:bucketKey-objects-:objectKey-signeds3upload-POST/


*************************
Step 1 - Create a Bucket
*************************

The first thing to do is create a Bucket to hold the Revit model. Once a Bucket is created, you can store files in it as Objects.


Request
=======

.. code-block:: ruby

    curl -X POST \
        'https://developer.api.autodesk.com/oss/v2/buckets' \
        -H 'Authorization: Bearer <YOUR_ACCESS_TOKEN>' \
        -H 'Content-Type: application/json' \
        -d '{
            "bucketKey": "<YOUR_BUCKET_KEY>",
            "access": "full",
            "policyKey": "transient"
            }'

**Notes:**

- You must specify a name for your Bucket.  Replace ``<YOUR_BUCKET_KEY>`` with the name for the Bucket.
- Bucket keys can only be made up of lower case characters, numbers 0-9, and the underscore (_) character.
- The Bucket key must be unique throughout all of the OSS service. If the Bucket key is already in use (even by another user) the server returns a ``409 Conflict`` error. In such a case, retry with another Bucket name.


|

Response
========

.. code-block:: json

  {
      "bucketKey": "<YOUR_BUCKET_KEY>",
      "bucketOwner": "<YOUR_FORGE_APP_CLIENT_ID>",
      "createdDate": 1571296694595,
      "permissions": [
          {
              "authId": "T05H372IE11Kmkksdh73ndj0qie2f6nib",
              "access": "full"
          }
      ],
      "policyKey": "transient"
  }

*****************************
Step 2 - Obtain signed URL
*****************************

To upload a file to an OSS bucket, you need to have a signed upload URL. To obtain a signed URL:

Request
=======

.. code-block:: ruby

    curl -X GET \
        'https://developer.api.autodesk.com/oss/v2/buckets/<YOUR_BUCKET_KEY>/objects/<YOUR_OBJECT_KEY>/signeds3upload?minutesExpiration=<LIFESPAN_OF_URL>' \
        -H 'Authorization: Bearer <YOUR_ACCESS_TOKEN>' \
        -H 'Content-Type: application/json' \
        -d '{
            "ossbucketKey": "<YOUR_BUCKET_KEY>",
            "ossSourceFileObjectKey": "<YOUR_OBJECT_KEY>",
            "access": "full",
            "policyKey": "transient"
            }'

**Notes:**

- Replace <YOUR_ACCESS_TOKEN> with the access token you obtained in Task 1.

- Replace <LIFESPAN_OF_URL> with 10. This will ensure that the signed URL that is returned will be valid for 10 minutes.

- Replace <YOUR_OBJECT_KEY> with *rac_basic_sample_project.rvt*, which is the name of the file we recommend that you upload.

|

Response
========

.. code-block:: json
  :emphasize-lines: 2,5

    {
        "uploadKey": "<YOUR_UPLOAD_KEY>",
        "uploadExpiration": "2022-04-08T00:00:00Z",
        "urlExpiration": "2022-04-06T17:59:46Z",
        "urls": ["<SIGNED_UPLOAD_URL>"],
        "location": "https://developer.api.autodesk.com/oss/v2/buckets/<YOUR_BUCKET_KEY>/objects/<YOUR_OBJECT_KEY>"
    }

Note down the values returned for ``uploadKey`` <YOUR_UPLOAD_KEY> and ``urls`` <SIGNED_UPLOAD_URL>. You will use these values in subsequent requests.

*************************
Step 3 - Upload the file
*************************

1. Download the file *rac_basic_sample_project.rvt* from https://github.com/Autodesk-Forge/forge-model-derivative-tutorial-postman/tree/master/tutorial_data.

2. Send a request to Forge, to upload the file to the Bucket.

Request
=======

.. code-block:: ruby

    curl -X PUT \
        '<SIGNED_UPLOAD_URL>'\
        --data-binary '@<PATH_TO_YOUR_FILE_TO_UPLOAD>'


*************************
Step 4 - Finalize upload
*************************

To make the uploaded file available for download, you must finalize the upload. To complete the upload:

Request
=======

.. code-block:: ruby

    curl -X POST \
        'https://developer.api.autodesk.com/oss/v2/buckets/<YOUR_BUCKET_KEY>/objects/<YOUR_OBJECT_KEY>/signeds3upload' \
        -H 'Authorization: Bearer <YOUR_ACCESS_TOKEN>' \
        -H 'Content-Type: application/json' \
        -d '{
            "ossbucketKey": "<YOUR_BUCKET_KEY>",
            "ossSourceFileObjectKey": "<YOUR_OBJECT_KEY>",
            "access": "full",
            "uploadKey": "<YOUR_UPLOAD_KEY>"
            }'

|

Response
========

.. code-block:: json

  {
      "bucketKey": "<YOUR_BUCKET_KEY>",
      "objectId": "<YOUR_OBJECT_ID>",
      "objectKey": "<YOUR_OBJECT_KEY>",
      "size": "17813504",
      "contentType": "application/octet-stream",
      "location": "https://developer.api.autodesk.com/oss/v2/buckets/<YOUR_BUCKET_KEY>/objects/<OBJECT_KEY_4_REVIT_FILE>",
      "permissions": [
          {
              "authId": "<YOUR_ACCESS_TOKEN>",
              "access": "full"
          }
      ],
      "policyKey": "transient"
  }


************************************
Step 3 - Convert the Revit File URN to a Base64-encoded URN
************************************

Most Model Derivative requests require the URN of the source file to be a Base64-encoded URN.

- Use `this online tool <http://www.freeformatter.com/base64-encoder.html>`_ to convert the URN of the source file (The value of ``objectId`` you obtained in the previous step).

For further information, see the full list of `Base64 variants`_.

.. _Base64 variants: https://en.wikipedia.org/wiki/Base64#Variants_summary_table

We recommend using the unpadded option (RFC 6920), since it uses URL-safe characters. The following example shows a URN, its Base64-encoded form, and its URL safe Base64-encoded form:

============================  ==================================================================================================
raw                           ``urn:adsk.objects:os.object:md_tute_01/Tuner.zip``
Base64                        ``dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfdHV0ZV8wMS9UdW5lci56aXA=``
URL-safe Base64 (no padding)  ``dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfdHV0ZV8wMS9UdW5lci56aXA``
============================  ==================================================================================================
