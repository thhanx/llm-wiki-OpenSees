---
title: "OpenSees Parallel Processing (공식 개요 페이지)"
source_url: https://opensees.berkeley.edu/OpenSees/parallel/parallel.php
retrieved: 2026-07-06
type: official-doc
method: WebFetch
---

# OpenSees Parallel Processing

![OpenSees logo](/images/logo.gif)

[PEER](http://peer.berkeley.edu/ "Pacific Earthquake Engineering Research Center")

## Navigation Menu

* [HOME](/index.php)
* [USER](/OpenSees/user/index.php)
* [DEVELOPER](/OpenSees/developer/index.php)
* [SUPPORT](/support.php)
* [PARALLEL](/OpenSees/parallel/parallel.php)
* [Copyright](/OpenSees/copyright.php)
* [SITEMAP](/sitemap.php)

## Quick Links

* [HOME](/index.php)
* [OPENSEESWIKI](/wiki/index.php)
* [MESSAGE BOARD](/community/index.php)
* [USER DOC](/wiki/index.php/OpenSees_User)
* [DOWNLOAD](/OpenSees/user/download.php)
* [SOURCE CODE](https://github.com/OpenSees/OpenSees/)
* [BUG REPORT](https://github.com/OpenSees/OpenSees/issues)

To customize the quicklinks, go to [Site Map](/sitemap.php)

[![](/images/peer_emblem.gif)](http://peer.berkeley.edu)[PEER](http://peer.berkeley.edu)

## OpenSees Parallel

"Parallel computation is becoming increasingly important for conducting realistic earthquake simulations of structural and geotechnical systems." Multi-core processors now make this capability essential even for personal computers. The OpenSees framework supports parallel application development. Two applications have been created for general users:

1. **OpenSeesSP**: For analyzing very large models
2. **OpenSeesMP**: For parameter studies or analysis of large models with user-defined partitions

## Documentation

These applications extend the basic OpenSees interpreter. Comprehensive documentation covering usage, additional commands, modifications, and examples is [available here](http://opensees.berkeley.edu/OpenSees/parallel/TNParallelProcessing.pdf).

Additional information appears in [presentations from the OpenSees Parallel Workshop](http://opensees.berkeley.edu/OpenSees/workshops/parallel/Agenda.htm).

## Availability

Both applications operate on NSF-sponsored XSEDE machines. Users must request their own allocations:

| XSEDE Machine | File Location |
|---|---|
| TACC Stampede2 | /home1/00477/tg457427/bin |
| TACC Frontera | /home1/00477/tg457427/bin |

Non-XSEDE users can access these applications through [DesignSafe-ci](https://www.designsafe-ci.org/), an NSF NHERI facility offering free registration.

## Download

Older prebuilt binaries for multi-core or clustered Intel machines are available:

* [Windows version](https://opensees.berkeley.edu/OpenSees/code/OpenSeesParallel2.5.0-x64.zip)
* [Mac version](https://opensees.berkeley.edu/OpenSees/code/OpenSeesParallel2.4.0Mac.tar.gz)

Required software includes MPICH2 (Windows) or OpenMPI (Apple).

### Usage Command

```
mpiexec -np numProcs applicationName inputFile
```

Where:
- `mpiexec` = MPICH2 or OpenMPI application
- `numProcs` = number of processors to use
- `applicationName` = OpenSeesSP or OpenSeesMP
- `inputFile` = TCL script

## Warning

"These applications are new. They have been checked and validated against the limited number of example scripts that we have to test them with." Users may encounter undiscovered bugs. Software is free and open-source; bug reports are encouraged.

**Contact:** opensees-support @ berkeley.edu

Supported by the [National Science Foundation](/nsf.php)

©2006, UC Regents
