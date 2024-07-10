## Analysis of GO term enrichment in predicted miRNA targets ##

#### miRNA target prediction download: ####

TargetScan mouse predictions ([v.8.0, dated September 2021](https://www.targetscan.org/mmu_80/)):

``wget https://www.targetscan.org/mmu_80/mmu_80_data_download/Summary_Counts.default_predictions.txt.zip;unzip Summary_Counts.default_predictions.txt.zip;awk -F '\t' '$4==10090 {print}' Summary_Counts.default_predictions.txt > mmu_Summary_Counts.default_predictions.txt;./Script_GO_terms_for_murine_TargetScan_predictions.sh``

Predicted target lists are named 'tmp\_pred\_mmu-miR-33' and 'tmp\_pred\_mmu-miR-96'.

#### GO term enrichment analysis: ####

Predicted target lists 'tmp\_pred\_mmu-miR-33' and 'tmp\_pred\_mmu-miR-96' are submitted to "GO Enrichment Analysis" of https://geneontology.org/ (selecting "biological process" as a category of terms). Resulting tables 'analysis\_miR-33.txt' and 'analysis\_miR-96.txt' are edited (some GO terms are supposed to contain a prime character, actually replaced by a quote sign in "RNA 3'-end processing"; this creates issues for data loading in R, so we have to remove that problematic character), then fed to 'R_\commands\_volcano\_plot\_GO\_term\_enrichment' to trace volcano plots:

``for file in analysis_miR-33.txt analysis_miR-96.txt;do sed -i "s|RNA 3'-end processing|RNA 3prime-end processing|g" $file;Rscript R_commands_volcano_plot_GO_term_enrichment $file;done``

Volcano plot files are named 'Volcano\_plot\_from\_\*.pdf' and annotation of highlighted terms is listed in files 'Annotations\_\*.csv'.

## Analysis of GO term enrichment in ChIP-Seq-identified targets for LIN-15B ##

#### LIN-15B ChIP-Seq data download: ####

File 'mmc3.xlsx' is Suppl. Table 2 from [Gal et al. (2021)](https://pubmed.ncbi.nlm.nih.gov/34686342/) (ChIP-Seq on several proteins, including lin-15B).

#### GO term enrichment analysis: ####

Gene list in the 5th column of 'mmc3.xlsx' is submitted to "GO Enrichment Analysis" of https://geneontology.org/ (selecting "biological process" as a category of terms). Resulting table 'analysis\_LIN-15B.txt' is edited (some GO terms are supposed to contain a prime character, actually replaced by a quote sign in "RNA 3'-end processing"; this creates issues for data loading in R, so we have to remove that problematic character), then fed to 'R_\commands\_volcano\_plot\_GO\_term\_enrichment' to trace volcano plots:

``sed -i "s|RNA 3'-end processing|RNA 3prime-end processing|g" analysis_LIN-15B.txt;Rscript R_commands_volcano_plot_GO_term_enrichment analysis_LIN-15B.txt``
