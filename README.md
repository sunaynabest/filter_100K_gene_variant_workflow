# filter_100K_gene_variant_workflow
# Script to filter 100,000 Genomes project (100K) output files from Gene-Variant Workflow script
# Script allows filtering of tsv output files generated by the Gene-Variant Workflow script available within the 100,000 Genomes Project research environment
# Gene-Variant Workflow script available from https://research-help.genomicsengland.co.uk/display/GERE/Gene-Variant+Workflow. 
# Gene-Variant Workflow script designed to extracts variants of the specified gene region(s) from the all Illumina VCF files within the 100,000 Genomes Project, aggregates them all together and annotates variants with Ensembl VEP.
# This script, filter_100K_gene_variant_workflow, provides steps to filter down large variants lists to potentially pathogenic shortlists.
# The script first excludes variants common (>0.002) in 100K and gnomAD.
# Next, variants called in non-canonical transcripts are excluded. 
# Finally, sub-lists of variants interest are created containing the following: 
# i) variants previously annotated by ClinVar as “pathogenic” or “likely_pathogenic"
# ii) variants annotated by VEP as “HIGH” impact (stop_gained, stop_lost, start_lost, splice_acceptor_variant, splice_donor_variant, frameshift_variant, transcript_ablation, transcript_amplification) 
# iii) missense variants predicted “deleterious” by the in-silico prediction tool SIFT