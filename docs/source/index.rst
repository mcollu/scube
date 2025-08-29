Scube manual
===================================
.. note::

   This project is under active development.

.. toctree::
   :maxdepth: 2
   :caption: Contents:
   
   sec_how_scube_works
   sec_installation
   sec_running_scube
   sec_scube_inputs
   sec_scube_analyses
   sec_scube_outputs
   sec_scube_examples
   sec_FAQ
   sec_contact_and_support

Outputs
=======

- **Summary report**

  - Validation reports are available in the folder ``scube/tests``
  - The naming convention is: ``validation_report_Axx_Ly.xlsx``.
  - For example, if you have run the analysis A01 L1, then the output
    spreadsheet name is: ``validation_report_A01_L0.xlsx``

Contact & Support
=================

- **Lead developer:** Prof. M. Collu (maurizio.collu@strath.ac.uk)
- **Issues/Bugs:** Please file issues through this `link <https://github.com/mcollu/scube/issues>`_
- **License:** See TIC LCPE agreement terms

Appendix
========
D
-

dot notation for nested keys
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+---------------------+----------------------+
| Dot Notation        | Equivalent YAML      |
+=====================+======================+
| foo.bar.baz         | foo:                 |
|                     |   bar:               |
|                     |     baz: value       |
+---------------------+----------------------+

H
-

hierarchical (dotted path) notation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
See "dot notation for nested keys"
