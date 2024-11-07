# ONT-Nanopore-bioinformatics-analysis
16S_Barcoding  ITS_1_4
Basecalling avec guppy
guppy_basecaller -c dna_r9.4.1_XXXbps_XXX.cfg -i -o -s --detect_adapter --trim_adapters --min_qscore 7 --cpu_threads_per_caller 8

Contrôle qualité de FASTQ avec Nanoplot
nanoplot -t --fastq sample.fastq --outdir nanoplot_output/

Nettoyage par mappage : Supprimer les lectures de l’hôte avec minimap
minimap2 -ax map-ont reference_genome.fasta sample.fastq > sample_aligned.sam

assemblage de lecture avec flye ou SPAdes
flye --meta --nano-hq --sample.fastq -o out_flye spades.py -o assembly_outfile --careful -i fichier_échantillon

Affiliation taxonomique
blastn -query 16s_output.fasta -db silva_16s_db -outfmt 6 -out blast_results.tsv kaiju kaiju -t nodes.dmp -z 4 -f -db silva_16s_db -i sample.fastq -v -o kaiju_file.out

Annontation
busco -i .. /assembly/assembly.fasta -o my_anno -l bacteria_odb10 -m geno --config config/config. augustus --progress=true --strand=les deux --species=E_coli_K12 --AUGUSTUS_CONFIG_PATH=config .. / ˓→assembly/assembly.fasta > augustus.gff prokka --règne Bactéries --genre Escherichia --espèce coli --outdir annotation assembly/ ˓→assembly.fasta

Phylogénie
