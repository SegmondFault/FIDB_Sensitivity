# FIDB sensitivity study

This study measures how controlled build changes and Ghidra's function-recovery settings affect Function ID (FID) hashes.

## Headline findings

- Small build changes, including compiler optimisation settings, can substantially change FIDs.
- The FID pipeline is lossy: different Ghidra recovery settings produce different levels of function coverage.
- Increasing recovery can introduce additional unverified function starts, creating a genuine false-positive risk.

## Build-factor sensitivity

| Route | Factor | Controlled comparison | Tracked functions | Available in both analyses | Hashable in both analyses | Same full hash | Same specific hash | Reference-matrix action |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| macOS ARM64 | AddressSanitizer | off to on | 14,871 | 15.20% (2,260/14,871) | 12.63% (1,878/14,871) | 0.91% of tracked; 7.24% of hashable | 0.51% of tracked; 4.05% of hashable | Do not admit unguarded; wrong label observed |
| macOS ARM64 | Analysis repeat | same bytes, fresh Ghidra project | 5,642 | 86.71% (4,892/5,642) | 70.72% (3,990/5,642) | 70.72% of tracked; 100.00% of hashable | 70.72% of tracked; 100.00% of hashable | Control: lock the analysis settings |
| macOS ARM64 | Compiler version | Apple Clang 21 to Clang 22 | 2,849 | 79.50% (2,265/2,849) | 66.09% (1,883/2,849) | 54.48% of tracked; 82.42% of hashable | 30.26% of tracked; 45.78% of hashable | Add compiler-version candidate |
| macOS ARM64 | Dead-code elimination | linker dead strip off to on | 2,821 | 0.00% (0/2,821) | 0.00% (0/2,821) | 0.00% of tracked; n/a of hashable | 0.00% of tracked; n/a of hashable | Redesign truth; no attributable pair |
| macOS ARM64 | Debug information | -g to -g0 | 2,821 | 86.71% (2,446/2,821) | 70.68% (1,994/2,821) | 70.65% of tracked; 99.95% of hashable | 70.33% of tracked; 99.50% of hashable | No extra reference on this route |
| macOS ARM64 | Build repeat | same inputs and flags | 5,642 | 86.71% (4,892/5,642) | 70.72% (3,990/5,642) | 70.72% of tracked; 100.00% of hashable | 70.72% of tracked; 100.00% of hashable | Control: no extra reference |
| macOS ARM64 | Link-time optimisation | off to on | 2,821 | 0.00% (0/2,821) | 0.00% (0/2,821) | 0.00% of tracked; n/a of hashable | 0.00% of tracked; n/a of hashable | Redesign truth; no attributable pair |
| macOS ARM64 | Frame pointer | kept to omitted | 4,305 | 56.82% (2,446/4,305) | 46.34% (1,995/4,305) | 14.68% of tracked; 31.68% of hashable | 11.68% of tracked; 25.21% of hashable | Add frame-pointer candidate |
| macOS ARM64 | Optimisation | O2 to O0 | 3,656 | 61.95% (2,265/3,656) | 51.50% (1,883/3,656) | 3.25% of tracked; 6.32% of hashable | 3.25% of tracked; 6.32% of hashable | Add optimisation candidate |
| macOS ARM64 | Optimisation | O2 to O1 | 2,887 | 84.41% (2,437/2,887) | 68.58% (1,980/2,887) | 36.99% of tracked; 53.94% of hashable | 21.13% of tracked; 30.81% of hashable | Add optimisation candidate |
| macOS ARM64 | Optimisation | O2 to O3 | 2,839 | 85.91% (2,439/2,839) | 69.99% (1,987/2,839) | 41.14% of tracked; 58.78% of hashable | 27.26% of tracked; 38.95% of hashable | Add optimisation candidate |
| macOS ARM64 | Optimisation | O2 to Os | 2,915 | 82.23% (2,397/2,915) | 66.90% (1,950/2,915) | 35.51% of tracked; 53.08% of hashable | 22.98% of tracked; 34.36% of hashable | Add optimisation candidate |
| macOS ARM64 | Optimisation | O2 to Oz | 4,695 | 48.80% (2,291/4,695) | 39.00% (1,831/4,695) | 7.65% of tracked; 19.61% of hashable | 6.67% of tracked; 17.09% of hashable | Add optimisation candidate |
| macOS ARM64 | PAC / BTI | off to standard | 4,305 | 55.70% (2,398/4,305) | 45.41% (1,955/4,305) | 0.16% of tracked; 0.36% of hashable | 0.09% of tracked; 0.20% of hashable | Guarded candidate; one ambiguity |
| macOS ARM64 | Position-independent code | PIC on to off | 2,821 | 86.71% (2,446/2,821) | 70.72% (1,995/2,821) | 70.72% of tracked; 100.00% of hashable | 70.72% of tracked; 100.00% of hashable | No extra reference on this route |
| macOS ARM64 | Symbol stripping | unstripped to stripped | 2,821 | 86.71% (2,446/2,821) | 70.72% (1,995/2,821) | 70.68% of tracked; 99.95% of hashable | 70.61% of tracked; 99.85% of hashable | No extra reference on this route |
| macOS ARM64 | Stack protection | off to strong | 2,821 | 86.71% (2,446/2,821) | 70.72% (1,995/2,821) | 60.23% of tracked; 85.16% of hashable | 32.83% of tracked; 46.42% of hashable | Add hardening candidate |
| macOS ARM64 | CPU target | generic ARM64 to Apple M1 | 2,821 | 86.71% (2,446/2,821) | 70.72% (1,995/2,821) | 70.72% of tracked; 100.00% of hashable | 70.72% of tracked; 100.00% of hashable | No extra reference on this route |
| ARM32 ELF | Analysis repeat | same bytes, fresh Ghidra project | 32,072 | 99.98% (32,064/32,072) | 99.98% (32,064/32,072) | 99.98% of tracked; 100.00% of hashable | 99.98% of tracked; 100.00% of hashable | Control: lock the analysis settings |
| ARM32 ELF | Debug information | debug on to off | 16,036 | 99.98% (16,032/16,036) | 99.98% (16,032/16,036) | 99.98% of tracked; 100.00% of hashable | 99.91% of tracked; 99.94% of hashable | No extra reference on this route |
| ARM32 ELF | Build repeat | same inputs and flags | 32,072 | 99.98% (32,064/32,072) | 99.98% (32,064/32,072) | 99.98% of tracked; 100.00% of hashable | 99.98% of tracked; 100.00% of hashable | Control: no extra reference |
| ARM32 ELF | Link-time optimisation | off to on | 16,036 | 0.00% (0/16,036) | 0.00% (0/16,036) | 0.00% of tracked; n/a of hashable | 0.00% of tracked; n/a of hashable | Redesign truth; no attributable pair |
| ARM32 ELF | Frame pointer | kept to omitted | 16,036 | 99.98% (16,032/16,036) | 76.22% (12,222/16,036) | 0.01% of tracked; 0.01% of hashable | 0.00% of tracked; 0.00% of hashable | Add frame-pointer candidate |
| ARM32 ELF | Optimisation | O2 to O0 | 22,633 | 69.87% (15,814/22,633) | 69.87% (15,814/22,633) | 0.63% of tracked; 0.90% of hashable | 0.63% of tracked; 0.90% of hashable | Add optimisation candidate |
| ARM32 ELF | Optimisation | O2 to O3 | 16,098 | 96.76% (15,576/16,098) | 96.76% (15,576/16,098) | 77.83% of tracked; 80.44% of hashable | 46.21% of tracked; 47.76% of hashable | Add optimisation candidate |
| ARM32 ELF | Symbol stripping | unstripped to stripped | 16,036 | 59.83% (9,594/16,036) | 59.83% (9,594/16,036) | 58.39% of tracked; 97.59% of hashable | 58.28% of tracked; 97.40% of hashable | Inspect discovery loss; signatures do not fix it |
| ARM32 ELF | Stack protection | off to strong | 16,036 | 99.98% (16,032/16,036) | 99.98% (16,032/16,036) | 86.16% of tracked; 86.18% of hashable | 51.15% of tracked; 51.17% of hashable | Add hardening candidate |
| Windows x86-64 | Analysis repeat | same bytes, fresh Ghidra project | 21,858 | 92.32% (20,180/21,858) | 86.06% (18,810/21,858) | 86.06% of tracked; 100.00% of hashable | 86.06% of tracked; 100.00% of hashable | Control: lock the analysis settings |
| Windows x86-64 | Compiler version | MSVC 14.37 to 14.36 | 10,930 | 92.26% (10,084/10,930) | 85.99% (9,399/10,930) | 79.93% of tracked; 92.95% of hashable | 79.93% of tracked; 92.95% of hashable | Add compiler-version candidate |
| Windows x86-64 | Debug information | debug on to off | 10,929 | 92.32% (10,090/10,929) | 86.06% (9,405/10,929) | 86.06% of tracked; 100.00% of hashable | 86.06% of tracked; 100.00% of hashable | No extra reference on this route |
| Windows x86-64 | Build repeat | same inputs and flags | 21,858 | 92.32% (20,180/21,858) | 86.06% (18,810/21,858) | 86.06% of tracked; 100.00% of hashable | 86.06% of tracked; 100.00% of hashable | Control: no extra reference |
| Windows x86-64 | Link-time optimisation | off to on | 11,308 | 65.32% (7,386/11,308) | 65.22% (7,375/11,308) | 12.43% of tracked; 19.06% of hashable | 12.42% of tracked; 19.04% of hashable | Retest admission; variant recovered little |
| Windows x86-64 | Optimisation | O2 to O0 | 14,882 | 67.76% (10,084/14,882) | 63.20% (9,405/14,882) | 0.07% of tracked; 0.11% of hashable | 0.07% of tracked; 0.11% of hashable | Add optimisation candidate |
| Windows x86-64 | Symbol stripping | unstripped to stripped | 10,929 | 92.32% (10,090/10,929) | 86.06% (9,405/10,929) | 86.06% of tracked; 100.00% of hashable | 86.06% of tracked; 100.00% of hashable | No extra reference on this route |
| Windows x86-64 | Stack protection | off to strong | 10,950 | 92.11% (10,086/10,950) | 85.85% (9,401/10,950) | 77.10% of tracked; 89.80% of hashable | 77.10% of tracked; 89.80% of hashable | Add hardening candidate |
| ARM32 ELF | Compiler family | GCC 14.2 to Clang 22 | 16,353 | 95.29% (15,583/16,353) | 94.98% (15,532/16,353) | 0.00% of tracked; 0.00% of hashable | 0.00% of tracked; 0.00% of hashable | Add compiler-family candidate |
| ARM32 ELF | CPU target | generic ARMv7 to Cortex-A7 | 16,036 | 99.98% (16,032/16,036) | 99.98% (16,032/16,036) | 11.13% of tracked; 11.13% of hashable | 10.88% of tracked; 10.88% of hashable | Add CPU-target candidate |
| macOS ARM64 | Compiler x optimisation | Clang 21/O2 to Clang 22/O0 | 3,625 | 62.48% (2,265/3,625) | 51.94% (1,883/3,625) | 2.34% of tracked; 4.51% of hashable | 1.79% of tracked; 3.45% of hashable | Optional interaction; single factors recover 78.55% |

