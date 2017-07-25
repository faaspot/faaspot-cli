.. title:: FaaSpot Documentation

=====================
FaaSpot Documentation
=====================

FaaSpot is a SaaS service that enables you to run Functions as a Service (FaaS),
and reduce your monthly Cloud expenses.

- You need us if you are using an expensive, powerful machine to run a small set of tasks multiple times.
- With FaaSpot you can replace the expensive machine with a much smaller one, and use FaaSpot to run your functions.
- The cost of FaaSpot is fixed. You decide how much money you'll pay at the end of the month.
- With FaaSpot you'll get scale out-of-the-box for less, much less, money.


.. code-block:: sh

    $ fas spots add --wait
    $ fas deployments samples --hello-world
    $ fas deployments create hello --file hello-world.py
    $ fas deployments run hello --parameters "name=user1" --wait


User Guide
==========

.. toctree::
    :maxdepth: 3

    overview
    thebasic
    quickstart
    cli
    pythonclient
    spots
    deployments
    executions
