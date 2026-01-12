##########
Prepare Models for Online Viewing
##########

.. code-block:: shell

  In this example, there are too many key concepts to be contained in a single API Basics page. 

  This page is a sub topic of the API Basics page. 

The Forge recommended way of viewing a model online is to display the model using the Forge Viewer Library. The Forge Viewer Library is a WebGL-based JavaScript SDK that can render 2D and 3D models in a browser. However, before you can display a model with the viewer, you must translate the model to a viewer-friendly format. The viewer currently supports two formats; SVF (Streaming Viewing Format) and its newer incarnation, SVF2. You can use the Model Derivative API to translate more than 70 model formats to SVF and SVF2. Statistics show that translating to SVF/SVF2 is the most common use of the Model Derivative API.


What is SVF?
======================

SVF is a vector graphics format designed for the web. It is the native file format of the viewer. SVF contains all the design information the viewer needs to display graphics, list the hierarchy of objects, override the visibility of objects, and display the properties of each object. Since it is a streaming format, it is the format of choice for online viewing for most models.

What is SVF2?
=============

 SVF2 is a vector graphics format like SVF, specifically designed to reduce the storage size of 3D Viewables generated from models containing repetitive geometry shapes.  Large models typically produced by the AEC (Architecture, Engineering & Construction) industry fall into this category.

SVF2 is an extension of the SVF format. So, if the Viewer is unable to load an SVF2 derivative, it can fall back to loading the equivalent SVF. The main difference between the two formats is the optimized 3D Viewable, which has a greatly reduced storage size. This optimization enables incremental loading and improved interactivity like zooming and exploding. Large models that intermittently fail to load when translated to SVF can be reliably displayed with SVF2.

Because SVF2 is an extension  of SVF, all post translation operations possible with SVF are possible with SVF2.  Even the process of extracting metadata and geometry is the same for both formats. However, because of the extra processing required for optimization, the time taken to translate to SVF2 can be greater than the time taken to translate to SVF.


SVF or SVF2?
============

The decision to use SVF or SVF2 is yours. For large models with considerable shape repetition, SVF2 is the better choice. However, for models that are already translated to SVF and are performing well, there is no significant advantage to be gained by switching to SVF2. For new model translations, especially from the AEC industry (for example, Revit and Navisworks models), SVF2 will likely be the best choice. However, consider the occurrence of repetitive shapes (or the lack of it), when you decide.

Differences between SVF and SVF2 in Manifests
=============================================

In the manifest, SVF2 derivatives do not have a URN. So, SVF2 derivatives cannot be downloaded.

When you translate a model to SVF2, and it already has an SVF derivative, the server does not add SVF2 as another derivative to the manifest.  Instead, the server extends the existing derivative by adding a new parameter, ``"overrideOutputType":"svf2"``, to the existing SVF derivative. See the examples below where Example 1 shows only SVF, Example 2 shows only SVF2, and Example 3 shows an SVF that was later translated to SVF2.

Example 1: Manifest of an Inventor part file (*box.ipt*) translated to SVF
*****************************************************************************


.. code-block:: json
  :emphasize-lines: 69

  {
      "urn": "dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ",
      "derivatives": [
          {
              "hasThumbnail": "true",
              "children": [
                  {
                      "guid": "614c6459-f9a4-4dcf-a264-cee70f958efe",
                      "type": "geometry",
                      "role": "3d",
                      "name": "Scene",
                      "status": "success",
                      "progress": "complete",
                      "hasThumbnail": "true",
                      "children": [
                          {
                              "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/box.svf",
                              "role": "graphics",
                              "mime": "application/autodesk-svf",
                              "guid": "ff2796e0-612c-40de-be5c-e3bafebcd31a",
                              "type": "resource"
                          },
                          {
                              "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/box.svf.png01_thumb_400x400.png",
                              "role": "thumbnail",
                              "mime": "image/png",
                              "guid": "25f134e0-bf9d-461b-85d8-48c5b051f562",
                              "type": "resource",
                              "resolution": [
                                  400,
                                  400
                              ]
                          },
                          {
                              "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/box.svf.png01_thumb_200x200.png",
                              "role": "thumbnail",
                              "mime": "image/png",
                              "guid": "80f23577-efa7-42b2-9043-240c84021b94",
                              "type": "resource",
                              "resolution": [
                                  200,
                                  200
                              ]
                          },
                          {
                              "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/box.svf.png01_thumb_100x100.png",
                              "role": "thumbnail",
                              "mime": "image/png",
                              "guid": "0a124150-ba62-41ca-94f7-8fd7bd323eb5",
                              "type": "resource",
                              "resolution": [
                                  100,
                                  100
                              ]
                          }
                      ]
                  },
                  {
                      "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/properties.db",
                      "role": "Autodesk.CloudPlatform.PropertyDatabase",
                      "mime": "application/autodesk-db",
                      "guid": "715269b4-da1d-4ed6-abe9-9aff13d3a526",
                      "type": "resource",
                      "status": "success"
                  }
              ],
              "name": "box.ipt",
              "progress": "complete",
              "outputType": "svf",
              "status": "success"
          }
      ],
      "hasThumbnail": "true",
      "progress": "complete",
      "type": "manifest",
      "region": "US",
      "version": "1.0",
      "status": "success"
  }

