# 2026-09-25 notebook on using separate dockers



## test if the images are available

on mustard

```
docker pull 

cutadapt_quay_loc=quay.io/ucsc_cgl/cutadapt:1.9--6bd44edd2b8f8f17e25c5a268fedaab65fa851d2
star_quay_loc=quay.io/ucsc_cgl/star:2.4.2a--bcbd5122b69ff6ac4ef61958e47bde94001cfe80
rsem_quay_loc=quay.io/ucsc_cgl/rsem:1.2.25--d4275175cc8df36967db460b06337a14f40d2f21
```



```bash
for loc in $cutadapt_quay_loc $star_quay_loc $rsem_quay_loc; do
echo $loc
docker pull $loc
done
```

 success

## Docker version

```bash
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ docker version --format '{{.Server.Version}}'
24.0.7

```



## make backup copies of images

## ucsc_cgl_rsem_1.2.25

```bash
cd /private/groups/treehouse/archive/Dockers

IMG=quay.io/ucsc_cgl/rsem:1.2.25--d4275175cc8df36967db460b06337a14f40d2f21
docker pull $IMG
docker inspect --format '{{index .RepoDigests 0}}' $IMG   # record the registry digest
docker save $IMG | gzip > ucsc_cgl_rsem_1.2.25.tar.gz
md5sum ucsc_cgl_rsem_1.2.25.tar.gz > ucsc_cgl_rsem_1.2.25.tar.gz.md5
```



std out

```bash
hcbeale@mustard:/private/groups/treehouse/archive$ IMG=quay.io/ucsc_cgl/rsem:1.2.25--d4275175cc8df36967db460b06337a14f40d2f21
hcbeale@mustard:/private/groups/treehouse/archive$ docker pull $IMG
1.2.25--d4275175cc8df36967db460b06337a14f40d2f21: Pulling from ucsc_cgl/rsem
[DEPRECATION NOTICE] Docker Image Format v1, and Docker Image manifest version 2, schema 1 support will be removed in an upcoming release. Suggest the author of quay.io/ucsc_cgl/rsem:1.2.25--d4275175cc8df36967db460b06337a14f40d2f21 to upgrade the image to the OCI Format, or Docker Image manifest v2, schema 2. More information at https://docs.docker.com/go/deprecated-image-specs/
53d2b72791c7: Already exists 
7887ca1c8ca7: Already exists 
6c04774cbdcc: Already exists 
a3ed95caeb02: Already exists 
b04a8c96ae2e: Already exists 
4bfd79acf4bc: Already exists 
4ec1cc307d03: Already exists 
58e5ba7ebde0: Already exists 
67b053239aab: Already exists 
db5047d305ac: Already exists 
f33cbfbd88fc: Already exists 
Digest: sha256:0b4c77d78199c4bd3569d31c10d82edb5d998287456083c95da0df6f5c64fd48
Status: Image is up to date for quay.io/ucsc_cgl/rsem:1.2.25--d4275175cc8df36967db460b06337a14f40d2f21
quay.io/ucsc_cgl/rsem:1.2.25--d4275175cc8df36967db460b06337a14f40d2f21
hcbeale@mustard:/private/groups/treehouse/archive$ docker inspect --format '{{index .RepoDigests 0}}' $IMG   # record the registry digest
quay.io/ucsc_cgl/rsem@sha256:0b4c77d78199c4bd3569d31c10d82edb5d998287456083c95da0df6f5c64fd48
hcbeale@mustard:/private/groups/treehouse/archive$ docker save $IMG | gzip > ucsc_cgl_rsem_1.2.25.tar.gz
hcbeale@mustard:/private/groups/treehouse/archive$ md5sum ucsc_cgl_rsem_1.2.25.tar.gz > ucsc_cgl_rsem_1.2.25.tar.gz.md5
hcbeale@mustard:/private/groups/treehouse/archive$ 
hcbeale@mustard:/private/groups/treehouse/archive$ ls
compendium  downstream  primary   references                   ucsc_cgl_rsem_1.2.25.tar.gz.md5
Dockers     metadata    projects  ucsc_cgl_rsem_1.2.25.tar.gz
hcbeale@mustard:/private/groups/treehouse/archive$ mv ucsc_cgl_rsem_1.2.25.tar.gz ucsc_cgl_rsem_1.2.25.tar.gz.md5  Dockers/

```



```bash
nano ucsc_cgl_rsem_1.2.25.README.txt
```



copy "The ucsc_cgl_rsem_1.2.25* files were created on Sept 28 2026 as follows", previous commands and outputs into README



## ucsc_cgl_star_2.4.2a

```bash
cd /private/groups/treehouse/archive/Dockers

IMG=quay.io/ucsc_cgl/star:2.4.2a--bcbd5122b69ff6ac4ef61958e47bde94001cfe80
prefix=ucsc_cgl_star_2.4.2a
docker pull $IMG
docker inspect --format '{{index .RepoDigests 0}}' $IMG   # record the registry digest
docker save $IMG | gzip > ${prefix}.tar.gz
md5sum ${prefix}.tar.gz > ${prefix}.tar.gz.md5

```



std out

