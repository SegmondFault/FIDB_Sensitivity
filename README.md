# FIDB sensitivity study

This study measures how controlled build changes and Ghidra's function-recovery settings affect Function ID (FID) hashes.

## Headline findings

- Identical rebuilds retained **100% of full and specific hashes among functions that remained hashable**. End-to-end identical-hash retention across all tracked functions was lower: **70.72% on macOS ARM64**, **99.98% on ARM32 ELF**, and **86.06% on Windows x86-64**.
- On a fixed ARM32 target, changing **GCC 14.2 to Clang 22 retained 0 of 15,532 comparable full hashes**.
- Changing the ARM32 CPU target from generic ARMv7 to Cortex-A7 retained **11.13% of full hashes** and **10.88% of specific hashes**.
- Optimisation, frame-pointer policy, stack protection, PAC/BTI, AddressSanitizer and other build choices also caused substantial FID churn. The size of the effect varied by platform.
- Ghidra's default analysis lost functions before FID hashing. On stripped binaries, the best tested recovery setting raised verified function recovery from **59.84% to 94.89% on ARM32** and from **92.32% to 97.27% on Windows**. No tested setting improved macOS ARM64.
- The selected settings added verified functions without losing verified functions or producing a verified wrong-library result on the held-out libraries. Additional unverified starts remain a **false-positive risk**.
- The selected exports repeated byte-for-byte. These are results for six libraries across three target routes, not universal Ghidra defaults.

The [notebook](FIDB_Sensitivity_Study.ipynb) contains the results and definitions. The `data` folder contains the two result tables and public source list used by the note.

Study code is licensed under Apache-2.0. The tested libraries remain under their upstream licences; their source code and binaries are not redistributed here.
