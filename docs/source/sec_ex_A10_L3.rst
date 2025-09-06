Analysis A03 Lx: tower natural frequencies of vibration
=======================================================

.. warning::

   WORK IN PROGRESS - IGNORE FOR THE TIME BEING

.. warning::

   This analysis will perform a number of OpenFAST time domain simulations (~100, depending on the metocean combinations), and therefore can take a considerable amount of computing time (2-3 days on an HPC desktop machine, using 1 core).

Introduction
------------
Aim
~~~
To perform a series of checks consideering the design load case DLC 1.6:

- Serviceability limit state chcks:

  - Mean tilt angle < constraint
  - Max tilt angle < constraint
  - MPM tilt angle < constraint
  - Max nacelle acceleration (x, y, z direction) < constraint
  - MPM nacelle acceleration (x, y, z direction) < constraint

- Geometric clearance checks:

  - Min distance between tower (axis) and blade tip > constraint

- Structural integrity checks:

  - Max tower base shear stress < constraint
  - MPM tower base shear stress < constraint
  - Max tower base bending moment < constraint
  - MPM tower base bending moment < constraint
  - Check for local buckling (DNV RP C202 2021)

Constraints
~~~~~~~~~~~
To be specified in CNSTR.xlsx .

Level 1 (L1)
^^^^^^^^^^^^

+----+---------------------+-------+--------+----------------------------------------------+
| ID | Constraint          | Units | Value  | Description                                  |
+====+=====================+=======+========+==============================================+
| 1  | Min_twr_1st_fa_freq | Hz    | 0.375^ | Min tower natural frequency, fore-aft modes  |
+----+---------------------+-------+--------+----------------------------------------------+
| 2  | Min_twr_1st_ss_freq | Hz    | 0.375^ | Min tower natural frequency, side-side modes |
+----+---------------------+-------+--------+----------------------------------------------+
| 3  | Max_twr_1st_fa_freq | Hz    | 0.50^  | Max tower natural frequency, fore-aft modes  |
+----+---------------------+-------+--------+----------------------------------------------+
| 4  | Max_twr_1st_ss_freq | Hz    | 0.50^  | Max tower natural frequency, side-side modes |
+----+---------------------+-------+--------+----------------------------------------------+


Level 2 (L2)
^^^^^^^^^^^^

+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| ID | Constraint                               | Units | Value   | Description                                                                    |
+====+==========================================+=======+=========+================================================================================+
| 1  | Max_tilt_mean                            | deg   | 5.0^^^  | Mean tilt angle less than constraint                                           |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 2  | Max_tilt_max                             | deg   | 10.0^^^ | Max tilt angle less than constraint                                            |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 3  | Max_tilt_MPM                             | deg   | 10.0^^^ | Most probable maximum tilt angle less than constraint                          |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 4  | Max_nac_acc_x_max                        | g     | 0.3^^^  | Max acceleration at tower top (x direction) less than constraint               |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 5  | Max_nac_acc_y_max                        | g     | 0.3^^^  | Max acceleration at tower top (y direction) less than constraint               |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 6  | Max_nac_acc_z_max                        | g     | 0.3^^^  | Max acceleration at tower top (z direction) less than constraint               |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 7  | Max_nac_acc_x_MPM                        | g     | 0.3^^^  | Most probable max acceleration at tower top (x direction) less than constraint |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 8  | Max_nac_acc_y_MPM                        | g     | 0.3^^^  | Most probable max acceleration at tower top (y direction) less than constraint |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 9  | Max_nac_acc_z_MPM                        | g     | 0.3^^^  | Most probable max acceleration at tower top (z direction) less than constraint |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 10 | Min_blade_tip_clrnc_from_twr             | m     | 5.0^    | Min blade tip clearance from tower AXIS higher than constraint (tower radius)  |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 11 | Max_twr_base_shear_max                   | kN    |         | Max tower base shear force less than constraint                                |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 12 | Max_twr_base_shear_MPM                   | kN    |         | Most probable max tower base shear force less than constraint                  |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 13 | Max_twr_base_bend_max                    | kNm   |         | Max tower base bending moment less than constraint                             |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 14 | Max_twr_base_bend_MPM                    | kNm   |         | Most probable max tower base bending moment less than constraint               |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+
| 15 | DNV_RP_C202_2021_buckling_res_cyl_shells | NA    | NA      | Check: buckling resistance of tower bottom can / shell                         |
+----+------------------------------------------+-------+---------+--------------------------------------------------------------------------------+


