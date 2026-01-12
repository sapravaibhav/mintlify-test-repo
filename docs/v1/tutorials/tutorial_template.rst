###########################################
Upload Files to BIM 360 Document Management
###########################################

.. code-block:: shell

  This template is meant for BIM360 and ACC Tutorials.

  Older tutorials were based on this template.

  On this page, instructions for each section are provided as italicized text encapsulated by angle brackets.

<*Write the purpose of the tutorial and list the steps*>

This tutorial demonstrates how to upload files to BIM 360 Document Management. The steps include finding the ID of the folder where you want to upload the files, creating an empty storage object for the file, uploading the file to the storage object, creating the first version of the file, and, optionally, creating additional versions of the file.

<*Add general important information and limitations*>

You can upload any type of document to the BIM 360 Project Files folder or to a folder nested under the Project Files folder. However, you can only upload the following types of documents to the Plans folder or to a folder nested under the Plans folder: DWF, DWFX, DWG, IFC, PDF, RVT.

Note that if you upload PDF files to the Plans folder or to a folder nested under the Plans folder, BIM 360 Docs splits the file into separate pages (sheets) and assigns a separate ID to each page. In order to complete the upload process, you need to manually review and publish the file in BIM 360 Docs. For more details, see the `Help documentation <http://help.autodesk.com/view/BIM360D/ENU/?guid=GUID-6B8BCC70-9A71-448E-A69A-1047564B331E>`_.

<*If possible, add a link to the product help documentation for more background information*>

For more details about BIM 360 Document management, see the `Data Management API </en/docs/data/v2/overview/>`_.

****************
Before You Begin
****************

* `Register an app </myapps>`_, and select the Data Management and BIM 360 APIs.
* Acquire a `3-legged OAuth token </en/docs/oauth/v1/tutorials/get-3-legged-token/>`_ with ``data:create`` ``data:read`` and ``data:write`` scopes.
* Verify that you have access to the relevant BIM 360 account, project, and folder.

***********************************************
Step 1: Find the Hub ID for the BIM 360 Account
***********************************************

<*If you need to carry out several steps to complete one larger step, you might need to add a summary paragraph*>

The first few steps of the tutorial demonstrate how to create an empty storage object in the folder where you want to upload the file. This involves iterating through several Forge Data Management endpoints to find the folder ID.

<*Write the task followed by the endpoint name you use to carry out the step. Add a link to the endpoint in the API Reference*>

Find the hub ID for the BIM 360 account that contains the folder you want to upload the file to, by calling `GET hubs </en/docs/data/v2/reference/http/hubs-GET/>`_.

Note that the BIM 360 account ID corresponds to a Data Management hub ID. To convert an account ID into a hub ID you need to add a "**b.**\" prefix. For example, an account ID of c8b0c73d-3ae9 translates to a hub ID of **b.**\c8b0c73d-3ae9.

<*Add request/response payloads in cURL*>

<*Highlight the important line in the response payload using the ``:emphasize-lines: <line number>`` command at the beginning of the code block*>

Request
=======

..  code-block:: shell

    curl -X GET -H "Authorization: Bearer nFRJxzCD8OOUr7hzBwbr06D76zAT" "https://developer.api.autodesk.com/project/v1/hubs"

Response
========

..  code-block:: json
  :emphasize-lines: 13

  {
    "jsonapi": {
      "version": "1.0"
    },
    "links": {
      "self": {
        "href": "https://developer.api.autodesk.com/project/v1/hubs"
      }
    },
    "data": [
      {
        "type": "hubs",
        "id": "b.cGVyc29uYWw6cGUyOWNjZjMy",
        "attributes": {
          "name": "My First Account",
          "extension": {
            "type": "hubs:autodesk.bim360:Account",
            "version": "1.0",
            "schema": {
              "href": "https://developer.api.autodesk.com/schema/v1/versions/hubs:autodesk.bim360:Account-1.0"
            },
            "data": {}
          }
        }
      }
    ]
  }

<*Highlight which attributes the developer needs to be aware of for future steps*>

In this example, assume that the folder you want to upload the file to is in a hub called ``My First Account``.

Find the hub (``data.name``), and note the hub ID - ``b.cGVyc29uYWw6cGUyOWNjZjMy``.

***************************
Step 2: Find the Project ID
***************************

