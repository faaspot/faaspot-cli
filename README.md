## FaaSpot Python Client

[![PypI](http://img.shields.io/pypi/v/fas.svg)](http://img.shields.io/pypi/v/fas.svg)

### Overview

#### What is FaaSpot?

FaaSpot is a SAAS service that enables you to run Functions as a Service (FAAS), and reduce your monthly Cloud expenses.

#### How can we help you make significant cost savings?

You need us if you are using an expensive, powerful machine in AWS to run a small set of tasks multiple times.

With FaaSpot, you can replace your expensive machine with a much smaller one, and use spot instances to run functions.
The cost of running four spot instances with a single CPU is much less than running a single Amazon machine with four CPUs.
With us, you'll get the same performance for less, much less, money.

#### How can I use it?

You can access FaaSpot using the FaaSpot client from the CLI, directly from the Python script (as a library), or by using the proprietary REST API.
The FaaSpot’s command-line interface is the default method for interacting with FaaSpot and managing your functions.

Version compatibility: Python 2.7, 3.3-3.5

> <b>Note:</b>
>
> Communication with the FaaSpot Webserver is encrypted and secured.

If you haven’t already obtained a FaaSpot API key, send us a request at info@faaspot.com.

### Installation

Run the following command:
```
pip install fas
```

### I want some code!

To test FaaSpot, you can try the following commands.

```
# Create a CLI profile that contains your FaaSpot token credentials.
fas profiles create my_profile --username MY_USERNAME --token MY_API_TOKEN

# Create a hello.py sample script
fas deployments samples --hello

# Create a new deployment with the hello.py script
fas deployments create hello --file hello.py

# For the example, create a spot that will run your deployments.
# In "real life" you'll probably have a running FaaSpot VM (spot).
# If you don't already have a FaaSpot VM, you need to increment your spots by 1..
fas spots add --wait

# run the deployment
fas deployments run hello -p "name=user1" --wait

# Just for the example - remove the spot.
fas spots remove --wait
```

### Sync deployments run

By default, deployments are run in the background.
You can run the deployments run command in a synced manner (wait until you have a response).

Using the cli:
```
fas deployments run hello -p "name=user1" --wait
```

Using curl:
```
curl --header "Authorization: Token MY_API_TOKEN" https://rest.faaspot.com/api/sync/deployments/hello/rpc/?name=user1
```


## Faaspot CLI

### Usage

You can access the CLI by running the `fas` command in your terminal.
Use `fas -h` to display a list of all the commands and their descriptions.

<pre lang="shell"><code>$ fas -h
usage: fas [-h] [--version] {spots,deployments,executions,profiles} ...

positional arguments:
  {spots,deployments,executions,profiles}
                        Manage FaaSpot account
    spots               Manage spots
    deployments         Manage deployments
    executions          Manage executions
    profiles            Manage profiles

optional arguments:
  -h, --help            Shows this help message and exits
  --version             Shows the  program version number and exits
</code></pre>

### Verbose Output

The `-v/--verbose` flag is available for all commands.
It sets the command verbosity level. There are three verbosity levels:

* Running a command without the verbose flag. (This is the default).
* Running a command with `-v` will show DEBUG log statements of fas events.
* Running a command with `-vv` will, in addition, show DEBUG log statements of external libs being used by fas.

### Configuration

A directory named .faaspot is created under ~(Home directory).
The directory contains a file named fasconf.yaml that contains the CLI configuration.
The file is created and managed by the CLI, but you can also customize the file according to your preferences.

## Profiles

The `fas profiles` command is used to manage your CLI profiles on your local machine.
Each profile contain connection parameters to FaaSpot.
To send REST API commands to FaaSpot, you need to have username and and auth credentials: Api-Token or Password.

You can use the command to list, create, update, get, use, retrieve the current profile, and delete a specific profile.


#### Commands

* <b>list</b>: Retrieves all the profiles

* <b>create</b>: Creates a new profile

* <b>update</b>: Updates an existing profile

* <b>get</b>: Retrieves a specific profile

* <b>use</b>: Uses a specific profile when communicating with FaaSpot

* <b>current</b>: Retrieves the current profile that is configured for use when communicating with FaaSpot

* <b>delete</b>: Deletes an existing profile


## Deployments

The `fas deployments` command is used to manage your functions.
Each deployment must contain the function code, that you want to run.
In addition, each deployment may contain:
* A Requirements file, which must be in a python [pip requirements file format](https://pip.pypa.io/en/stable/reference/pip_install/#requirements-file-format)
* A Context file, which must be a text file in which each line is in the format of: KEY=Value

You can use the command to list, get, create, update, delete, run a specific deployment.

The create, update, and run commands might take some time, therefore they start background execution in the FaaSpot server.
The return value of the create, update, and run commands will be the execution ID, which will require you to pull request the execution-id status, until the task completes.
Therefore, the optional argument: `--wait` is also available. Running the above commands with the `--wait` flag,
will run the status pull requests for you, and will return the output at the end of the task.

#### Commands

* <b>list</b>: Retrieves all deployments

* <b>create</b>: Creates a deployment

* <b>update</b>: Updates a deployment

* <b>get</b>: Retrieves a deployment

* <b>delete</b>: Deletes a deployment

* <b>run</b>: Runs a deployment, with optional input parameters

* <b>samples</b>: Creates a sample script locally that can be used for testing the deployment process


## Spots

The `fas spots` command is used to manage your spots.
You need at least one FaaSpot VM (spot) to run a function.
A FaaSpot spot is like a regular VM, but costs less.
The more spots that you have, the more times that you can run your function in a given period of time.

You can use the command to list, add, get, remove, and update a specific spot.


#### Commands

* <b>list</b>: Retrieves all spots

* <b>add</b>: Increases the number of spots by one

* <b>update</b>: Updates parameters for a specific spot

* <b>get</b>: Retrieves info about a specific spot

* <b>remove</b>: Reduces the number of spots by one

* <b>refresh_ip</b>: Refreshes a spot instance public IP


## Executions

The `fas executions` command is used to manage your executions.

You can use the command to list executions and to cancel or retrieve information about a single execution.

#### Commands

* <b>list</b>: Retrieves all executions

* <b>get</b>: Retrieves info for a specific execution

* <b>cancel</b>: Cancels an existing execution
