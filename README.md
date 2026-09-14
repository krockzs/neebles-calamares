# N.E.E.B.L.E.S. Calamares

This repository contains a modified version of **Calamares**, used as part of the N.E.E.B.L.E.S. OS installer.

## Upstream Project

The original source code belongs to the **Calamares project and its contributors**.

Upstream project:

https://github.com/calamares/calamares

N.E.E.B.L.E.S. does not claim authorship of the original Calamares codebase.

This repository exists only to maintain the changes required by N.E.E.B.L.E.S. OS without modifying, disrupting, or imposing project-specific requirements on the upstream Calamares repository.

## Base Version

The N.E.E.B.L.E.S. branch was created from:

- Calamares version: `v3.3.14`
- Upstream commit: `21ea803527735cfaf54fa6059e71d1ef65004864`

All N.E.E.B.L.E.S.-specific modifications are maintained separately from upstream.

## Purpose

N.E.E.B.L.E.S. OS requires some installer behavior and presentation that differ from the default Calamares implementation.

This repository preserves those modifications in a dedicated codebase so that:

- the upstream Calamares project remains untouched;
- the origin of the code remains explicit and traceable;
- N.E.E.B.L.E.S.-specific changes can be developed and maintained independently;
- modified Calamares components can be rebuilt when required;
- the N.E.E.B.L.E.S. OS build remains reproducible and auditable.

## Relationship With N.E.E.B.L.E.S. OS

The N.E.E.B.L.E.S. OS build repository already contains the compiled Calamares components required by the current OS build.

This repository preserves the corresponding modified source code used to produce those components.

Therefore, this repository is primarily the source-of-truth for future Calamares development, recompilation, maintenance, and traceability within the N.E.E.B.L.E.S. ecosystem.

## Attribution

Full credit for Calamares itself belongs to the Calamares developers and contributors.

N.E.E.B.L.E.S. only maintains its own modifications on top of that work.

Original copyright notices, licenses, contributor information, and attribution from the upstream project must remain intact.

## Development Policy

Changes made here should be limited to functionality required by N.E.E.B.L.E.S. OS.

Whenever possible, N.E.E.B.L.E.S.-specific behavior should remain isolated and clearly identifiable so that differences from upstream Calamares can be reviewed and maintained over time.

---

**N.E.E.B.L.E.S.**  
Nested Evolutionary Engine for Behavioral Language Emergent Systems