```bash
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ cd /private/groups/treehouse/archive/Dockers
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ 
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ IMG=quay.io/ucsc_cgl/star:2.4.2a--bcbd5122b69ff6ac4ef61958e47bde94001cfe80
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ prefix=ucsc_cgl_star_2.4.2a
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ docker pull $IMG
2.4.2a--bcbd5122b69ff6ac4ef61958e47bde94001cfe80: Pulling from ucsc_cgl/star
[DEPRECATION NOTICE] Docker Image Format v1, and Docker Image manifest version 2, schema 1 support will be removed in an upcoming release. Suggest the author of quay.io/ucsc_cgl/star:2.4.2a--bcbd5122b69ff6ac4ef61958e47bde94001cfe80 to upgrade the image to the OCI Format, or Docker Image manifest v2, schema 2. More information at https://docs.docker.com/go/deprecated-image-specs/
53d2b72791c7: Already exists 
7887ca1c8ca7: Already exists 
6c04774cbdcc: Already exists 
a3ed95caeb02: Already exists 
4048fffc34d7: Already exists 
24f52ca496bc: Already exists 
fc47e88c6eeb: Already exists 
941750c00fd1: Already exists 
7f3d2bbf9180: Already exists 
ba61db1cb251: Already exists 
e4a4deca36a4: Already exists 
923971eafce8: Already exists 
e78dd6244e18: Already exists 
2b6e2809ef0b: Already exists 
8498c7f75b4e: Already exists 
Digest: sha256:76340fed87832a3a56c62ae47b7dca13dd92ab8cff81fb89af49ffa55ee9cc04
Status: Image is up to date for quay.io/ucsc_cgl/star:2.4.2a--bcbd5122b69ff6ac4ef61958e47bde94001cfe80
quay.io/ucsc_cgl/star:2.4.2a--bcbd5122b69ff6ac4ef61958e47bde94001cfe80
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ docker inspect --format '{{index .RepoDigests 0}}' $IMG   # record the registry digest
quay.io/ucsc_cgl/star@sha256:76340fed87832a3a56c62ae47b7dca13dd92ab8cff81fb89af49ffa55ee9cc04
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ docker save $IMG | gzip > ${prefix}.tar.gz
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ md5sum ${prefix}.tar.gz > ${prefix}.tar.gz.md5
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ 

```



```bash
nano ${prefix}.README.txt
```



copy "The [prefix]* files were created on Sept 28 2026 as follows", previous commands and outputs into README





## ucsc_cgl_rsem

```bash
cd /private/groups/treehouse/archive/Dockers

IMG=quay.io/ucsc_cgl/rsem:1.2.25--d4275175cc8df36967db460b06337a14f40d2f21
prefix=ucsc_cgl_rsem_1.2.25
docker pull $IMG
docker inspect --format '{{index .RepoDigests 0}}' $IMG   # record the registry digest
docker save $IMG | gzip > ${prefix}.tar.gz
md5sum ${prefix}.tar.gz > ${prefix}.tar.gz.md5

```



std out

```bash
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ cd /private/groups/treehouse/archive/Dockers
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ 
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ IMG=quay.io/ucsc_cgl/rsem:1.2.25--d4275175cc8df36967db460b06337a14f40d2f21
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ prefix=ucsc_cgl_rsem_1.2.25
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ docker pull $IMG
1.2.25--d4275175cc8df36967db460b06337a14f40d2f21: Pulling from ucsc_cgl/rsem
[DEPRECATION NOTICE] Docker Image Format v1, and Docker Image manifest version 2, schema 1 support will be removed in an upcoming release. Suggest the author of quay.io/ucsc_cgl/rsem:1.2.25--d4275175cc8df36967db460b06337a14f40d2f21 to upgrade the image to the OCI Format, or Docker Image manifest v2, schema 2. More information at https://docs.docker.com/go/deprecated-image-specs/
53d2b72791c7: Already exists 
7887ca1c8ca7: Already exists 
6c04774cbdcc: Already exists 
a3ed95caeb02: Already exists 
b04a8c96ae2e: Already exists 
4bfd79acf4bc: Already exists 
4ec1cc307d03: Already exists 
58e5ba7ebde0: Already exists 
67b053239aab: Already exists 
db5047d305ac: Already exists 
f33cbfbd88fc: Already exists 
Digest: sha256:0b4c77d78199c4bd3569d31c10d82edb5d998287456083c95da0df6f5c64fd48
Status: Image is up to date for quay.io/ucsc_cgl/rsem:1.2.25--d4275175cc8df36967db460b06337a14f40d2f21
quay.io/ucsc_cgl/rsem:1.2.25--d4275175cc8df36967db460b06337a14f40d2f21
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ docker inspect --format '{{index .RepoDigests 0}}' $IMG   # record the registry digest
quay.io/ucsc_cgl/rsem@sha256:0b4c77d78199c4bd3569d31c10d82edb5d998287456083c95da0df6f5c64fd48
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ docker save $IMG | gzip > ${prefix}.tar.gz
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ md5sum ${prefix}.tar.gz > ${prefix}.tar.gz.md5
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ 
```



```bash
nano ${prefix}.README.txt
```

copy "The [prefix]* files were created on Sept 28 2026 as follows", previous commands and outputs into README



"ucsc_cgl_rsem_1.2.25"



# note on schema versions

Claude says 

```bash
tar -tzf ucsc_cgl_star_2.4.2a.tar.gz | grep -E '^(index.json|oci-layout|manifest.json)$'
```

```bash
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ tar -tzf ucsc_cgl_star_2.4.2a.tar.gz | grep -E '^(index.json|oci-layout|manifest.json)$'
manifest.json
hcbeale@mustard:/private/groups/treehouse/archive/Dockers$ 
```

If `index.json` and `oci-layout` are listed, it's an OCI archive. If only `manifest.json` is listed, it's the older Docker layout. That's still fine for `docker load`