+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Notes |                                                                                                                                                                                 |
+=======+=================================================================================================================================================================================+
| ^     | For IEA 15MW Reference Wind Turbine (Updated reference values `here <https://github.com/IEAWindSystems/IEA-15-240-RWT/blob/master/Documentation/IEA-15-240-RWT_tabular.xlsx>`_) |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ^^    | Value agreed in WIND-14 STIFF-STIFF TOWER DESIGN FOR FLOATING WIND TURBINES (Previous TIC LCPE project)                                                                         |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ^^^   | DNV-RP-0289, Section 5.5 Serviceability limit state                                                                                                                             |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note::
   The suggested min value for the blade tip distance from the tower axis is 5m since this is the max diameter of the tower. Remember that the distance is NOT measured from the surface of the tower, but from the tower axis.

Methodology
~~~~~~~~~~~

Level 1 (L1)
^^^^^^^^^^^^
N/A

Level 2 (L2)
^^^^^^^^^^^^
N/A

Level 3 (L2)
^^^^^^^^^^^^
TBA

Perform the analysis
--------------------

Prepare the input file
~~~~~~~~~~~~~~~~~~~~~~
The SCUBE input data can be found in the folder ``scube\data``.

Tower
^^^^^

- Open the file ``INPUT_tower.xlsx``
- Familiarise yourself with the variables, explained in the ``legend`` sheet
- Specify the geometry of the cans in the ``geometry`` sheet
- Specify the aerodynamic drag properties of the tower in the ``drag`` sheet (if unsure, leave the default values, they can be applied to a wide range of dimensions)
- Specify the tower material characteristics in the ``material`` sheet (the default values are for the steel	ASTM A572 Grade 50, see more `here	<http://www.matweb.com/search/DataSheet.aspx?MatGUID=9ced5dc901c54bd1aef19403d0385d7f>`_)

Metocean
^^^^^^^^

- Open the file ``INPUT_environment.xlsx``
- Familiarise yourself with the variables, explained in the ``legend`` sheet
- Specify the metocean conditions in the ``wind_wave`` sheet

.. note::

   For DLC 1.6, only the following columns of the ``wind_wave`` sheet are used:

   - V_hub__mps (m/s), hub height wind speed
   - V_10__mps (m/s), wind speed at 10m height (above sea level)
   - Hs_SSS__m (m), Severe Sea State, Spectral significant wave height conditional on V_10_mps
   - Tp_SSS__s (s), Severe Sea State, Peak spectral period conditional on V_10_mps and Hs

Run the analysis
~~~~~~~~~~~~~~~~
- Open a miniforge/miniconda/conda terminal prompt
- Activate the WEIS environment you set up (see :ref:`sec_installation`)

.. code:: bash

  conda activate weis-env

- Navigate to the root folder ``scube``

- Launch the analysis with the following command

.. code:: bash

  python main.py A10 L3

Expected conda prompt outcome
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If all goes well, you should see something similar to the following.

Level 1 (L1)
^^^^^^^^^^^^

