Docker container run enviornment for PGAP (Pan-Genome Analysis Pipeline)

> PGAP is a pan-genomes analysis pipeline developed with Perl. It could perform five analytic functions with only one command, including cluster analysis of functional genes, pan-genome profile analysis, genetic variation analysis of functional genes, species evolution analysis and function enrichment analysis of gene clusters.

## Usage

To run, mount your input and output directories with -v, and then use a standard PGAP.pl call with options:

``` bash
seq_dir=/path/to/input/{.nuc|.pep|.function}  # PGAP-formatted input files
out_dir=/path/to/output  # Output directory (should exist)

docker run \
  -v "${seq_dir}":/input \
  -v "${out_dir}":/output \
  -w /pgap kastman/pgap:1.12 \
    perl ./PGAP.pl --strains $strains \
    --input /input --output /output \
    --cluster --pangenome --variation  --evolution --function \
    --method MP --thread 1
```

This will run produce a full run with all 5 steps of analysis, using a single thread. Note that I had problems with segfaults in the docker container when using multiple threads, but there may be a way to adjust that?

Also note the -w /pgap default working directory - this is important for PGAP.pl to correctly find sub-modules in the /pgap directory.

## Notes

Adapts the Dockerfile from https://hub.docker.com/r/lfelipedeoliveira/pgap/ . (However, that contained version 2.12 of blast, which seemed to be incompatible with PGAP - specifically the -C option to blastall called by inparanoid.pl).

## Reference

For more information, see: Zhao Y, Wu J, Yang J, Sun S, Xiao J, Yu J. PGAP: pan-genomes analysis pipeline. Bioinformatics. 2012;28(3):416-418. [doi:10.1093/bioinformatics/btr655](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3268234/).
