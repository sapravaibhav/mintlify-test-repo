##################################
About the OAS Example Single Project API
##################################

.. code-block:: shell

   This page, addresses the following:

    - What is this API?
    - What can it do?
    - Who is it for?
    - Scope and limitations
    - Link to Terms of Service

The following example is from the Fusion Data API. Customize the page with information for your own API. Guidelines are available here: https://wiki.autodesk.com/x/P-7yUQ


.. image:: /oas_example_single_project/_static/pim_model.png



The Fusion Data API allows developers to read, write, and extend product design and manufacturing data that is stored in the cloud. These capabilities enable cloud-based workflows that react to data changes, without the need for desktop authoring applications like Fusion 360 or Autodesk Inventor. This GraphQL API exposes queries and mutations to access built-in and custom properties in designs saved to the Fusion Cloud Information Model.

The Fusion Cloud Information Model is your manufacturing design data stored in the cloud, in a granular and accessible way. Through the Fusion Data API, you can read, write, and extend subsets of your design model through cloud-based workflows. All this without the need for desktop authoring applications like Fusion 360 or Autodesk Inventor. Fusion Data adopts similar concepts to the `Fusion 360 client API <https://help.autodesk.com/view/fusion360/ENU/?guid=GUID-88A4DB43-CFDD-4CFF-B124-7EE67915A07A>`_ but within a cloud environment.

**********
Common Uses
**********

With the Fusion Data API, you can programmatically access manufacturing information and achieve a variety of cloud-based functionality, such as:

- Navigating hub project folders, through files, all the way down to component properties, and sharing just the right data with collaborators.
- Request only the Fusion cloud data you need through tailored GraphQL queries
- Retrieving the entire model hierarchy at once, eliminating the need to fetch children of an assembly one level at a time.
- Accessing the Bill of Materials of a design, traversing the hierarchy of parts within the model for integration with enterprise resource planning (ERP) systems using unique part identifiers.
- Managing compliance by tracing the latest design data related to engineering workflow approvals, for example: change orders, release data, and automated part numbering.
- Subscribe to specific Fusion cloud data events and be notified via Webhooks when data changes.

*********
Next Steps
*********

- Get started with the `Step-by-Step Tutorials </en/docs/fusiondata/v1/tutorials/before_you_begin>`_ .
- Explore `Code Samples </en/docs/fusiondata/v1/code-samples/read-model>`_  to discover how the API is used in fully functional applications.

****************
Terms of Service
****************

**Fusion Data API** is subject to `Autodesk Platform Services Terms of Service. <https://www.autodesk.com/company/legal-notices-trademarks/terms-of-service-autodesk360-web-services/forge-platform-web-services-api-terms-of-service>`_
