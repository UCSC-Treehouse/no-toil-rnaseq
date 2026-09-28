repo: https://github.com/UCSC-Treehouse/no-toil-rnaseq/tree/master



server: mustard



```bash
cd /scratch/hbeale/no-toil-rnaseq-test
git clone https://github.com/UCSC-Treehouse/no-toil-rnaseq
```



build docker

```bash
release_version=1
image_base_name=hbeale/no-toil-rnaseq

cd /scratch/hbeale/no-toil-rnaseq-test/no-toil-rnaseq
docker build -t ${image_base_name}:${release_version} .

```



## get references 

### chr6 approach failed; files are no longer on courtyard



```bash
cd /scratch/hbeale/no-toil-rnaseq-test/data
wget http://courtyard.gi.ucsc.edu/~jvivian/toil-rnaseq-inputs/continuous_integration/rsem_ref_chr6.tar.gz
wget http://courtyard.gi.ucsc.edu/~jvivian/toil-rnaseq-inputs/continuous_integration/starIndex_chr6.tar.gz
tar -xzvf rsem_ref_chr6.tar.gz
tar -xzvf starIndex_chr6.tar.gz

https://public.gi.ucsc.edu/~jvivian/toil-rnaseq-inputs/continuous_integration/rsem_ref_chr6.tar.gz # also failed
```



### get from central mustard loc

```bash
STAR_fusion_refs=/private/groups/treehouse/archive/references/STARFusion-GRCh38gencode23
# STAR_fusion_refs are already uncompressed

# uncompressed rsem_refs
cd /scratch/hbeale/no-toil-rnaseq-test/ref
cp /private/groups/treehouse/archive/references/rsem_ref_hg38_no_alt.tar.gz
tar -xzf rsem_ref_hg38_no_alt.tar.gz



```

that was a mistake to use star fusion refs



```bash

# uncompressed star refs
cd /scratch/hbeale/no-toil-rnaseq-test/ref
cp /private/groups/treehouse/archive/references/starIndex_hg38_no_alt.tar.gz .
tar -xzf starIndex_hg38_no_alt.tar.gz

```



## get seq data

```bash
cd /scratch/hbeale/no-toil-rnaseq-test/data
wget https://public.gi.ucsc.edu/~hcbeale//THR13_2673_S01/SU-DIPG_19_S5_L001_R1_001.fastq.gz
wget https://public.gi.ucsc.edu/~hcbeale//THR13_2673_S01/SU-DIPG_19_S5_L001_R2_001.fastq.gz

```



## run expr - attempt 1

```bash
base_dir=/scratch/hbeale/no-toil-rnaseq-test/

docker run --rm \
-v ${base_dir}/data:/data \
-v ${STAR_fusion_refs}:/data/starIndex \
-v ${base_dir}/ref/rsem_ref_hg38:/data/rsem_ref_hg38 \
${image_base_name}:${release_version} \
/data/SU-DIPG_19_S5_L001_R1_001.fastq.gz /data/SU-DIPG_19_S5_L001_R2_001.fastq.gz

```

hm, it's going very slowly.



consider trying in parallel with the tiny files

## run expr - attempt 2

using tiny files

```bash
cd /scratch/hbeale/no-toil-rnaseq-test/
git clone https://github.com/UCSC-Treehouse/pipelines.git

cd /scratch/hbeale/no-toil-rnaseq-test/data
cp ../pipelines/samples/*fastq.gz .

```



### run expr

```bash
base_dir=/scratch/hbeale/no-toil-rnaseq-test/

run_version=2
this_run_data_dir=${base_dir}/run${run_version}
mkdir -p $this_run_data_dir/star
fq1_name=TEST_R1.fastq.gz
fq2_name=TEST_R2.fastq.gz
cp ${base_dir}/data/$fq1_name $this_run_data_dir
cp ${base_dir}/data/$fq2_name $this_run_data_dir

STAR_refs=${base_dir}/ref/starIndex
rsem_ref_hg38_dir=${base_dir}/ref/rsem_ref_hg38

docker run --rm \
-v ${this_run_data_dir}:/data \
-v ${STAR_refs}:/data/starIndex \
-v ${rsem_ref_hg38_dir}:/data/rsem_ref_hg38 \
${image_base_name}:${release_version} \
/data/$fq1_name /data/$fq2_name

```

output files

```bash
hcbeale@mustard:/scratch/hbeale/no-toil-rnaseq-test/run2$ ls -alth
total 26M
drwxr-xr-x 6 hcbeale prismuser 4.0K Sep 24 16:52 .
-rw-r--r-- 1 root    root      1.6M Sep 24 16:52 rsem.transcript.sorted.bam.bai
-rw-r--r-- 1 root    root      1.9M Sep 24 16:52 rsem.transcript.sorted.bam
-rw-r--r-- 1 root    root      6.2M Sep 24 16:52 rsem.genes.results
-rw-r--r-- 1 root    root       13M Sep 24 16:52 rsem.isoforms.results
-rw-r--r-- 1 root    root      1.9M Sep 24 16:52 rsem.transcript.bam
drwxr-xr-x 2 root    root        74 Sep 24 16:52 rsem.stat
drwxr-xr-x 2 hcbeale prismuser  174 Sep 24 16:52 star
-rw-r--r-- 1 root    root      647K Sep 24 16:51 R1_cutadapt.fastq
-rw-r--r-- 1 root    root      647K Sep 24 16:51 R2_cutadapt.fastq
drwxr-xr-x 2 root    root        10 Sep 24 16:40 rsem_ref_hg38
drwxr-xr-x 2 root    root        10 Sep 24 16:40 starIndex
-rw-r--r-- 1 hcbeale prismuser 226K Sep 24 16:37 TEST_R2.fastq.gz
-rw-r--r-- 1 hcbeale prismuser 227K Sep 24 16:37 TEST_R1.fastq.gz
drwxr-xr-x 7 hcbeale prismuser  104 Sep 24 16:32 ..


```

gene expression



```bash
cat rsem.genes.results  | cut -f1,4,6 | grep -v 0.00$ | head

```

```bash
gene_id effective_length        TPM
ENSG00000008128.22      491.18  4963.10
ENSG00000011275.18      3056.48 768.37
ENSG00000067225.17      1626.48 660.91
ENSG00000101608.12      613.26  21749.26
ENSG00000104824.16      2234.48 481.08
ENSG00000108298.9       453.48  7111.46
ENSG00000109332.19      310.49  20772.98
ENSG00000116580.18      1647.48 3341.29
ENSG00000118680.12      788.48  29437.20
```

