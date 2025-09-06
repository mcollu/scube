.. function:: postprocess_openfast_kpis(output_file, kpi_list=None, channels=None)

   Process OpenFAST output file and extract wind turbine performance KPIs.

   This function reads simulation output (usually `.outb` or `.out` files), parses relevant channels,
   and computes specified KPIs (Key Performance Indicators) such as average power, turbine loads,
   and structural response metrics.

   :param output_file: Path to the OpenFAST output file to be processed.
   :type output_file: str

   :param kpi_list: List of KPIs to calculate (e.g., ['mean_power', 'max_load']), defaults to all.
   :type kpi_list: list[str], optional

   :param channels: Specific output channels required for KPI calculation, if restricted; otherwise, all.
   :type channels: list[str], optional

   :returns: Dictionary containing KPI names and their calculated values.
   :rtype: dict[str, float]

   :Example:

       >>> kpis = postprocess_openfast_kpis('TestSim.outb', kpi_list=['mean_power', 'max_load'])
       >>> print(kpis)
       {'mean_power': 4020.5, 'max_load': 10542.7}

   .. note::
      Channels must match those present in the output file. For more details, refer to the OpenFAST output documentation.

   .. seealso:: For advanced analysis, see related tools in the OpenFAST Python toolbox.


