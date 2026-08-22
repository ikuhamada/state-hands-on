.. _instruction_ohtaka:

:orphan:

Minimum description of the job script
=====================================

Example of the job script

.. code:: bash

  #!/bin/sh
  #SBATCH -J  CO
  #SBATCH -p  cmdinteractive
  #SBATCH -N  1
  #SBATCH -n  4
  #SBATCH -t  00:30:00
  
  # Load the modules
  
  module purge
  module load oneapi_compiler/2023.0.0
  module load oneapi_mkl/2023.0.0
  module load oneapi_mpi/2023.0.0

  # Set this variable to use with OpenAPI and IntelMPI
  
  export FI_PROVIDER=psm3
  
  # Set the STATE executable
  
  ln -fs ${HOME}/STATE/src/state/src/STATE .
  
  # Set the pseudopotential files
  
  ln -fs ../gncpp/pot.C_pbe1
  ln -fs ../gncpp/pot.O_pbe1
  
  # Set the input/output files
  
  INPUT_FILE=nfinp_scf
  OUTPUT_FILE=nfout_scf
  
  # Run!
  
  ulimit -s unlimited
  
  srun ./STATE < ${INPUT_FILE} > ${OUTPUT_FILE}

Header
------

.. code:: bash

  #!/bin/sh
  #SBATCH -J  CO
  #SBATCH -p  cmdinteractive
  #SBATCH -N  1
  #SBATCH -n  4
  #SBATCH -t  00:30:00

* 1st line:

.. code:: bash

  #!/bin/sh

Name of the shell used

* 2nd line:

.. code:: bash

  #SBATCH -J  CO

Name of the job, which is shown when you use the ``squeue`` command

* 3rd line:

.. code:: bash

  #SBATCH -p  cmdinteractive

Name of the partition (queue)

* 4th line:

.. code:: bash

  #SBATCH -N  1

Number of nodes 

* 5th line:

.. code:: bash

  #SBATCH -n  4

Number of MPI processes

In the case OpenMPI (thread) parallelization needs to be activated, add the following (in this case, 2 thread parallelization will be performed):

.. code:: bash

  #SBATCH -c  2

In this example, we use 4 cores with 1 node.

.. note::
	Each node of ohtaka has 128 cores, and (number of MPI processes) times (number of OpenMPI processes) should be within the resource you request.

* 6th line:

.. code:: bash

  #SBATCH -t  00:30:00

Body
----

.. code:: bash

  # Load the modules
  
  module purge
  module load oneapi_compiler/2023.0.0
  module load oneapi_mkl/2023.0.0
  module load oneapi_mpi/2023.0.0

  # Set this variable to use with OpenAPI and IntelMPI
  
  export FI_PROVIDER=psm3
  export MKL_NUM_THREADS=1
 
  # Set the STATE executable
   
  ln -fs ${HOME}/STATE/src/state/src/STATE .
   
  # Set the pseudopotential files
   
  ln -fs ../gncpp/pot.C_pbe1
  ln -fs ../gncpp/pot.O_pbe1
    
  # Set the input/output files
   
  INPUT_FILE=nfinp_scf
  OUTPUT_FILE=nfout_scf
  
  # Run!
   
  ulimit -s unlimited
   
  srun ./STATE < ${INPUT_FILE} > ${OUTPUT_FILE}

* Modules

.. code::

  # Load the modules
   
  module purge
  module load oneapi_compiler/2023.0.0
  module load oneapi_mkl/2023.0.0
  module load oneapi_mpi/2023.0.0

Please do not change them, unless you build STATE with different modules.

* Platform specific variable

.. code::

  # Set this variable to use with OpenAPI and IntelMPI
  
  export FI_PROVIDER=psm3
  export MKL_NUM_THREADS=1

These variables are necessary to run a program properly with the above modules. Please also do not change it.

* STATE executable

.. code::

  # Set the STATE executable
   
  ln -fs ${HOME}/STATE/src/state/src/STATE .
 
Please do not change this line, unless you don't change the name of the STATE executable

* Pseudopotentials

.. code::

  # Set the pseudopotential files
   
  ln -fs ../gncpp/pot.C_pbe1
  ln -fs ../gncpp/pot.O_pbe1
  
Please choose all the pseudopotentials you need to use and write here (change ``C_pbe1`` and ``O_pbe1``, and add more lines if necessary)

* Input/output files

.. code::

  # Set the input/output files
    
  INPUT_FILE=nfinp_scf
  OUTPUT_FILE=nfout_scf

* Execution

.. code::

  # Run!
   
  ulimit -s unlimited
   
  srun ./STATE < ${INPUT_FILE} > ${OUTPUT_FILE}

Please change the input (``nfinp_scf``) and output (``nfout_scf``) file names as necessary.
 
You don't have to change the following lines in the job script.


Minimum list of submission commands
===================================

* Submission of a job

.. code::

  $ sbatch [job script]

* Check the job status

.. code::

  $ squeue

* Cancel the job

.. code::

  $ scancel [Job ID]

Use ``squeue`` to know your JOB ID


Available resources on ohtaka
=============================

+-------------------+-----------------+-----------------+------------+
| Partition (queue) | Min. # of nodes | Max. # of nodes | Max. hours |
+===================+=================+=================+============+
| cmdinteractive    | 1               | 8               | 24 hours   |
+-------------------+-----------------+-----------------+------------+
| cmd1cpu           | 1               | 1               | 24 hours   |
+-------------------+-----------------+-----------------+------------+
| cmd16cpu          | 2               | 16              | 24 hours   |
+-------------------+-----------------+-----------------+------------+

.. warning::
	Max. number of nodes allocated for the CMD workshop is 36, depending on the queue/partion, but the max. number of nodes which can be used at the same time is limited to 16 to all the CMD perticipants.
	If you used too may cores, other users in the course have to wait until their reqired number nodes are available.
	Please choose the number of cores with care.
	In the begininng, it is better to play within one node (128 cores) using ``cmd1cpu`` partion or to use many nodes but with limited computational time, to get accustomed to use many cores/nodes.

