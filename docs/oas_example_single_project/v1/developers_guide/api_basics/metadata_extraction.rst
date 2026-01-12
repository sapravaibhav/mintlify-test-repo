*******************
Extract Metadata
*******************

.. code-block:: shell

  In this example, there are too many key concepts to be contained in a single API Basics page. 

  This page is a sub topic of the API Basics page. 
  

When you translate a model into the SVF format, the Model Derivative service saves information about the derivatives (metadata) in the manifest. When the source model is large, the manifest can become difficult to parse. As such, the Model Derivative API provides endpoints to specifically query metadata.

`GET :urn/metadata </en/docs/model-derivative/v2/reference/http/urn-metadata-GET/>`_ lets you extract information about the 3D Views and 2D sheets/views referenced in the manifest. These derivatives are the Viewables that you can typically display in a browser using the Forge Viewer SDK. See the tutorial on `Extract Metadata from a Source Model </en/docs/model-derivative/v2/tutorials/xtract-metadata/>`_ to see how you can extract the names of Viewables and their metadata GUIDs (Global Unique Identifier).

Source models from applications such as Autodesk Inventor and Fusion 360 produce only one Viewable per model. However, source models from applications such as Autodesk Revit can contain multiple Viewables.

Once you obtain the GUID of a Viewable, you can use `GET :urn/metadata/:guid </en/docs/model-derivative/v2/reference/http/urn-metadata-guid-GET/>`_ to obtain the object/component hierarchy of the model. In addition to the hierarchy, the list provides the ``objectid`` of each object. See the tutorial on `Extract Geometry from a Source File </en/docs/model-derivative/v2/tutorials/xtract-geometry-from-source-file/>`_ for a demonstration on how the object hierarchy and objectsids are used to uniquely identify geometry and thereafter extracted as OBJ files.

Using  `GET :urn/metadata/:guid/properties </en/docs/model-derivative/v2/reference/http/urn-metadata-guid-properties-GET/>`_ you can obtain a flat list of objects in that Viewable. It also returns the properties of each object. Using a query parameter, you can filter the results to provide the properties of one specific object.


The following image shows the object hierarchy and the properties of a selected object, as displayed in a browser using the Forge Viewer SDK.

.. image:: ../../../_static/model_2.png
