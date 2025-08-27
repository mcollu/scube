Scube manual
===================================
.. note::

   This project is under active development.

.. contents::

Overview
========

**Project Name:** SCUBE **Version:** 1.0.0

SCUBE is a Python package for simulating and analysing wind turbine
structural responses in offshore conditions. Developed at the University
of Strathclyde for Scottish Power and SSE Renewables, in the TIC-LCPE
project “Wind-19 Stiff-stiff tower for FOWT: development of a design and
benchmarking tool”, between January and October 2025.

**Key Features:**

- It leverages the capabilities of the software WEIS by NREL
  `Link <https://weis.readthedocs.io/en/latest/>`__
- Can be used to benchmark a given offshore wind turbine tower design,
  or to support its design
- Simple input spreadsheets for: tower characteristics, environmental
  characteristics, analysis and/or design criteria to be checked
- Simple output spreadsheets 

Installation
============

Pre-requisites
--------------

Requires the installation of WEIS by NREL. Info on how to install it `here <https://weis.readthedocs.io/en/latest/installation.html>`__\ *.*

It is recommended to use miniforge to create conda environments and install WEIS. Miniforge can be obtained here <https://github.com/conda-forge/miniforge>`__ *In the following,
it is assumed that you named ``weis-env`` the WEIS environment where to
run SCUBE.*

Installation instructions
-------------------------

1. Make sure to have the zip file “scube.zip” provided
2. Expand the file in the desired place in your disk
3. Open a conda command prompt (*if you are using miniforge, use the
   Miniforge Prompt*)
4. Make sure to activate the conda environment you have created to run
   WEIS and SCUBE (``e.g., weis-env``)
5. Navigate, within the command prompt, to the folder ``scube\dist``
6. Install the latest version of the SCUBE .whl file that you can find
   in that folder. For example, for SCUBE v1.0.0

.. code:: bash

   pip install scube-1.0.0-py3-none-any.whl

This should install all the SCUBE scripts and all the required packages.

*N.B. If you have installed a previous version of SCUBE, make sure to
uninstall that first, and then install the new version of SCUBE:*

.. code:: bash

   pip uninstall scube

Getting started
===============

After installation, you can run a quick check to verify the correct installation.

1. Open a conda prompt (miniforge prompt, for example), navigate to
   the folder ``scube/tests/output``
2. Run the following command 

.. code:: bash

   python test_ALL.py

This runs a demonstration of a series of typical wind turbine analyses and writes results into the ``scube/tests`` folder.
In the miniforge prompt command window, the following should appear (multiple times, once for each analysis).
N.B. Some WEIS "Warnings" may appear. There are not critical.

.. code:: bash

   Running: test.py A03 L2
   Output:
   Using weis.aeroelasticse in rosco.toolbox...
   
    ******* SCUBE: preprocessing - updating tower geometry *******
   
    ******* SCUBE: postprocessing - results VS constraints analysis *******
   
            ******* Constraint definitions imported *******
   
            ******* Simulation output xlsx and yaml files data loaded *******
   
            ******* Constraint verification started *******
   
                    Check of constraint Min_twr_1st_fa_freq_L2
   
                    Check of constraint Min_twr_1st_ss_freq_L2
   
            ******* Constraint verification completed *******
                  Constraint  ...                                      Description
   0  Min_twr_1st_fa_freq_L2  ...   Min tower natural frequency, 1st fore-aft mode
   1  Min_twr_1st_ss_freq_L2  ...  Min tower natural frequency, 1st side-side mode
   
   [2 rows x 7 columns]
   
   ******* SCUBE: Validation report with formatting exported successfully *******
   
   [INFO] Time taken: 0:00:03

3. Check in the folder scube\tests\output the validation reports created (e.g., validation_report_A03_L2.xlsx)

Main analysis file
==================

*Main analysis file:* ``scube\main.py``

- Edit ``config.yaml`` with your desired parameters before running the
  analysis.

