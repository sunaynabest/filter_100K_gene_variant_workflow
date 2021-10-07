# 100K Gene Variant Filter

### What does this script do
This script filters output files from the 100,000 Genomes project (100K) Gene-Variant Workflow

The [100K Gene-Variant Workflow](https://research-help.genomicsengland.co.uk/display/GERE/Gene-Variant+Workflow) available within the 100,000 Genomes Project research environment is designed to extract variants of the specified gene region(s) from the all Illumina VCF files within the 100,000 Genomes Project, aggregate them and annotate variants with Ensembl VEP. The output from this script is a .tsv file. These files are large, and difficult to analyse.

The script in this repository, filter_100K_gene_variant_workflow, provides steps to filter down large variants lists to potentially pathogenic shortlists.

### Summary of the script
1. The script excludes common variants (>0.002) in 100K and gnomAD.
2. Variants called in non-canonical transcripts are excluded.
3. Finally, sub-lists of variants interest are created containing the following:
		- i) variants previously annotated by ClinVar as “pathogenic” or “likely_pathogenic"
		- ii) variants annotated by VEP as “HIGH” impact (stop_gained, stop_lost, start_lost, splice_acceptor_variant, splice_donor_variant, frameshift_variant, transcript_ablation, transcript_amplification)
		- iii) missense variants predicted “deleterious” by the in-silico prediction tool SIFT


### STEPS TO USE THIS SCRIPT WITHIN THE RESEARCH ENVIRONMENT
1. Ensure you are runing python in the correct environment. There should be "(idppy3)" at the start of your terminal line
	If not:
			1a) `. /resources/conda/miniconda3/etc/profile.d/conda.sh`
			1b) `conda activate idppy3`

2. Navigate to the GeneVariantWorkflow folder containing this script (for me this is under /re_gecip/GW_SB/GeneVariantWorkflow)

3. To run on the command line enter: `python filter_gene_variant_workflow.py <enter-the-path-to-the-folder-containing-your-tsv-file-here> <enter-your-tsv-file-name-here>`

NOTE: path to your folder should be the one containing the GeneVariantWorkflow data output files: for me it's at /home/sbest1/re_gecip/shared_allGeCIPs/GW_SB/GeneVariantWorkflow/<gene_name>/v1.7/final_output/data

Note: The tsv file must end in ".tsv". It must be tab delimited.

### Output Files

Output files will be placed into a new folder named after the input tsv file within the final_output/data folder that contains your input file.

- **gene_variant_workflow_gel_gnomAD_rare.csv**: Remove all entries with MAF_variant > 0.002. These are common variants (>0.2%) called in the 100K data set
- **gene_variant_workflow_gel_gnomAD_rare.csv**: Remove all entries with gnomAD_AF_annotation > 0.002 from the above file. These are common variants (>0.2%) called in the gnomAD dataset.
- **gene_variant_workflow_canonical_transcript_rare.csv**: Retain only variants called in the canonical transcript from the file above.
- **gene_variant_workflow_rare_homozygous**: Remove all entries with AC_Hom_variant = 0 from the above file. This will leave variants called at least once as homozygous in the rare disease 100K dataset.
- **gene_variant_workflow_rare_HIGH_impact**: Retain all entries with "HIGH" in the Impact_annotation column from gene_variant_workflow_canonical_transcript_rare.csv. This will give a file of variants rare in 100K and gnomAD that are high impact.
- **gene_variant_workflow_homozygous_rare_HIGH_impact**: Retain all entries with "HIGH" amongst the homozygous variants
- **gene_variant_workflow_rare_ClinVAR_pathogenic**: Retain all entries annotated as pathogenic in ClinVar_CLNSIG_annotation column from from gene_variant_workflow_canonical_transcript_rare.csv. This catches variants rare in 100K and gnomAD called 'pathogenic' and 'likely_pathogenic'.
- **gene_variant_workflow_rare_missense_all**: Retain all entries with missense in CLIN_SIG_annotation column from gene_variant_workflow_canonical_transcript_rare.csv. Gives output file of rare missenses from 100K and gnomAD. NOTE: don't have CADD scores, may need to run through VEP with plugins if want this.
- **gene_variant_workflow_rare_missense_SIFT_deleterious**: Retain all entries with SIFT_annotation entry containing deleterious fro the above file. NOTE: don't have CADD scores, may need to run through VEP with plugins if want this.

### ERRORS:
ERR1: You have not included the file path after running the script

ERR2: The input file you have linked to does not end in ".tsv"

ERR3: The input file does not exist at the path you have provided

ERR4: The input tsv file you have provided is not in the expected format. It must be tab delineated.
