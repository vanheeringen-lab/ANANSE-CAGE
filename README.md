# ANANSE-CAGE
Supporting scripts and notebooks detailing how to use ANANSE with CAGE data.

* [CAGEfightR_to_ANANSE.Rmd](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/CAGEfightR_to_ANANSE.Rmd) - Rmarkdown script used to generate input files for ANANSE-CAGE from CAGE Transcription Start Site data (.ctss)

* [basic_ANANSE-CAGE_run.txt](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/basic_ANANSE-CAGE_run.txt) - Simple example how to run ANANSE-CAGE 
* [ANANSE-CAGE-run.ipynb](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/ANANSE-CAGE-run.ipynb) - Script used to run multiple ANANSE-CAGE analyses

* [model_training_ANANSE-CAGE.ipynb](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/model_training_ANANSE-CAGE.ipynb) - Training of binding prediction model (CAGE data only)
* [model_training_ANANSE-CAGE-H3K27ac.ipynb](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/model_training_ANANSE-CAGE-H3K27ac.ipynb) - Training of binding prediction model (CAGE and H3K27ac data)

## How to use
To perform the CAGE module of ANANSE we first need to generate the necessary input files using R, specifically the CAGEfightR package. Use [this](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/CAGEfightR_to_ANANSE.Rmd) R markdown as a reference. **Note, the best prediction algorithm currently only supports hg19 and hg38**. **We recommend to use triplicates.**

### Initialize
First, edit the first chunk of the [this](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/CAGEfightR_to_ANANSE.Rmd) R markdown according to your own data and structure. For example, make sure you use the correct assembly, working directory, paths, and names.

### Pre-processing
You can use BED files (tab seperated) with 6 columns: 

1. chr
2. start
3. end
4. chr:start..end,strand
5. tag count
6. strand

For example:
*chr1	564586	564587	chr1:564586..564587,+	2	+*

CAGEfightR can turn this BED format and convert it to BigWig files that are then used for tag quantification. Make sure to set "PREPROCESSING = TRUE" in [this](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/CAGEfightR_to_ANANSE.Rmd) R markdown script in order to convert the BED files to BigWigs.

### Generate ANANSE input files
Use the BigWig files to run the third chunk of [this](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/CAGEfightR_to_ANANSE.Rmd) R markdown script. You should end up with these files:

- enhancer_source.tsv
- enhancer_target.tsv
- TPM_source_N.txt
- TPM_target_N.txt
- DE_genes.txt

### ANANSE-CAGE
Install ANANSE according to the [documentation](https://anansepy.readthedocs.io/en/master/installation/)

Required layout:

- work_dir/
     - data/
       - enhancer_source.tsv
       - enhancer_target.tsv
       - TPM_source_N.txt
       - TPM_target_N.txt
       - DE_genes.txt
     - results/
       - (ouput files will be deposited here)

Perform the following steps:

#### ANANSE Binding
```bash
ananse binding -C ./data/enhancers_source.tsv -o ./results/source_binding -g hg19;
ananse binding -C ./data/enhancers_target.tsv -o ./results/target_binding -g hg19;
```

#### ANANSE Network
```bash
ananse network ./results/source_binding/binding.h5 -e ./data/TPM_source_*.txt -o ./results/network_source.tsv -g hg19 -n 4;
ananse network ./results/target_binding/binding.h5 -e ./data/TPM_target_*.txt -o ./results/network_target.tsv -g hg19 -n 4;
```

#### ANANSE Influence
```bash
ananse influence -s ./results/network_source.tsv -t ./results/network_target.tsv -d ./data/DE_genes.txt -o ./results/influence.tsv -i 500000 -n 12;
```

#### ANANSE Plot
```bash
ananse plot ./results/influence.tsv -d ./results/influence_diffnetwork.tsv -o ./results/influence_results;
```
