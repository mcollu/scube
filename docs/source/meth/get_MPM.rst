get_tilt_MPM_L3
================

Derive the Most Probable Maximum, given .

This function:

- reads simulation output (usually ``.outb`` files),
- extract the platform roll (``PtfmRoll``) and platform pitch (``PtfmPitch``) time signals,
- derive from roll and pitch the total platform tilt signal, as :math:`tilt = \sqrt{PtfmRoll^2 + PtfmPitch^2}`
- calculate the Most Probable Maximum of the platform tilt signal, with the methodology described :doc:`here <meth/get_MPM>` 

.. seealso:: Function ``get_MPM`` in the file ``scube\src\scube\postpro\postprocessing.py``
