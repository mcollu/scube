.. function:: get_tilt_mean_L3

   Derive the average platform tilt angle for each OpenFAST simulation.

   This function:
   
   - reads simulation output (usually ``.outb`` files),
   - extract the platform roll and platform pitch time signals,
   - derive from roll and pitch the total platform tilt signal,
   - calculate the average of the platform tilt signal

   .. seealso:: Function ``get_tilt_mean_L3`` in the file ``scube\src\scube\postpro\postprocessing.py``


