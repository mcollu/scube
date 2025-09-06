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

+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
| Type       | Check                             | OpenFAST variable                           | Method                                 |
+============+===================================+=============================================+========================================+
| SLS        |                                   |                                             |                                        |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
|            | Mean tilt angle                   | ``PtfmPitch``, ``PtfmRoll``                 | `Link <meth_get_tilt_mean_L3>`_        |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
|            | Max tilt angle                    | ``PtfmPitch``, ``PtfmRoll``                 | `Link <meth_get_tilt_max_L3>'_         |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
|            | MPM tilt angle                    | ``PtfmPitch``, ``PtfmRoll``                 | `Link <meth_get_tilt_MPM_L3>`_         |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
|            | Max nac. acc. (x,y,z)             | ``NcIMUTAxs``, ``NcIMUTAys``, ``NcIMUTAzs`` | `Link <meth_get_nac_acc_xyz_max_L3>'_  |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
|            | MPM nac. acc. (x,y,z)             | ``NcIMUTAxs``, ``NcIMUTAys``, ``NcIMUTAzs`` | `Link <meth_get_nac_acc_xyz_MPM_L3>'_  |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
| Geometric  |                                   |                                             |                                        |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
|            | Min distance blade tip-tower axis | ``TipClrnc1``, ``TipClrnc2``, ``TipClrnc3`` | `Link <meth_get_bld_tip_clr_twr_L3>`_  |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
| Structural |                                   |                                             |                                        |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
|            | Max tower base shear              | ``TwrBsFxt``, ``TwrBsFyt``                  | `Link <meth_get_twr_bs_shear_max_L3>`_ |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
|            | MPM tower base sheat              | ``TwrBsFxt``, ``TwrBsFyt``                  | `Link <meth_get_twr_bs_shear_MPM_L3>`_ |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
|            | Max tower base bending moment     | ``TwrBsMxt``, ``TwrBsMyt``                  | `Link <meth_get_twr_bs_bend_max_L3>`_  |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
|            | MPM tower base bending moment     | ``TwrBsMxt``, ``TwrBsMyt``                  | `Link <meth_get_twr_bs_bend_MPM_L3>`_  |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+
|            | Local buckling                    | Various                                     | Please refer to Eq 3.11, DNV RP C202   |
+------------+-----------------------------------+---------------------------------------------+----------------------------------------+



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

Level 3 (L3)
^^^^^^^^^^^^

See the full output :doc:`here <sec_ex_A10_L3_prompt_output>`

Common errors
-------------

Permission error
~~~~~~~~~~~~~~~~
.. code:: bash

  PermissionError: [Errno 13] Permission denied: 'data/INPUT_tower.xlsx'

The file ``INPUT_tower.xlsx`` is still open on your pc. In order to be safely read by SCUBE, the file needs to be closed.

A similar error can occur for ``CNSTR.xlsx``

