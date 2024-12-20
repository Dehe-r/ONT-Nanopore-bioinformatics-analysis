# ONT-Nanopore-bioinformatics-analysis
Necrosis_16S  Necrosis_ITS
# Basecalling avec guppy
guppy_basecaller -i /projects/large/CIBIG_root/Necrosis_16S/fast5_pass -r -s Basecalling_work/ -c dna_r9.4.1_450bps_hac.cfg --min_qscore 8 --cpu_threads_per_caller 8 --detect_adapter --trim_adapters --detect_barcodes --compress_fastq --num_
callers 8 --chunks_per_runner 6
# Contrôle qualité de FASTQ avec Nanoplot
NanoPlot -t 10 --fastq /scratch/CIBIG_root/FASTQ/pass/barcode02/*.fastq.gz -o QC_16s/NANOPLOT_barcode02
# Nettoyage par mappage : Supprimer les lectures de l’hôte avec minimap
minimap2 -ax map-ont reference_genome.fasta sample.fastq > sample_aligned.sam
# assemblage de lecture avec canu
 canu -p necrosis_16S -d /scratch/CIBIG_root/ASS_barcode11/ genomeSize=1500 -nanopore-raw /scratch/CIBIG_root/FASTQ/pass/barcode11/*.fastq.gz maxThreads=10
# Affiliation taxonomique
blastn -query 16s_output.fasta -db silva_16s_db -outfmt 6 -out blast_results.tsv kaiju kaiju -t nodes.dmp -z 4 -f -db silva_16s_db -i sample.fastq -v -o kaiju_file.out
# Annotation
busco -i .. /assembly/assembly.fasta -o my_anno -l bacteria_odb10 -m geno --config config/config. augustus --progress=true --strand=les deux --species=E_coli_K12 --AUGUSTUS_CONFIG_PATH=config .. / ˓→assembly/assembly.fasta > augustus.gff prokka --règne Bactéries --genre Escherichia --espèce coli --outdir annotation assembly/ ˓→assembly.fasta
# Phylogénie
