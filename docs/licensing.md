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

The same release also provides these companion downloads:

| File | Contents |
| --- | --- |
| `Bob-Scheduler-<version>-sbom.cdx.json` | Inventory of shipped npm libraries, generated-helper attributions, Electron and native license evidence. |
| `Bob-Scheduler-<version>-build-sbom.cdx.json` | Build dependency inventory; this includes tools that are not shipped in the application. |
| `Bob-Scheduler-<version>-Open-Source.zip` | Notices, both SBOMs, matching FFmpeg source and Electron source/build patches. It does not contain Bob Scheduler's application source tree. |
| `SHA256SUMS` | SHA-256 hashes for the app, optional MCP setup, both SBOMs and Open-Source ZIP. |
| `release-validation.json` | Release validation results and asset hashes. |

Preserve the notices and corresponding library sources when redistributing the
application. They are also included inside the app itself.

## Runtime library source availability

Version 0.1.9 uses Electron 44.7.0, Chromium 152.0.7977.130 and Node.js 24.21.0.
FFmpeg is LGPL-2.1-or-later. Its matching Chromium source revision is
`2b68d2babae73714846961fb0ee47e3b3d2e39a9`; its source archive and the matching
Electron source/build patches are included under
`legal/runtime-sources/` in the Open-Source ZIP and the app. The included
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
