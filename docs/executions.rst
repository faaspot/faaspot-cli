.. _executions:

==========
Executions
==========

Overview
--------

When you run some command in a non-blocking way, for example: :ref:`deployment run<run_deployment>`,
the command will create an execution that will run in the background,
and the result of the API will be the execution ID.
You can then use the executions API to retrieve the execution status, or to see which executions are currently running.


.. _get_execution_status:

Get Execution Status
--------------------

To get the status of an execution, you need to use the spots API:


..  admonition:: CLI
    :class: open-toggle

    You can get the status of an execution using the CLI:

    .. code-block:: sh

        $ fas executions get EXECUTION_ID


..  admonition:: Python
    :class: toggle

    You can get the status of an execution using the python-client:

    .. code-block:: python

       from faaspot import Faaspot
       faaspot = Faaspot()
       faaspot.executions.get(EXECUTION_ID)


..  admonition:: HTTP Request
    :class: toggle

    You can get the status of an execution using direct HTTP requests:

    .. code-block:: sh

       $ curl -X GET --header "Authorization: Token MY_TOKEN" https://api.faaspot.com/v1/executions/EXECUTION_ID

A result of the API should look as follows:

    {
        u'created': u'2017-07-21 15:10:52.537686+00:00',

        u'name': u'tasks.create_spot',

        u'output': None,

        u'status': u'Started',

        u'uuid': u'9cfcd860-9a5d-414f-8d2b-317d59d3f486'

    }


.. note::
    The CLI and the python-client support the ``wait`` argument, meaning that using ``fas executions get EXECUTION_ID --wait`` or using ``faaspot.executions.get(EXECUTION_ID, wait=True)``,
    will wait until the execution is completed.


Get Bulk Execution Status
-------------------------

Sometimes, you have multiple executions running simultaneously.
In that case, you might want to query the status of all of them using a single request,
instead of generating an ``execution get`` request for each of the exceutions separately.
You can achieve that using the ``execution get_bulk`` command.

The results will be a list of execution-statuses.


..  admonition:: CLI
    :class: open-toggle

    You can retrieve the statuses of multiple executions using the CLI:

    .. code-block:: sh

        $ fas executions get_bulk -u EXECUTION_ID_1 -u EXECUTION_ID_2


..  admonition:: Python
    :class: toggle

    You can retrieve the statuses of multiple executions using the python-client:

    .. code-block:: python

       from faaspot import Faaspot
       faaspot = Faaspot()
       faaspot.executions.get([EXECUTION_ID_1, EXECUTION_ID_1])


..  admonition:: HTTP Request
    :class: toggle

    You can retrieve the statuses of multiple executions using direct HTTP requests:

    .. code-block:: sh

       $ curl -X POST --header "Content-Type: application/json" --header "Authorization: Token MY_API_TOKEN" \
       https://api.faaspot.com:443/v1/executions/bulk/ -d '["EXECUTION_ID_1", "EXECUTION_ID_2"]'



Get Executions List
-------------------

To retrieve a list of the current running executions.


..  admonition:: CLI
    :class: open-toggle

    You can retrieve a list of the current running executions using the CLI:

    .. code-block:: sh

        $ fas executions list

    The executions status command parameters:

    - (Optional) **--include_completed** Boolean parameter. Whether to include completed executions. Default is False.


..  admonition:: Python
    :class: toggle

    You can retrieve a list of the current running executions using the python-client:

    .. code-block:: python

       from faaspot import Faaspot
       faaspot = Faaspot()
       faaspot.executions.list()

    The executions status command parameters:

    - (Optional) **include_completed** Boolean parameter. Whether to include completed executions. Default is False.


..  admonition:: HTTP Request
    :class: toggle

    You can retrieve a list of the current running executions using direct HTTP requests:

    .. code-block:: sh

       $ curl -X GET --header "Authorization: Token MY_TOKEN" https://api.faaspot.com/v1/executions/?include_completed=False

    You can add ``?include_completed=False`` or ``?include_completed=True`` to the request,
    to include completed executions, or not.
