get_MPM
=======

This function takes as input the identified maxima, and output the Most Probable Maximum.

The algorithm is as follow, according to DNV-OS-E301 Guidance Note (Dec 2024), Ch.2 Sec.2:

- Fit a Weibull distribution to your dataset of maxima:
  
  - Weibull cumulative distribution function:
    
    .. math::
      
      F(x; k, \lambda) = 1 - e^{-(x / \lambda)^k}
  
  - Fit data to obtain:
    - ``shape`` parameter: \( k \)
    - ``scale`` parameter: \( \lambda \)
    - ``location`` parameter (if using 3-parameter): \( \theta \) [optional]
    
- For maxima analysis according to extreme value theory, the logarithm of a Weibull random variable is Gumbel distributed:
  
  - If \( X \sim \mathrm{Weibull}(\lambda, k) \), then \( G = \log(X) \) is Gumbel (minimum) distributed with:
    - Location parameter: \( \mu = \log(\lambda) \)
    - Scale parameter: \( \beta = 1/k \)
    - So \( G \sim \mathrm{Gumbel}_{\min}(\log(\lambda), 1/k) \) [web:24][web:27]
    
- For the transformation used in engineering standards (maxima conversion):
  
  - Number of expected independent maxima:

    .. math::
    
      n = \frac{\text{total_duration}}{\text{average_period}}
  
  - Use 3600 seconds as the reference duration (e.g., for 1 hour maxima count):

    .. math::
      n = \frac{3600}{T_\text{avg}}
  
  - The Most Probable Maximum (MPM) for the Gumbel distribution:
    .. math::
      \text{MPM} = \mu + \beta \cdot \ln(n)
      
    where:
      - \( \mu \) is the location parameter (from Weibull fit)
      - \( \beta \) is the scale parameter (from Weibull fit or conversion)
      - \( n \) is the expected number of maxima
      
  - If Weibull fit gives shape \( k \), scale \( \lambda \):
    .. math::
      \mu = \log(\lambda)
      \quad
      \beta = 1/k
      \quad
      \text{MPM} = \log(\lambda) + \frac{1}{k} \cdot \ln(n)

References:  
- DNV-OS-E301 Guidance Note (Dec 2024), Ch.2 Sec.2

.. seealso:: Function ``get_MPM`` in the file ``scube\src\scube\postpro\postprocessing.py``
