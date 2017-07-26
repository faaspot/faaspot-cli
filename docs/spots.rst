
.. _spots:

=====
Spots
=====

Overview
--------

A FaaSpot spot is a `VM <https://en.wikipedia.org/wiki/Virtual_machine>`_ that can run your functions.
You can run multiple concurrent functions on one spot. Nevertheless,
the more spots that you have, the more concurrent functions you'll be able to run.

The cool thing about FaaSpot, is that you pay for each spot that you have,
regardless to the number of functions that you run on it.
Having said that, to run a function in FaaSpot you will need at least one FaaSpot spot.


.. _add_spot:

Add a Spot
----------

To add a spot to your spot's pool, you need to use the spots API:


..  admonition:: CLI
    :class: open-toggle

    You can manage your spots using the ``fas`` CLI:

    .. code-block:: sh

        $ fas spots add --wait


..  admonition:: Python
    :class: toggle

    You can manage your spots using the ``fas`` python-client:

    .. code-block:: python

       from faaspot import Faaspot
       faaspot = Faaspot()
       faaspot.spots.add(wait=True)


..  admonition:: HTTP Request
    :class: toggle

    You can manage your spots using the direct HTTP requests:

    .. code-block:: sh

       $ curl -X PUT --header "Authorization: Token MY_TOKEN" https://dev.faaspot.com:443/api/spots/

.. note::
    What is the ``wait`` argument?

    By default, requests run in the background, in a synchronized manner.
    This means that the request will return the execution ID.
    You can then check that execution status (completed or not), using the execution ID.

    When using the FaaSpot client, you can run the command in a synced manner (wait until you receive a response),
    using the ``wait`` argument.

    When using the HTTP request to add a spot, you'll need to check the execution status manually.
    To see how to do it, go to the :ref:`executions <get_execution_status>` page.


List Spots
----------

To retrieve a list of all the spots that you have, use the spots list API:


..  admonition:: CLI
    :class: open-toggle

    You can manage your spots using the ``fas`` CLI:

    .. code-block:: sh

        $ fas spots list


..  admonition:: Python
    :class: toggle

    You can manage your spots using the ``fas`` python-client:

    .. code-block:: python

       from faaspot import Faaspot
       faaspot = Faaspot()
       faaspot.spots.list()


..  admonition:: HTTP Request
    :class: toggle

    You can manage your spots using the direct HTTP requests:

    .. code-block:: sh

       $ curl -X GET --header "Authorization: Token MY_TOKEN" https://dev.faaspot.com:443/api/spots/


Remove a Spot
-------------

To remove a spot from your spot's pool, you need to use the spots API:


..  admonition:: CLI
    :class: open-toggle

    You can remove one spot from your spots pool using the CLI:

    .. code-block:: sh

        $ fas spots remove --wait


..  admonition:: Python
    :class: toggle

    You can remove one spot from your spots pool using the python-client:

    .. code-block:: python

       from faaspot import Faaspot
       faaspot = Faaspot()
       faaspot.spots.remove(wait=True)


..  admonition:: HTTP Request
    :class: toggle

    You can remove one spot from your spots pool using a direct HTTP request:

    .. code-block:: sh

       $ curl -X DELETE --header "Authorization: Token MY_TOKEN" https://api.faaspot.com/v1/spots/

    This API will return the execution ID of the spot removal task.
    To get the execution status of that task, you will need to query the execution status.
    You can see how to do it in the :ref:`executions <get_execution_status>` page.

.. note::
    What is the ``wait`` argument?

    By default, requests run in the background, in a synchronized manner.
    This means that the request will return the execution ID.
    You can then check that execution status (completed or not), using the execution ID.

    When using the FaaSpot client, you can run the command in a synced manner (wait until you receive a response),
    using the ``wait`` argument.

    When using the HTTP request to remove a spot, you'll need to check the execution status manually.
    To see how to do it, go to the :ref:`executions <get_execution_status>` page.


Refresh a Spot IP Address
-------------------------

The spots are actual VMs, with a public IP address.
Sometimes, there is a need to give the spots a new IP,
not a specific IP, just a different one.
You can do it using a FaaSpot refresh_ip request.


..  admonition:: CLI
    :class: open-toggle

    You can refresh the IP address of your spots, using the CLI:

    .. code-block:: sh

        $ fas spots refresh_ip --wait

    The refresh_ip command parameters:

    - (Optional) **- -ip** Specifies which spot IP to refresh. Default is to refresh all spots IPs.

    - (Optional) **- -wait** Boolean parameter, whether to wait for completion. Default is False.


..  admonition:: Python
    :class: toggle

    You can refresh the IP address of your spots, using the python-client:

    .. code-block:: python

       from faaspot import Faaspot
       faaspot = Faaspot()
       faaspot.spots.refresh_ip(wait=True)

    The refresh_ip command parameters:

    - (Optional) **ip** Specifies which spot IP to refresh. Default is to refresh all spots IPs.

    - (Optional) **wait** Boolean parameter, whether to wait for completion. Default is False.

..  admonition:: HTTP Request
    :class: toggle

    You can refresh the IP address of your spots, using direct HTTP requests:

    .. code-block:: sh

       $ curl -X PATCH --header "Authorization: Token MY_TOKEN" https://api.faaspot.com/v1/spots/ \
       --data '{"refresh_ip": "all"}'
       $ curl -X PATCH --header "Authorization: Token MY_TOKEN" https://api.faaspot.com/v1/spots/ \
       --data '{"refresh_ip": "SPOT_IP_TO_REFRESH"}'

    This API will return the execution ID of the spot refresh_ip task.
    To get the execution status of that task, you will need to query the execution status.
    You can see how to do it in the :ref:`executions <get_execution_status>` page.
