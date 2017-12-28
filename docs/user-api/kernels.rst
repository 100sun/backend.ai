Kernel Management
=================

Creating Kernel Session
-----------------------

* URI: ``/v2/kernel/create``
* Method: ``POST``

Creates a kernel session to run user-input code snippets.

Parameters
""""""""""

.. list-table::
   :widths: 15 5 80
   :header-rows: 1

   * - Parameter
     - Type
     - Description

   * - ``lang``
     - ``str``
     - The kernel type, usually the name of one of our supported programming languages.

   * - ``clientSessionToken``
     - ``str``
     - Client session token. Should be unique for continuous execution (for REPL).

   * - ``config``
     - ``object``
     - An optional :ref:`creation-config-object` to specify extra kernel
       configuration.

Example:

.. code-block:: json

   {
     "lang": "python3",
     "clientSessionToken": "EXAMPLE:STRING",
     "config": {
       "clusterSize": 1,
       "instanceMemory": 51240,
       "environ": {
         "MYCONFIG": "XXX",
       },
       "mounts": [
         "mydata",
         "mypkgs:.local/lib/python3.6/site-packages"
       ],
     }
   }


Response
""""""""

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - HTTP Status Code
     - Description
   * - 201 Created
     - The kernel is successfully created.
   * - 406 Not acceptable
     - The requested resource limits exceed the server's own limits.

.. list-table::
   :widths: 15 5 80
   :header-rows: 1

   * - Fields
     - Type
     - Values
   * - ``kernelId``
     - ``slug``
     - The kernel ID used for later API calls.


Example:

.. code-block:: json

   {
     "kernelId": "TSSJT2Z4SnmQhxjWMnJljg"
   }


Getting Kernel Information
--------------------------

* URI: ``/v2/kernel/:id``
* Method: ``GET``

Retrieves information about a kernel session.
For performance reasons, the returned information may not be real-time; usually
they are updated every a few seconds in the server-side.

Parameters
""""""""""

.. list-table::
   :widths: 15 5 80
   :header-rows: 1

   * - Parameter
     - Type
     - Description
   * - ``:id``
     - ``slug``
     - The kernel ID.

Response
""""""""

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - HTTP Status Code
     - Description
   * - 200 OK
     - The information is successfully returned.
   * - 404 Not Found
     - There is no such kernel.

.. list-table::
   :widths: 15 5 80
   :header-rows: 1

   * - Fields
     - Type
     - Values
   * - ``item``
     - ``object``
     - :ref:`session-item-object`.


Destroying Kernel Session
-------------------------

* URI: ``/v2/kernel/:id``
* Method: ``DELETE``

Terminates a kernel session.

Parameters
""""""""""

.. list-table::
   :widths: 15 5 80
   :header-rows: 1

   * - Parameter
     - Type
     - Description
   * - ``:id``
     - ``slug``
     - The kernel ID.

Response
""""""""

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - HTTP Status Code
     - Description
   * - 204 No Content
     - The kernel is successfully destroyed.
   * - 404 Not Found
     - There is no such kernel.


Restarting Kernel Session
-------------------------

* URI: ``/v2/kernel/:id``
* Method: ``PATCH``

Restarts a kernel session.
The idle time of the kernel will be reset, but other properties such as the age and CPU credit will continue to accumulate.
All global states such as global variables and modules imports are also reset.

Parameters
""""""""""

.. list-table::
   :widths: 15 5 80
   :header-rows: 1

   * - Parameter
     - Type
     - Description
   * - ``:id``
     - ``slug``
     - The kernel ID.

Response
""""""""

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - HTTP Status Code
     - Description
   * - 204 No Content
     - The kernel is successfully restarted.
   * - 404 Not Found
     - There is no such kernel.
