
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


All the samples are available using the ``fas functions samples`` command:


.. code-block:: sh

    $ fas functions samples -h
    usage: fas functions samples [-h] [--hello-world | --get_content | --sleep]
                               [-v]

    optional arguments:
      --hello-world  Generates a hello-world function
      --get-content  Generates a get-content function
      --sleep        Generates a sleep function
      -h, --help     Shows this help message and exits
      -v, --verbose  Increases output verbosity. -v will print debug messages. -vv
                     will additionally print 3rd-party info

The following command will create a `hello-world.py` script, which we will use later on and create a FaaSpot Function from it:

.. code-block:: sh

    $ fas functions samples --hello-world



Create A Function
=================

If you don't already have a :ref:`FaaSpot spot<spots>`, you need to :ref:`get one<add_spot>`, in order to run the samples.

Now you need to create a :ref:`function <functions>`.
To create a function, you just need a python script that contains a function to run.
If you run the command from the section above, you already have a sample hello-world python script.

To create a function from the python script, you need to use the ``functions create`` API:

.. code-block:: sh

    $ fas functions create hello --file hello-world.py

This command creates a new function, named hello, which contain the hello_world.py file.
To see how to create a new function using an HTTP request or using the python-client, go to the :ref:`function <create_function>` page.


Run The Function
================

Now that you have a spot and a function, you're ready to run the function.
You can run the function using the CLI:

.. code-block:: sh

    $ fas functions run hello --parameters "name=user1" --wait

We used the `--wait` parameters, so the command will wait until the function completes,
and will return the function result, and not the execution ID.

To see how to run the function using HTTP request or using the python-client,
go to the :ref:`run function <run_function>` section.
