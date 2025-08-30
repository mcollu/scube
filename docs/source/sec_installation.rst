.. _sec_installation:

Installation
============

Pre-requisites
--------------

Requires the installation of WEIS by NREL.

Info on how to install it can be found `here <https://weis.readthedocs.io/en/latest/installation.html>`_.

It is recommended to use miniforge to create conda environments and install WEIS. Miniforge can be obtained `here <https://github.com/conda-forge/miniforge>`_.

.. note::
   
   In the following, it is assumed that you named ``weis-env`` the WEIS environment where to run SCUBE.

Installation instructions
-------------------------

1. Make sure to have the zip file “scube.zip” provided
2. Expand the file in the desired place in your disk
3. Open a conda command prompt (*if you are using miniforge, use the Miniforge Prompt*)
4. Make sure to activate the conda environment you have created to run WEIS and SCUBE (``e.g., weis-env``)

.. code:: bash

   conda activate weis-env

.. note::
   If you do not remember what name you gave to the environment, you can run the following to know the names of all the environments

   .. code:: bash

      conda info --envs

5. Navigate, within the command prompt, to the folder ``scube\dist``
6. Install the latest version of the SCUBE .whl file that you can find in that folder. For example, for SCUBE v1.0.0

.. code:: bash

   pip install scube-1.0.0-py3-none-any.whl

This should install all the SCUBE scripts and all the required packages.

.. note::

   If you have installed a previous version of SCUBE, make sure to uninstall that first, and then install the new version of SCUBE, with the following command

   .. code:: bash
   
      pip uninstall scube

Testing
-------
After installation, you can run a quick check to verify the correct installation.

1. Open a conda prompt (miniforge prompt, for example), navigate to the folder ``scube\tests``
2. Run the following command 

.. code:: bash

   python test_ALL.py

This runs a demonstration of a series of typical wind turbine analyses and writes results into the ``scube\tests\output`` folder.
In the miniforge prompt command window, the following should appear (multiple times, once for each analysis).

N.B. Some WEIS "Warnings" may appear. They are not critical.

.. code:: bash

   Running: test.py A03 L2
   Output:
   Using weis.aeroelasticse in rosco.toolbox...
   
    ******* SCUBE: preprocessing - updating tower geometry *******
   
    ******* SCUBE: postprocessing - results VS constraints analysis *******
   
            ******* Constraint definitions imported *******
   
            ******* Simulation output xlsx and yaml files data loaded *******
   
            ******* Constraint verification started *******
   
                    Check of constraint Min_twr_1st_fa_freq_L2
   
                    Check of constraint Min_twr_1st_ss_freq_L2
   
            ******* Constraint verification completed *******
                  Constraint  ...                                      Description
   0  Min_twr_1st_fa_freq_L2  ...   Min tower natural frequency, 1st fore-aft mode
   1  Min_twr_1st_ss_freq_L2  ...  Min tower natural frequency, 1st side-side mode
   
   [2 rows x 7 columns]
   
   ******* SCUBE: Validation report with formatting exported successfully *******
   
   [INFO] Time taken: 0:00:03

3. Check in the folder ``scube\tests\output`` the validation reports created (e.g., validation_report_A03_L2.xlsx)
