# PSA Pilot Placement SSM
Placement SSM to be used for PSA pilot.

## Requires
* Docker
* Python3.4
* RabbitMQ

## Implementation
* Implemented in Python 3.4
* Dependencies: amqp-storm
* The main implementation can be found in: `son-ssm-examples/PSA_placement/placement/placement.py`

## How to run it

* To run the placement SSM locally, you need:
 * a running RabbitMQ broker (see general README.md of [son-mano framework repository](https://github.com/sonata-nfv/son-mano-framework) for info on how to do this)
 * a running Service Specific Registry (SMR) connected to the broker (see general README.md of [SMR repository](https://github.com/sonata-nfv/son-mano-framework) for info on how to do this)

* Run the placement SSM (in a Docker container):
 * (do in `son-sm/`)
 * `docker build -t sonssmpsaplacement1 -f son-ssm-examples/PSA_placement/Dockerfile .`
 * `docker run --name sonssmpsaplacement1 --net=sonata --network-alias=sonssmpsaplacement1 sonssmpsaplacement1`

* Or: Run the placement SSM (directly in your terminal not in a Docker container):
 * (In `son-sm/son-sm-template/`)
    * `python3.4 setup.py install`
 * (In `son-sm/son-ssm-examples/PSA_placement/`)
    * `python3.4 setup.py develop`
 * (In `son-sm/`)
    * `python3.4 son-ssm-examples/PSA_placement/placement/placement.py`

## How to test it
* Do the following; each in a separate terminal.
    1. Run the fake_SMR
    2. Run the placement container
    3. In son-sm/son-ssm-examples/PSA_placement/placement/test run python3.4 placementtrigger.py

* The expected results are as follows:

    * In the placementtrigger terminal:

        ```
        {'status': 'COMPLETED', 'mapping': {'vnfd2': {'vim': '1234'}, 'vnfd1': {'vim': '1234'}}, 'error': None}
        ```

    * In placement terminal:

         ```
         INFO:son-sm-base:Starting sonssmpsaplacement1 ...
         INFO:son-sm-base:Sending registration request...
         INFO:son-sm-base:sonssmpsaplacement1 registered with uuid:23345
         DEBUG:PSA-ssm-placement:Received registration ok event.
         INFO:PSA-ssm-placement:Subscribed to placement.ssm.1234
         INFO:PSA-ssm-placement:Placement started
         INFO:PSA-ssm-placement:Mapping algorithm started.
         INFO:PSA-ssm-placement:Mapping succeeded: {'vnfd1': {'vim': '1234'}, 'vnfd2': {'vim': '1234'}}
         INFO:PSA-ssm-placement:The mapping calculation has succeeded.
         ```
