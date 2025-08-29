Analysis A01 L0: Tower geometrical characteristics check
========================================================

Introduction
------------
Aim
~~~
To verify that the geometrical characteristics of the tower are compatible against a series of simple geometrical checks

Constraints
~~~~~~~~~~~
To be specified in CNSTR.xlsx

+---+------------------+------+-----------------+---------------------------------------------------------------------+
| # | Constraint name  | Unit | Suggested value | Description                                                         |
+===+==================+======+=================+=====================================================================+
| 1 | Eq_twr_top_OD    | m    | 6.5^            | Compatibility. Tower top diameter compatible with RNA bedplate      |
+---+------------------+------+-----------------+---------------------------------------------------------------------+
| 2 | Eq_twr_top_thick | m    | 0.050^          | Compatibility. Tower top thickness compatible with RNA bedplate     |
+---+------------------+------+-----------------+---------------------------------------------------------------------+
| 3 | Max_twr_OD       | m    | 10^^            | Manufacturability. Max tower outer diameter                         |
+---+------------------+------+-----------------+---------------------------------------------------------------------+
| 4 | Max_twr_slope    | deg  | 4.00^^          | Structural integrity. Each can cannot have a slope higher than this |
+---+------------------+------+-----------------+---------------------------------------------------------------------+
| 5 | Min_twr_d_to_t   | N/A  | 80^^?           | Manufacturability. Min tower outer diameter to thickness ratio      |
+---+------------------+------+-----------------+---------------------------------------------------------------------+
| 6 | Max_twr_d_to_t   | N/A  | 150^^?          | Manufacturability. Max tower outer diameter to thickness ratio      |
+---+------------------+------+-----------------+---------------------------------------------------------------------+


+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Notes |                                                                                                                                                                                 |
+=======+=================================================================================================================================================================================+
| ^     | For IEA 15MW Reference Wind Turbine (Updated reference values `here <https://github.com/IEAWindSystems/IEA-15-240-RWT/blob/master/Documentation/IEA-15-240-RWT_tabular.xlsx>`_) |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ^^    | Value agreed in WIND-14 STIFF-STIFF TOWER DESIGN FOR FLOATING WIND TURBINES (Previous TIC LCPE project)                                                                         |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Methodology
~~~~~~~~~~~
The values of the parameters, read from the WEIS output files, are compared against the constraint values.
No postprocessing of the WEIS output necessary.

Prepare the input file
----------------------
The SCUBE input data can be found in ``scube\data``.

Tower
~~~~~
- Open the file ``INPUT_tower.xlsx``
- Familiarise yourself with the variables, explained in the ``legend`` sheet
- Specify the geometry of the cans in the ``geometry`` sheet
- Specify the aerodynamic drag properties of the tower in the ``drag`` sheet (if unsure, leave the default values, they can be applied to a wide range of dimensions)
- Specify the tower material characteristics in the ``material`` sheet (the default values are for the steel	ASTM A572 Grade 50, see more `here	<http://www.matweb.com/search/DataSheet.aspx?MatGUID=9ced5dc901c54bd1aef19403d0385d7f>`_


Run the analysis
----------------
- Open a miniforge/miniconda/conda terminal prompt
- Activate the WEIS environment you set up (see :ref:`sec_installation`)
.. code:: bash
  conda activate weis-env
- Navigate to the root folder `scube'
- Launch the analysis Axy Lz with the following command
.. code:: bash
  `python main.py A01 L1'

Interpret the output file (validation report)
---------------------------------------------
