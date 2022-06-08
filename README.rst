DFTB+ school in Daresbury
=========================

This repository contains the material for the session on extended tight-binding for the `DFTB+ school in Daresbury <https://www.cecam.org/workshop-details/1163>`__, made available in the public domain (Unlicense).
It is highly appreciated if you credit this work if you reuse it for whatever purpose, but not required.


Building the pages
------------------

If you do not have a conda environment get a `mambaforge <https://github.com/conda-forge/miniforge/releases>`__ installer.
Create a new environment using

.. code::

   mamba create -n sphinx -f environment.yml
   mamba activate sphinx

Now you can compile the pages with

.. code::

   sphinx-build pages _doc

You can view the pages by starting a webserver with

.. code::

   python3 -m http.server -d _doc

Visit the shown URL to inspect the pages.
