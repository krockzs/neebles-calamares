<!-- SPDX-FileCopyrightText: 2026 N.E.E.B.L.E.S. contributors
     SPDX-License-Identifier: CC0-1.0
-->

# N.E.E.B.L.E.S. Calamares

N.E.E.B.L.E.S. Calamares is the downstream Calamares source tree used to maintain the installer changes required by **N.E.E.B.L.E.S. OS**.

This repository is **not the original Calamares project** and does not claim authorship of Calamares. It preserves the upstream project history and maintains a small, isolated set of N.E.E.B.L.E.S.-specific modifications on top of it.

## Upstream Project

Calamares is developed by the **Calamares project and its contributors**.

Official upstream repository:

https://github.com/calamares/calamares

Full credit for the original architecture, source code, modules, documentation, and installer framework belongs to the Calamares developers and contributors.

N.E.E.B.L.E.E.S. OS uses Calamares as its installer framework and maintains only the modifications required for its own distribution.

Original copyright notices, licenses, authorship information, contributor history, and licensing files from upstream are intentionally preserved.

## Base Version

The current N.E.E.B.L.E.S. branch is based on:

- **Calamares:** `v3.3.14`
- **Upstream commit:** `21ea803527735cfaf54fa6059e71d1ef65004864`
- **N.E.E.B.L.E.S. branch:** `neebles-v3.3.14`

The upstream Git history is preserved below the N.E.E.B.L.E.S. changes so authorship and project provenance remain traceable.

N.E.E.B.L.E.S.-specific development begins after the upstream `v3.3.14` base.

## Why This Repository Exists

N.E.E.B.L.E.S. OS requires a few installer behaviors and visual adaptations that differ from the default Calamares implementation.

Rather than modifying the upstream project or mixing distribution-specific requirements into the Calamares repository, those changes are maintained here as a separate downstream codebase.

The goals are to:

- preserve the original Calamares project and its history;
- clearly separate upstream code from N.E.E.B.L.E.S.-specific changes;
- make every local modification auditable;
- preserve the exact source used to build the modified installer modules;
- allow the modules to be rebuilt in the future;
- keep N.E.E.B.L.E.S. OS reproducible;
- make future rebasing against newer Calamares versions possible.

# N.E.E.B.L.E.S. Modifications

The initial N.E.E.B.L.E.S. customization commit modifies **43 files** in total:

- `README.md`;
- 2 keyboard-module source files;
- 39 locale/timezone image assets;
- 1 partition-view source file.

No other Calamares source files were modified in the initial downstream baseline.

## 1. Explicit Keyboard Layout Configuration

Modified files:

```text
src/modules/keyboard/Config.cpp
src/modules/keyboard/Config.h
```

The keyboard module was extended so N.E.E.B.L.E.S. OS can explicitly provide:

```yaml
keyboardLayout:
keyboardVariant:
```

These values are read from the Calamares configuration and stored internally as:

```cpp
QString m_configuredLayout;
QString m_configuredVariant;
```

During keyboard-layout detection, Calamares now checks whether an explicit configuration was supplied before falling back to the existing `locale1` detection path.

Conceptually, the resolution order becomes:

```text
Explicit configured layout / variant
              ↓
       locale1 detection
              ↓
       existing fallback logic
```

This allows N.E.E.B.L.E.S. OS to preserve an explicitly selected or configured keyboard layout instead of having it immediately replaced by auto-detection.

The original Calamares behavior remains available whenever no explicit layout is configured.

## 2. Locale / Timezone Visual Customization

Modified directory:

```text
src/modules/locale/images/
```

The N.E.E.B.L.E.S. baseline replaces or customizes **39 image assets** used by the locale/timezone interface:

- `bg.png`;
- `pin.png`;
- all modified `timezone_*.png` map layers present in the commit.

These changes adapt the locale/timezone selector to the visual language of the N.E.E.B.L.E.S. installer.

The initial N.E.E.B.L.E.S. commit does **not** modify the Calamares locale or timezone-selection C++ logic. The locale change in this baseline is visual/assets-only.

## 3. Partition View Dark-Theme Adaptation

Modified file:

```text
src/modules/partition/gui/PartitionLabelsView.cpp
```

The partition visualization originally used hard-coded black and gray label colors. Those colors have poor contrast on the dark N.E.E.B.L.E.S. installer theme.

The label drawing colors were changed to:

```text
Primary text:   #F5F5F5
Secondary text: #A3A3A3
```

The affected `QPainter` pen values are used only for partition-label rendering.

No partitioning logic, filesystem behavior, disk operations, partition calculations, or partition-management semantics were changed by this modification.

## Slideshow integration used by N.E.E.B.L.E.S. OS

The Calamares source changes above are only one part of the N.E.E.B.L.E.S. installer integration.

