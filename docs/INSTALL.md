# Software installation

CUT&RUNTools 2.0 requires **Python** (3.6), **R** (3.6), **Java** and **Perl**. Installation also requires **GCC** to compile some C-based source code. Additionally, the following required tools should be already installed before running the setup.   

## Prerequisites

**Part 1.**  
CUT&RUNTools 2.0 may work with different version of each tool, in the bracket is the version we tested.

* Bowtie2 (2.2.9) [link](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml)
* Samtools (1.8) [link](http://samtools.sourceforge.net/)
* MACS2 (2.1.1) [link](https://github.com/taoliu/MACS)
* MEME (4.12.0) [link](http://meme-suite.org/tools/meme)
* Bedops (2.4.30) [link](https://bedops.readthedocs.io/en/latest/)
* Bedtools (2.26.0) [link](https://bedtools.readthedocs.io/en/latest/)
* Deeptools (3.5.0) [link](https://deeptools.readthedocs.io/en/develop/)
* GNU parallel (20200522) [link](https://www.gnu.org/software/parallel/)
* tabix (0.2.5) [link](https://davetang.org/muse/2013/02/22/using-tabix/)


We recommend the user to install and manage the software using [conda system](https://docs.conda.io/en/latest/) which could incorporate the dependencies for each software. 

```
conda install -c bioconda bowtie2
conda install -c bioconda samtools
conda install -c bioinfo macs2
conda install -c bioconda/label/cf201901 meme
conda install -c bioconda bedops
conda install -c bioconda bedtools
conda install -c bioconda deeptools
conda install -c conda-forge parallel
conda install -c bioconda tabix
```  

On a Linux system, paths of software (e.g. /home/fyu/anaconda2/envs) from the anaconda environment might be obtained in the following:

```
conda info -e
# conda environments:
#
base                     *  /home/fyu/anaconda2/envs
bedGraphToBigWig            /home/fyu/anaconda2/envs/bedGraphToBigWig
bedops                      /home/fyu/anaconda2/envs/bedops
bedtools                    /home/fyu/anaconda2/envs/bedtools
bowtie2                     /home/fyu/anaconda2/envs/bowtie2
deeptools                   /home/fyu/anaconda2/envs/deeptools
macs2                       /home/fyu/anaconda2/envs/macs2
samtools                    /home/fyu/anaconda2/envs/samtools
```

**Part 2.**   
R packages including reticulate ("1.15"), leiden ("0.3.3"), data.table ("1.11.6"), Matrix ("1.2.18"), irlba ("2.3.3"), Rtsne ("0.15"), RANN ("2.6.1"), igraph ("1.2.4.2"), uwot ("0.1.8"), rGREAT ("1.14.0"), ggplot2 ("3.3.0"), CENTIPEDE ("1.2") need to be installed in the library of R you specified in the JSON configuration file. The user can install these R packages automatically with the script ***r-pkgs-install.r*** provided by the package.

**Part 3.**  
Three python modules (umap, leidenalg, and igraph) are required to be installed on your system.  
To install them  

```
pip install umap-learn leidenalg igraph
```  

To test if the package is installed successfully, for example:

```
pip list | grep umap
```

**Part 4.**  

We also provided special notes for **Atactk** and **kseq**, the installation files were already included in the packages.

Two patches of [`make_cut_matrix.patch`](make_cut_matrix.patch) and [`metrics.py.patch`](metrics.py.patch) for Atactk [(link)](https://github.com/ParkerLab/atactk) were provided to accurately estimate the cut frequency at single-base resolution. Install the patched version of the package by:

```
source atactk.install.sh
```
This will use pip to install the patched Atactk to the user's home directory (~/.local/bin).

Another software `kseq` for a special trimmer we wrote, which can further trim the reads by 6 nt to get rid of the problem of possible adapter run-through. To install:

```
source make_kseq_test.sh
```

**Part 5.**  
We provided script or method to download the reference genome and bowtie2 indexes. The genome sequence of a specific organism build (such as hg19, hg38) is required for genome alignment and motif discovery. We provide a script [`assemblies.install`](assemblies.install) to download this automatically from UCSC. We specifically require repeat-**masked** version of genome sequence file. Two parameters were needed to be specified by the user, genome assembly (hg38, hg19, mm10 or mm9) and the path of software bedtools. 

```
chmod +x assemblies.install.sh
assemblies.install.sh hg19 /path/to/bedtools
```
To download proper indexes of bowtie2, just use the either the downloads on the [Bowtie2 homepage] (http://bowtie-bio.sourceforge.net/bowtie2/index.shtml) or the [Illumina iGenomes] (https://support.illumina.com/sequencing/sequencing_software/igenome.html).  

**Part 6.**  
In this part, some necessary tools and data were already contained in CUT&RUNTools, so no installation is required.  
* SEACR (1.3) [link](https://github.com/FredHutch/SEACR)  
* picard (0.1.8) [link](http://broadinstitute.github.io/picard/command-line-overview.html)  
* trimmomatic (0.36) [link](https://github.com/timflutre/trimmomatic)  

Files frequently used which were already contained in CUT&RUNTools 2.0:
* genome size files (`assemblies` folder contains genome size files)
* blacklist regions (`blacklist` folder contains blacklist sequences)
* adaptor files (`adapters` folder contains Illumina Truseq3-PE adapter sequences)
* example fastq data (`exampleData` folder contains example data for the data analysis)


## Configuration file

The configuration file tells CutRunTools where to locate the prerequisite tools. This is a [JSON](http://www.json.org/) file. The sample JSON files `bulk-config.json` and `sc-config.json` for are provided below for bulk- and single-cell data processing and analysis, respectively. Filling in the information should be pretty easy: in most cases we need to provide the path to the `bin` directory of each tool.

**The sample JSON file of `bulk-config.json`**

```json
{
    "software_config": {
        "Rscriptbin": "/path/to/R/3.3.3/bin", 
        "pythonbin": "/path/to/python/2.7.12/bin", 
        "perlbin": "/path/to/perl/5.24.0/bin",
        "javabin": "/path/to/java/jdk-1.8u112/bin",
        "bowtie2bin": "/path/to/bowtie2/2.2.9/bin", 
        "samtoolsbin": "/path/to/samtools/1.3.1/bin", 
        "macs2bin": "/path/to/macs2/2.1.1.20160309/bin", 
        "memebin": "/home/user/meme/bin", 
        "bedopsbin": "/path/to/bedops/2.4.30", 
        "bedtoolsbin": "/path/to/bedtools/2.26.0/bin", 
        "path_deeptools": "/path/to/deeptools",
        "bt2idx": "/n/groups/bowtie2_indexes", 
        "genome_sequence": "/home/user/chrom.hg19/hg19.fa", 
        "spike_in_bt2idx": "/n/groups/ecoli/bowtie2_indexes", 
        "spike_in_sequence": "/home/user/chrom.ecoli/ecoli6.fa", 
        "extratoolsbin": "/path/to/cutrun_pipeline2.0", 
        "extrasettings": "/path/to/cutrun_pipeline2.0",
        "kseqbin": "/path/to/cutrun_pipeline2.0", 
        "adapterpath": "/path/to/cutrun_pipeline2.0/adapters", 
        "trimmomaticbin": "/path/to/cutrun_pipeline2.0", 
        "picardbin": "/path/to/cutrun_pipeline2.0", 
        "picardjarfile": "picard-2.8.0.jar", 
        "trimmomaticjarfile": "trimmomatic-0.36.jar", 
        "makecutmatrixbin": "/home/user/.local/bin"
    }, 
    "input_output": {
        "fastq_directory": "/n/scratch2/user/Nan_18_demo/sorted.chr11", 
        "workdir": "/n/scratch2/user/workdir", 
        "fastq_sequence_length": 42, 
        "organism_build": "hg19",
        "spike_in_align": "FALSE",
        "spike_in_norm": "FALSE",
        "spikein_scale": "10000",
        "frag_120": "TRUE",
        "peak_caller": "macs2",
        "dup_peak_calling": "FALSE",
        "cores": "8",
        "experiment_type": "CUT&Tag"
    }, 
    "motif_finding": {
        "num_bp_from_summit": 150, 
        "num_peaks": 1000, 
        "total_peaks": 5000, 
        "motif_scanning_pval": 0.0005, 
        "num_motifs": 10
    }
}

```

The `software_config` section (the first 24 lines) concerns the software installation, all the requirements of software can be defined here. The rest is related to an actual analysis (explained in [USAGE.md](USAGE.md)).


**The sample JSON file of `sc-config.json`** 

```json
{
	"software_config": {
        "Rscriptbin": "/path/to/R/bin",
		"pythonbin": "/path/to/python/bin",
		"perlbin": "/path/to/perl/bin",
		"javabin": "/path/to/java/bin",
		"trimmomaticbin": "/path/to/trimmomatic/bin",
		"trimmomaticjarfile": "trimmomatic-0.36.jar",
		"bowtie2bin": "/path/to/bowtie2/bin",
		"samtoolsbin": "/path/to/samtools/bin",
		"adapterpath": "/path/to/cutrun_pipeline/adapters", 
		"picardbin": "/path/to/picard/bin",
		"picardjarfile": "picard-2.8.0.jar",
		"macs2bin": "/path/to/macs2/bin",
		"macs2pythonlib": "/path/to/macs2/2.1.1.20160309/lib/python2.7/site-packages",
		"kseqbin": "/path/to/cutrun_pipeline", 
		"memebin": "/path/to/meme/bin", 
		"bedopsbin": "/path/to/bedops/bin", 
		"bedtoolsbin": "/path/to/bedtools/bin",
		"makecutmatrixbin": "/home/user/.local/bin",
		"bt2idx": "/path/to/bowtie2_indexes",
		"genome_sequence": "/path/to/chrom.hg19/hg19.fa",
		"extratoolsbin": "/home/user/cutrun_pipeline", 
		"path_parallel": "/path/to/parallel/bin", 
		"path_deeptools": "/path/to/deeptools/bin",
		"path_tabix": "/path/to/tabix/bin", 
	},
	"sc_parameters": {
		"single_cell": "TRUE", 
		"fastq_directory": "/path/to/fastq", 
		"workdir": "/path/to/workdir", 
		"genome": "hg38", 
		"chrome_sizes_file": "/path/to/hg38.chrom.sizes",
		"cores": "8", 
		"percentage_rip": "30", 
		"num_reads_threshold": "10000", 
		"peak_caller": "macs2", 	
		"peak_type": "narrow", 
		"matrix_type": "peak_by_cell", 
		"bin_size": "5000", 
		"feature_file": "/path/to/feature_file", 
		"experiment_type": "CUT&Tag", 
		"cluster_resolution": "0.8", 
		"cluster_pc": "30", 
		"experiment_name": "scCUT&Tag", 
	},
	"run_pipeline": {
		"entire_pipeline": "TRUE", 
		"individual_step": "NULL", 
		"step2_bamfile_dir": "$workdir/sc_aligned.aug10/dup.marked.clean", 
		"step2_output_dir": "$workdir/sc_countMatrix", 
		"step2_qc_pass_file": "$workdir/sc_qc/report/statistics_QCpassed.txt", 
		"step3_count_matrix": "$workdir/sc_countMatrix/feature-by-cell_matrix.txt", 
		"step3_output_dir": "$workdir/sc_cluster", 
		"step4_bamfile_dir": "$workdir/sc_aligned.aug10/dup.marked.clean", 
		"step4_cell_anno_file": "$workdir/sc_cluster/leiden_cluster_annotation.txt", 
		"step4_output_dir": "$workdir/sc_pseudoBulk", 
	}
}
```

The `software_config` section (the first 24 lines) concerns the software installation.
Similar to the configure.json of the bulk data processing, all the requirements of software can be defined here. Three more paths (path_parallel, path_deeptools and path_tabix) should be specified compared to the bulk configure.json file. The rest is related to an actual analysis (explained in [USAGE.md](USAGE.md)).



## Validate prerequisites

Briefly, a user writes a `bulk-config.json` or `sc-config.json` configuration file for a new analysis. Then, CutRunTools 2.0 can be run directly from the directory containing the CutRunTools scripts. To check if the paths are correct and if the softwares in these paths indeed work:

```
./validate.py bulk-config.json --ignore-input-output --software
```

OR

```
./validate.py sc-config.json --ignore-input-output --software
```

This script checks that your configuration file is correct and all paths are correct. You will get an empty line if the validate.py script runs without errors.