To get a list of all the projects in the hub (account), use the hub ID (``b.cGVyc29uYWw6cGUyOWNjZjMy``) to call `GET hubs/:hub_id/projects </en/docs/data/v2/reference/http/hubs-hub_id-projects-GET>`_. Find the project ID of the project that contains the folder you want to upload the file to.

Note that the project ID in BIM 360 corresponds to the project ID in the `Data Management API </en/docs/data/v2/>`_. To convert a project ID in BIM 360 to a project ID in the Data Management API, you need to add a "**b.**\" prefix. For example, a project ID of a4be0c34a-4ab7 translates to a project ID of **b.**\a4be0c34a-4ab7.

Request
=======

..  code-block:: shell

    curl -X GET -H "Authorization: Bearer nFRJxzCD8OOUr7hzBwbr06D76zAT" "https://developer.api.autodesk.com/project/v1/hubs/b.cGVyc29uYWw6cGUyOWNjZjMy/projects"

Response
========

..  code-block:: json
  :emphasize-lines: 13

  {
    "jsonapi": {
      "version": "1.0"
    },
    "links": {
      "self": {
        "href": "https://developer.api.autodesk.com/project/v1/hubs/b.cGVyc29uYWw6cGUyOWNjZjMy/projects"
      }
    },
    "data": [
      {
        "type": "projects",
        "id": "b.cGVyc29uYWw6d2l",
        "attributes": {
          "name": "My First Project",
          "extension": {
            "type": "projects:autodesk.core:Project",
            "version": "1.0"
          }
        }
      }
    ]
  }


In this example, assume that ``My First Project`` is the project that contains the folder you want to upload the file to.

Find the project (``data.attributes.name``), and note the project ID (``data.id``) - ``b.cGVyc29uYWw6d2l``.

****************************************
Step 3: Find the Folder ID
****************************************

To get the Project Files folder or the Plans folder ID, use the hub ID (``b.cGVyc29uYWw6cGUyOWNjZjMy``) and the project ID (``b.cGVyc29uYWw6d2l``) to call `GET hubs/:hub_id/projects/:project_id/topFolders </en/docs/data/v2/reference/http/hubs-hub_id-projects-project_id-topFolders-GET>`_.

Request
=======

..  code-block:: shell

    curl -X GET -H "Authorization: Bearer nFRJxzCD8OOUr7hzBwbr06D76zAT"
                "https://developer.api.autodesk.com/project/v1/hubs/b.cGVyc29uYWw6cGUyOWNjZjMy/projects/b.cGVyc29uYWw6d2l/topFolders"

Response
========

..  code-block:: json
  :emphasize-lines: 8

  {
    "jsonapi": {
      "version": "1.0"
    },
    "data": [
      {
        "type": "folders",
        "id": "urn:adsk.wipprod:fs.folder:co.BJU3PTc4Sd2CmXM492XUiA",
        "attributes": {
          "name": "Project Files",
          "displayName": "Project Files",
          "createTime": "2017-07-17T13:06:56.0000000Z",
          "createUserId": "",
          "createUserName": "",
          "lastModifiedTime": "2017-09-24T07:46:08.0000000Z",
          "lastModifiedUserId": "X9WYLGPNCHSL",
          "lastModifiedUserName": "John Smith",
          "objectCount": 4,
          "hidden": false,
          "extension": {
            "type": "folders:autodesk.bim360:Folder",
            "version": "1.0",
            "schema": {
              "href": "https://developer.api.autodesk.com/schema/v1/versions/folders:autodesk.bim360:Folder-1.0"
            },
            "data": {
              "visibleTypes": [
                "items:autodesk.bim360:File"
              ],
              "actions": [
                "CONVERT"
              ],
              "allowedTypes": [
                "items:autodesk.bim360:File",
                "folders:autodesk.bim360:Folder"
              ]
            }
          }
        }
      }
    ]
  }

Find the folder (``data.attributes.name``); in this example, the Project Files folder, and note the folder ID (``data.id``) - ``urn:adsk.wipprod:fs.folder:co.BJU3PTc4Sd2CmXM492XUiA``

*********************************
Step 4: Find the Nested Folder ID
*********************************

If you want to upload the document to a folder nested under the Project Files folder or the Plans folder, you need to call `GET projects/:project_id/folders/:folder_id/contents </en/docs/data/v2/reference/http/projects-project_id-folders-folder_id-contents-GET>`_ repeatedly through the hierarchy of folders until you find the Folder ID of the folder you want to upload the document to. For the first iteration, use the Project Files ID (``urn:adsk.wipprod:fs.folder:co.BJU3PTc4Sd2CmXM492XUiA``) or the Plans folder ID.

