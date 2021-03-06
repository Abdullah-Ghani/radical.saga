
**************
Logging System
**************

In a distributed environment unified error logging and reporting is a crucial
capability for debugging and monitoring. SAGA has a configurable logging system
that  captures debug, info, warning and error messages across all of
its middelware adaptors. The logging system can be controlled in two different
ways: via :ref:`env_vars` variables, which should be sufficient in most
scenarios, and via the :ref:`log_api`, which provides programmatic access to
the logging system for advanced use-cases.


.. _env_vars:

Environment Variables
---------------------

Several environment variables can be used to control SAGA's logging behavior from
the command line. Obviously, this can come in handy when debugging a problem
with an existing SAGA application. Environment variables are set in the
executing shell and evaluated by SAGA at program startup.

.. envvar:: RADICAL_SAGA_LOG_LVL

   Controls the log level. This controls the amount of output generated by the
   logging system. ``RADICAL_LOG_LVL`` expects either a numeric (0-4) value or a
   string (case insensitive) representing the log level:

   +---------------+---------------------+------------------------------------+
   | Numeric Value | Log Level           | Type of Messages Displayed         |
   +===============+=====================+====================================+
   | 0             | ``CRITICAL``        | Only fatal events that will cause  |
   | (default)     |                     | SAGA to abort.                     |
   |               |                     |                                    |
   +---------------+---------------------+------------------------------------+
   | 1             | ``ERROR``           | Errors that will not necessarily   |
   |               |                     | cause SAGA to abort.               |
   |               |                     |                                    |
   +---------------+---------------------+------------------------------------+
   | 2             | ``WARNING``         | Warnings that are generated by     |
   |               |                     | SAGA and its middleware adaptors.  |
   |               |                     |                                    |
   +---------------+---------------------+------------------------------------+
   | 3             | ``INFO``            | Useful (?) runtime information     |
   |               |                     | that is generated by SAGA and its  |
   |               |                     | middleware adaptors.               |
   +---------------+---------------------+------------------------------------+
   | 4             | ``DEBUG``           | Debug message added to the code    |
   |               |                     | by the developers. (Lots of output)|
   |               |                     |                                    |
   +---------------+---------------------+------------------------------------+

   For example, if you want to see the debug messages that SAGA generates during
   program execution, you would set :envvar:`RADICAL_LOG_LVL` to ``DEBUG``
   before you run your program::

       RADICAL_SAGA_LOG_LVL=DEBUG python mysagaprog.py


  .. envvar:: RADICAL_LOG_LVL

     Controls the message sources displayed. RCT use an hierarchal structure for
     its log sources. Starting with the root logger ``RADICAL``, sub loggers are
     defined for internal logging events (``RADICAL_SAGA``,
     ``RADICAL_SAGA_ENGINE`` etc.) and individual middleware adaptors, e.g.,
     ``RADICAL_SAGA_ADAPTORS_NAME``.  ``LOG_LVL`` and ``LOG_TGT`` can be set
     individually for those loggers.

     For example, if you want to see only the debug messages generated by
     ``saga.engine`` and a specific middleware adaptor called ``xyz`` you would
     set the following environment variables::

         RADICAL_LOG_LVL=ERROR \                       # mute everything
         RADICAL_SAGA_ENGINNE_LOG_LVL=DEBUG \          # enable engine logger
         RADICAL_SAGA_ADAPTORS_XYZ_LOG_LVL=DEBUG \     # enable XYZ logger
         python mysagaprog.py


.. envvar:: RADICAL_SAGA_LOG_TGT

   Controls where the log messages go. Multiple concurrent locations are
   supported.  ``RADICAL_LOG_TGT`` expects either a single location or
   a comma-separated list of locations, where a location can either be
   a path/filename or the ``stdout``/``stderr`` keyword (case sensitive) for
   logging to the console.

   For example, if you want to see debug messages on the console but also
   want to log them in a file for further analysis, you would set the the
   following environment variables::

       RADICAL_SAGA_LOG_LVL=DEBUG RADICAL_SAGA_LOG_TGT=stdout,./rs.log \
       python mysagaprog.py


