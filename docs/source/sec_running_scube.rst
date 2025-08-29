Running SCUBE
=============
How do you run an analysis with SCUBE?
--------------------------------------

- Open a miniforge/miniconda/conda terminal prompt
- Activate the WEIS environment you set up
.. code:: bash
  conda activate weis-env
- Navigate to the root folder `scube'
- Launch the analysis Axy Lz with the following command
.. code:: bash
  `python main.py A01 L1'

.. _sec_config:
Configuration file
------------------

- Edit ``config.yaml`` with your desired parameters before running the
  analysis.

.. code:: yaml

   ### INPUT
   reference_turbine_yaml: "data/ref_turbines/IEA-15-240-RWT_VolturnUS-S.yaml"     # Reference floating wind turbine to be modified (all systems, defined as per WEIS convention)
   environment_input:      "data/INPUT_environment.xlsx"                           # Input spreadsheet - location environmental conditions (wind, wave)
   tower_geometry_input:   "data/INPUT_tower.xlsx"                                 # Input spreadsheet - tower characteristics (geometry, material)
   
   ### ANALYSIS
   constraints_xlsx:       "CNSTR.xlsx"                        # Input spreadsheet - constraints to be inposed. NB All the constraints for all the analyses are in here. One tab for each analysis
   constraints_sheet_fmt:  "constraints_{ANALYSIS}"            # Input spreadsheet - constraints for the specific analysis, naming convention
   
       # WEIS folder names
   output_dir_base:        "data/weis_analyses"                # WEIS - WEIS output folder root
   outputs_subdir:         "outputs"                           # WEIS - WEIS output folder
   modeling_options_fmt:   "modeling_options_{ANALYSIS}.yaml"  # WEIS - WEIS input - modeling options yaml file
   analysis_options_fmt:   "analysis_options_{ANALYSIS}.yaml"  # WEIS - WEIS input - analysis options yaml file
   
   output_file_xlsx:       "{ANALYSIS}_output.xlsx"            # WEIS - WEIS output - xlsx output filename
   output_file_yaml:       "{ANALYSIS}_output.yaml"            # WEIS - WEIS output - yaml output filename
   
   
   ### OUTPUT
   modified_turbine_yaml:  "output/modified_IEA-15-240-RWT.yaml"   # WEIS - WEIS output - File of the modified floating wind turbine system (all the subsystems)
   validation_report_folder: "output"                              # Scube - Output folder where the validation reports are saved