.. code:: yaml
   
   ### INPUT
   reference_turbine_yaml: "data/ref_turbines/IEA-15-240-RWT_VolturnUS-S.yaml" # Reference floating wind turbine to be modified
   environment_input:      "data/INPUT_environment.xlsx"
   tower_geometry_input:   "data/INPUT_tower.xlsx"
   
   ### ANALYSIS
   constraints_xlsx:       "CNSTR.xlsx"
   
       # WEIS folder names
   outputs_subdir:         "outputs"
   output_dir_base:        "data/weis_analyses"
   modeling_options_fmt:   "modeling_options_{ANALYSIS}.yaml"
   analysis_options_fmt:   "analysis_options_{ANALYSIS}.yaml"
   
   output_file_xlsx:       "{ANALYSIS}_output.xlsx"
   output_file_yaml:       "{ANALYSIS}_output.yaml"
   constraints_sheet_fmt:  "constraints_{ANALYSIS}"
   
   ### OUTPUT
   modified_turbine_yaml:  "output/modified_IEA-15-240-RWT.yaml" # File of the modified floating wind turbine
   validation_report_folder: "output"

Input Data Format
=================

Basic level
-----------

If you want to use the pre-set wind turbine and support structure (IEA15MW UMaine semisub), and the pre-set metocean combinations for DLC1.2, DLC1.6, DLC6.1:

- Tower geometry and material: ``scube\data\sample_INPUT_tower.xlsx``
- Environmental conditions: ``scube\data\sample_INPUT_environment.xlsx``
- Tower check criteria: ``scube\data\sample_CNSTR.xlsx``

Medium level
------------
If you want to use the pre-set wind turbine and support structure (IEA15MW UMaine semisub), but change the metocean combinations for DLC1.2, DLC1.6, DLC6.1:

- Tower geometry and material: ``scube\data\sample_INPUT_tower.xlsx``
- Environmental conditions: ``scube\data\sample_INPUT_environment.xlsx``
- Tower check criteria: ``scube\data\sample_CNSTR.xlsx``
- DLC metocean combinations (please refer to `WEIS manual <https://weis.readthedocs.io/en/latest/dlc_generator.html>`__ for detailed explanation on these parameters):

  - For analysis Axx Ly, open the file\ ``scube\data\weis_analyses\Axx_Ly\modeling_options_A10_L3.yaml``
  - This is a ``.yaml`` file. You can change the DLC settings in the section (example for DLC1.6):

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

*NB Note the slight difference in user_group: the “-” in front means that this is an indipendent series of values, while the absence of the “-” in front means that these values are coupled with the previous series of values with the “-”.* *For example, in this case, 9 simulations will be carried out: 3 wave directions, due to “- wave_dir”, times 3 nacelle directions, due to “nace_dir”. The wind propagation direction, “pro_dir”, is coupled with the nacelle direction, i.e., for nace_dir = -90, the prop_dir is automatically 90, for nace_dir = 0, the prop_dir is automatically 0, and so on.*

Running and Analysis
====================

1. Open a conda prompt
2. Activate the WEIS conda environment, for example:
3. Navigate to the ``scube\test`` folder
4. Launch the analysis with the following command:

.. code:: bash

   python test_serial.py A01 L1

Outputs
=======

- **Summary report**

  - Validation reports are available in the folder ``scube/tests``
  - The naming convention is: ``validation_report_Axx_Ly.xlsx``.
  - For example, if you have run the analysis A01 L1, then the output
    spreadsheet name is: ``validation_report_A01_L0.xlsx`` ## FAQ

**Q: Can I ….?** 
A: Yes, … 

Contact & Support
=================

- **Lead developer:** Prof. M. Collu (maurizio.collu@strath.ac.uk)
- **Issues/Bugs:** Please file issues via email (support available until
  October 2025)
- **License:** See TIC LCPE agreement terms
