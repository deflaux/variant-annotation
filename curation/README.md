Curation Scripts
================

The scripts in this portion of the repository were used to ingest, reshape, and
store variant annotations in a cloud analysis-ready format.  You can use them to
bring in a fresh copy of an annotation resource or as a starting point for
curation of a new annotation resource.

## Status of this sub-project

This code currently works with annotation resources such
as [dbSNP](https://www.ncbi.nlm.nih.gov/projects/SNP/)
and [ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/) along with variant allele
frequencies
from
[NHLBI GO Exome Sequencing Project (ESP)](http://evs.gs.washington.edu/EVS/),
[1000 Genomes](http://www.internationalgenome.org/),
[ExAC](http://exac.broadinstitute.org/),
and [Genome Aggregation Database (gnomAD)](http://gnomad.broadinstitute.org/)
but similar techniques could be applied to other annotation resources.

All steps are run in the cloud, but each individual step is launched manually.

## Overview

### Curate Individual Annotation Sources

Many variant annotation sources are encoded as VCF files.  Therefore we can
use [Google Genomics](https://cloud.google.com/genomics/) to import the resource
and export it to BigQuery.

[Follow the tutorial](./tables) to run
a [dsub](https://github.com/googlegenomics/dsub) script to create individual
tables holding dbSNP, ClinVar, ESP, etc.

### Create an "All-Possible SNPs" Table

A table with annotations for all possible SNPs of a particular genome reference
is useful for:

* Examining SNP variation across different regions of the genome.
* Quickly annotating the SNPs for a cohort using a simple JOIN.
* Generating synthetic sequence variant datasets using the SNP allele
  frequencies from this table.

[Follow the tutorial](./allPossibleSNPs) to create an all-possible-SNPs tables
for build 38 of the human genome reference.

### Add Column Descriptions to a BigQuery Table

The `variants` table generated by performing
an
[export from Google Genomics](https://cloud.google.com/genomics/reference/rest/v1/variantsets/export) does
not include the field descriptions for the fields.

See [add BigQuery descriptions](./tables/AddBigQueryDescriptions.md) for
instructions on how to automatically populate the BigQuery schema
description with the information from the VCF header.
