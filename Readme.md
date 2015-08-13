Python implementation of HMM regression of segmental ploidy state for DNA chip
===============================================================================================================

This 

This is a small tool based on the HMMs and t_test series to determine the most likely chromosome-level
karyotype of a sample while keeping the information about local amplifications.

As an input it requires a file that contains per row the following informations:
 - site ID
 - chromosome on which the site is
 - absolute genome location (required for ordering)
 - for each sample, relative gain/loss and 'nan' for the absent data.

First row is expected to be a set of sample identifiers, so that this information can be used in the subsequent
computations

Required packages:
 * numpy
 * scipy
 * matplotlib
 * chiffatools


chiffatools can be downloaded and installed via
    > pip install git+https://github.com/ciffa/chiffatools

Pay attention, you will need to manually select the file you are willing to analyze and type it's name in the
source code for now.

In order to execute the pipeline, setup the environement with a
```
    > import Karyotype_rertiever as KR
    > env = KR.Environement(path_to_your_file, file_name)
    > result = env.compute_all_karyotypes()
```

In order, you will be shown gain/losses on the chromosome level, label level and chromosome level.

The result is a 3-level dict:
```
    { <Strain Name>:
        {<chromosome#>-p/q:
            {'arm_level': arm-level aneuploidy,
             'segmental_aneuploidy': [[positions of arm start and segmental aneuploidy breakpoints (kb)],
                                      [ploidy level after the breakpoint above]]
             }
        ...},
    ....}
```

**Static data files:**
 - CytoBands.txt courtesy of http://hgdownload.cse.ucsc.edu/goldenPath/hg18/database/cytoBand.txt.gz

**Future developments:**
 - Made the HMM aware of distances between locuses measured on affymetrix chip (1) and recombination hotspots (2)
 - Collapse HMM predictions onto a chromosome limits or centromere limits if the transition boundaries are close (critcial)
 - Reformulate as Bayesian choice: state of markers =  evidence; distance = prob. of transition or collapse (?)