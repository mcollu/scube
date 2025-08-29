Analysis A01 L0: Tower geometrical characteristics check
========================================================

Introduction
------------
- Aim: to verify that the geometrical characteristics of the tower are compatible against a series of simple geometrical checks
- Constraints (to be specified in CNSTR.xlsx)
+---+------------------+------+-----------------+---------------------------------------------------------------------+
| # | Constraint name  | Unit | Suggested value | Description                                                         |
+===+==================+======+=================+=====================================================================+
| 1 | Eq_twr_top_OD    | m    | 6.5*            | Compatibility. Tower top diameter compatible with RNA bedplate      |
+---+------------------+------+-----------------+---------------------------------------------------------------------+
| 2 | Eq_twr_top_thick | m    | 0.050*          | Compatibility. Tower top thickness compatible with RNA bedplate     |
+---+------------------+------+-----------------+---------------------------------------------------------------------+
| 3 | Max_twr_OD       | m    | 10**            | Manufacturability. Max tower outer diameter                         |
+---+------------------+------+-----------------+---------------------------------------------------------------------+
| 4 | Max_twr_slope    | deg  | 4.00**          | Structural integrity. Each can cannot have a slope higher than this |
+---+------------------+------+-----------------+---------------------------------------------------------------------+
| 5 | Min_twr_d_to_t   | -    | 80**?           | Manufacturability. Min tower outer diameter to thickness ratio      |
+---+------------------+------+-----------------+---------------------------------------------------------------------+
| 6 | Max_twr_d_to_t   | -    | 150**?          | Manufacturability. Max tower outer diameter to thickness ratio      |
+---+------------------+------+-----------------+---------------------------------------------------------------------+


+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Notes |                                                                                                                                                                                 |
+=======+=================================================================================================================================================================================+
| *     | For IEA 15MW Reference Wind Turbine (Updated reference values `here <https://github.com/IEAWindSystems/IEA-15-240-RWT/blob/master/Documentation/IEA-15-240-RWT_tabular.xlsx>`_) |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **    | Value agreed in WIND-14 STIFF-STIFF TOWER DESIGN FOR FLOATING WIND TURBINES (Previous TIC LCPE project)                                                                         |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+


Prepare the input file
----------------------

Run the analysis
----------------

Interpret the output file (validation report)
---------------------------------------------