Request
=======

..  code-block:: shell

    curl -X GET -H "Authorization: Bearer nFRJxzCD8OOUr7hzBwbr06D76zAT"
        "https://developer.api.autodesk.com/data/v1/projects/b.cGVyc29uYWw6d2l/folders/urn:adsk.wipprod:fs.folder:co.BJU3PTc4Sd2CmXM492XUiA/contents"

Response
========

..  code-block:: json
  :emphasize-lines: 8

  {
    "jsonapi": {
      "version": "1.0"
    },
    "data": [
      {
        "type": "folders",
        "id": "urn:adsk.wipprod:fs.folder:co.QneBBX7evT2JSrpeQXga0",
        "attributes": {
          "name": "My First Folder",
          "displayName": "My First Folder",
          "createTime": "2017-07-17T13:06:56.0000000Z",
          "createUserId": "",
          "createUserName": "",
          "lastModifiedTime": "2017-09-24T07:46:08.0000000Z",
          "lastModifiedUserId": "X9WYLGPNCHSL",
          "lastModifiedUserName": "John Smith",
          "objectCount": 4,
          "hidden": false,
          "extension": {
            "type": "folders:autodesk.bim360:Folder",
            "version": "1.0",
            "schema": {
              "href": "https://developer.api.autodesk.com/schema/v1/versions/folders:autodesk.bim360:Folder-1.0"
            },
            "data": {
              "visibleTypes": [
                "items:autodesk.bim360:File"
              ],
              "actions": [
                "CONVERT"
              ],
              "allowedTypes": [
                "items:autodesk.bim360:File",
                "folders:autodesk.bim360:Folder"
              ]
            }
          }
        }
      }
    ]
  }

In this example, assume that ``My First Folder`` is the folder that contains the file you want to download.

Find the folder (``data.attributes.name``), and note the folder ID (``data.id``) - ``urn:adsk.wipprod:fs.folder:co.QneBBX7evT2JSrpeQXga0``.


*******************************
Step 5: Create a Storage Object
*******************************

To create an empty storage object for the file in the folder, use the project ID (``b.cGVyc29uYWw6d2l``) and the folder ID (``urn:adsk.wipprod:fs.folder:co.QneBBX7evT2JSrpeQXga0``) to call `POST projects/:project_id/storage </en/docs/data/v2/reference/http/projects-project_id-storage-POST>`_.

Note that you need to assign a filename and file extension (PNG or JPG) to the ``name`` parameter (``My First File.jpg``).

Request
=======

..  code-block:: shell

    curl -X POST -H "Content-Type: application/vnd.api+json" -H "Accept: application/vnd.api+json" -H "Authorization: Bearer nFRJxzCD8OOUr7hzBwbr06D76zAT"
    "https://developer.api.autodesk.com/data/v1/projects/b.cGVyc29uYWw6d2l/storage"
    -d '{
          "jsonapi": { "version": "1.0" },
          "data": {
            "type": "objects",
            "attributes": {
              "name": "My First File.jpg"
            },
            "relationships": {
              "target": {
                "data": { "type": "folders", "id": "urn:adsk.wipprod:fs.folder:co.QneBBX7evT2JSrpeQXga0" }
              }
            }
          }
    }'

Repsonse
========

..  code-block:: json
  :emphasize-lines: 7

  {
    "jsonapi": {
      "version": "1.0"
    },
    "data": {
      "type": "objects",
      "id": "urn:adsk.objects:os.object:wip.dm.prod/2a6d61f2-49df-4d7b.jpg",
      "relationships": {
        "target": {
          "data": {
            "type": "folders",
            "id": "urn:adsk.wipprod:fs.folder:co.mgS-lb-BThaTdHnhiN_mbA"
          }
        }
      }
    }
  }

Note the object ID (``data.id``) - ``urn:adsk.objects:os.object:wip.dm.prod/2a6d61f2-49df-4d7b.jpg``.

The object ID includes of the following sections: ``<urn:adsk.objects:os.object>:<bucket_key>/<object_name>``

Note the bucket key - ``wip.dm.prod`` and the object name - ``2a6d61f2-49df-4d7b.jpg``

