# DOSBox Staging for POWER

**DOSBox Staging POWER** is the restoration of dropped Linux PPC64LE JIT backend with the goal of creating downstream patch for Linux packagers to consume. If you run into bugs with PPC64LE, please file [bug report](https://github.com/runlevel5/dosbox-staging-power/issues) here.

## History

- The PPC64LE dynamic recompiler backend was initially written by Cameron
  Kaiser, documented in
  [DOSBox JIT on ppc64le and how you can too](https://www.talospace.com/2020/01/dosbox-jit-on-ppc64le-and-how-you-can.html).
- It was then upstreamed into DOSBox Staging via
  [pull request #2828](https://github.com/dosbox-staging/dosbox-staging/pull/2828).
- The DOSBox Staging project later decided to drop PPC64LE support, citing
  maintenance concerns and a lack of ppc64 expertise
  ([PR #4796](https://github.com/dosbox-staging/dosbox-staging/pull/4796)).

This fork restores that backend and maintains it as a downstream patch.

## Build notes for packagers

- Upstream is CMake-only; configure with the system-libraries preset, e.g.
  `cmake --preset release-linux`.
- The backend runs on **POWER8 and later**. POWER10 (PowerISA 3.1) instructions
  are emitted only when detected at run time (`getauxval(AT_HWCAP2) &
  PPC_FEATURE2_ARCH_3_1`), so a single binary built with a POWER8 baseline
  (`-mcpu=power8`) stays portable and still uses the POWER10 fast path where
  available.

## FAQ

**Does the JIT backend work on big-endian PPC64 (PPC64BE)?**

No. The backend is little-endian (PPC64LE) only. Big-endian support would
require restoring the byte-swapping code paths that upstream also removed.
Contributions adding PPC64BE support are welcome.

**Why not start a separate DOSBox fork?**

Maintaining a full fork is a large amount of work, and it would be hard to
convince mainstream Linux distributions to package yet another DOSBox fork when
DOSBox Staging already exists. Staying aligned with DOSBox Staging — and shipping
PPC64LE support as a downstream patch — keeps the maintenance burden low and the
path to distribution packaging realistic.

**Will DOSBox Staging change their stance on the PPC64 JIT backend?**

Unknown. Perhaps one day a maintainer will get their hands on POWER hardware and
take an interest in supporting it again.
