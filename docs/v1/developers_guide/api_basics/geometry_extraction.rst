##########
Extract Geometry
##########


.. code-block:: shell

  In this example, there are too many key concepts to be contained in a single API Basics page.

  This page is a sub topic of the API Basics page.


You can use the Model Derivative API to select specific parts (objects) of a model and export their geometric representations as OBJ files. See the tutorial `Extract Geometry From a Source File </en/docs/model-derivative/v2/tutorials/xtract-geometry-from-source-file/>`_ for the workflow used to extract geometry.

When you translate a model to the SVF format, the Mode Derivative service extracts information about the objects within the model and writes that information to the manifest. You can use the endpoints that query metadata to obtain information required to identify these objects, from the manifest. Using this information you can use `POST job </en/docs/model-derivative/v2/reference/http/job-POST>`_ to translate specified objects to the OBJ format.

**********
Correlation of IDs in OBJ files to the IDs in the source model
**********

When you follow the `Extract Geometry From a Source File </en/docs/model-derivative/v2/tutorials/xtract-geometry-from-source-file/>`_ tutorial, you will arrive at a `step where you retrieve the object hierarchy </en/docs/model-derivative/v2/tutorials/xtract-geometry-from-source-file/task4-extract_metadata/#step-3-retrieve-the-object-tree-of-the-viewable>`_ of the model. The resultant object tree is shown in the following code block. Note the object IDs 5 and 7 that are highlighted on lines 22 and 32 in the code block.

.. code-block:: json
  :emphasize-lines: 22,32

  {
      "data": {
          "type": "objects",
          "objects": [
              {
                  "objectid": 1,
                  "name": "Stapler",
                  "objects": [
                      {
                          "objectid": 2,
                          "name": "Stapler",
                          "objects": [
                              {
                                  "objectid": 3,
                                  "name": "Bottom:1",
                                  "objects": [
                                      {
                                          "objectid": 4,
                                          "name": "Bottom - Back:1",
                                          "objects": [
                                              {
                                                  "objectid": 5,
                                                  "name": "Solid1"
                                              }
                                          ]
                                      },
                                      {
                                          "objectid": 6,
                                          "name": "Bottom - Front:1",
                                          "objects": [
                                              {
                                                  "objectid": 7,
                                                  "name": "Solid1"
                                              }
                                          ]
                                      },
                                      {
                                          "objectid": 8,
                                          "name": "Lower Mount:1",
                                          "objects": [
                                              {
                                                  "objectid": 9,
                                                  "name": "Solid1"
                                              }
                                          ]
                                      },
                                      {
                                          "objectid": 10,
                                          "name": "Spring:1",
                                          "objects": [
                                              {
                                                  "objectid": 11,
                                                  "name": "Solid1"
                                              }
                                          ]
                                      },
                                      {
                                          "objectid": 12,
                                          "name": "Bottom - Anvil:1",
                                          "objects": [
                                              {
                                                  "objectid": 13,
                                                  "name": "Solid1"
                                              }
                                          ]
                                      }
                                  ]
                              },
                              {
                                  "objectid": 14,
                                  "name": "Upper:1",
                                  "objects": [
                                      {
                                          "objectid": 15,
                                          "name": "Upper - Back:1",
                                          "objects": [
                                              {
                                                  "objectid": 16,
                                                  "name": "Solid1"
                                              }
                                          ]
                                      },
                                      {
                                          "objectid": 17,
                                          "name": "Upper - Front:1",
                                          "objects": [
                                              {
                                                  "objectid": 18,
                                                  "name": "Solid1"
                                              }
                                          ]
                                      },
                                      {
                                          "objectid": 19,
                                          "name": "Channel - Spring Clip:1",
                                          "objects": [
                                              {
                                                  "objectid": 20,
                                                  "name": "Solid1"
                                              }
                                          ]
                                      },
                                      {
                                          "objectid": 21,
                                          "name": "Channel:1",
                                          "objects": [
                                              {
                                                  "objectid": 22,
                                                  "name": "Channel - Base:1",
                                                  "objects": [
                                                      {
                                                          "objectid": 23,
                                                          "name": "Solid1"
                                                      }
                                                  ]
                                              },
                                              {
                                                  "objectid": 24,
                                                  "name": "Channel - Pusher:1",
                                                  "objects": [
                                                      {
                                                          "objectid": 25,
                                                          "name": "Solid1"
                                                      }
                                                  ]
                                              },
                                              {
                                                  "objectid": 26,
                                                  "name": "Channel - Cover:1",
                                                  "objects": [
                                                      {
                                                          "objectid": 27,
                                                          "name": "Solid1"
                                                      }
                                                  ]
                                              },
                                              {
                                                  "objectid": 28,
                                                  "name": "Channel - Hinge Pin:1",
                                                  "objects": [
                                                      {
                                                          "objectid": 29,
                                                          "name": "Solid1"
                                                      }
                                                  ]
                                              },
                                              {
                                                  "objectid": 30,
                                                  "name": "Staples:1",
                                                  "objects": [
                                                      {
                                                          "objectid": 31,
                                                          "name": "Solid1"
                                                      }
                                                  ]
                                              },
                                              {
                                                  "objectid": 32,
                                                  "name": "Channel - Bumper:1",
                                                  "objects": [
                                                      {
                                                          "objectid": 33,
                                                          "name": "Solid1"
                                                      }
                                                  ]
                                              }
                                          ]
                                      }
                                  ]
                              }
                          ]
                      }
                  ]
              }
          ]
      }
  }

If you extract object 3, and you open the resulting OBJ file in a text editor, you should see a screen that resembles the following image.

.. image:: /oas_example_single_project/_static/obj_file.png

Note the highlighted lines 5 and 2701. The `g Obj` value corresponds to the object IDs highlighted in the object tree in the code block shown earlier. Note the absence of the IDs 3, 4, and 6 in the OBJ file. OBJ files contain only the IDs of leaf nodes of the object tree. The object IDs 3, 4, and 6 represent parent nodes in the object tree and hence are not written to the OBJ file.

In the context of the Inventor model used in the tutorial `Extract Geometry From a Source File </en/docs/model-derivative/v2/tutorials/xtract-geometry-from-source-file/>`_, parts can be identified in the OBJ file by their object ID. Assemblies however cannot be identified by an ID in the OBJ file.
