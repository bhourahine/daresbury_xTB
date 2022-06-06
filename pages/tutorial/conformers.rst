Conformational ensembles
========================

For many larger molecules a single structure approach for computing properties is insufficient since several relevant structures are populated at room temperature.
In this tutorial we will use the conformer–rotamer ensemble search tool (CREST) for generating a conformational ensemble.\ :footcite:`pracht2020`
An interface from CREST to DFTB+ is currently under development, therefore we will use the ``xtb`` program package\ :footcite:`bannwarth2020` to provide access to the extended tight binding (xTB) Hamiltonian.


Setting up CREST
----------------

First, ensure that the ``crest`` and the ``xtb`` binary are found in your ``PATH``.
You can install the programs with the *mamba* package manager using

.. code-block:: text

   mamba create -n conf crest xtb 'libblas=*=*mkl*'
   mamba activate conf

For optimal performance pin the BLAS library to the Intel MKL if available on your platform.


Environment effects on side-chains
----------------------------------

For the compound 31357 we download the structure from the PubChem database.
The three butyl chains provide conformational flexibility and the just retrieved structure might not describe the energetically most favorable conformation.

.. tab-set::

   .. tab-item:: coordinates
      :sync: xyz

      .. literalinclude:: conformers/tbnp/pubchem.xyz
         :caption: pubchem.xyz
         :language: none

   .. tab-item:: image
      :sync: png

      .. image:: conformers/tbnp/pubchem.png
         :scale: 50%

To start a calculation with CREST we create a new directory and add the initial structure.

.. code-block:: text

   ❯ tree .
   .
   └── pubchem.xyz

For the conformer search itself we will use the program defaults which will use the GFN2-xTB method for the sampling

.. code-block:: text

   ❯ crest pubchem.xyz -T $(nproc) | tee crest.out

.. note::

   The runtime for sampling the conformer ensemble depends mainly on the flexibility of the system.
   The conformer search can be parallelized on a shared memory system using the ``-T`` option to speed-up the calculation.

After the calculation has finished we check the files produced by CREST

.. code-block:: text

   ❯ tree .
   .
   ├── ...
   ├── crest.out
   ├── crest_best.xyz
   ├── crest_conformers.xyz
   ├── ...
   └── pubchem.xyz

``crest_best.xyz``:
   Contains the lowest conformer of the ensemble

``crest_conformers.xyz``:
   Contains the whole conformer ensemble ordered energetically.
   A viewer, like `molden <https://www.theochem.ru.nl/molden/>`__, can be used to inspect the ensemble and the relative conformational energies (in kcal/mol).

.. admonition:: Exercise
   :class: info

   Compare the found ensemble with the energetically most favorable conformer found by DFT.

   .. tab-set::

      .. tab-item:: coordinates
         :sync: xyz

         .. literalinclude:: conformers/tbnp/crenso-woctanol.xyz
            :caption: dft-lowest.xyz
            :language: none

      .. tab-item:: image
         :sync: png

         .. image:: conformers/tbnp/crenso-woctanol.png
            :scale: 50%

   The DFT ensemble was generated in an implicit solvent, namely octanol.
   Check the influence of including implicit solvation in the generation of the ensemble.
   The help page of CREST can give you insights which options are required for activating the implicit solvation model.


Summary
-------

.. admonition:: You learned...
   :class: important

   - to find the energetically most favorable conformer of a compound
   - sample a conformational ensemble and compare it with a reference
   - include environment effects like solvation in your conformer search


Literature
----------

.. footbibliography::
