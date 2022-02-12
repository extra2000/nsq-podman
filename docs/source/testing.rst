Testing
=======

These simple tests are to ensure NSQ has been successfully deployed.

Push a simple message to a new topic ``test``:

.. code-block:: bash

    curl -d 'hello world 1' 'http://127.0.0.1:4151/pub?topic=test'

Start a new terminal and execute the following command:

.. code-block:: bash

    podman run -it --network=host --rm docker.io/nsqio/nsq:v1.2.1 sh
    nsq_to_file --topic=test --output-dir=/tmp --lookupd-http-address=127.0.0.1:4161

Push more messages to the topic ``test``:

.. code-block:: bash

    curl -d 'hello world 2' 'http://127.0.0.1:4151/pub?topic=test'
    curl -d 'hello world 3' 'http://127.0.0.1:4151/pub?topic=test'
