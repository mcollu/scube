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

+----+---------------------+-------+-------+------------------------------------------------------------------------+
| ID | Constraint          | Units | Value | Description                                                            |
+====+=====================+=======+=======+========================================================================+
| 1  | Min_twr_1st_fa_freq | Hz    |       | Min allowable tower natural frequency, 1st fore-aft mode of vibration  |
+----+---------------------+-------+-------+------------------------------------------------------------------------+
| 2  | Min_twr_1st_ss_freq | Hz    |       | Min allowable tower natural frequency, 1st side-side mode of vibration |
+----+---------------------+-------+-------+------------------------------------------------------------------------+

Level 2 (L2)

+----+------------------------+-------+-------+------------------------------------------------------------------------+
| ID | Constraint             | Units | Value | Description                                                            |
+====+========================+=======+=======+========================================================================+
| 1  | Min_twr_1st_fa_freq_L2 | Hz    |       | Min allowable tower natural frequency, 1st fore-aft mode of vibration  |
+----+------------------------+-------+-------+------------------------------------------------------------------------+
| 2  | Min_twr_1st_ss_freq_L2 | Hz    |       | Min allowable tower natural frequency, 1st side-side mode of vibration |
+----+------------------------+-------+-------+------------------------------------------------------------------------+


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

   This is not a realistic wind, waves, and currents situation, since there are no waves, no currents, and the wind speed is constant in time (steady), therefore the metocean conditions will NOT be taken from the Metocean input xlsx.

   It is only used to have an estimation of the average tilt angle of the overall system with a constant aerodynamic thrust force. In the simulation, the platform will initially drift toward the horizontal equilibrium condition (transient), and will then reach the final equilibrium position.

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

  python main.py A01 L0

or

.. code:: bash

  python main.py A01 L2

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

A similar error can occur for ``CNSTR.xlsx``
