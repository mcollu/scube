Analysis A03 Lx: tower natural frequencies of vibration
=======================================================

.. warning::

   WORK IN PROGRESS - IGNORE FOR THE TIME BEING

Introduction
------------
Aim
~~~
To verify if the natural frequencies of the tower's flexible modes are sufficiently detuned from the excitation frequencies.

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

+----+------------------------+-------+--------+-------------------------------------------------+
| ID | Constraint             | Units | Value  | Description                                     |
+====+========================+=======+========+=================================================+
| 1  | Min_twr_1st_fa_freq_L2 | Hz    | 0.375^ | Min tower natural frequency, 1st fore-aft mode  |
+----+------------------------+-------+--------+-------------------------------------------------+
| 2  | Min_twr_1st_ss_freq_L2 | Hz    | 0.375^ | Min tower natural frequency, 1st side-side mode |
+----+------------------------+-------+--------+-------------------------------------------------+
| 3  | Max_twr_1st_fa_freq_L2 | Hz    | 0.50^  | Max tower natural frequency, fore-aft modes     |
+----+------------------------+-------+--------+-------------------------------------------------+
| 4  | Max_twr_1st_ss_freq_L2 | Hz    | 0.50^  | Max tower natural frequency, side-side modes    |
+----+------------------------+-------+--------+-------------------------------------------------+


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
   The suggested min value, to compare the tower natural frequency against, is, for a stiff-stiff tower, equal to three times the max rotational speed of the rotor (i.e., 3P max).

   The suggested max value, to compare the tower natural frequency against, is, for a stiff-stiff tower, equal to six times the min rotational speed of the rotor (i.e., 6P min).

Methodology
~~~~~~~~~~~

Level 1 (L1)
^^^^^^^^^^^^
At level 1, the WEIS eigenanalysis capability is leveraged, and the natural frequencies of the modes of vibration of the tower are read from the WEIS output xlsx file.

The two parameters read are:
- ``floatingse.fore_aft_freqs``: natural frequencies of vibration of the fore-aft modes of vibration of the tower.
- ``floatingse.side_side_freqs``: natural frequencies of vibration of the side-side modes of vibration of the tower.

The advantage of this analysis at this level is its speed, but it is considered less accurate than the level 2 analysis (see below).

Level 2 (L2)
^^^^^^^^^^^^
At level 2, two OpenFAST aero-hydro-servo-elastic coupled model of dynamics simulations are conducted, more precisely two free decay tests.

Then, a frequency analysis of the free decay tower top displacement response signal is performed (SCUBE postprocessing), identifying the peak frequency, which is assumed to be the 1st natural frequency of the relevant tower mode of vibration.

These frequencies are then compared against the values specified in the constraint input spreadsheet.

.. note::

   Differently from L1, at this analysis level two OpenFAST aero-hydro-servo-elastic coupled model of dynamics simulations are run, and postrocessed.

   Nonetheless, since only two simulations are run, and since they are rather short (around 5 seconds), also this analysis is rather fact, taking only a few minutes.

Considering a no wind, no waves, no currents environment, the tower top is deformed in the fore-aft direction (1st simulation) and side-to-side direction (2nd simulation), by imposing a displacement of the tower top as starting value of the simulation.

Perform the analysis
--------------------

Prepare the input file
~~~~~~~~~~~~~~~~~~~~~~
The SCUBE input data can be found in ``scube\data``.

Tower
^^^^^

- Open the file ``INPUT_tower.xlsx``
- Familiarise yourself with the variables, explained in the ``legend`` sheet
- Specify the geometry of the cans in the ``geometry`` sheet
- Specify the aerodynamic drag properties of the tower in the ``drag`` sheet (if unsure, leave the default values, they can be applied to a wide range of dimensions)
- Specify the tower material characteristics in the ``material`` sheet (the default values are for the steel	ASTM A572 Grade 50, see more `here	<http://www.matweb.com/search/DataSheet.aspx?MatGUID=9ced5dc901c54bd1aef19403d0385d7f>`_

Metocean
^^^^^^^^

This analysis does not need to consider the conditions specified in the metocean input spreadsheet, so this can be ignored.

Run the analysis
~~~~~~~~~~~~~~~~
- Open a miniforge/miniconda/conda terminal prompt
- Activate the WEIS environment you set up (see :ref:`sec_installation`)

.. code:: bash

  conda activate weis-env

- Navigate to the root folder ``scube``

- Launch the analysis with the following command

.. code:: bash

  python main.py A03 L1

or

.. code:: bash

  python main.py A03 L2

Expected conda prompt outcome
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If all goes well, you should see something similar to the following.

Level 1 (L1)
^^^^^^^^^^^^

.. code:: bash
  
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
