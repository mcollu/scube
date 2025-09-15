Analysis A02 Lx: max average tilt angle (serviceability limit state)
====================================================================

Introduction
------------
Aim
~~~
To verify that the *average* whole platform inclination angle is less than the specified angle (serviceability limit state).

Constraints
~~~~~~~~~~~

+----+---------------+-------+-------+---------------------------------------------------------------------------------+
| ID | Constraint    | Units | Value | Description                                                                     |
+====+===============+=======+=======+=================================================================================+
| 1  | Max_tilt_mean | deg   | 5.00  | Max allowable average tilt (global platform response inclination) in roll/pitch |
+----+---------------+-------+-------+---------------------------------------------------------------------------------+


+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Notes |                                                                                                                                                                                 |
+=======+=================================================================================================================================================================================+
| ^     | For IEA 15MW Reference Wind Turbine (Updated reference values `here <https://github.com/IEAWindSystems/IEA-15-240-RWT/blob/master/Documentation/IEA-15-240-RWT_tabular.xlsx>`_) |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ^^    | Value agreed in WIND-14 STIFF-STIFF TOWER DESIGN FOR FLOATING WIND TURBINES (Previous TIC LCPE project)                                                                         |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ^^^   | DNV-RP-0289, Section 5.5 Serviceability limit state                                                                                                                             |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Methodology
~~~~~~~~~~~

Level 0 (L0)
^^^^^^^^^^^^
The average tilt angle is obtained with an analytical approach, using the following equation.

.. math::
   \theta = \arcsin \left( \frac{F_{Trated} * (z_h - z_m)} { F_B * GM_{pitch} } \right) / \pi*180

where 

+----------------------+------+------------------------------------------------------------+
| Variable             | u.m. | Description                                                |
+======================+======+============================================================+
|  :math:`F_{Trated}`  | N    | Aerodynamic thrust at rated wind speed                     |
+----------------------+------+------------------------------------------------------------+
|  :math:`z_h`         | m    | Hub height vertical coordinate                             |
+----------------------+------+------------------------------------------------------------+
|  :math:`z_m`         | m    | Fairlead vertical coordinate                               |
+----------------------+------+------------------------------------------------------------+
|  :math:`F_B`         | N    | Buoyancy force (at rest)                                   |
+----------------------+------+------------------------------------------------------------+
|  :math:`GM_{pitch}`  | m    | Metacentric height for a rotation around the y axis        |
+----------------------+------+------------------------------------------------------------+

Level 1 (L1)
^^^^^^^^^^^^
Not implemented.

Level 2 (L2)
^^^^^^^^^^^^
.. warning::

   Differently from L0, at this analysis level, a series of OpenFAST aero-hydro-servo-elastic coupled model of dynamics analyses are run, and postprocessed, therefore while A02 L0 takes a minute or so, A02 L2 may take 20-30 minutes to run.

OpenFAST is utilised to derive this parameter, adopting the following settings:

- Metocean conditions:
   - Wind
      - Speed: steady at rated wind speed (10.64 m/s)
      - Turbulence: No
      - Direction: 5 directions, 0:30:120
   - Waves: no waves
   - Currents: no currents
- Wind turbine
   - Situation: power production
- Simulation:
   - Analysis time: 600 seconds

SCUBE postprocessing extracts the last 60s of the roll (``PtfmRoll``) and pitch (``PtfmPitch``) OpenFAST output signals, derives the total tilt angle (square root of sum of squares), and then calculates its average, which is then compared against the average specified in the constraint file.

.. note::

   This is not a realistic wind, waves, and currents situation, since there are no waves, no currents, and the wind speed is constant in time (steady), therefore the metocean conditions will NOT be taken from the Metocean input xlsx.

   It is only used to estimate the average tilt angle of the overall system with a constant aerodynamic thrust force. In the simulation, the platform will initially drift toward the horizontal equilibrium condition (transient), and will then reach the final equilibrium position.

Perform the analysis
--------------------

Prepare the input file
~~~~~~~~~~~~~~~~~~~~~~
The SCUBE input data can be found in ``scube\data``.

Constraints
^^^^^^^^^^^

- Open the file ``CNSTR.xlsx``
- Familiarise yourself with the variables, explained in the ``legend`` sheet
- Select the sheet ``constraints_A02_L0`` for level 0 (L0) analysis, or ``constraints_A02_L2`` for level 2 (L2) analysis
- A pre-prepared list of contraints and values can be found. Adjust the value for each constraint (where available) if necessary
- Save and close the spreadsheet file

Tower
^^^^^

