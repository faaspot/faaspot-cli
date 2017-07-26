
.. _deployments:

===========
Deployments
===========

.. _deployment_overview:

Overview
--------

In FaaSpot, deployment is a combination of:

- The **function code** that you want to run.

- (Optional) A **Requirements file**, which must be in a python `pip requirements file format <https://pip.pypa.io/en/stable/reference/pip_install/#requirements-file-format>`_.

- (Optional) A **Context file**, which might contain secrets credentials that you need to be accessible in the function, but don't want to send in every deployment run command.


The Function
^^^^^^^^^^^^

The function is basically a python script that contains a main function in a specific format.
If the python script doesn't have a main function in the required format, it will not run.

The script must contain a ``main`` function, that receives 2 parameters:

- **args** A dictionary that contains the arguments to the function. These are the parameters that are sent to the deployment to run command.

- **context** A dictionary that contains the Context variables, which you set in the deployment creation command.

The very basic template of the python script:

.. code-block:: python

    def main(args, context):
        return ''

The Requirements
^^^^^^^^^^^^^^^^

If your function reuires third-party python packages, which are available via `pip install <https://pip.pypa.io/>`_,
you can add Requirements, in `pip requirements file format <https://pip.pypa.io/en/stable/reference/pip_install/#requirements-file-format>`_.
The Requirements file is added during the creation of the deployment.

A very basic Requirements file might look like this:

.. code-block:: sh

    Jinja2==2.7.2


The Context
^^^^^^^^^^^

The Context is a dictionary that might contain secret data to be sent to all of the specific deployment instances.
You can create a deployment without it, in which case the context dict will be empty.

A very basic Context file might look like this:

.. code-block:: sh

    access_key_is = "XYZ"
    secret_access_key = "123"



.. _create_deployment:

Create Deployment
-----------------

To create a new deployment, you need to use the ``deployments create`` API.
You can see info about the API arguments in the :ref:`overview <deployment_overview>` section.


..  admonition:: CLI
    :class: open-toggle

    You can create a new deployment using the ``CLI``, for example:

    .. code-block:: sh

        $ fas deployments create DEPLOYMENT_NAME --file FILE_PATH

    The create command parameters:

    - **--file** - Path to he file code

    - (Optional) **--requirements-file** Path to the requirements file

    - (Optional) **--context-file** Path to the context file

    - (Optional) **--wait** Boolean parameter. Whether to wait for completion. Default is False.


..  admonition:: Python
    :class: toggle

    You can create a new deployment using the ``fas`` python-client, for example:

    .. code-block:: python

       from faaspot import Faaspot
       faaspot = Faaspot()
       faaspot.deployments.create(DEPLOYMENT_NAME, file=FILE_PATH)

    The create function parameters:

    - **--file** - Path to he file code

    - (Optional) **requirements-file** Path to the Requirements file

    - (Optional) **context-file** Path to the Context file

    - (Optional) **wait** Boolean parameter. Whether to wait for completion. Default is False.


..  admonition:: HTTP Request
    :class: toggle

    You can create a new deployment using direct HTTP requests:

    .. code-block:: sh

       $ curl -X PUT --header "Authorization: Token MY_TOKEN" --header "Content-Type: application/json"
       --data '{"name": "DEPLOYMENT_NAME", "code": "THE_CODE", "requirements": "THE_REQUIREMENTS", "context": "THE_CONTEXT"}'
       https://dev.faaspot.com:443/api/deployments/


    The code argument is mandatory, the requirements and context are optional.
    You must provide the actual code, requirements and context in UTF-8 format.
    To encode your text to UTF-8 format, you can use this `on-line converter <https://perishablepress.com/tools/utf8-hex/>`_,
    or use python:

    .. code-block:: python

       from six.moves.urllib.parse import quote
       encoded_str = quote(str_to_encode.encode("utf-8"))



.. _run_deployment:

Run a Deployment
----------------

To run a deployment, you need to use the ``deployments run`` API.


..  admonition:: CLI
    :class: open-toggle

    You can run a deployment using the ``CLI``, for example:

    .. code-block:: sh

        $ fas deployments run hello --parameters "name=user1" --wait

    The deployments run command parameters:

    - **name** The name of the deployment to run
    - (Optional) **--wait** Boolean parameter. Whether to wait for completion. Default is False.

    To run a deployment with multiple parameters, add them in the parameter argument, for example:

    .. code-block:: sh

        $ fas deployments run DEPLOYMENT_NAME -p "param_1=value_1, param_2=value_2" --wait

    As you can see in the example above, you can use ``-p`` as a shortcut for ``--parameters``

    In this example, we run the ``fas deployments run`` command in a blocked manner (wait until you have a response),
    using the ``--wait`` parameter.

    By default, without ``--wait``, the command will run in a non-blocking manner,
    and the ``fas deployments run`` command will return the execution ID of the task.
    You can then check that status of the execution task (completed or not), using the ``fas executions get`` command.
    You can read about the execution API in the :ref:`execution <get_execution_status>` page.

    For example:

    .. code-block:: sh

        $ UUID=`fas deployments run hello --parameters "name=user1"`
        $ fas executions get $UUID


