---
title: 使用集群上的 gpu
---

# 使用集群上的 gpu

## 提交任务至 gpu

在 `210.34.15.205` 上，可直接使用 lsf 为任务自动分配 gpu 资源，而不用指定 `CUDA_VISIBLE_DEVICES` 。

```bash
#!/bin/bash

#BSUB -q large
#BSUB -W 24:00
#BSUB -J test
#BSUB -o %J.stdout
#BSUB -e %J.stderr
#BSUB -n 1
#BSUB -R "rusage[ngpus_excl_p=1]"
```

> lsf 提交脚本需要含有 `BSUB -R` 选项，且不要包含 `export $CUDA_VISIBLE_DEVICES=`

比如，在 `210.34.15.205` 使用 dpgen 的提交脚本如下（目前 `large` 队列未设限制，若不设 walltime ，则可一直使用）：

```bash
#!/bin/bash

#BSUB -q large
#BSUB -J train
#BSUB -o %J.stdout
#BSUB -e %J.stderr
#BSUB -n 1
#BSUB -R "rusage[ngpus_excl_p=1]"

module add deepmd/1.0.2
dp train input.json > train.log
```
