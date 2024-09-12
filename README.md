# ARVO-Meta

This repository contains metadata and usage instructions for the ARVO vulnerability dataset described in our paper [ARVO: Atlas of Reproducible Vulnerabilities for Open Source Software](https://arxiv.org/abs/2408.02153).

The code to generate the ARVO dataset will be published soon. The generated dataset and related metadata are updated in this repository. Each report file represents one found vulnerability on OSS-Fuzz.

# TL;DR

Run the following command to feed the proof-of-concept (POC) to a vulnerability found on [this page][3]. You should see an ASAN report for a heap overflow bug.

```bash
docker run -it n132/arvo:25402-vul arvo
```

<img width="842" alt="image" src="https://github.com/user-attachments/assets/7dfedc58-96af-4aa1-a620-beabb181bcc0">

# How to use ARVO

ARVO uses source metadata from OSS-Fuzz to solve reproducing problems and build a reproducible dataset: each vulnerability can be compiled from source at its vulnerable version, triggered using the PoC input found by the fuzzer, compiled at the patched version, and finally the patch can be verified by checking that the PoC input no longer triggers.

The [meta][0] folder includes metadata for all the recompilable vulnerabilities. You can find the original report on the [oss-fuzz issue tracker][1]. The patching commits are identified by ARVO, achieving over 80% correctness based on our evaluation. Additionally, we provide an interactive recompiling environment on our [Docker Hub Repository][2].

1. Select interesting vulnerabilities from the [meta][0] folder (e.g., [25402][7]).

2. Run a Docker container to create an interactive environment for these vulnerabilities:

```bash
docker run -it n132/arvo:25402-vul bash # vulnerable version
docker run -it n132/arvo:25402-fix bash # fixed version
```
3. [Optional] Modify the code or change the compile settings and recompile it:

```bash
# Run this command inside the Docker container
arvo compile
```
4. Feed the POC to the vulnerable/fixed binary to verify the vulnerability/fix:
```bash
# Run this command inside the Docker container
arvo
```
# Patches

In the [patches][5] folder, we provide the patches ARVO located for each vulnerability.

# Bug Report

If you find any cases that are not reproducible, please [open an issue][6] for the case.

# Known Issues

There is a reported bug that some cases can't be re-compiled for recent versions of Kernel (V5.15.0 was confirmed not affected). After investigation, it could be related to MSAN's improper parsing of `/proc/.*/maps` file. We suggest you try to turn off ASLR and re-compile. 

[0]: ./meta
[1]: https://bugs.chromium.org/p/oss-fuzz/issues/list
[2]: https://hub.docker.com/repository/docker/n132/arvo/general
[3]: https://bugs.chromium.org/p/oss-fuzz/issues/detail?id=25402&q=25402&can=2
[4]: https://x.com/moyix/status/1788943761352888777
[5]: ./patches
[6]: https://github.com/n132/ARVO-Meta/issues/new
[7]: ./meta/25402.json
