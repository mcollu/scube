get_tilt_MPM_L3
================

Derive the Most Probable Maximum platform tilt angle for each OpenFAST simulation.

This function:

- reads simulation output (usually ``.outb`` files),
- extract the platform roll (``PtfmRoll``) and platform pitch (``PtfmPitch``) time signals,
- derive from roll and pitch the total platform tilt signal, as :math:`tilt = \sqrt{PtfmRoll^2 + PtfmPitch^2}`
- calculate the Most Probable Maximum of the platform tilt signal, with the methodology described here

.. seealso:: Function ``get_tilt_MPM_L3`` in the file ``scube\src\scube\postpro\postprocessing.py``