- Open the file ``INPUT_tower.xlsx``
- Familiarise yourself with the variables, explained in the ``legend`` sheet
- Specify the geometry of the cans in the ``geometry`` sheet
- Specify the aerodynamic drag properties of the tower in the ``drag`` sheet (if unsure, leave the default values, they can be applied to a wide range of dimensions)
- Specify the tower material characteristics in the ``material`` sheet (the default values are for the steel	ASTM A572 Grade 50, see more `here	<http://www.matweb.com/search/DataSheet.aspx?MatGUID=9ced5dc901c54bd1aef19403d0385d7f>`_
- Save and close the spreadsheet file

Environment
^^^^^^^^^^^

For this analysis, this input file is not used, so you can ignore it.

OpenFAST simulation settings
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As mentioned, this analysis, at level 2 (L2), runs a series of (simple) OpenFAST analyses (see Methodology above).

The default values are relevant for the IEA 15MW Umaine semisubmrsible platform (see `here <https://github.com/IEAWindSystems/IEA-15-240-RWT/blob/master/Documentation/IEA-15-240-RWT_tabular.xlsx>`_).
If another wind turbine generator and/or another floating support structure are considered, the OpenFAST settings should be checked and, if necessary, adjusted.

If you wish to change the OpenFAST settings mentioned above, these can be changed by modifying the relevant values in the WEIS input file ``modeling_options_A02_L2.yaml``, which can be found in the folder ``scube\data\weis_anlyses\A02_L2\``: look at the values at line 110 and beyond. Please look at the in-line comments (i.e,, the text after the "#" symbol) for further info on the parameter description.

   .. code:: yaml

      DLC_driver:
          openfast_input_map:
              inflow_prop_dir: [InflowWind,PropagationDir]
              nac_yaw_dir: [ElastoDyn,NacYaw]
          DLCs:
              - DLC: "1.2" # With the new WEIS release, to be substituted by "Steady"
                wind_speed:           [10.64] # (m/s) Wind turbine rated wind speed
                user_group:
                  - inflow_prop_dir:  [0., 30., 60., 90., 120.]    # (deg) Wind propagation directions (for direction convention, please refer to the parameter "PropagationDir" in here: https://openfast.readthedocs.io/en/dev/source/user/fast.farm/InputFiles.html#ambient-wind-with-inflowwind-module-input-files
                    nac_yaw_dir:      [0., -30., -60., -90., -120.]    # (deg) Initial or fixed nacelle-yaw angle (degrees). NB To align wind and nacelle, nac_yaw_dir = -inflow_prop_dir, e.g., if inflow_prop_dir= [30.], nac_yaw_dir= [-30.]
                analysis_time: 600 #600       # (s)   OpenFAST simulation analysis time.
                transient_time: 0
                turbulent_wind:
                  flag: True
                  HubHt: 150                  # (m)   Wind turbine rotor hub height, inertial axis system
                  WindProfileType: 'PL'
                  RefHt: 150                  # (m)   Reference height, height at which the wind speed (i.e, "wind_speed") is given. Keep equal to HubHt
                  PLExp: 0.12                 # (N/A) Wind profile power law exponent
                  TurbModel: 'NONE'

For example, it may be necessary to change:

- The rated wind speed at which these simulations are done: change the value of ``wind_speed``
- The number of wind directions over which the simulation is performed: change the values in the ``inflow_prop_dir`` vector, and accordingly the ``nac_yaw_dir`` vector. For notation, please see `here <https://openfast.readthedocs.io/en/dev/source/user/fast.farm/InputFiles.html#ambient-wind-with-inflowwind-module-input-files>`_
- The simulation time length: change ``analysis_time``, making sure it is long enough to have the initial transient responses disappear
- The hub height: change ``HubHt``
- The reference height, at which the wind speed is specified: change ``RefHt``
- The wind profile power law exponent: change ``PLExp``

Run the analysis
~~~~~~~~~~~~~~~~
- Open a miniforge/miniconda/conda terminal prompt
- Activate the WEIS environment you set up (see :ref:`sec_installation`)

.. code:: bash

  conda activate weis-env

- Navigate to the root folder ``scube``

- Launch the analysis with the following command

.. code:: bash

  python main.py A02 L0

or

.. code:: bash

  python main.py A02 L2

Expected conda prompt outcome
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If all goes well, you should see something similar to the following.

Level 0 (L0)
^^^^^^^^^^^^

.. code:: bash
  
  Using weis.aeroelasticse in rosco.toolbox...

    ******* SCUBE: preprocessing - updating tower geometry *******
   
    ******* SCUBE: processing - WEIS analysis *******
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
   
   Run time (A02_L0): 11.181295156478882
   
    ******* SCUBE: postprocessing - results VS constraints analysis *******
   UserWarning: C:\Users\mauri\miniforge3\envs\weis-env2\Lib\site-packages\openpyxl\worksheet\_read_only.py:85
   Data Validation extension is not supported and will be removed
            ******* Constraint definitions imported *******
   
            ******* Simulation output xlsx and yaml files data loaded *******
   
            ******* Constraint verification started *******
   
                    Check of constraint Max_tilt_mean
   
            ******* Constraint verification completed *******
   
    ******* SCUBE: Validation report with formatting exported successfully *******
   
   [INFO] Time taken: 0:00:14
   
Level 2 (L2)
^^^^^^^^^^^^
See the full output :doc:`here <sec_ex_A02_L2_prompt_output>`

Common errors
-------------

Permission error
~~~~~~~~~~~~~~~~
.. code:: bash

  PermissionError: [Errno 13] Permission denied: 'data/INPUT_tower.xlsx'

The file ``INPUT_tower.xlsx`` is still open on your pc. In order to be safely read by SCUBE, the file needs to be closed.

A similar error can occur for ``CNSTR.xlsx``.
