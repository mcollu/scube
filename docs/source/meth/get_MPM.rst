get_MPM
=======

- Converts the input ``maxima`` into a NumPy array using ``np.asarray`` for robust numerical operations. [web:16]

- Defines the number of cycles ``n`` as ``3600. / average_period``, referencing DNV-OS-E301 guidance for normalization over 1 hour. [web:19]

- Fits a Weibull distribution to ``maxima``, extracting the ``shape``, ``loc``, and ``scale`` parameters. [web:5][web:11]

- Computes intermediary variables:
  
  - ``alpha = scale``
  - ``beta = shape``
  - ``gamma = loc``

- Uses the fitted Weibull parameters to derive Gumbel distribution parameters via the following transformation: [web:16][web:4]

  - \[
    \alpha_g = \frac{\beta}{\alpha} \cdot [\ln(n)]^{\frac{\beta - 1}{\beta}}
    \]
  - \[
    \beta_g = \gamma + \alpha \cdot [\ln(n)]^{1/\beta}
    \]

  - The resulting Gumbel parameters are:
    - ``scale_gumbel = 1 / alpha_g``
    - ``loc_gumbel = beta_g``

- Defines the Most Probable Maximum (``MPM``) as the location parameter of the derived Gumbel distribution: ``MPM = loc_gumbel``. [web:6][web:16]

- Returns a tuple ``(MPM, shape, loc, scale, scale_gumbel, loc_gumbel)`` containing the derived values.

- Handles any errors by returning a tuple of ``np.nan`` values.

Schematic docstring for Weibull-to-Gumbel parameter mapping (as LaTeX math):

.. math::

   n = \frac{3600}{T_\mathrm{avg}} \\

   \text{Fit:} ~ Weibull(x; \beta, \gamma, \alpha) \\

   \alpha_g = \frac{\beta}{\alpha} \cdot [\ln(n)]^{(\beta-1)/\beta} \\

   \beta_g  = \gamma + \alpha \cdot [\ln(n)]^{1/\beta} \\

   \text{Gumbel~parameters:} ~ scale = 1/\alpha_g, ~ location = \beta_g \\

   MPM = \beta_g

References:  
- DNV-OS-E301 Guidance Note (Dec 2024), Ch.2 Sec.2 [web:19][web:13]
- Weibull and Gumbel extreme value theory [web:4][web:16][web:6]

.. seealso:: Function ``get_MPM`` in the file ``scube\src\scube\postpro\postprocessing.py``