*********************************************
Step 6: Upload the File to the Storage Object
*********************************************

To upload the file to the storage object, use the bucket key (``wip.dm.prod``) and the object name (``2a6d61f2-49df-4d7b.jpg``) to call `PUT buckets/:bucket_key/objects/:object_name </en/docs/data/v2/reference/http/buckets-:bucketKey-objects-:objectName-PUT>`_.

Request
=======

..  code-block:: shell

    curl -X PUT -H "Authorization: Bearer nFRJxzCD8OOUr7hzBwbr06D76zAT" --data-binary @D:\My First File.jpg "https://developer.api.autodesk.com/oss/v2/buckets/wip.dm.prod/objects/2a6d61f2-49df-4d7b.jpg"

Response
========

..  code-block:: json

  {
    "bucketKey" : "wip.dm.prod",
    "objectId" : "urn:adsk.objects:os.object:wip.dm.prod/2a6d61f2-49df-4d7b.jpg",
    "objectKey" : "2ac28abc-9f6e-463d-bcc4-5c194d552beb.jpg",
    "sha1" : "a4b31905990233a2b0374b2b3a666116cfef12ca",
    "size" : 879394,
    "contentType" : "application/octet-stream",
    "location" : "https://developer.api.autodesk.com/oss/v2/buckets/wip.dm.prod/objects2a6d61f2-49df-4d7b.jpg"
  }

The file has been uploaded to the storage object.

********************************************
Step 7: Create the First Version of the File
********************************************

To create the first version of the uploaded file, use the project ID (``b.cGVyc29uYWw6d2l``), the folder ID (``urn:adsk.wipprod:fs.folder:co.BJU3PTc4Sd2CmXM492XUiA``), and the Object ID (``urn:adsk.objects:os.object:wip.dm.prod/2a6d61f2-49df-4d7b.jpg``) to call `POST projects/:project_id/items </en/docs/data/v2/reference/http/projects-project_id-items-POST>`_.

Note that if you upload PDF files to the Plans folder or to a folder nested under the Plans folder, BIM 360 Docs splits the file into separate pages (sheets) and assigns a separate ID to each page. In order to complete the upload process, you need to manually review and publish the file in BIM 360 Docs. For more details, see the `Help documentation <http://help.autodesk.com/view/BIM360D/ENU/?guid=GUID-6B8BCC70-9A71-448E-A69A-1047564B331E>`_.

Request
=======

..  code-block:: shell

    curl -X POST -H "Authorization: Bearer nFRJxzCD8OOUr7hzBwbr06D76zAT" -H "Content-Type: application/vnd.api+json" -H "Accept: application/vnd.api+json"
    "https://developer.api.autodesk.com/data/v1/projects/b.cGVyc29uYWw6d2l/items" -d '{
        "jsonapi": { "version": "1.0" },
        "data": {
          "type": "items",
          "attributes": {
            "displayName": "My First File.jpg",
            "extension": {
              "type": "items:autodesk.bim360:File",
              "version": "1.0"
            }
          },
          "relationships": {
            "tip": {
              "data": {
                "type": "versions", "id": "1"
              }
            },
            "parent": {
              "data": {
                "type": "folders",
                "id": "urn:adsk.wipprod:fs.folder:co.BJU3PTc4Sd2CmXM492XUiA"
              }
            }
          }
        },
        "included": [
          {
            "type": "versions",
            "id": "1",
            "attributes": {
              "name": "My First File.jpg",
              "extension": {
                "type": "versions:autodesk.bim360:File",
                "version": "1.0"
              }
            },
            "relationships": {
              "storage": {
                "data": {
                  "type": "objects",
                  "id": "urn:adsk.objects:os.object:wip.dm.prod/2a6d61f2-49df-4d7b.jpg"
                }
              }
            }
          }
        ]
      }'

Response
========

