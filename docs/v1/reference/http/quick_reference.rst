################
Quick Reference
################

.. code-block:: shell

  This page is meant for those who have a hazy idea of an endpoint or the operation,
  but cannot recall its name, or vice versa. They can perform a Ctrl/Cmd +F search
  to find endpoints by keyword.

  This page is currently updated manually.

<*For guidelines, see* https://wiki.autodesk.com/x/VDIKHQ>.

*************
Company
*************

======================================	====================================================
Operation                               Method + Endpoint
======================================	====================================================
`Create a Company`_                     POST /companies
`Fetch a Company`_                      GET /companies/{companyId}
`Update a Company`_                     PUT /companies/{companyId}
`Change Company Permissions`_           PATCH /companies/{companyId}
======================================	====================================================


.. _Create a Company: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-post-companies-POST
.. _Fetch a Company: en/docs/oas_example_single_project/v1/reference/quick_reference/schema-get-companies-companyid-GET
.. _Update a Company: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-put-companies-companyid-PUT
.. _Change Company Permissions: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-patch-companies-companyid-PATCH


*************
GDPR
*************

======================================	====================================================
Operation                               Method + Endpoint
======================================	====================================================
`Check Status of a Process`_            GET /gdpr/jobs/{jobId}
`Delete a Namespace`_                   DELETE /gdpr/companies/{companyId}/namespaces/{namespaceId} /companies/{companyId}
======================================	====================================================


.. _Check Status of a Process: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-get-gdpr-jobs-jobid-GET
.. _Delete a Namespace: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-delete-gdpr-companies-companyid--DELETE


*************
Namespaces
*************

======================================	====================================================
Operation                               Method + Endpoint
======================================	====================================================
`Fetch All Namespaces`_                 GET /companies/{companyId}/namespaces
`Delete a Namespace`_                   DELETE /gdpr/companies/{companyId}/namespaces/{namespaceId} /companies/{companyId}
`Create a Namespace`_                   POST /{companyId}/namespaces
`Fetch a Namespace`_                    GET /companies/{companyId}/namespaces/{namespaceId}
`Update a Namespace`_                   PUT /companies/{companyId}/namespaces/{namespaceId}
`Change Namespace Permissions`_         PATCH /companies/{companyId}/namespaces/{namespaceId}
======================================	====================================================


.. _Fetch All Namespaces: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-get-companies-companyid-namespac-GET
.. _Delete a Namespace: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-delete-gdpr-companies-companyid--DELETE
.. _Create a Namespace: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-post-companies-companyid-namespa-POST
.. _Fetch a Namespace: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-Fetch-a-Namespace-GET
.. _Update a Namespace: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-put-companies-companyid-namespac-PUT
.. _Change Namespace Permissions: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-patch-companies-companyid-namesp-PATCH

*************
Schema
*************

======================================	====================================================
Operation                               Method + Endpoint
======================================	====================================================
`List Schema`_                          GET /companies/{companyId}/namespaces/{namespaceId}/schemas
`Create a Published Schema`_            POST /companies/{companyId}/namespaces/{namespaceId}/schemas
`Create a Draft Schema`_                POST /companies/{companyId}/namespaces/{namespaceId}/schemas/draft
`List All Versions of a Schema`_        GET /schemas/{schemaId}/versions
`Fetch a Version of a Schema`_          GET /schemas/{schemaVersionId}
`Fetch Multiple Schemas`_               GET /schemas:batch-get
`Create a Batch of Published Schema`_   POST /companies/{companyId}/namespaces/{namespaceId}/schemas-batch
======================================	====================================================


.. _List Schema: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-List-Schema-GET
.. _Create a Published Schema: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-Create-a-Published-Schema-POST
.. _Create a Draft Schema: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-Create-a-Draft-Schema-POST
.. _List All Versions of a Schema: en/docs/oas_example_single_project/v1/reference/quick_reference/schema-get-schemas-schemaid-versions-GET
.. _Fetch a Version of a Schema: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-get-schemas-schemaversionid-GET
.. _Fetch Multiple Schemas: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-get-schemas-batch-get-POST
.. _Create a Batch of Published Schema: /en/docs/oas_example_single_project/v1/reference/quick_reference/schema-Create-a-Batch-of-Published-Schema-POST
