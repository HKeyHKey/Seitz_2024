## Comparison of cumulative plot and boxplot representation (panel "a") ##

#### Origin of experimental data: ####

miRNA transfection experiments described in [Grimson et al. (2007)](https://www.cell.com/molecular-cell/fulltext/S1097-2765(07)00407-8) (NCBI GEO dataset [GSE8501](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE8501)).

* miR-7 vs. mock, 12 h post-transfection: GSM210896 (channel 1: mock-transfected; channel 2: miR-7-transfected)
* miR-7 vs. mock, 24 h post-transfection: GSM210897 (channel 1: mock-transfected; channel 2: miR-7-transfected)
* miR-9 vs. mock, 24 h post-transfection: GSM210898 (channel 1: mock-transfected; channel 2: miR-9-transfected) (that is the dataset shown in Figure 1a in Seitz (2024))
* miR-9 vs. mock, 12 h post-transfection: GSM210899 (channel 1: mock-transfected; channel 2: miR-9-transfected)
* miR-122a vs. mock, 12 h post-transfection: GSM210900 (channel 1: mock-transfected; channel 2: miR-122a-transfected)
* miR-122a vs. mock, 24 h post-transfection: GSM210901 (channel 1: mock-transfected; channel 2: miR-122a-transfected)
* miR-128a vs. mock, 12 h post-transfection: GSM210902 (channel 1: mock-transfected; channel 2: miR-128a-transfected)
* miR-128a vs. mock, 24 h post-transfection: GSM210903 (channel 1: mock-transfected; channel 2: miR-128a-transfected)
* miR-132 vs. mock, 24 h post-transfection: GSM210904 (channel 1: mock-transfected; channel 2: miR-132-transfected)
* miR-132 vs. mock, 12 h post-transfection: GSM210905 (channel 1: mock-transfected; channel 2: miR-132-transfected)
* miR-133a vs. mock, 12 h post-transfection: GSM210906 (channel 1: mock-transfected; channel 2: miR-133a-transfected)
* miR-133a vs. mock, 24 h post-transfection: GSM210907 (channel 1: mock-transfected; channel 2: miR-133a-transfected)
* miR-142 vs. mock, 12 h post-transfection: GSM210908 (channel 1: mock-transfected; channel 2: miR-142-transfected)
* miR-142 vs. mock, 24 h post-transfection: GSM210909 (channel 1: mock-transfected; channel 2: miR-142-transfected)
* miR-148b vs. mock, 12 h post-transfection: GSM210910 (channel 1: mock-transfected; channel 2: miR-148b-transfected)
* miR-148b vs. mock, 24 h post-transfection: GSM210911 (channel 1: mock-transfected; channel 2: miR-148b-transfected)
* miR-181a vs. mock, 12 h post-transfection: GSM210912 (channel 1: mock-transfected; channel 2: miR-181a-transfected)
* miR-181a vs. mock, 24 h post-transfection: GSM210913 (channel 1: mock-transfected; channel 2: miR-181a-transfected)

(data downloaded from NCBI GEO, saved as 'GSM\*.dat' files)

#### Human 3´ UTR sequence download: ####

``wget ftp://ftp.ncbi.nlm.nih.gov/genomes/H_sapiens/RNA/rna.asn.gz;gunzip rna.asn.gz;./Module_UTR_coordinates.pl rna.asn;wget ftp://ftp.ncbi.nlm.nih.gov/genomes/H_sapiens/RNA/rna.fa.gz;gunzip rna.fa.gz`` (downloaded on July 19, 2018)

The resulting file ('CDS\_coord\_in\_rna.asn') contains coding sequence coordinates for each mRNA in 'rna.asn'.

Download of human 3´ UTRs from Ensembl (see 'Screenshot\_UTR\_download\_from\_Ensembl.png' for setting details); file saved as 'mart\_export.txt.gz'.

Download of microarray probeset/gene correspondance table: from NCBI's GEO [GPL3991](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GPL3991) platform description (table saved as 'GPL3991.dat').

Gene name synchronization between NCBI's 'rna.asn' file and microarray 'GPL3991.dat' file:

``sed '1,/^\*ID\*\*/ d' GPL3991.dat | grep -v '^[ \*]*$' | awk '{print $2}' > list_GPL;awk '{print $1}' CDS_coord_in_rna.asn > list_CDS;cat list_CDS list_GPL list_GPL | sort | uniq -c | awk '{print $1}' | sort -g | uniq -c``

Result: out of 23,653 RNAs in 'GPL3991.dat', 16,056 are also found in 'CDS\_coord\_in\_rna.asn', and the remaining 7,597 have to be identified:

``cat list_CDS list_GPL list_GPL | sort | uniq -c | grep '^ *2 ' | awk '{print $2}' > orphans;./Script_synchronizes_annotations.sh orphans;grep -c ' ' Synchronized_annotations_from_orphans``

Result: none of the orphans could be matched to an existing GenBank RNA (they all correspond to retired RNA sequences, which were removed for good reasons, or to weird EST's or "contig" sequences). We will therefore proceed with the old RNA names that were also found in 'CDS\_coord\_in\_rna.asn'.

#### Seed match count in human 3´ UTRs and graph plotting: ####

~~~~
for miRNA in miR-7 miR-9 miR-122a miR-128a miR-132 miR-133a miR-142 miR-148b miR-181a
do case "$miRNA" in "miR-7") seed=GGAAGAC;datasets='GSM210896 GSM210897';;
                    "miR-9") seed=CUUUGGU;datasets='GSM210898 GSM210899';;
                    "miR-122a") seed=GGAGUGU;datasets='GSM210900 GSM210901';;
                    "miR-128a") seed=CACAGUG;datasets='GSM210902 GSM210903';;
                    "miR-132") seed=AACAGUC;datasets='GSM210904 GSM210905';;
                    "miR-133a") seed=UUGGUCC;datasets='GSM210906 GSM210907';;
                    "miR-142") seed=AUAAAGU;datasets='GSM210908 GSM210909';;
                    "miR-148b") seed=CAGUGCA;datasets='GSM210910 GSM210911';;
                    "miR-181a") seed=ACAUUCA;datasets='GSM210912 GSM210913';;
   esac
   ./Module_seed_match_number.pl $seed > Seed_match_count_$miRNA'.dat'
   for dataset in `echo $datasets`
   do Rscript R_commands_array_signal_vs_seed_match_number $dataset'.dat' Seed_match_count_$miRNA'.dat'
   done
done
~~~~

## Comparison of Kolmogorov-Smirnov test and t-test (panel "b") ##

``R CMD BATCH R_commands_distribution_comparison``
