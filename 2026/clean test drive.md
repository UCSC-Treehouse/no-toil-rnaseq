# Clean test drive



## goal

have a simple reproducible example of how the expression pipeline runs without toil





## context

repo: https://github.com/UCSC-Treehouse/no-toil-rnaseq/tree/master



server: mustard

## remove previous attempts:

```bash
rm -fr /scratch/hbeale/no-toil-rnaseq-test
```



## clone code

```bash
base_dir=/scratch/hbeale/no-toil-rnaseq-test
mkdir $base_dir/
cd $base_dir
git clone https://github.com/UCSC-Treehouse/no-toil-rnaseq
```



##  build docker

```bash
release_version=2026.09.24_17.03.28
image_base_name=hbeale/no-toil-rnaseq

cd /scratch/hbeale/no-toil-rnaseq-test/no-toil-rnaseq
docker build -t ${image_base_name}:${release_version} .

```



## get references from central mustard loc



```bash

#  rsem_refs
mkdir $base_dir/ref
cd $base_dir/ref
cp /private/groups/treehouse/archive/references/rsem_ref_hg38_no_alt.tar.gz .
tar -xzf rsem_ref_hg38_no_alt.tar.gz


#  star refs
cp /private/groups/treehouse/archive/references/starIndex_hg38_no_alt.tar.gz .
tar -xzf starIndex_hg38_no_alt.tar.gz

```



## get seq data

```bash
mkdir $base_dir/data

cd $base_dir
git clone https://github.com/UCSC-Treehouse/pipelines.git

cd $base_dir/data
cp $base_dir/pipelines/samples/*fastq.gz .

```



## run expr

```bash

run_version=$release_version
this_run_data_dir=${base_dir}/output${run_version}
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
ls -alth $this_run_data_dir
```



```bash
hcbeale@mustard:/scratch/hbeale/no-toil-rnaseq-test/data$ ls -alth $this_run_data_dir
total 26M
drwxr-xr-x 6 hcbeale prismuser 4.0K Sep 24 17:15 .
-rw-r--r-- 1 root    root      1.6M Sep 24 17:15 rsem.transcript.sorted.bam.bai
-rw-r--r-- 1 root    root      1.9M Sep 24 17:15 rsem.transcript.sorted.bam
-rw-r--r-- 1 root    root      6.2M Sep 24 17:15 rsem.genes.results
-rw-r--r-- 1 root    root       13M Sep 24 17:15 rsem.isoforms.results
-rw-r--r-- 1 root    root      1.9M Sep 24 17:15 rsem.transcript.bam
drwxr-xr-x 2 root    root        74 Sep 24 17:15 rsem.stat
drwxr-xr-x 2 hcbeale prismuser  174 Sep 24 17:15 star
-rw-r--r-- 1 root    root      647K Sep 24 17:14 R1_cutadapt.fastq
-rw-r--r-- 1 root    root      647K Sep 24 17:14 R2_cutadapt.fastq
drwxr-xr-x 2 root    root        10 Sep 24 17:14 rsem_ref_hg38
drwxr-xr-x 2 root    root        10 Sep 24 17:14 starIndex
-rw-r--r-- 1 hcbeale prismuser 226K Sep 24 17:14 TEST_R2.fastq.gz
-rw-r--r-- 1 hcbeale prismuser 227K Sep 24 17:14 TEST_R1.fastq.gz
drwxr-xr-x 8 hcbeale prismuser  141 Sep 24 17:14 ..


```

gene expression



```bash
cat $this_run_data_dir/rsem.genes.results  | cut -f1,4,6 | grep -v 0.00$ | head

```

```bash
hcbeale@mustard:/scratch/hbeale/no-toil-rnaseq-test/data$ cat $this_run_data_dir/rsem.genes.results  | cut -f1,4,6 | grep -v 0.00$ | head
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
hcbeale@mustard:/scratch/hbeale/no-toil-rnaseq-test/data$ 
```