..  admonition:: Python
    :class: toggle

    You can run a deployment in a blocking way (wait until the execution is completed), using the python-client:

    .. code-block:: python

       from faaspot import Faaspot
       faaspot = Faaspot()
       faaspot.deployments.run(DEPLOYMENT_NAME, {'PARAMETER_1': 'VALUE_1', 'PARAMETER_2': 'VALUE_2'}, wait=True)

    The deployments run function parameters:

    - **name** The name of the deployment to run
    - (Optional) **wait** Boolean parameter. Whether to wait for completion. Default is False.

    To run the deployment in non-blocking way, and then check the execution status of the deployment,
    you can run:

    .. code-block:: python

       from faaspot import Faaspot
       faaspot = Faaspot()
       execution_id = faaspot.deployments.run('DEPLOYMENT_NAME', {'PARAMETER': 'VALUE'})
       execution = faaspot.executions().get(execution_id)
       print execution['status']


..  admonition:: HTTP Request
    :class: toggle

    You can run a deployment using direct HTTP requests.
    You can run deployment in a blocking way (wait until the execution is completed), with ``/sync/`` in the url.

    .. code-block:: sh

       $ curl -X PUT --header "Authorization: Token MY_API_TOKEN"
       https://api.faaspot.com/v1/sync/deployments/DEPLOYMENT_NAME/rpc/?PARAMETER_1=VALUE_1&PARAMETER_2=VALUE_2

    You can also run the deployment in non-blocking way, without ``/sync/`` in the url:

    .. code-block:: sh

        $ EXECUTION_ID_STR=`curl --header "Authorization: Token MY_API_TOKEN" https://api.faaspot.com/v1/deployments/DEPLOYMENT_NAME/rpc/?PARAMETER=VALUE`
        $ EXECUTION_ID=`sed -e 's/^"//' -e 's/"$//' <<< "$EXECUTION_ID_STR"`
        $ curl --header "Authorization: Token MY_API_TOKEN" https://api.faaspot.com/v1/executions/$EXECUTION_ID

    In the above sample you can see how to run a deployment using an HTTP Request,
    and then how query the execution status of the deployment run task.

    For the non-blocking approach, you can also use a POST request, to enable you to send the parameters in the request body,
    instead of in the request URL. For example:

    .. code-block:: sh

        $ curl -X POST --header "Content-Type: application/json" --header "Authorization: Token MY_API_TOKEN" \
        https://api.faaspot.com:443/v1/deployments/hello/rpc/ -d '{"PARAMETER_1": "VALUE_1", "PARAMETER_2": "VALUE_2"}'



Run Deployments In Bulk
-----------------------

Sometimes, you want to run the same deployment with different arguments.
One way to do it, is to run the :ref:`run deployment <run_deployment>` multiple times, each time with different arguments.

A faster way, is to use a single request, with the data of all the different arguments.
The way to do that, is to use the ``deployment run_bulk`` request.
The ``run_bulk`` request requires a list of group-of-parameters, meaning that every item in the input list represents a call to  the``deployments run`` request, with a group-of-parameters.


..  admonition:: CLI
    :class: open-toggle

    You can run a bulk deployment using the ``CLI``, for example:

    .. code-block:: sh

        $ fas deployments run_bulk DEPLOYMENT_NAME -p "k1=v1, k2=v2" -p "k3=v3, k4=v4"

    The sample above, will execute two ``DEPLOYMENT_NAME`` tasks.
    One with the arguments k1=v1, k2=v2, and another with the arguments k3=v3, k4=v4.
    The result of the sample above will be a list of two execution IDs.


..  admonition:: Python
    :class: toggle

    You can run a bulk deployment using the python-client:

    .. code-block:: python

       from faaspot import Faaspot
       faaspot = Faaspot()
       args_list = [{'k1': 'v1', 'k2': 'v2'}, {'k3': 'v3', 'k4': 'v4'}]
       id_list = faaspot.deployments.run_bulk(DEPLOYMENT_NAME, args_list)


..  admonition:: HTTP Request
    :class: toggle

    If you want to create a bulk run request using an HTTP request,
    you will need to create a POST request to: https://api.faaspot.com/v1/deployments/DEPLOYMENT_NAME/bulk_rpc/,
    and to add to the request body the list of the parameters, in the following format: ``'[{"k1": "v1", "k2": "v2"}, {"k3": "v3", "k4": "v4"}]'``

    .. code-block:: sh

       $ curl -X POST --header "Content-Type: application/json" --header "Authorization: Token MY_API_TOKEN" \
       https://api.faaspot.com:443/v1/deployments/DEPLOYMENT_NAME/bulk_rpc/ -d '[{"k1": "v1", "k2": "v2"}, {"k3": "v3", "k4": "v4"}]'

    The result of the above request is a list of executions IDs of all the related deployment executions.

.. note::

    The ``bulk_run`` call doesn't support blocking requests. The response is a list of executions IDs.
    To get the executions status, you need to run the :ref:`executions get<get_execution_status>` command.



Delete Deployments
------------------

To delete a new deployment, you need to use the ``deployments delete`` API.

..  admonition:: CLI
    :class: open-toggle

    You can delete a deployment using the ``CLI``, for example:

    .. code-block:: sh

        $ fas deployments delete DEPLOYMENT_NAME


..  admonition:: Python
    :class: toggle

    You can delete a deployment using the python-client:

    .. code-block:: python

       from faaspot import Faaspot
       faaspot = Faaspot()
       faaspot.deployments.delete(DEPLOYMENT_NAME)


..  admonition:: HTTP Request
    :class: toggle

    You can delete a deployment using an HTTP request:

    .. code-block:: sh

       $ curl -X DELETE --header "Authorization: Token MY_API_TOKEN" https://api.faaspot.com:443/v1/deployments/DEPLOYMENT_NAME

