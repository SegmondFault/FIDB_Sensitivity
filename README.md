# FIDB sensitivity study

This study measures how controlled build changes and Ghidra's function-recovery settings affect Function ID (FID) hashes.

## Headline findings

- Small build changes, including compiler optimisation settings, can substantially change FIDs.
- The FID pipeline is lossy: different Ghidra recovery settings produce different levels of function coverage.
- Increasing recovery can introduce additional unverified function starts, creating a genuine false-positive risk.

The [notebook](FIDB_Sensitivity_Study.ipynb) contains the results and definitions. The `data` folder contains the two result tables and public source list used by the note.

Study code is licensed under Apache-2.0. The tested libraries remain under their upstream licences; their source code and binaries are not redistributed here.
