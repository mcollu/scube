Analysis A02 Lx: max average tilt angle (serviceability limit state)
====================================================================

.. note::

   WORK IN PROGRESS - IGNORE FOR THE TIME BEING

Introduction
------------
Aim
~~~
To verify that the *average* whole platform inclination angle is less than the specified angle (serviceability limit state).

Constraints
~~~~~~~~~~~
To be specified in CNSTR.xlsx

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
|  :math:`GM_{pitch}`  | m    | Metacentric height around for a rotation around the y axis |
+----------------------+------+------------------------------------------------------------+


Level 1 (L1)
^^^^^^^^^^^^
Not implemented.

Level 2 (L2)
^^^^^^^^^^^^
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

SCUBE postprocessing extracts the last 60s of the roll (``PtfmRoll``) and pitch (``PtfmPitch``) OpenFAST output signals, derive the total tilt angle (square root of sum of squares), and then calculates its average, which is then compared against the average specified in the constraint file.

.. note::

   This is not a realistic win, wave, currents situation, since there are no waves, no currents, and the wind speed is constant in time (steady). It only used to have an estimation of the average tilt angle of the overall system with a constant aerodynamic thrust force. In the simulation, the platform will initially drift toward the horizontal equilibrium condition (transient), and will then reach the final equilibrium position.

.. note::

   If you wish to change the OpenFAST settings mentioned above, these can be changed by modifying the relevant values in the WEIS input file ``modeling_options_A02_L2.yaml``, which can be found in the folder ``scube\data\weis_anlyses\A02_L2\``: look at the values at line 110 and afterward, which shold look like the following code.

   .. code:: yaml

      DLC_driver:
    openfast_input_map:
        inflow_prop_dir: [InflowWind,PropagationDir]
        nac_yaw_dir: [ElastoDyn,NacYaw]
    DLCs:
        - DLC: "1.2" # With the new WEIS release, to be substituted by "Steady"
          wind_speed:           [10.64]
          user_group:
            - inflow_prop_dir:  [0.]
            - nac_yaw_dir:      [0.]
          analysis_time: 600 #600
          transient_time: 0
          turbulent_wind:
            flag: True
            HubHt: 150
            WindProfileType: 'PL'
            RefHt: 150
            PLExp: 0.12
            TurbModel: 'NONE'
        - DLC: "1.2" # With the new WEIS release, to be substituted by "Steady"
          wind_speed:           [10.64]
          user_group:
            - inflow_prop_dir:  [30.]
            - nac_yaw_dir:      [-30.]
          analysis_time: 600 #600
          transient_time: 0
          turbulent_wind:
            flag: True
            HubHt: 150
            WindProfileType: 'PL'
            RefHt: 150
            PLExp: 0.12
            TurbModel: 'NONE'
        (and other similar for the other directions)

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


Run the analysis
~~~~~~~~~~~~~~~~
- Open a miniforge/miniconda/conda terminal prompt
- Activate the WEIS environment you set up (see :ref:`sec_installation`)

.. code:: bash

  conda activate weis-env

- Navigate to the root folder ``scube``

- Launch the analysis with the following command

.. code:: bash

  python main.py A01 L0

or

.. code:: bash

  python main.py A01 L2

Expected conda prompt outcome
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If all goes well, you should see something similar to the following.

.. code:: bash
  
  <to insert>


Common errors
-------------

Permission error
~~~~~~~~~~~~~~~~
.. code:: bash

  PermissionError: [Errno 13] Permission denied: 'data/INPUT_tower.xlsx'

The file ``INPUT_tower.xlsx`` is still open on your pc. In order to be safely read by SCUBE, the file needs to be closed.

A similar error can occur for ``CNSTR.xlsx``
