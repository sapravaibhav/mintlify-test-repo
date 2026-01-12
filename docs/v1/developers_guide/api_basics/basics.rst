##########
API Basics
##########

.. code-block:: shell

  Describes key concepts an App Developer needs to know in order to use the API effectively. They include:

  - Key concepts and terminology
    Describe them in the order users encounter them

  - Specific features
    Describe why and when would they use the features

  - How the elements flow
    Can have sub topics, if content is too large for a single page

The Model Derivative API gives you the ability to translate source files into output files (derivatives) of  `different formats </en/docs/model-derivative/v2/supported-translations>`_. While translating, the Model Derivative logs information to a JSON file known as a manifest. The manifest contains information about the derivatives that are produced. You can use the manifest to access the derivatives, check the status of the translation job, and obtain information about the derivatives.

The most common use of the Model Derivative API is to translate models to the SVF or SVF2 formats so that they can be displayed in a browser. See the tutorial `Prepare a File for the Viewer </en/docs/model-derivative/v2/tutorials/prep-file4viewer/>`_ for details. Also, the Model Derivative API is often used just to translate models from one format to another. You can also use the Model Derivative API to select parts/components of a model and export their geometric representations to OBJ files.

.. image:: /oas_example_single_project/_static/fieldGuide.png


The following table defines important terms for the Model Derivative API:

===========   ======================================================================================
Term          Definition
===========   ======================================================================================
source file   | The model that is translated to other formats; sometimes called "seed file".
              |
              | For information about supported file formats, see the
                `GET formats </en/docs/model-derivative/v2/reference/http/formats-GET>`_ endpoint.
derivatives   The translated output files
manifest      A JSON file listing the derivatives produced by a translation job. It contains information such as the derivative type, the URN of the derivative, and the translation job status (how far translation has progressed) of each derivative. It also contains metadata about each derivative, which lets you identify individual objects within a model, their geometric representations, and associated properties.
===========   ======================================================================================
