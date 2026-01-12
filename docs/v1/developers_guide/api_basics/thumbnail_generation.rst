##########
Extract Thumbnails
##########

.. code-block:: shell

  In this example, there are too many key concepts to be contained in a single API Basics page. 

  This page is a sub topic of the API Basics page. 


The Model Derivative API can be used to generate thumbnails of a model. You can generate three different-sized thumbnails: 100x100, 200x200, and 400x400.

The following workflow describes how to generate thumbnails:

#. Call `POST job </en/docs/model-derivative/v2/reference/http/job-POST>`_ to generate the thumbnails. Set the output format type to ``thumbnail``, and specify the thumbnail size.

#. Call `GET :urn/manifest </en/docs/model-derivative/v2/reference/http/urn-manifest-GET>`_ to verify whether the thumbnail generation is complete and the thumbnail is ready to be downloaded.

#. When the translation is complete, use the `GET :urn/manifest </en/docs/model-derivative/v2/reference/http/urn-manifest-GET>`_ endpoint to obtain the URN of the thumbnail.

#. Use the thumbnail URN to call `GET :urn/thumbnail </en/docs/model-derivative/v2/reference/http/urn-thumbnail-GET>`_ to download the thumbnail.
