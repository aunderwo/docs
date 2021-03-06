---
layout: docpost
title: Providing metadata
date_published: 2020-03-29 22:30:00 +0000
date_modified:  2020-04-02 12:00:00 +0000
author: samstudio8
maintainer: samstudio8
---

# Basics and terminology

* A **biosample** refers to a single, physical sample taken from a patient or environment, such as a swab. It should be assigned a COGUK identifier as soon as possible.
* A **COGUK identifier** (alternatively known as a Heron barcode, or central sample ID) is a randomly generated string prefixed with a four letter site name. Where it is impractical for a site to use these identifiers, you may use your own, providing that you use those identifiers consistently.
* A **biosample source** refers to the physical patient or environment from where a sample was taken.
It is not possible (for various reasons) for a patient to be identified with a single, unique, shareable identifier when a sample is made available to the consortium.
It is our intention that working between the health agencies, the NHS and HDR-UK, that such identifiers can be made available to the project in the near future.

* To ensure a biosample can be correctly matched to its biosample source later, we must try to record:
    * If possible, the **PHx sample ID**: an identifier assigned to the sample by a health agency. For example, in this context, Public Health England samples IDs are prefixed with `H20`.
    * Otherwise, a **local lab ID**: this could be any identifier used at the hospital or point of origin. Ideally this identifier will be used by the hospital to submit information about positive samples to SGSS. You must ensure you have permission to use these identifiers in case they are considered personally identifiable.
* Providing these identifiers is not a requirement of the consortium, but consider that being unable to provide them will mean it is unlikely they will ever be linked back to their source.
* Where possible, the **local lab ID** should be an identifier that can be mapped quickly and securely. Sites should not be encouraged to hold this linkage information themselves.

* Where a patient or environment has been **sampled multiple times**: each biosample should have its own COGUK identifier. You can use the *first* COGUK identifier for that patient or environment as the **biosample source** as a means to link them together in the interim.
* Where a biosample is split into **multiple aliquots** or **shared with other sequencing centres**, that biosample **should not be relabelled** by the receiving site. This ambiguity can be resolved later by providing a library and sequencing run name that corresponds to your site.

* A **library** is a collection of extracted and processed **biosamples** that have been prepared for simultaneous sequencing.
* A library should be given a `library_name` that is unique across the whole project. There are no limitations on what this name should look like but it should be at least five characters. Consider using the date, or site name if you don't have a generated identifier to use. "BIRM-001" would be reasonable, "1" would not.

* A **sequencing run** corresponds to the sequencing of a single library. *e.g.* A flowcell.
* A sequencing run should be given a `run_name` that is unique across the whole project. Ideally this will be the name your sequencing software generates for the run. For example, an Illumina run: `<date>_<machine_id>_<run_no>_<some_zeros>_<flowcell>` or a Nanopore run `<date>_<time>_<position>_<flowcell>_<id>`.
If for some reason you don't have these, a string of at least five characters that helps you identify this run is fine. Consider using your site and the date if you don't have another identifier. "BIRM-RUN-20200402-1" is fine, "RUN1" "birm run" or "nanopore" are not.
* If a library is sequenced multiple times, each run should be considered a separate and distinct sequencing run.

* Be aware that once you have provided metadata, many fields can still be changed by resubmitting the data. **However, the primary keys: `central_sample_id`, `library_name` and `run_name` cannot be changed. Ensure they are correct before uploading your data to avoid complications.**

* More generally, an object such as a biosample, sequencing library or data file may be referred to as an **artifact**. Something that causes a change or creates a new object (such as library pooling, or sequencing) is referred to as a **process**.

* Insufficient metadata will preclude the use of your sample in downstream analyses and project reports. It may also prevent automated uploading of your data to ENA and GISAID.

## Reporting standards for the consortium

### Biosamples

For a sample to be accepted for upload to CLIMB, and used in downstream analyses, you must provide the following:

* A single, unique, shareable identifier the "COGUK ID" (either a Heron barcode, or a suitable barcode from your own lab)
* Country of origin (UK-ENG, UK-SCT, UK-WLS, UK-NIR)
* Either a sample collection date (YYYY-MM-DD) **or** a sample `received_date` (YYYY-MM-DD). Do not attempt to impute the collection date. You must provide the day of the month.
* A GISAID strain ID and GISAID strain accession **if** you have already made the sequence available on GISAID yourself. **If you do not provide a strain ID and accession, and your organisation has requested that we upload strains for you, it will be automatically uploaded after it passes QC**. If you have submitted to GISAID, please provide this identifier to prevent duplicate strains being added to public databases. If you have submitted to GISAID and you are waiting on the accession, set the secondary_accession in your request to "PENDING".

