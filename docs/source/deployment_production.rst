Example Podman Deployment for Production
========================================

Example how to deploy a small NSQ instance for production.

Getting Started
---------------

Clone this repository:

.. code-block:: bash

    git clone https://github.com/extra2000/nsq-podman.git

``cd`` into project's root directory:

.. code-block:: bash

    cd nsq-podman

Deploy ``nsqlookupd``
---------------------

From the project's root directory, ``cd`` into ``deployment/production/nsqlookupd/``:

.. code-block:: bash

    cd deployment/production/nsqlookupd/

Create configmap file:

.. code-block:: bash

    cp -v configmaps/nsq-nsqlookupd.yaml{.example,}

Create pod file:

.. code-block:: bash

    cp -v nsq-nsqlookupd-pod.yaml{.example,}

Load SELinux Security Policy:

.. code-block:: bash

    sudo semodule -i selinux/nsq_nsqlookupd.cil /usr/share/udica/templates/base_container.cil

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "nsq_nsqlookupd"

Deploy ``nsqlookupd``:

.. code-block:: bash

    podman play kube --configmap configmaps/nsq-nsqlookupd.yaml --seccomp-profile-root ./seccomp nsq-nsqlookupd-pod.yaml

Generate ``systemd`` files and enable on ``boot``:

.. code-block:: bash

    mkdir -pv ~/.config/systemd/user
    cd ~/.config/systemd/user
    podman generate systemd --files --name nsq-nsqlookupd-pod
    systemctl --user enable pod-nsq-nsqlookupd-pod.service container-nsq-nsqlookupd-pod-srv01.service

.. note::

    If the pod is destroyed and recreated, the ``systemd`` files must be recreated using the command above.

Deploy ``nsqd``
---------------

From the project's root directory, ``cd`` into ``deployment/production/nsqd/``:

.. code-block:: bash

    cd deployment/production/nsqd/

Create configmap file:

.. code-block:: bash

    cp -v configmaps/nsq-nsqd.yaml{.example,}

Create pod file:

.. code-block:: bash

    cp -v nsq-nsqd-pod.yaml{.example,}

Load SELinux Security Policy:

.. code-block:: bash

    sudo semodule -i selinux/nsq_nsqd.cil /usr/share/udica/templates/base_container.cil

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "nsq_nsqd"

Deploy ``nsqd``:

.. code-block:: bash

    podman play kube --configmap configmaps/nsq-nsqd.yaml --seccomp-profile-root ./seccomp nsq-nsqd-pod.yaml

Generate ``systemd`` files and enable on ``boot``:

.. code-block:: bash

    mkdir -pv ~/.config/systemd/user
    cd ~/.config/systemd/user
    podman generate systemd --files --name nsq-nsqd-pod
    systemctl --user enable pod-nsq-nsqd-pod.service container-nsq-nsqd-pod-srv01.service

.. note::

    If the pod is destroyed and recreated, the ``systemd`` files must be recreated using the command above.

Deploy ``nsqadmin``
-------------------

From the project's root directory, ``cd`` into ``deployment/production/nsqadmin/``:

.. code-block:: bash

    cd deployment/production/nsqadmin/

Create configmap file:

.. code-block:: bash

    cp -v configmaps/nsq-nsqadmin.yaml{.example,}

Create pod file:

.. code-block:: bash

    cp -v nsq-nsqadmin-pod.yaml{.example,}

Load SELinux Security Policy:

.. code-block:: bash

    sudo semodule -i selinux/nsq_nsqadmin.cil /usr/share/udica/templates/base_container.cil

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "nsq_nsqadmin"

Deploy ``nsqadmin``:

.. code-block:: bash

    podman play kube --configmap configmaps/nsq-nsqadmin.yaml --seccomp-profile-root ./seccomp nsq-nsqadmin-pod.yaml

Generate ``systemd`` files and enable on ``boot``:

.. code-block:: bash

    mkdir -pv ~/.config/systemd/user
    cd ~/.config/systemd/user
    podman generate systemd --files --name nsq-nsqadmin-pod
    systemctl --user enable pod-nsq-nsqadmin-pod.service container-nsq-nsqadmin-pod-srv01.service

.. note::

    If the pod is destroyed and recreated, the ``systemd`` files must be recreated using the command above.
