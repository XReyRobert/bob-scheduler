# Licenses and dependency information

Copyright © 2026 Xavier Rey-Robert.

The distributed Bob Scheduler application retains its [MIT License](../LICENSE).
This public repository provides downloads and user documentation. The
application's development source tree and commit history are not published here.
Third-party components retain their own licenses. IBM Bob Shell is installed
separately and is not redistributed with Bob Scheduler.

## Included notices and library sources

Open **Help → Open Source Licenses** in the app to view
`Contents/Resources/legal/`, including the project's license, full third-party
notices, CycloneDX SBOMs and corresponding runtime library sources.

The application ZIP is the only uploaded release asset. All license material is
already inside the app; no companion download is needed.

| Path under `Contents/Resources/legal/` | Contents |
| --- | --- |
| `THIRD-PARTY-NOTICES.txt` | Full third-party notices and attributions. |
| `sbom.cdx.json` | Inventory of shipped npm libraries, generated-helper attributions, Electron and native license evidence. |
| `sbom-build.cdx.json` | Build dependency inventory, including tools that are not shipped in the application. |
| `runtime-sources/` | Matching FFmpeg source and Electron source/build patches. |
| `SOURCE-AVAILABILITY.md` | Source provenance, library rebuilding and replacement information. |

Preserve the included notices and corresponding library sources when
redistributing the application.

## Runtime library source availability

Version 0.1.10 uses Electron 44.7.0, Chromium 152.0.7977.130 and Node.js 24.21.0.
FFmpeg is LGPL-2.1-or-later. Its matching Chromium source revision is
`2b68d2babae73714846961fb0ee47e3b3d2e39a9`; its source archive and the matching
Electron source/build patches are included under
`Contents/Resources/legal/runtime-sources/` in the app. The included
`legal/SOURCE-AVAILABILITY.md` explains the source provenance, library rebuilding
and replacement. Bob Scheduler imposes no additional prohibition on reverse
engineering for debugging modifications to these libraries.

Upstream native dependency version and graph gaps are explicitly recorded in
the SBOM. It is not a complete resolved native dependency graph. The original
upstream credits and license texts are retained alongside it.

## Independence

Bob Scheduler is an independent project and is not affiliated with, sponsored
by, or endorsed by International Business Machines Corporation (IBM).
IBM and IBM Bob are trademarks or registered trademarks of IBM. All other
trademarks are the property of their respective owners.
