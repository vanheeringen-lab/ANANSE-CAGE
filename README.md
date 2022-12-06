# ANANSE-CAGE
Supporting scripts and notebooks detailing how to use ANANSE with CAGE data.

* [CAGEfightR_to_ANANSE.Rmd](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/CAGEfightR_to_ANANSE.Rmd) - Rmarkdown script used to generate input files for ANANSE-CAGE from CAGE Transcription Start Site data (.ctss)

* [basic_ANANSE-CAGE_run.txt](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/basic_ANANSE-CAGE_run.txt) - Simple example how to run ANANSE-CAGE 
* [ANANSE-CAGE-run.ipynb](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/ANANSE-CAGE-run.ipynb) - Script used to run multiple ANANSE-CAGE analyses

* [model_training_ANANSE-CAGE.ipynb](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/model_training_ANANSE-CAGE.ipynb) - Training of binding prediction model (CAGE data only)
* [model_training_ANANSE-CAGE-H3K27ac.ipynb](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/model_training_ANANSE-CAGE-H3K27ac.ipynb) - Training of binding prediction model (CAGE and H3K27ac data)

## How to use
We can generate the necessary input files using the CAGEfightR package. Use [this](https://github.com/vanheeringen-lab/ANANSE-CAGE/blob/main/CAGEfightR_to_ANANSE.Rmd) script as a reference and change it according to your needs. 

To start, you can use .bed files with 6 columns (chr  start end  chr:start..end,strand  tags  strand). For example: chr1	564586	564587	chr1:564586..564587,+	2	+
