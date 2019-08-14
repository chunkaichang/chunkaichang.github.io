---
layout: single
title:  "Failure Analyses: FMEA, FMEDA, FTA, and DFA"
date:   2019-08-13 22:11:00 -0500
categories: autonomous-vehicles
tags:
  - functional safety
  - fault injection
---

I was perplexed by terminology used in the functional safety literature, including FMEA, FMEDA, FTA, and DFA.
The goal of this post is to draw the relationships between different failure analyses.

## FMEA (Failure Mode and Effect Analysis)
FMEA is used to identify the failure modes (e.g., stuck-at faults and single-event upset) 
and their consequences (e.g., incorrect addition results, incorrect control flow, etc.) for modules in a system. 
The output is a FMEA table which can be used to derive the most vulnerable module and help design of safety mechanisms. 

## FMEDA (Failure Mode Effect and Diagnostic Analysis)
FMEDA is an extension of FMEA. In addition to identifying failure modes and their effects, the diagnostic coverage of
each safety mechanism for each failure mode is also derived. 
For some safety mechanisms (e.g., ECC), there exist analytical models for the diagnostic coverage.
However, in other cases, fault injection is required to derive diagnostic coverage.

## FTA (Fault Tree Analysis)
FTA is probably the most familiar term for people who have some knowledge in reliability but are new to functional safety.
It is a top-down approach used to estimate failure rate of a module based on its architecture.
To compute the module-level failure rate, in addition to the architecure, it also needs the fault rate of each component, 
the expected operation time of the module, and the diagnostic coverage of the related safety mechanisms from FMEDA.

## DFA (Dependent Failure Analysis)
DFA is used to identify common-source failures and how failure propagate in the system. 
Examples of common-source failures include: voltage droops in a power delivery network, noises in a clock tree, etc. 
Essentially, it help identify possible single points of failures.

