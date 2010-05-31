---
title: GSOC2010 Joao
permalink: wiki/GSOC2010_Joao
layout: wiki
---

Author & Mentors
----------------

[João Rodrigues](User%3AJoaor "wikilink") anaryin@gmail.com

**Mentors**

  
Eric Talevich

Diana Jaunzeikare

Peter Cock

Abstract
--------

Biopython is a very popular library in Bioinformatics and Computational
Biology. Its Bio.PDB module, originally developed by Thomas Hamelryck,
is a simple yet powerful tool for structural biologists. Although it
provides a reliable PDB parser feature and it allows several
calculations (Neighbour Search, RMS) to be made on macromolecules, it
still lacks a number of features that are part of a researcher's daily
routine. Probing for disulphide bridges in a structure and adding polar
hydrogen atoms accordingly are two examples that can be incorporated in
Bio.PDB, given the module's clever structure and good overall
organisation. Cosmetic operations such as chain removal and residue
renaming – to account for the different existing nomenclatures – and
renumbering would also be greatly appreciated by the community.

Another aspect that can be improved for Bio.PDB is a smooth
integration/interaction layer for heavy-weights in macromolecule
simulation such as MODELLER, GROMACS, AutoDock, HADDOCK. It could be
argued that the easiest solution would be to code hooks to these
packages' functions and routines. However, projects such as the recently
developed
[edPDB](http://sbcb.bioch.ox.ac.uk/oliver/software/GromacsWrapper/html/edpdb.html)
or the more complete [Biskit library](http://biskit.pasteur.fr/) render,
in my opinion, such interfacing efforts redundant. Instead, I believe it
to be more advantageous to include these software' input/output formats
in Biopython's SeqIO and AlignIO modules. This, together with the
creation of interfaces for model validation/structure checking
services/software would allow Biopython to be used as a pre- and
post-simulation tool. Eventually, it would pave the way for its
inclusion in pipelines and workflows for structure modelling, molecular
dynamics, and docking simulations.

Project Schedule
----------------

The schedule below was organised to be flexible, which means that some
features will likely be done early. Also, the weeks include
documentation and unit testing efforts for the features, with extended
periods for reviewing these efforts at the two points during the project
(halfway, final week).

### Community Bonding Period

-   Getting familiar with development environment (Git Hub account, Git,
    Biopython's repository, Bug tracking system, etc)

<!-- -->

-   Gather scientific literature and discuss some of the
    to-be-implemented methods.

### Week 1 \[31st May - 6th June\]

#### Renumbering residues of a structure

-   Read SEQRES record to account for gaps
-   Alternatively read ATOM records.

#### Probe disulphide bridges in the structure

-   Via NeighbourSearch class
-   Also use SSBOND in header

#### Extract Biological Unit

-   REMARK350 contains rotation and translation information
-   If REMARK is absent, do nothing.

### Week 2 \[7th – 13th June\]

#### Structure Hydrogenation

-   Add all/polar hydrogens through interface with WHATIF server.
-   Optionally define a set pH
    -   [pKa values
        algorithm](http://www3.interscience.wiley.com/journal/112117957/abstract)

#### Hydrogenation Report

-   Produces a brief list of polar hydrogen atoms in the structure.
    -   Chain | Residue \[number\] | Atom

### Weeks 3-5 \[14th June- 4th July\]

#### Removal of disordered atoms

-   [Solution proposed in the Biopython
    wiki](Remove_PDB_disordered_atoms "wikilink")

#### Residue name normalisation

-   Build conversion table from different nomenclatures (research them
    during c.bonding period )
-   Write function to make a given structure compliant with a given
    software nomenclature:
    -   Amber
    -   CNS/HADDOCK
    -   GROMACS

#### Coarse Grain Structure

-   Implement function to reduce complexity of a structure
    -   1pt\*c-alpha
    -   2pt\*c-alpha / c-beta
    -   3pt\*c-alpha / c-beta / side-chain pseudo-centroid OR side-chain
        centroid

### Week 6 (Mid-Term) \[5th - 11th July\]

  
Testing and consolidating the features thoroughly.

Write documentation & examples for each feature, to be included in
Biopython's Wiki and Bio.PDB's FAQ.

Mid-term Evaluations. Discussing with mentors current state of project
and adjust following schedule to comply with project's needs.

### Week 7 \[12th - 19th July\]

#### Add support for MODELLER's PIR format to Biopython

-   [Format
    Description](http://www.salilab.org/modeller/manual/node445.html#alignmentformat)
-   SeqIO
-   AlignIO

#### Allow conversion of Structure Object to Sequence Object

-   Based on Bio.PDB.Polypeptide function

### Weeks 8-10 \[20th July - 9th August\]

#### Add Sequence/Structure Homology functions

-   Create call to Biopython's BLAST interfaces
    -   Allow direct blast from structure object ( e.g.
        protein.find\_homoseq() )
    -   Returns list of tuples with E-Value \*Dictionary (name, length
        of alignment, etc..)
-   Create interface with structural homology web services
    -   e.g. [Dali
        server](http://ekhidna.biocenter.helsinki.fi/dali_server/)
    -   Return list of tuples with Z-Score\*Dictionary (name, etc...)

#### Implement basic structure validation checks

-   Via NeighbourSearch class
    -   Same Charge contacts
    -   Atom Clashes
-   Via ResidueDepth Class
    -   Buried Charges
-   Interface WHATIF PDBReport web service
    -   Parse WARNING and ERROR messages

### Week 11 \[10th - 17th August\]

#### Reviewing documentation, code, write tests for new functions.