N.E.E.B.L.E.S. OS also provides a custom slideshow runtime in the `neebles-os` repository. That runtime can progressively replace locally bundled slides with remote media while Calamares is running.

Remote slideshow assets are maintained under:

```text
neebles-os/calamares/slides/
```

The current runtime supports:

- PNG, JPG and JPEG images;
- MP4 video;
- numeric slots from 1 to 20;
- progressive preparation during installation;
- protection of the currently active slot so it is not replaced while being displayed;
- temporary-file downloads followed by atomic replacement;
- local fallback content when the network or remote assets are unavailable;
- independent remote updates without rebuilding the ISO.

The corresponding runtime implementation is integrated into the OS build as:

```text
config/includes.chroot/usr/lib/neebles/neebles-calamares-slides
```

This slideshow system is **N.E.E.B.L.E.S. OS integration code**, not part of upstream Calamares. It is documented and maintained in `krockzs/neebles-os` rather than in the upstream Calamares source tree.

## Scope of the Changes

The N.E.E.B.L.E.S. modifications are intentionally narrow.

The project does **not** attempt to redesign Calamares, replace its architecture, or present Calamares as N.E.E.B.L.E.S.-authored software.

The current downstream source changes are limited to:

```text
Keyboard configuration behavior
Locale/timezone presentation assets
Partition-view text colors
```

The wider N.E.E.B.L.E.S. OS integration additionally supplies its own branding, configuration, build-time integration and remote slideshow runtime outside this source repository.

Where possible, distribution-specific behavior should remain isolated so differences from upstream can be reviewed, rebuilt, and rebased cleanly.

## Relationship With `neebles-os`

This repository contains the **modified Calamares source code**.

The `neebles-os` repository contains the N.E.E.B.L.E.S. OS build configuration, the compiled Calamares components currently injected into the OS build, and the N.E.E.B.L.E.S.-specific installer runtime integrations such as the remote slideshow system.

Repository responsibilities are therefore separated:

```text
neebles-calamares
    Modified Calamares source
    Development
    Recompilation
    Traceability

neebles-os
    N.E.E.B.L.E.S. OS build configuration
    Installer configuration
    Branding and OS assets
    Remote image/video slideshow runtime
    Runtime integration
    Compiled installer modules used by the current build
```

This separation allows the OS build to remain reproducible while preserving the exact modified source from which its Calamares components originate.

## Repository Relationship

The original Calamares repository remains separate from the N.E.E.B.L.E.S. repository.

Typical local remotes:

```text
upstream  https://github.com/calamares/calamares.git
origin    https://github.com/krockzs/neebles-calamares.git
```

N.E.E.B.L.E.S.-specific commits belong in this repository.

Changes must not be pushed to the upstream Calamares repository as though they were part of the original project.

If a future modification becomes generally useful outside N.E.E.B.L.E.S. OS, it can be evaluated independently for possible upstream contribution following the Calamares project's own contribution process.

## Attribution and Licensing

This repository contains substantial source code originating from Calamares.

**Calamares remains the work of the Calamares project and its contributors.**

N.E.E.B.L.E.S. claims authorship only for its own modifications and distribution-specific additions.

Existing upstream copyright notices, SPDX metadata, licenses, `AUTHORS`, `CONTRIBUTING.md`, `LICENSES/`, Git commit authorship, and contributor history must remain intact.

Nothing in this repository should be interpreted as transferring ownership of upstream Calamares code to N.E.E.B.L.E.S.

Please refer to the licensing information already included in the Calamares source tree for the licenses applicable to individual upstream components.

## Development Policy

When modifying this repository:

1. Keep N.E.E.B.L.E.S.-specific changes as small and isolated as practical.
2. Preserve upstream authorship and licensing information.
3. Document why a modification is necessary for N.E.E.B.L.E.S. OS.
4. Avoid changing unrelated upstream behavior.
5. Keep the upstream base traceable.
6. Prefer configuration and branding over source modification whenever Calamares already provides the required mechanism.
7. Maintain source compatibility and rebaseability where reasonably possible.

## Current N.E.E.B.L.E.S. Customization Baseline

Initial downstream customization:

```text
fed9b6b12ccf4e40c41d244d6f8f9c6d6a575be5
Add N.E.E.B.L.E.S. Calamares customizations
```

Upstream baseline:

```text
21ea803527735cfaf54fa6059e71d1ef65004864
Calamares v3.3.14
```

This provides a clear boundary between the upstream project and the N.E.E.B.L.E.S.-specific work.

---

## N.E.E.B.L.E.S.

**Nested Evolutionary Engine for Behavioral Language Emergent Systems**

N.E.E.B.L.E.S. OS is built by integrating existing open-source technologies with its own operating-system architecture, tooling, configuration, branding, and distribution-specific components.

Calamares is one of those upstream technologies, and its contribution to the N.E.E.B.L.E.S. OS installer is explicitly acknowledged here.