.. code:: bash
   
   Using weis.aeroelasticse in rosco.toolbox...

    ******* SCUBE: preprocessing - updating tower geometry *******
   
    ******* SCUBE: processing - WEIS analysis *******
   
   ================
   wisdem.wt.wt_rna
   ================
   NL: NLBGS 1 ; 5.75918932e+11 1
   NL: NLBGS 2 ; 12997931 2.2569029e-05
   NL: NLBGS 3 ; 302117.706 5.24583738e-07
   NL: NLBGS 4 ; 7598.06643 1.31929444e-08
   NL: NLBGS 5 ; 190.765705 3.3123708e-10
   NL: NLBGS Converged
   RuntimeWarning: C:\Users\mauri\miniforge3\envs\weis-env2\Lib\site-packages\wisdem\commonse\utilization_dnvgl.py:322
   The number of calls to function has reached maxfev = 50.RuntimeWarning: C:\Users\mauri\miniforge3\envs\weis-env2\Lib\site-packages\wisdem\commonse\cylinder_member.py:513
   divide by zero encountered in scalar divideRuntimeWarning: C:\Users\mauri\miniforge3\envs\weis-env2\Lib\site-packages\wisdem\commonse\cylinder_member.py:514
   divide by zero encountered in scalar divide----------------
   Design Variables
   ----------------
   name  val  size  lower  upper
   ----  ---  ----  -----  -----
   
   -----------
   Constraints
   -----------
   name  val  size  lower  upper  equals
   ----  ---  ----  -----  -----  ------
   
   ----------
   Objectives
   ----------
   name  val  size
   ----  ---  ----
   
   Run time (A03_L1): 42.0291702747345
   
    ******* SCUBE: postprocessing - results VS constraints analysis *******
   UserWarning: C:\Users\mauri\miniforge3\envs\weis-env2\Lib\site-packages\openpyxl\worksheet\_read_only.py:85
   Data Validation extension is not supported and will be removed
            ******* Constraint definitions imported *******
   
            ******* Simulation output xlsx and yaml files data loaded *******
   
            ******* Constraint verification started *******
   
                    Check of constraint Min_twr_1st_fa_freq
   
                    Check of constraint Min_twr_1st_ss_freq
   
                    Check of constraint Max_twr_1st_fa_freq
   
                    Check of constraint Max_twr_1st_ss_freq
   
            ******* Constraint verification completed *******
                Constraint Constraint Type Constraint um  Constraint Value  Simulated Value Status                                   Description
   0   Min_twr_1st_fa_freq             Min            Hz             0.375            0.537   Pass   Min tower natural frequency, fore-aft modes
   1   Min_twr_1st_fa_freq             Min            Hz             0.375            1.100   Pass   Min tower natural frequency, fore-aft modes
   2   Min_twr_1st_fa_freq             Min            Hz             0.375            1.630   Pass   Min tower natural frequency, fore-aft modes
   3   Min_twr_1st_ss_freq             Min            Hz             0.375            0.529   Pass  Min tower natural frequency, side-side modes
   4   Min_twr_1st_ss_freq             Min            Hz             0.375            1.430   Pass  Min tower natural frequency, side-side modes
   5   Min_twr_1st_ss_freq             Min            Hz             0.375            3.980   Pass  Min tower natural frequency, side-side modes
   6   Max_twr_1st_fa_freq             Max            Hz             0.500            0.537   Fail   Max tower natural frequency, fore-aft modes
   7   Max_twr_1st_fa_freq             Max            Hz             0.500            1.100   Fail   Max tower natural frequency, fore-aft modes
   8   Max_twr_1st_fa_freq             Max            Hz             0.500            1.630   Fail   Max tower natural frequency, fore-aft modes
   9   Max_twr_1st_ss_freq             Max            Hz             0.500            0.529   Fail  Max tower natural frequency, side-side modes
   10  Max_twr_1st_ss_freq             Max            Hz             0.500            1.430   Fail  Max tower natural frequency, side-side modes
   11  Max_twr_1st_ss_freq             Max            Hz             0.500            3.980   Fail  Max tower natural frequency, side-side modes
   
    ******* SCUBE: Validation report with formatting exported successfully *******
   
   [INFO] Time taken: 0:00:45

Level 2 (L2)
^^^^^^^^^^^^

See the full output :doc:`here <sec_ex_A03_L2_prompt_output>`

Common errors
-------------

Permission error
~~~~~~~~~~~~~~~~
.. code:: bash

  PermissionError: [Errno 13] Permission denied: 'data/INPUT_tower.xlsx'

The file ``INPUT_tower.xlsx`` is still open on your pc. In order to be safely read by SCUBE, the file needs to be closed.

A similar error can occur for ``CNSTR.xlsx``