## Function-recovery sensitivity

### macOS ARM64

| Setting | All functions Ghidra reported | Verified library functions found | Verified functions with a usable FID hash | Verified functions without a usable FID hash | Extra verified functions vs default | Extra possible functions vs default (unverified) | Verified functions lost vs default | Functions assigned to the wrong library |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Default — selected | 3,120 | 2,446/2,821 (86.71%) | 1,995/2,821 (70.72%) | 451 | 0 | 0 | 0 | 0 |
| Maximal built-in search | 3,120 | 2,446/2,821 (86.71%) | 1,995/2,821 (70.72%) | 451 | 0 | 0 | 0 | 0 |
| Permissive shared returns | 3,120 | 2,446/2,821 (86.71%) | 1,995/2,821 (70.72%) | 451 | 0 | 0 | 0 | 0 |
| Direct-call recovery | 3,120 | 2,446/2,821 (86.71%) | 1,995/2,821 (70.72%) | 451 | 0 | 0 | 0 | 0 |
| Isolated-flow recovery | 3,121 | 2,446/2,821 (86.71%) | 1,995/2,821 (70.72%) | 451 | 0 | 1 | 0 | 0 |
| Maximal + isolated-flow | 3,131 | 2,446/2,821 (86.71%) | 1,995/2,821 (70.72%) | 451 | 0 | 11 | 0 | 0 |

