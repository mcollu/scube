SCUBE Inputs
============

Basic level
-----------

If you want to use the pre-set wind turbine and support structure (IEA15MW UMaine semisub), and the pre-set metocean combinations for DLC1.2, DLC1.6, DLC6.1:

- Tower geometry and material: ``scube\data\sample_INPUT_tower.xlsx``
- Tower check criteria: ``scube\data\sample_CNSTR.xlsx``

If you are running an analysis of level 3 (L3), then you may want to modify the metocean conditions for your specific site:

- Environmental conditions: ``scube\data\sample_INPUT_environment.xlsx``

Each of this spreadsheet file as a "legend" sheet, with explanations.

Medium level
------------
If you want to use the pre-set wind turbine and support structure (IEA15MW UMaine semisub), but change the metocean combinations for DLC1.2, DLC1.6, DLC6.1, first of all modify as desired the basic set of inputs:

- Tower geometry and material: ``scube\data\sample_INPUT_tower.xlsx``
- Environmental conditions: ``scube\data\sample_INPUT_environment.xlsx``
- Tower check criteria: ``scube\data\sample_CNSTR.xlsx``

Then, to modify the DLC metocean combinations (please refer to `WEIS manual <https://weis.readthedocs.io/en/latest/dlc_generator.html>`__ for detailed explanation on these parameters):

- For analysis Axx Ly, open ``scube\data\weis_analyses\Axx_Ly\modeling_options_Axx_Ly.yaml``

This is a ``.yaml`` file. You can change the DLC settings in the ``DLC_driver`` → ``DLCs`` section (example for DLC1.6):

.. code:: yaml

   DLC_driver:
       DLCs:
       - DLC: '1.6'
           label: '1.6'
           analysis_time: 3600   #3600
           transient_time: 600 #600
           turbulent_wind:
               AnalysisTime: 600
               UsableTime: ALL
           wave_heading: [-90]
           pitch_initial: [2.426047, 2.426047, 0.377375, 0.000535, 0.000535, 1.170321,
               6.052129, 9.189114, 11.824437, 14.19975, 16.42107, 18.525951, 20.553121,
               20.553121, 20.553121]
           rot_speed_initial: [5.000012, 5.000012, 5.000012, 5.000012, 6.390847, 7.559987,
               7.559987, 7.559987, 7.559987, 7.559987, 7.559987, 7.559987, 7.559987,
               7.559987, 7.559987]
           user_group:
             - wave_dir: [-90., 0., 90.] # x3 wave directions with respect to wave_heading, i.e. -90
             - nace_dir: [-90, 0, 90]
               prop_dir: [90., 0., -90.] # sign of propagation direction is opposite to nacelle heading in OpenFAST
           yaw_misalign: [0]

.. note::
   Note the slight difference in ``DLC_driver`` → ``DLCs`` → ``user_group``: the “-” in front means that this is an independent series of values, while the absence of the “-” in front means that these values are coupled with the previous series of values with the “-”.
   
   For example, in this case, 9 simulations will be carried out: 3 wave directions, due to “- wave_dir”, times 3 nacelle directions, due to “nace_dir”. The wind propagation direction, “pro_dir”, is coupled with the nacelle direction, i.e., for nace_dir = -90, the prop_dir is automatically set to 90, for nace_dir = 0, the prop_dir is automatically set to 0, and so on.

Advanced level
--------------

This level allows you to introduce new constraints.

Create a new constraint (if not already available)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Open ``scube/data/CNSTR.xlsx``
- Go to the "LIST" tab
- Add a line for the new constraint, with the following columns:
   - Constraint: give a name to the constraint (please double-check the nomenclature used)
      - Example: "Max_twr_1st_fa_freq"
   - Type
      - Min: if the value to be checked should be higher than the value specified
      - Eq: if the value to be checked should be equal
      - Max: if the value to be checked should be lower than the value specified
      - Check: for some constraints, it is not possible to specify a numerical value (e.g., local buckling check)
   - Which_out: this is to specify where the value to be compared against the constraint is
      - yaml, if it is in the WEIS yaml output file (see file output_file_yaml in :ref:`sec_config`)
      - xlsx, if it is in the WEIS xlsx output file (see file output_file_xlsx in :ref:`sec_config`)
      - derived, if the output is not calculated by WEIS, but needs to be calculated through postprocessing
   - WEIS_out_yaml_param, WEIS_out_xlsx, scube_fun
      - if Which_out is yaml, then the parameter name, in the WEIS  output yaml file, to be checked against the constraint should be specified here, using a *dot notation for nested keys*, also known as *hierarchical (dotted path) notation* (see Appendix)
      - if Which_out is xlsx, then the parameter name, in the WEIS output xlsx file, to be checked against the constraint should be specified here
      - if Wich_out is scube_fun, then the name of the scube_fun (scube postprocessing function implemented to derive the parameter value to be compared against the constraint) is specified here. <ADD HERE LINK TO SECTION SPECIFYING HOW TO CREATE NEW POSTPROCESSING FUNCTION>

Use the new constraint
^^^^^^^^^^^^^^^^^^^^^^

- Open ``scube/data/CNSTR.xlsx``
- Go to the constraints_Axy_L0z tab, where you would like to add the constraint
- Add the constraint
   - ID: add a number after the existing one
   - Constraint: click on the drop-down menu arrow, and choose the constraint you just created (Example: "Max_twr_1st_fa_freq")
   - Units: specify the unit of measure in which the constraint value (see next column) is specified
   - Value: give the numerical value of the constraints
   - Description (optional): provide a description of the constraint
