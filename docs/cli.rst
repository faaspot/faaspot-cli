
.. _cli:

============================
Command Line Interface (CLI)
============================

Overview
--------

The FaaSpot command-line interface (CLI) is the default method for interacting with FaaSpot, to manage your functions.

For installation instructions, please refer to the :ref:`installation` section.

Usage
=====

You can access the CLI by running the ``fas`` command in your terminal.
Use ``cfy -h`` to display a list of all the commands and their descriptions.

.. code-block:: sh

    $ fas -h
    usage: fas [-h] [--version] {spots,functions,executions,profiles} ...

    positional arguments:
      {spots,functions,executions,profiles}
                            Manages the FaaSpot account
        spots               Manages spots
        functions           Manages functions
        executions          Manages executions
        profiles            Manages profiles

    optional arguments:
      -h, --help            Shows this help message and exits
      --version             Show the version number and exits


Verbosity Level
===============

Any command in the CLI has the option to show you the command logs.
You find this option in the help message for the command, for example: ``fas spots list -h``

The verbosity levels are:

- **-v** Print debug messages.

- **-vv** Additionally print 3rd-party info log messages (of utils that are being used by ``fas``).

- **-vvv** Additionally print 3rd-party debug log messages.


.. note::
    To directly use HTTP requests, you can run a command using ``-vvv``.  It will show you the HTTP requests that have been used.