..  code-block:: json
  :emphasize-lines: 33

  {
    "jsonapi": {
      "version": "1.0"
    },
    "links": {
      "self": {
        "href": "https://developer.api.autodesk.com/data/v1/projects/b.cGVyc29uYWw6d2l/items"
      }
    },
    "data": {
      "type": "items",
      "id": "urn:adsk.wipprod:dm.lineage:AeYgDtcTSuqYoyMweWFhhQ",
      "attributes": {
        "displayName": "My First File.jpg",
        "createTime": "2016-05-30T15:32:05+00:00",
        "createUserId": "KPN8P8P65K",
        "createUserName": "John Smith",
        "lastModifiedTime": "2016-05-30T15:32:05+00:00",
        "lastModifiedUserId": "KPN8P8P65K",
        "lastModifiedUserName": "John Smith",
        "extension": {
          "type": "items:autodesk.core:File",
          "version": "1.0",
          "schema": {
            "href": "https://developer.api.autodesk.com/schema/v1/versions/items%3Aautodesk.core%3AFile-1.0"
          },
          "data": {}
        }
      },
      "included": [
        {
          "type": "versions",
          "id": "urn:adsk.wipprod:fs.file:vf.AeYgDtcTSuqYoyMweWFhhQ?version=1",
          "attributes": {
            "name": "My First File.jpg",
            "displayName": "My First File.jpg",
            "createTime": "2016-05-30T15:32:05+00:00",
            "createUserId": "KPN8P8P65K",
            "createUserName": "John Smith",
            "lastModifiedTime": "2016-05-30T15:32:05+00:00",
            "lastModifiedUserId": "KPN8P8P65K",
            "lastModifiedUserName": "John Smith",
            "versionNumber": 1,
            "mimeType": "application/image",
            "fileType": "jpg",
            "storageSize": 879394,
            "extension": {
              "type": "versions:autodesk.core:File",
              "version": "1.0",
              "schema": {
                "href": "https://developer.api.autodesk.com/schema/v1/versions/versions%3Aautodesk.core%3AFile-1.0"
              },
              "data": {}
            }
          }
        }
      ]
    }
  }

Note the item ID (``data.id``) - ``urn:adsk.wipprod:dm.lineage:AeYgDtcTSuqYoyMweWFhhQ``, and version ID (``inlcuded[id]``) - ``urn:adsk.wipprod:fs.file:vf.AeYgDtcTSuqYoyMweWFhhQ?version=1``.


**********************************************
Step 8: Create Additional Versions of the File
**********************************************

To create an additional version of the file, you need to create a new storage object (step 5), upload the new version to the object (step 6), and use the item ID you retrieved in step 7 to call `POST projects/:project_id/versions </en/docs/data/v2/reference/http/projects-project_id-versions-POST>`_.

Request
=======

..  code-block:: shell

    curl -X POST -H "Authorization: Bearer nFRJxzCD8OOUr7hzBwbr06D76zAT" -H "Content-Type: application/vnd.api+json" -H "Accept: application/vnd.api+json" -d '{
   "jsonapi": { "version": "1.0" },
     "data": {
      "type": "versions",
        "attributes": {
           "name": "New Version of My First File.jpg",
           "extension": { "type": "versions:autodesk.bim360:File", "version": "1.0"}
        },
         "relationships": {
         "item": { "data": { "type": "items", "id": "urn:adsk.wipprod:dm.lineage:AeYgDtcTSuqYoyMweWFhhQ" } },
         "storage": { "data": { "type": "objects", "id": "<new_object_ID>" } }
       }
    }
 }' "https://developer.api.autodesk.com/data/v1/projects/b.cGVyc29uYWw6d2l/versions"



Response
========

..  code-block:: json

  {
    "jsonapi": {
      "version": "1.0"
    },
    "data": {
      "type": "versions",
      "id": "urn:adsk.wipprod:fs.file:vf.1HROnsnfQgq4N0b-nUoGge?version=2",
      "attributes": {
        "name": "New Version of My First File.jpg",
        "displayName": "new version of my first file.jpg",
        "extension": {
          "type": "versions:autodesk.bim360:File",
          "version": "1.0",
          "data": {}
        }
      },
      "relationships": {}
    },
    "included": [
      {
        "type": "items",
        "id": "urn:adsk.wipprod:dm.lineage:AeYgDtcTSuqYoyMweWFhhQ",
        "attributes": {
          "extension": {
            "type": "items:autodesk.bim360:File",
            "version": "1.0",
            "data": {}
          }
        },
        "links": {},
        "relationships": {
          "tip": {
            "data": {
              "type": "versions",
              "id": "urn:adsk.wipprod:fs.file:vf.1HROnsnfQgq4N0b-nUoGge?version=2"
            },
            "links": {}
          },
          "versions": {},
          "parent": {},
          "refs": {},
          "links": {}
        }
      }
    ]
  }
