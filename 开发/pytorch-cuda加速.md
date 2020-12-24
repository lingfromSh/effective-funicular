---
title: "Pytorch CUDA加速"
tags: ""
---

# Pytorch CUDA加速

## 环境

pytorch==1.7.0

python3.8

## 代码

```python
import torch
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
```
