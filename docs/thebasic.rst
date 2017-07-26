==========
The Basics
==========

This page provides a quick introduction to FaaSpot.


FaaSpot Spots
=============

A FaaSpot spot is a `VM <https://en.wikipedia.org/wiki/Virtual_machine>`_ that can run your functions.
This VM is totally managed by FaaSpot. You can add or remove spots using the FaaSpot API.
For more info, please read  the :ref:`spots` page.


Deployment
==========

A FaaSpot deployment is a package of function to run, with optional meta-data related to 
requirements and context. For more info, please read  the :ref:`deployments` page.


Making a Request
================

FaaSpot exposes a set of REST APIs. You can call the APIs manually, using cURL for example,
or you can use the FaaSpot client, which wraps the HTTP calls with a user friendly interface.

There are multiple ways to send requests to FaaSpot:

- **Command Line Interface (CLI)**. For usage info, please read  the :ref:`CLI <cli>` page.

- **python-client**. For usage info, please read  the :ref:`python client <python_client>` page.

- **HTTP Requests**. You can manually send HTTP requests to the REST API. We'll cover the usage in the API sections in the documentation.


Async Requests
--------------

Some of the requests are executed as non-blocking.
When you run a non-blocking request, the result will be an execution ID.
In that case, you'll need to run another request to retrieve that execution status,
in order to see if it's still in progress, or completed.

You can send blocking requests that will block the client until there is a response.
In the CLI, you can use the ``--wait`` parameter.


Concurrent Requests
-------------------

You can send multiple requests concurrently, and send a single bulk request.
This is a more advanced topic.
