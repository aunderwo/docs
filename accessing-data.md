---
layout: docpost
title: Accessing and using CLIMB data
date_published: 2020-03-29 23:20:00 +0000
date_modified:  2020-04-21 10:50:00 +0000
author: samstudio8
maintainer: samstudio8
---

# Basics
## The CLIMB QC pipeline

Data uploaded by users to CLIMB with sufficient metadata is periodically pulled through the [`elan` nextflow pipeline](https://github.com/SamStudio8/elan-nextflow).
`elan` is responsible for basic quality checking including the following:

* Filtering unmapped reads and sorting uploaded BAMs (to ensure this step is done)
* Ensuring the BAM is valid with `samtools quickcheck`
* Counting the proportion of non-ambiguous, ambigious and invalid bases in the consensus FASTA
* Counting the proportion of positions in the aligned BAM that are above certain coverage thresholds
* Pruning potentially spurious or human-looking reads from uploaded BAMs
* Where applicable, checks the depth of tiles amplified by the ARTIC protocol

Once `elan` has finished, the following artifacts are automatically published with a version number based on the date:

* `fasta`: All consensus FASTA with the naming strategy `<coguk_id>.<run_name>.climb.fasta`
* `alignment`: Each filtered, sorted and checked BAM with the naming strategy `<coguk_id>.<run_name>.climb.bam`
* `qc`: A basic quality report for each COGUK ID.
* `dh`: A simple report on BAM reads dropped by elan due to alignment length or matches to human references (for ENA BAMs only)

Data is automatically discarded by the following criteria, according to the version of the QC schema used:

### v1.1 2020-04-21

* Illumina 
    * Average BAM depth less than 10x
    * BAM depth less than 10x for over 50% of the reference positions
    * Consensus FASTA containing more than 50% Ns
* Nanopore
    * Average BAM depth less than **20x**
    * BAM depth less than **20x** for over 50% of the reference positions
    * Consensus FASTA containing more than 50% Ns

### v1.0 2020-03-29
* Illumina 
    * Average BAM depth less than 10x
    * BAM depth less than 10x for over 50% of the reference positions
    * Consensus FASTA containing more than 50% Ns
* Nanopore
    * Average BAM depth less than 25x
    * BAM depth less than 25x for over 50% of the reference positions
    * Consensus FASTA containing more than 50% Ns


## Accessing CLIMB data that has passed QC

You need a CLIMB account to upload or access data. If you haven't got one, [see instructions on registering and uploading to CLIMB](upload-instructions). You will need to SSH into the CLIMB COVID server.

The latest artifacts are published in `/cephfs/covid/bham/artifacts/published/latest`.

For just the sequences, you can use `/cephfs/covid/bham/artifacts/published/elan.latest.consensus.fasta`.
The FASTA headers are encoded with metadata of most utility, using the format:

```
COG-UK/`central_sample_id`/`sequencing_center`:`run_name`|`central_sample_id`|`adm0`|`adm1`|`sampling_center`|`collected_or_received_date*`|`sequencing_center`|`sequencing_date`
```
* Received dates are prefixed with `R` and are output when collection dates are not available for a sample.

For all metadata, use `/cephfs/covid/bham/artifacts/published/majora.latest.metadata.tsv`.

Note that the merged consensus FASTA will also include resequencing. That is, a biosample may have more than one genome in the consensus FASTA, you can identify them as they will have the same `central_sample_id` in their header.
