#######################################
Model Derivative Rate Limits and Quotas
#######################################

.. code-block:: shell

  This page must be customized for your API

The Model Derivative service observes a rate limit and a set of quotas to ensure that all clients get sufficient service and that runaway applications don't consume excessive resources. `Forge Rate Limits and Quotas`_ describes rate limits and quotas in general.

Rate Limits
===========

Rate limits specify a maximum number of requests per minute.

Scope
-----

The Model Derivative service sets a separate rate limit for each application making requests (specified by client ID) per API endpoint.

Violation Notification
----------------------

If an application exceeds an endpoint's rate limit, the server returns an ``HTTP 429`` error (described in detail in `Forge Rate Limits and Quotas`_).

Endpoint Rate Limits
--------------------

These rate limits apply to some of the Model Derivative API's endpoints. Note that these rates are not service guarantees. In the uncommon case where total service use is too high across all clients, accepted request rates may drop until traffic subsides.

.. list-table::
   :widths: 5 20 10
   :header-rows: 1

   * - Method
     - Endpoint
     - Limit (requests/minute)
   * - POST
     - job
     - 500
   * - GET
     - :/urn/metadata/:guid/properties?forceget=true
     - 60
   * - GET
     - :urn/metadata/:guid?forceget=true
     - 60
   * -
     - Other endpoint limits coming soon
     -

Quotas
======

Model Derivative quotas limit resource consumption when using the Model Derivative service.

Scope
-----

Quotas are enforced individually for each Forge application making requests (specified by client ID) that uses the Model Derivative service, and are set per endpoint.

Endpoint Quotas
---------------

These quotas limit an endpoint's resource consumption.

.. list-table::
   :widths: 5 20 20 5 5 10
   :header-rows: 1

   * - Method
     - Endpoint
     - Limit Description
     - Limit
     - Units
     - Notification
   * - GET
     - :/urn/metadata/:guid/properties
     - The maximum download size for a request
     - 20
     - MB
     - HTTP 413 response
   * - GET
     - :urn/metadata/:guid/properties?objectid={id}
     - The maximum size of property to query for an object
     - 300
     - MB
     - HTTP 406 response
   * - GET
     - :urn/manifest/:derivativeurn
     - The maximum download size for derivativeurn which ends with ``.db`` or ``.sdb``
     - 256
     - MB
     - HTTP 413 response
   * -
     - Other quotas coming soon
     -
     -
     -
     -

Quota Violation Notification
----------------------------

The Model Derivative service reports quota violations using different HTTP error responses.

HTTP 413 "Request Entity Too Large"
+++++++++++++++++++++++++++++++++++

This response reports a quota violation that occurs when requesting a resource that's too large. These are examples of a 413 response header and body:

Example Response Header (413)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: shell

  HTTP/1.1 413 Request Entity Too Large
  Content-Type: application/json
  date: Fri, 03 Aug 2018 08:17:33 GMT
  x-ads-app-identifier=platform-viewing-2018.06.02.63.f1ad853-production
  x-ads-startup-time: Fri Aug 03 05:34:31 UTC 2018
  x-ads-duration: 126 ms
  x-ads-size: 529625314

Example Response Body (413)
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: json

  {
    "diagnostic": "Please use the 'forceget' parameter to force querying the data."
  }

.. _Forge Rate Limits and Quotas: /en/docs/example_projecr/v1/developers_guide/rate-limiting/forge-rate-limits
