.. code:: bash

  Using weis.aeroelasticse in rosco.toolbox...
  
   ******* SCUBE: preprocessing - updating tower geometry *******
  
   ******* SCUBE: processing - WEIS analysis *******
  Found existing potential model: ../../01_aeroelasticse/OpenFAST_models/IEA-15-240-RWT/IEA-15-240-RWT-UMaineSemi/HydroData/IEA-15-240-RWT-UMaineSemi
      - Trying to use this instead of running PyHAMS.
  
  ================
  wisdem.wt.wt_rna
  ================
  NL: NLBGS 1 ; 5.75918932e+11 1
  NL: NLBGS 2 ; 12997931 2.2569029e-05
  NL: NLBGS 3 ; 302117.706 5.24583738e-07
  NL: NLBGS 4 ; 7598.06643 1.31929444e-08
  NL: NLBGS 5 ; 190.765705 3.3123708e-10
  NL: NLBGS Converged
  RuntimeWarning: C:\Users\mauri\miniforge3\envs\weis-env2\Lib\site-packages\wisdem\commonse\utilization_dnvgl.py:322
  The number of calls to function has reached maxfev = 50.RuntimeWarning: C:\Users\mauri\miniforge3\envs\weis-env2\Lib\site-packages\wisdem\commonse\cylinder_member.py:513
  divide by zero encountered in scalar divideRuntimeWarning: C:\Users\mauri\miniforge3\envs\weis-env2\Lib\site-packages\wisdem\commonse\cylinder_member.py:514
  divide by zero encountered in scalar divideCp-Ct-Cq surfaces completed at 5 %
  Cp-Ct-Cq surfaces completed at 10 %
  Cp-Ct-Cq surfaces completed at 15 %
  Cp-Ct-Cq surfaces completed at 20 %
  Cp-Ct-Cq surfaces completed at 25 %
  Cp-Ct-Cq surfaces completed at 30 %
  Cp-Ct-Cq surfaces completed at 35 %
  Cp-Ct-Cq surfaces completed at 40 %
  Cp-Ct-Cq surfaces completed at 45 %
  Cp-Ct-Cq surfaces completed at 50 %
  Cp-Ct-Cq surfaces completed at 55 %
  Cp-Ct-Cq surfaces completed at 60 %
  Cp-Ct-Cq surfaces completed at 65 %
  Cp-Ct-Cq surfaces completed at 70 %
  Cp-Ct-Cq surfaces completed at 75 %
  Cp-Ct-Cq surfaces completed at 80 %
  Cp-Ct-Cq surfaces completed at 85 %
  Cp-Ct-Cq surfaces completed at 90 %
  Cp-Ct-Cq surfaces completed at 95 %
  Cp-Ct-Cq surfaces completed at 100 %
  -----------------------------------------------------------------------------
     Tuning a reference wind turbine controller using NREL's ROSCO toolbox
  -----------------------------------------------------------------------------
  WARNING: You are adding in additional drag terms that may double count strip theory estimated viscous drag terms.  Please zero out the BQuad entries or use modeling options SimplCd/a/p and/or potential_model_override and/or potential_bem_members to suppress strip theory for the members
  Unknown keyword argument, ult_stress
  Unknown keyword argument, ult_stress
  Unknown keyword argument, ult_stress
  Unknown keyword argument, ult_stress
  Unknown keyword argument, ult_stress
  Unknown keyword argument, ult_stress
  
   **************************************************************************************************
   OpenFAST
  
   Copyright (C) 2025 National Renewable Energy Laboratory
   Copyright (C) 2025 Envision Energy USA LTD
  
   This program is licensed under Apache License Version 2.0 and comes with ABSOLUTELY NO WARRANTY.
   See the "LICENSE" file distributed with this software for details.
   **************************************************************************************************
  
   OpenFAST-v4.0.4
   Compile Info:
    - Compiler: GCC version 14.2.0
    - Architecture: 64 bit
    - Precision: single
    - OpenMP: No
    - Date: Apr 29 2025
    - Time: 14:28:26
   Execution Info:
    - Date: 08/31/2025
    - Time: 17:19:05+0100
  
   OpenFAST input file heading:
       Generated with OpenFAST_IO
  
    Running ElastoDyn.
   Nodal outputs section of ElastoDyn input file not found or improperly formatted.
    Running SeaState.
     Setting WaveTMax to TMax since WaveMod = 0
    Running HydroDyn.
    Reading in WAMIT output with root name "C:\Users\mauri\OneDrive - University of
    Strathclyde\NUM_FMS21289\scube\data\01_aeroelasticse\OpenFAST_models\IEA-15-240-RWT\IEA-15-240-RW
    T-UMaineSemi\HydroData\IEA-15-240-RWT-UMaineSemi".
    Computing radiation impulse response functions and wave diffraction forces.
    Running MoorDyn (v2.3.8, 2025-02-27).
      This is MoorDyn v2, with significant input file changes from v1.
  
   **************************************************************************************************
   MoorDyn
  
   Copyright (C) 2025 National Renewable Energy Laboratory
   Copyright (C) 2025 Envision Energy USA LTD
  
   This program is licensed under Apache License Version 2.0 and comes with ABSOLUTELY NO WARRANTY.
   See the "LICENSE" file distributed with this software for details.
   **************************************************************************************************
  
      Parsing MoorDyn input file: .\DLCfreedecay_0_weis_job_0_MoorDyn.dat
      Created mooring system:  3 lines, 6 points, 0 rods, 0 bodies.
      Finalizing initial conditions using dynamic relaxation.
  
     t=60  FairTen 1: 9.81109E+06, 9.79521E+06, 9.77602E+06
      Fairlead tensions did not converge within TMaxIC=60 seconds.
      MoorDyn initialization completed.
  
   FAST_InitializeAll:ED_Init:ED_ReadInput:ReadBladeInputs:ReadBladeFile:The ElastoDyn Blade file,
   .\DLCfreedecay_0_weis_job_0_ElastoDynBlade.dat, DISTRIBUTED BLADE PROPERTIES table contains the
   PitchAxis column.  This column is unused and will be removed in future releases
   FAST_InitializeAll:SeaSt_Init:SeaStateInput_ProcessInitData:WvHiCOff adjusted to 0.62832 rad/s,
   based on WaveDT.
  
    Time: 0 of 5 seconds.
  
   FAST_Solution:FAST_UpdateStates:FAST_AdvanceStates:ED_ABM4:ED_CalcContStateDeriv:SetCoordSy:Small
   angle assumption violated in SUBROUTINE SmllRotTrans() due to a large blade deflection (ElastoDyn
   SetCoordSy). The solution may be inaccurate. Simulation continuing, but future warnings from
   SmllRotTrans() will be suppressed.
    Additional debugging message from SUBROUTINE SmllRotTrans(): 1.46 s
   ED_CalcContStateDeriv:SetCoordSy:Small angle assumption violated in SUBROUTINE SmllRotTrans() due
   to a large blade deflection (ElastoDyn SetCoordSy). The solution may be inaccurate. Simulation
   continuing, but future warnings from SmllRotTrans() will be suppressed.
    Additional debugging message from SUBROUTINE SmllRotTrans(): 1.46 s
  
  
    Total Real Time:       4.527 seconds
    Total CPU Time:        4.4375 seconds
    Simulation CPU Time:   2.1875 seconds
    Simulated Time:        5 seconds
    Time Ratio (Sim/CPU):  2.2857
  
    OpenFAST terminated normally.
  
  Runtime:        DLCfreedecay_0_weis_job_0.fst = 4.56  s
  
   **************************************************************************************************
   OpenFAST
  
   Copyright (C) 2025 National Renewable Energy Laboratory
   Copyright (C) 2025 Envision Energy USA LTD
  
   This program is licensed under Apache License Version 2.0 and comes with ABSOLUTELY NO WARRANTY.
   See the "LICENSE" file distributed with this software for details.
   **************************************************************************************************
  
   OpenFAST-v4.0.4
   Compile Info:
    - Compiler: GCC version 14.2.0
    - Architecture: 64 bit
    - Precision: single
    - OpenMP: No
    - Date: Apr 29 2025
    - Time: 14:28:26
   Execution Info:
    - Date: 08/31/2025
    - Time: 17:19:10+0100
  
   OpenFAST input file heading:
       Generated with OpenFAST_IO
  
    Running ElastoDyn.
   Nodal outputs section of ElastoDyn input file not found or improperly formatted.
    Running SeaState.
     Setting WaveTMax to TMax since WaveMod = 0
    Running HydroDyn.
    Reading in WAMIT output with root name "C:\Users\mauri\OneDrive - University of
    Strathclyde\NUM_FMS21289\scube\data\01_aeroelasticse\OpenFAST_models\IEA-15-240-RWT\IEA-15-240-RW
    T-UMaineSemi\HydroData\IEA-15-240-RWT-UMaineSemi".
    Computing radiation impulse response functions and wave diffraction forces.
    Running MoorDyn (v2.3.8, 2025-02-27).
      This is MoorDyn v2, with significant input file changes from v1.
  
   **************************************************************************************************
   MoorDyn
  
   Copyright (C) 2025 National Renewable Energy Laboratory
   Copyright (C) 2025 Envision Energy USA LTD
  
   This program is licensed under Apache License Version 2.0 and comes with ABSOLUTELY NO WARRANTY.
   See the "LICENSE" file distributed with this software for details.
   **************************************************************************************************
  
      Parsing MoorDyn input file: .\DLCfreedecay_1_weis_job_0_MoorDyn.dat
      Created mooring system:  3 lines, 6 points, 0 rods, 0 bodies.
      Finalizing initial conditions using dynamic relaxation.
  
     t=60  FairTen 1: 9.81109E+06, 9.79521E+06, 9.77602E+06
      Fairlead tensions did not converge within TMaxIC=60 seconds.
      MoorDyn initialization completed.
  
   FAST_InitializeAll:ED_Init:ED_ReadInput:ReadBladeInputs:ReadBladeFile:The ElastoDyn Blade file,
   .\DLCfreedecay_1_weis_job_0_ElastoDynBlade.dat, DISTRIBUTED BLADE PROPERTIES table contains the
   PitchAxis column.  This column is unused and will be removed in future releases
   FAST_InitializeAll:SeaSt_Init:SeaStateInput_ProcessInitData:WvHiCOff adjusted to 0.62832 rad/s,
   based on WaveDT.
  
    Time: 0 of 5 seconds.
  
   FAST_Solution:FAST_UpdateStates:FAST_AdvanceStates:ED_ABM4:ED_CalcContStateDeriv:SetCoordSy:Small
   angle assumption violated in SUBROUTINE SmllRotTrans() due to a large blade deflection (ElastoDyn
   SetCoordSy). The solution may be inaccurate. Simulation continuing, but future warnings from
   SmllRotTrans() will be suppressed.
    Additional debugging message from SUBROUTINE SmllRotTrans(): 1.47 s
   ED_CalcContStateDeriv:SetCoordSy:Small angle assumption violated in SUBROUTINE SmllRotTrans() due
   to a large blade deflection (ElastoDyn SetCoordSy). The solution may be inaccurate. Simulation
   continuing, but future warnings from SmllRotTrans() will be suppressed.
    Additional debugging message from SUBROUTINE SmllRotTrans(): 1.47 s
  
  
    Total Real Time:       4.546 seconds
    Total CPU Time:        4.4375 seconds
    Simulation CPU Time:   2.1875 seconds
    Simulated Time:        5 seconds
    Time Ratio (Sim/CPU):  2.2857
  
    OpenFAST terminated normally.
  
  Runtime:        DLCfreedecay_1_weis_job_0.fst = 4.57  s
  WARNING: DLC 1.1 is being used for AEP calculations.  Use the AEP DLC for more accurate wind modeling with constant TI.
  RuntimeWarning: C:\Users\mauri\miniforge3\envs\weis-env2\Lib\site-packages\pCrunch\crunch.py:409
  invalid value encountered in divideRuntimeWarning: C:\Users\mauri\miniforge3\envs\weis-env2\Lib\site-packages\wisdem\commonse\utilization_dnvgl.py:322
  The number of calls to function has reached maxfev = 50.########################################
  Objectives
  Turbine AEP: 0.0000000000 GWh
  Blade Mass:  68058.3521941191 kg
  LCOE:        96.7346869586 USD/MWh
  Tip Defl.:   0.0000000000 m
  IPC Ki1p = 0.000e+00
  IPC Ki1p = 0.000e+00
  ########################################
  ----------------
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
  
  Run time (A03_L2): 74.32020354270935
  
   ******* SCUBE: postprocessing - results VS constraints analysis *******
  UserWarning: C:\Users\mauri\miniforge3\envs\weis-env2\Lib\site-packages\openpyxl\worksheet\_read_only.py:85
  Data Validation extension is not supported and will be removed
           ******* Constraint definitions imported *******
  
           ******* Simulation output xlsx and yaml files data loaded *******
  
           ******* Constraint verification started *******
  
                   Check of constraint Min_twr_1st_fa_freq_L2
  
                   Check of constraint Min_twr_1st_ss_freq_L2
  
                   Check of constraint Max_twr_1st_fa_freq_L2
  
                   Check of constraint Max_twr_1st_ss_freq_L2
  
           ******* Constraint verification completed *******
                 Constraint Constraint Type Constraint um  Constraint Value  Simulated Value Status                                      Description
  0  Min_twr_1st_fa_freq_L2             Min            Hz             0.375            0.399   Pass   Min tower natural frequency, 1st fore-aft mode
  1  Min_twr_1st_ss_freq_L2             Min            Hz             0.375            0.399   Pass  Min tower natural frequency, 1st side-side mode
  2  Max_twr_1st_fa_freq_L2             Max            Hz             0.500            0.399   Pass      Max tower natural frequency, fore-aft modes
  3  Max_twr_1st_ss_freq_L2             Max            Hz             0.500            0.399   Pass     Max tower natural frequency, side-side modes
  
   ******* SCUBE: Validation report with formatting exported successfully *******
  
  [INFO] Time taken: 0:01:17
