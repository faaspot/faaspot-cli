
.. _quick_start:

==========
Quickstart
==========

This page provides quick introductory examples for using FaaSpot.
If you have not already installed FaaSpot, head over to the :ref:`installation` section.

Get A Spot
==========

The first thing that you need to run functions on FaaSpot is a FaaSpot spot.

You can easily add, remove, and get a list of your spots using the CLI:


.. code-block:: sh

        $ fas spots add --wait
        $ fas spots list
        $ fas spots remove --wait


To see how to add spots using an HTTP request or using the python-client, go to the :ref:`spots <add_spot>` page.


Get A Function
==============

To make life easier, we have some built-in samples that come with the ``fas`` CLI.
The samples are very simple python scripts, which can run on FaaSpot as a quick test, or be used as a reference.
The samples are:

* **hello-world function**. The hello-world function receives a name as input, and replies with hello.

* **get-content function**. The get-content function receives a URL as input, downloads it's content, and returns it.

* **sleep function**. The sleep function receives optional input that indicates the sleep time, and then returns a string.


All the samples are available using the ``fas deployments samples`` command:


.. code-block:: sh

    $ fas deployments samples -h
    usage: fas deployments samples [-h] [--hello-world | --get_content | --sleep]
                               [-v]

    optional arguments:
      --hello-world  Generates a hello-world function
      --get-content  Generates a get-content function
      --sleep        Generates a sleep function
      -h, --help     Shows this help message and exits
      -v, --verbose  Increases output verbosity. -v will print debug messages. -vv
                     will additionally print 3rd-party info

The following command will create a `hello-world.py` script, which we will use later on and create a FaaSpot Deployment from it:

.. code-block:: sh

    $ fas deployments samples --hello-world



Create A Deployment
===================

If you don't already have a :ref:`FaaSpot spot<spots>`, you need to :ref:`get one<add_spot>`, in order to run the samples.

Now you need to create a :ref:`deployment<deployments>`.
To create a deployment, you just need a python script that contains a function to run.
If you run the command from the section above, you already have a sample hello-world python script.

To create a deployment from the python script, you need to use the ``deployments create`` API:

.. code-block:: sh

    $ fas deployments create hello --file hello-world.py

This command creates a new deployment, named hello, which contain the hello_world.py file.
To see how to create a new deployment using an HTTP request or using the python-client, go to the :ref:`deployment <create_deployment>` page.


Run The Deployment
==================

Now that you have a spot and a deployment, you're ready to run the deployment function.
You can run the deployment using the CLI:

.. code-block:: sh

    $ fas deployments run hello --parameters "name=user1" --wait

We used the `--wait` parameters, so the command will wait until the function completes,
and will return the function result, and not the execution ID.

To see how to run the deployment using HTTP request or using the python-client,
go to the :ref:`deployment <run_deployment>` page.
