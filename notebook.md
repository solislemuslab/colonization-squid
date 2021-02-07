# Initial meeting with Mark Mandel (12/15)

- He is studying colonization factors in bacteria (transposon)
- Experiment: 50k mutants and 6 pools of squid. There is an input pool and an output pool and there is a comparison of where is the transposon in the input pool and the output pool
- they check the genes that are high in the input but low in the output as evidence of colonization factor. But the genes identified are all false positive as even without the gene, the mutants are able to colonized compared to wildtype
- the data (spreadsheet)
    - 4000 rows=genes in bacteria
    - column input01 corresponds to the proportion of mutants that have the gene out of 1 million (there are input01-input10 as the 10 replicates)
    - if this number is 0 that means that they did not detect any mutants in this read. This happens when it is a very essential gene
    - there are other columns corresponding to the squid
- they want to use the information on the drop (number in squid compared to number in input) to identify colonization factors. If the drop is big, they assign "category" of 6 (colonization factor)
- they want to use machine-learning tools to predict this category (not by eye) from the drop, and then use this model to predict function on other unknown genes
- **main goal** categorize genes based on drop with machine-learning methods


## Questions
- why the drop corresponds to colonization factor? what is the biology?


# From Mark Mandel email

1. Brooks et al. 2014 PNAS:

- Main manuscript: Figure 3 shows examples of genes that we predicted to be colonization factors, and they were verified in 1 x 1 competition assays of a defined mutant and wild type strain. (Note that we have other examples of true colonization factors from the literature that we can annotate. We also have other candidates that seemed really great but when we tested them they had no defect (e.g., VF_2264).)

- Supplementary material PDF: Figure S4 shows examples of genes that we predicted to be colonization factors, and they did not verify in a similar assay.

- Supplementary Dataset S1: Raw data from the experiment. Each row is a gene. Each column is a treatment. Columns H-Q are 10 replicates of the input library. Columns R-S are 2 replicates of the input library passaged for 15 generations in rich medium (LBS). Columns T-AE are 6 biological replicates for the library passaged through 250 squid (2 technical replicates of each output). 

Each data point is the counts-per-million (CPM) for mutants in that gene in that sample. All of the transposon mutants in a given sample are normalized to 1 million reads. To make this table we ignore reads that map in intergenic region or the 3’ end of the gene (since they probably don’t impact function). Therefore, if you add up all of the CPM in a column, you’ll get a bit less than 1 million. When the CPM counts stay constant, there is no fitness deficit or benefit for the gene under that condition. This is easiest to see under rich medium. In the squid, the data are very noisy! We noticed that most known colonization factors were down in most of the squid pools (but not all the way to zero). For example, genes VF_A1020 through VF_A1037 are known squid colonization factors. We codified this in the legend on the next sheet in the workbook. However, we are hoping that your approach can make better predictions here.

2. pyinseq software: https://github.com/mjmlab/pyinseq

We developed this after the paper so that we would have a stable way to analyze our Illumina data. A talented undergrad, Emanuel Burgos, is playing around with using Snakemake to make the pipeline more modular. If you have input for us on that we’d be eager for any feedback!!