### ARM32 ELF

| Setting | All functions Ghidra reported | Verified library functions found | Verified functions with a usable FID hash | Verified functions without a usable FID hash | Extra verified functions vs default | Extra possible functions vs default (unverified) | Verified functions lost vs default | Functions assigned to the wrong library |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Default | 10,697 | 9,594/16,032 (59.84%) | 9,594/16,032 (59.84%) | 0 | 0 | 0 | 0 | 0 |
| Maximal built-in search | 12,088 | 10,964/16,032 (68.39%) | 10,964/16,032 (68.39%) | 0 | 1,391 | 0 | 0 | 0 |
| Permissive shared returns | 10,697 | 9,594/16,032 (59.84%) | 9,594/16,032 (59.84%) | 0 | 0 | 0 | 0 | 0 |
| Direct-call recovery | 10,697 | 9,594/16,032 (59.84%) | 9,594/16,032 (59.84%) | 0 | 0 | 0 | 0 | 0 |
| Isolated-flow recovery | 11,659 | 10,472/16,032 (65.32%) | 10,472/16,032 (65.32%) | 0 | 929 | 33 | 0 | 0 |
| Maximal + isolated-flow — selected | 16,549 | 15,213/16,032 (94.89%) | 15,213/16,032 (94.89%) | 0 | 5,738 | 114 | 0 | 0 |

