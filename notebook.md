# Initial tests

## Baseline prediction
- We are using Category as our label to predict
- On the data visualization, we identified some features that do not contain any information on Category
- We also identified some linear relationships between input and LBS, but no clear pattern between LBS and squid
- Brian performed a baseline prediction based on 1) dummy classifier (most frequent) and 2) random forest with default settings
- The data was divided on testing, and training using a stratified approach so that proportions of categories are preserved
- Accuracy is already quite high (greater than 91% in all cases)


# Meeting Feb 10 with Mark Mandel, Songyang Cheng and Hector Burgos
- Mandel lab studies how animals are colonized by bacteria after being born and how the right bacteria gets to colonize (and not the wrong bacteria)
- the focus on a type of squid as model organism. In particular, this squid has one organ that is colonized by only one bacteria
- they study which genes in bacteria are necessary for colonization by performing experiments in which certain genes are knocked out (mutant bacteria) and then it is tested how well these mutant bacteria colonize the squid
- they have two main measurements: input (counts of bacteria in the dish), squid (counts of bacteria inside the squid)
- 6 replicates with 1500 squid each. Note that Squid01_1 and Squid01_2 correspond to 250 squid
- they also measure bacterial growth (LBS). This is done to separate the effect of growth from the effect of colonization. That is, a mutant might not be able to colonize but only because it was not able to grow (the knocked out gene was important for growth, not for colonization)
- In the data, we have ~3800 genes (rows). Some of these genes are already known to be important for colonization
- Input 01-10: count per million of mutants of that type (note that interrption of the gene is done once, and then bacteria are divided in 10 different tubes)
- If the input is 0, that means that an essential gene was knocked out
- the knocking out process (also denoted interruption process) of a gene was done randomly, so some times you interrupt a place in between genes or at the very end of the gene (which does not affect function). For this reason, if you sum the whole column Input01, you do not get exactly 1,000,000 (which is the number of mutants)
- Input=0 could also mean that that gene is very small so it was not really knocked out with these random interruptions
- Category is a manually created label based on visualization of the data. For example, they found out that the known colonization factors do not go to zero in the squid, but they also reduce the number a certain k-fold
- Mark and Hector will create a new csv file with information on known colonization factors

## Ideas of analyses
- Data visualization: are there strange things in the data?
- Supervised methods with Category as label
- Unsupservised clustering and then see if the clusters are related to the categories
- Semi-supervised methods with "known colonization" as label. We are waiting for these measurements


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