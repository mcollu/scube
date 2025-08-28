Getting started
===============

After installation, you can run a quick check to verify the correct installation.

1. Open a conda prompt (miniforge prompt, for example), navigate to the folder ``scube/tests/output``
2. Run the following command 

.. code:: bash

   python test_ALL.py

This runs a demonstration of a series of typical wind turbine analyses and writes results into the ``scube/tests`` folder.
In the miniforge prompt command window, the following should appear (multiple times, once for each analysis).
N.B. Some WEIS "Warnings" may appear. There are not critical.

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

3. Check in the folder scube\tests\output the validation reports created (e.g., validation_report_A03_L2.xlsx)