### Windows x86-64

| Setting | All functions Ghidra reported | Verified library functions found | Verified functions with a usable FID hash | Verified functions without a usable FID hash | Extra verified functions vs default | Extra possible functions vs default (unverified) | Verified functions lost vs default | Functions assigned to the wrong library |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Default | 10,563 | 10,090/10,929 (92.32%) | 9,405/10,929 (86.06%) | 685 | 0 | 0 | 0 | 0 |
| Maximal built-in search | 10,563 | 10,090/10,929 (92.32%) | 9,405/10,929 (86.06%) | 685 | 0 | 0 | 0 | 0 |
| Permissive shared returns | 10,534 | 10,067/10,929 (92.11%) | 9,392/10,929 (85.94%) | 675 | 0 | 0 | 23 | 0 |
| Direct-call recovery | 10,563 | 10,090/10,929 (92.32%) | 9,405/10,929 (86.06%) | 685 | 0 | 0 | 0 | 0 |
| Isolated-flow recovery — selected | 11,121 | 10,631/10,929 (97.27%) | 9,546/10,929 (87.35%) | 1,085 | 541 | 17 | 0 | 0 |
| Maximal + isolated-flow | 11,126 | 10,631/10,929 (97.27%) | 9,546/10,929 (87.35%) | 1,085 | 541 | 22 | 0 | 0 |
