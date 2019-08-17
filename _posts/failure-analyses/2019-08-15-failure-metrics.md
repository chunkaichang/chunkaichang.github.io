---
layout: single
title:  "Funtional Safety: Fault Types and Failure Metrics"
date:   2019-08-15 21:36:00 -0500
mathjax: true
categories: autonomous-vehicles
tags:
  - functional safety
  - fault injection
---

## Fault Types
- Safe faults: faults that are masked within the affected safety-related module
- Single-point faults: faults that affect one element and corrupt the ouput of the affected module
- Residual faults: single-point faults that escape detection of safety mechanism(s)
- Multi-point faults: faults that affect multiple elements 
- Latent faults: multi-point faults that are neither detected by safety mechanisms nor perceived by the driver

See [this][fault-types] slide deck for visualization of different fault types and [this][fault-classfy] site for classification of faults.

## Probabilistic Metric for Hardware Failure (PMHF)
In ISO 26262, PMHF is defined as the average failure rate per hour for an item during its operational lifetime.
It can be expressed in the FIT unit (i.e., failures per one billion hours).
For instance, ASIL D requires PMHF below $$10^{-8}$$/hr, which equals to 10 FIT.

## Single-Point Failure Metric (SPFM)
SPFM shows the effectiveness of the safety mechanisms against single-point faults.
<center> $$SPFM = 1 - \frac{\sum (\lambda_{spf} + \lambda_{rf})}{\sum \lambda} = \frac{\sum (\lambda_{mpf} + \lambda_{s})}{\sum \lambda} $$ </center>

where $$\lambda_{spf}$$ is the single-point fault rate, $$\lambda_{rf}$$ is the residual fault rate, $$\lambda_{mpf}$$ is the multi-point fault rate, $$\lambda_{s}$$ is the safe fault rate, and $$\lambda$$ is the total fault rate.

ASIL B, ASIL C, and ASILD requires SPFM of at least 90%, 97%, and 99%, respectively.
 
## Latent Failure Metric (LFM)
LFM shows the effectiveness of the safety mechanisms against latent faults.
<center> $$LFM = 1 - \frac{\sum \lambda_{lf}}{\sum \lambda}$$ </center>

where $$\lambda_{lf}$$ is the latent fault rate.

ASIL B, ASIL C, and ASILD requires LFM of at least 60%, 80%, and 90%, respectively.

[fault-types]: https://www.testandverification.com/wp-content/uploads/2018/DVClub_Europe/Sergio_Marchese-OneSpin.pdf
[fault-classfy]: https://www.e-devices.ricoh.co.jp/en/technology/automotive/fault.html
