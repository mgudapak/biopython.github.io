---
title: PDBParser
permalink: wiki/PDBParser
layout: wiki
---

This is a draft page for the PDBParser class.

Benchmark
=========

A performance benchmark of the parser was carried out to evaluate wether
the development of new features degraded the overall parsing speed.

Datasets
--------

[CATH Domain
Collection](http://release.cathdb.info/v3.4.0/CathDomainList) - 11330
Structures containing only coordinate information (no Element assigned).

[Protein Data Bank
Collection](ftp://ftp.wwpdb.org/pub/pdb/data/structures/divided/pdb/) -
72836 Structures containing both headers and coordinate information.

Versions Tested
---------------

[Biopython 1.49](http://biopython.org/DIST/biopython-1.49.zip) (Nov.
2008)

[Biopython 1.57+](https://github.com/biopython/biopython) (May 2011) |
Element column auto-assignment.

[Biopython PDB
branch](https://github.com/JoaoRodrigues/biopython/tree/pdb_enhancements)
(May 2011 @ Github) | Warnings module replaced
\_handle\_pdb\_exception() && Other minor changes

Benchmarking Script
-------------------

The following script was used to benchmark the parser. The garbage
collection module - <b>gc</b> - was necessary to avoid dead objects
still in memory causing the machine to start swapping.

``` python
#!/usr/bin/env python
""" Script to Benchmark Bio.PDB PDBParser """

import sys, os, warnings

# Parsing Function
def parse_structure(path):
    """ Parses a PDB file """
    
    s = P.get_structure('test', path)

    return 0

def fancy_output(tps):
    """ Outputs the results in a nicer way """

    print "# Bio.PDB PDBParser Benchmark"
    print
    print "Structure \tLenght \tTime Spent (ms)"
    for i,s in enumerate(tps):
        print " %s\t (%s) \t%3.3f" %(os.path.basename(pdb_library[i]), pdb_length[i], s)
    print 
    print "Total time spent: %5.3fs" %(sum(tps)/1000)
    print "Average time per structure: %5.3fms" %(sum(tps)/len(tps))

if __name__=='__main__':

    import time, gc
    from Bio.PDB import PDBParser
    P = PDBParser(PERMISSIVE=1) # For the pdb_enhancements branch benchmarking, PERMISSIVE was set to 2 (silence warnings).
   
    library_path = sys.argv[1]

    pdb_library = [os.path.join(library_path, f) for f in os.listdir(library_path)]
    pdb_length = [len(set([int(l[23:26]) for l in open(f) if l.startswith('ATOM')])) for f in pdb_library] # Unique counting of residues
    sys.stderr.write("Loaded %s structures (Average Length: %4.3f residues)\n" %(len(pdb_length), (sum(pdb_length)/float(len(pdb_length)))))

    tps = []
    # Run the Test
    for i, pdb_file in enumerate(pdb_library):    
        sys.stderr.write( "[%s] %i Structure(s) Parsed \n" %(os.path.basename(pdb_file), i+1) )
        a = time.time()
        parse_structure(pdb_file)
        b = time.time()-a
        tps.append(b*1000)
        gc.collect()
    # Output Results
    fancy_output(tps)
```

Results
-------

### CATH Dataset

Average Structure Length: 147 residues

#### Biopython 1.49

`Total Time Spent: 530.686s`  
`Average Time per Structure: 46.84ms/structure`  
`Average Structures per Second: 21.38 structures/s`  
`Failed to parse 0 structures due to errors.`

`Length                N. Structures   Average Time Spent (ms)`  
`< 100                 3663            25.11`  
`100 =< x < 200        5295            44.68`  
`200 =< x < 500        2328            83.41`  
`500 =< x < 1000       44              180.35`

`TOTAL                 11330           46.84`

[Link to full
results](http://nmr.chem.uu.nl/~joao/f/benchmark_CATH-biopython_149.time)
[Plot of the full
results](http://nmr.chem.uu.nl/~joao/f/benchmark_CATH-biopython_149.png)

#### Biopython 1.57+

`Total Time Spent: 686.176s`  
`Average Time per Structure: 60.56ms/structure`  
`Average Structures per Second: 16.51 structures/s`  
`Failed to parse 0 structures due to errors.`

`Length                N. Structures   Average Time Spent (ms)`  
`< 100                 3663            32.57`  
`100 =< x < 200        5295            57.76`  
`200 =< x < 500        2328            107.56`  
`500 =< x < 1000       44              242.30`

`TOTAL                 11330           60.56`

[Link to full
results](http://nmr.chem.uu.nl/~joao/f/benchmark_CATH-biopython_current.time)

#### Biopython PDB Branch

`Total Time Spent: 695.405s`  
`Average Time per Structure: 61.37ms/structure`  
`Average Structures per Second: 16.29 structures/s`  
`Failed to parse 0 structures due to errors.`

`Length                N. Structures   Average Time Spent (ms)`  
`< 100                 3663            33.24`  
`100 =< x < 200        5295            58.46`  
`200 =< x < 500        2328            108.77`  
`500 =< x < 1000       44              247.171`

`TOTAL                 11330           61.38`

[Link to full
results](http://nmr.chem.uu.nl/~joao/f/benchmark_CATH-biopython_pdb_enhancements.time)
[Plot of the full
results](http://nmr.chem.uu.nl/~joao/f/benchmark_CATH-biopython_pdb_enhancements.png)

### PDB Dataset

Average Structure Length: 263 residues

#### Biopython 1.49

`Total Time Spent: 27801.934s (7.72h)`  
`Average Time per Structure: 381.706ms/structure`  
`Average Structures per Second: 2.62 structures/s`  
`Failed to parse 2 structures due to errors:`  
`  1. 3NH3 (negative occupancy) - fix: `[`http://bit.ly/ks6PDN`](http://bit.ly/ks6PDN)  
`  2. 2WMW (invalid ANISOU field) - fix: `[`http://bit.ly/ld9BWs`](http://bit.ly/ld9BWs)

`Length                N. Structures   Average Time Spent (ms)`  
`< 100                 10986           392.227`  
`100 =< x < 200        19641           371.716`  
`200 =< x < 500        35299           301.699`  
`500 =< x < 1000       6291            597.472`  
`>= 1000               619             2881.520`

`TOTAL                 72836           381.706`

[Link to full
results](http://nmr.chem.uu.nl/~joao/f/benchmark_PDB-biopython1.49.time)

#### Biopython 1.57+ (In progress)

In progress

#### Biopython PDB Branch (In progress)

In progress