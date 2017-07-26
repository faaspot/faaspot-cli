========
Overview
========

What Is FaaSpot?
================

FaaSpot is a SaaS service that enables you to run Functions as a Service (FaaS),
and reduce your monthly Cloud expenses.
The concept provides the ability for you to run your functions on a FaaSpot spot. A FaaSpot spot is a VM that can run your functions.
You can run multiple concurrent functions on one spot. Nevertheless,
the more spots that you have, the more concurrent functions you'll be able to run.
The cool thing about FaaSpot, is that you pay for each spot that you have,
regardless of the number of functions that you run on it, and it actually uses AWS spots to reduce costs.


Requirements
============

#. You need a FaaSpot token. If you haven’t already obtained a FaaSpot API key, send us a request at info@faaspot.com.
#. If you're using the python-client, python-client version compatibility is: Python 2.7, 3.3-3.5.

.. note::

    Communication with the ``FaaSpot`` webserver is encrypted and secured.

    You can access FaaSpot using the FaaSpot client from the `VM <https://en.wikipedia.org/wiki/Command-line_interface>`_,
    directly from the Python script (as a library), or by using the proprietary REST API.
    The FaaSpot’s command-line interface is the default method for interacting with FaaSpot and managing your functions.


Limitations
===========
#. FaaSpot currently supports only python scripts.
#. The python script needs a main function with a specific format.
#. The function run time must be less than a minute.


.. _installation:

Installation
============

You can install the FaaSpot Client via `pip <https://pip.pypa.io/>`_:

.. code-block:: sh

    $ pip install faaspot

.. note::
    After installing the FaaSpot client you'll have both the command-line interface (CLI) and a python-client library.
    The CLI client name is ``fas`` and the python-client library name is ``faaspot``.

    To upgrade the FaaSpot client version, run: ``pip install faaspot --upgrade``.

    For info about the CLI, go to the :ref:`CLI <cli>` page.
    For info about the python library, go to the :ref:`deployments <deployments>` page.


Setting Up The Environment
--------------------------

To use the FaaSpot client, you need to configure your profile.
This configuration will apply to both the CLI and the python-client library.
Create a FaaSpot profile that contains your FaaSpot token credentials.

.. code-block:: sh

    $ fas profiles create --token MY_API_TOKEN

.. note::
    The ``fas profiles create`` command will create a global configuration file located at ~/.faaspot folder,
    which contain the connection configuration to FaaSpot.

    You can also manually edit the ``~/.faaspot/conf.yaml`` file.