**You cannot change the COGUK ID once it has been submitted**.

You should aim to provide as much information as possible about a sample to the consortium. 
[See a full list of fields that have been deemed acceptable for the consortium to collect](majora/add_sample).

Be sure that you have the governance and permission in place to share metadata **before** it is uploaded to CLIMB. Uploaded information may quickly be incorporated into downstream artifacts and shared around the consortium. **Do not simply follow what other sites are doing, rules will differ between countries and health boards in the UK**.

### Libraries

For a library to be listed on CLIMB, you must provide the following:

* A unique library name. It must be unique across the entire project, so consider prefixing yours with the date and or your site.
* One or more COGUK IDs that refer to the biosamples that were pooled into the library
    * Additionally, for each biosample you must provide the library source, selection and strategy.
* The sequencing kit
* The sequencing protocol
* Whether the run has single or paired reads

**You cannot change the library_name once it has been submitted**.
* You can use the `library_name` as your `run_name` (or vice versa) if they have a 1:1 mapping (*i.e.* A single library was sequencing on a single run). If you provide sample information row-by-row using the uploader, each biosample that is pooled into the same library should have the same library_name. Each library that is on the same run should have the same run_name.

You *should* also provide information pertinent to performing QC and filtering on your downstream analyses here, such as the version of the ARTIC protocol and primer pools (if used).

[See more information on library metadata](majora/add_library).

### Sequencing Runs

* A unique run name. It must be unique across the entire project, so consider prefixing yours with the date and or your site if you aren't using the run name from the sequencer.
* The corresponding library name
* The sequencing instrument make and model

**You cannot change the run_name once it has been submitted**.
* You can use the `library_name` as your `run_name` (or vice versa) if they have a 1:1 mapping (*i.e.* A single library was sequencing on a single run). If you provide sample information row-by-row using the uploader, each biosample that is pooled into the same library should have the same library_name. Each library that is on the same run should have the same run_name.

The API will allow you to tag your sequencing runs with arbitrary key value pairs if you wish to provide more information about your sequencing run, or the downstream bioinformatics.

[See more information on run metadata](majora/add_sequencing).


## Metadata storage

Metadata is stored by `Majora`. [You can log into Majora](https://majora.covid19.climb.ac.uk/) with your CLIMB unified account. New users [should register](https://majora.covid19.climb.ac.uk/forms/register) and wait to have their account approved by the system administrators.

## Methods for metadata submission

Metadata can be submitted in three ways.
No matter which option you use, you will need to know your unified CLIMB user name and get an API token.
Your username should have been emailed to you once your account was approved. You can see your token on your [user profile](https://majora.covid19.climb.ac.uk/accounts/profile/).

* By uploading a spreadsheet, or CSV/TSV file to [cog-uk.io](https://docs.cog-uk.io/metadata/), supported by Anthony Underwood (CGPS)
    * [Uploading biosamples](https://docs.cog-uk.io/metadata/bulk-upload-1/bulk-upload)
    * [Uploading biosamples with library and sequencing](https://docs.cog-uk.io/metadata/bulk-upload-1/samples-and-sequencing)
* Via our [`ocarina` command line tool](https://github.com/SamStudio8/ocarina/), supported by Sam Nicholls (UoB)
    * [Uploading metadata with Ocarina](majora/ocarina)
* By sending POST requests to our [JSON endpoints directly](https://docs.covid19.climb.ac.uk/majora-api), supported by Sam Nicholls (UoB)
    * [Submit a biosample](majora-api)
    * [Submit a library](majora/add_library)
    * [Submit sequencing runs](majora/add_sequencing)

We anticipate that sites will find using the CSV/TSV interface for uploading biosamples the most straightforward, and the `ocarina` command line tool for providing library and sequencing information. Advanced users are welcome to communicate with the API directly.

## Help and support

* For questions about metadata collection and minimum reporting, see `#metadata`
* For questions about metadata submission or using the APIs, see `#metadata-apis`