Example 2: Manifest of an Inventor part file (*box.ipt*) translated to SVF2
*******************************************************************************

.. code-block:: json
  :emphasize-lines: 68

  {
      "urn": "dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ",
      "derivatives": [
          {
              "hasThumbnail": "true",
              "children": [
                  {
                      "role": "3d",
                      "hasThumbnail": "true",
                      "children": [
                          {
                              "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/box.svf.png01_thumb_400x400.png",
                              "role": "thumbnail",
                              "mime": "image/png",
                              "guid": "728033a4-1de2-445a-9b59-8c5fe83fe22d",
                              "type": "resource",
                              "resolution": [
                                  400,
                                  400
                              ]
                          },
                          {
                              "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/box.svf.png01_thumb_200x200.png",
                              "role": "thumbnail",
                              "mime": "image/png",
                              "guid": "569e9647-ded5-4352-a778-5c68d9dd8c0e",
                              "type": "resource",
                              "resolution": [
                                  200,
                                  200
                              ]
                          },
                          {
                              "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/box.svf.png01_thumb_100x100.png",
                              "role": "thumbnail",
                              "mime": "image/png",
                              "guid": "b4459f80-6c4f-4823-9029-fae156855d61",
                              "type": "resource",
                              "resolution": [
                                  100,
                                  100
                              ]
                          },
                          {
                              "role": "graphics",
                              "mime": "application/autodesk-svf2",
                              "guid": "c5a580ba-44fe-4d69-844f-1db8ad9ef551",
                              "type": "resource"
                          }
                      ],
                      "name": "Scene",
                      "guid": "c389be1c-ee08-47c4-851e-a61b2e06e7fc",
                      "progress": "complete",
                      "type": "geometry",
                      "status": "success"
                  },
                  {
                      "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/properties.db",
                      "role": "Autodesk.CloudPlatform.PropertyDatabase",
                      "mime": "application/autodesk-db",
                      "guid": "d3b82922-eb3b-4a83-98ac-cfa0371aaab9",
                      "type": "resource",
                      "status": "success"
                  }
              ],
              "name": "box.ipt",
              "progress": "complete",
              "outputType": "svf2",
              "status": "success"
          }
      ],
      "hasThumbnail": "true",
      "progress": "complete",
      "type": "manifest",
      "region": "US",
      "version": "1.0",
      "status": "success"
  }

Example 3: Manifest of an Inventor part file (*box.ipt*) that was first translated to SVF and after that translated to SVF2
***************************************************************************************************************************

.. code-block:: json
  :emphasize-lines: 6,70

  {
      "urn": "dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ",
      "derivatives": [
          {
              "hasThumbnail": "true",
              "overrideOutputType": "svf2",
              "children": [
                  {
                      "guid": "614c6459-f9a4-4dcf-a264-cee70f958efe",
                      "type": "geometry",
                      "role": "3d",
                      "name": "Scene",
                      "status": "success",
                      "progress": "complete",
                      "hasThumbnail": "true",
                      "children": [
                          {
                              "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/box.svf",
                              "role": "graphics",
                              "mime": "application/autodesk-svf",
                              "guid": "ff2796e0-612c-40de-be5c-e3bafebcd31a",
                              "type": "resource"
                          },
                          {
                              "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/box.svf.png01_thumb_400x400.png",
                              "role": "thumbnail",
                              "mime": "image/png",
                              "guid": "25f134e0-bf9d-461b-85d8-48c5b051f562",
                              "type": "resource",
                              "resolution": [
                                  400,
                                  400
                              ]
                          },
                          {
                              "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/box.svf.png01_thumb_200x200.png",
                              "role": "thumbnail",
                              "mime": "image/png",
                              "guid": "80f23577-efa7-42b2-9043-240c84021b94",
                              "type": "resource",
                              "resolution": [
                                  200,
                                  200
                              ]
                          },
                          {
                              "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/box.svf.png01_thumb_100x100.png",
                              "role": "thumbnail",
                              "mime": "image/png",
                              "guid": "0a124150-ba62-41ca-94f7-8fd7bd323eb5",
                              "type": "resource",
                              "resolution": [
                                  100,
                                  100
                              ]
                          }
                      ]
                  },
                  {
                      "urn": "urn:adsk.viewing:fs.file:dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6bWRfMjAxMDEwL2JveC5pcHQ/output/1/properties.db",
                      "role": "Autodesk.CloudPlatform.PropertyDatabase",
                      "mime": "application/autodesk-db",
                      "guid": "715269b4-da1d-4ed6-abe9-9aff13d3a526",
                      "type": "resource",
                      "status": "success"
                  }
              ],
              "name": "box.ipt",
              "progress": "complete",
              "outputType": "svf",
              "status": "success"
          }
      ],
      "hasThumbnail": "true",
      "progress": "complete",
      "type": "manifest",
      "region": "US",
      "version": "1.0",
      "status": "success"
    }
