.. _slurm_command_ohtaka:

:orphan:

Minimum description of the SLURM commands
=========================================

sbatch
------
This command is used to submit a job. Usage:

.. code:: bash

  sbatch [job_script]

squeue
------
This command is used to see the jobs status. Usage:

.. code:: bash

  squeue

scancel
-------
This command is used to cancel the job. Usage:

.. code:: bash

  scancel [job_ID]

sacct
-----
This command is used to see the accounting information of finished jobs. Usage:

.. code:: bash

  sacct -j [job_ID]

