# Veld: MPL-2.0 Source Code Form for modified GeckoView files

This repository is the source-code offer for the Mozilla Public License 2.0 (MPL-2.0)
files that the Veld browser for Meta Horizon OS ships in modified form. It satisfies
MPL-2.0 section 3.2(a): anyone who receives the Veld app can get the Source Code Form
of every MPL-covered file Veld changed, free of charge.

It contains only Mozilla source files (with Veld's changes applied) and the patches
that describe those changes. It contains no Veld application code, which is
separate, proprietary work and is not MPL-covered (MPL-2.0 section 3.3 permits this
for a "Larger Work").

## Which release this corresponds to

| Item | Value |
|---|---|
| Veld app | version 1.0 (versionCode 2) |
| GeckoView artifact | `org.mozilla.geckoview:geckoview-omni:157.0.20260924084938` (Firefox 157.0, build ID 20260924084938) |
| Upstream Gecko revision | `8eb25af4acf031ab1e06abf1a912275083c820ed` (mozilla-release) |
| Upstream source archive | `firefox-157.0.source.tar.xz`, SHA-256 `259c564dd4bbd56bbe8a9a6cf001fc8812853567fd3a78e77c9ddf10d7d4e982` |
| Modified GeckoView AAR shipped in Veld | SHA-256 `c954536835512f2bcff7d140f4151940caef840e27037a20bb20a120c0fb56da` |

Every "original" file named below was taken from that exact source archive and its
SHA-256 is listed in `gecko-157/SHA256SUMS`.

## What Veld changed (Gecko 157)

All changes are Java or JavaScript inside GeckoView. No C++, Rust or native library
was changed: the native libraries in the shipped AAR are Mozilla's published ones,
byte for byte.

1. **Privacy repair** (`patches/01-privacy-diagnostics.patch`): in a build that is
   not an official Mozilla build, these seven media files switch on debug logging of
   media details. Veld's build is not an official Mozilla build, so the patch sets
   their `DEBUG` flag to `false` so that no such diagnostics are logged. The existing
   assertions and the unmapped-track early return are kept.
   Files: `GeckoHlsAudioRenderer.java`, `GeckoHLSDemuxerWrapper.java`,
   `GeckoHlsPlayer.java`, `GeckoHlsRendererBase.java`, `GeckoHLSResourceWrapper.java`,
   `GeckoHlsVideoRenderer.java`, `MediaManager.java`.
2. **WebAuthn without Google Play services** (`patches/02-webauthn-absent-gms.patch`):
   `WebAuthnTokenManager.java` checks whether the Google Play services FIDO classes
   are present before using them, without initialising them, and reports WebAuthn
   as unavailable when they are absent (Meta Quest devices have no Google Play
   services).
3. **Page-zoom API** (`patches/03-page-zoom-java.patch`,
   `patches/04-page-zoom-js-handler.patch`): adds `GeckoSession.setPageZoom(float)`
   and `GeckoSession.getPageZoom()` (range 0.30 to 5.00), their `api.txt` signatures,
   a `CHANGELOG.md` entry, and the matching handler in `GeckoViewSettings.sys.mjs`.

All twelve complete modified files are under `gecko-157/modified/`, at the same
paths as in the Mozilla tree, with their original MPL-2.0 headers unchanged.
`gecko-157/SHA256SUMS` lists the SHA-256 of every original and of every modified
file, so the result can be checked byte for byte.

## How to rebuild the modified GeckoView

1. Get the exact upstream source (either source works):
   - Source archive: <https://archive.mozilla.org/pub/firefox/releases/157.0/source/firefox-157.0.source.tar.xz>
     (verify the SHA-256 above, or Mozilla's published `SHA512SUMS` next to it).
   - Mercurial: <https://hg.mozilla.org/releases/mozilla-release/rev/8eb25af4acf031ab1e06abf1a912275083c820ed>
2. Apply the four patches from the top of the source tree, in order:

   ```sh
   cd firefox-157.0
   for p in /path/to/this/repo/gecko-157/patches/0*.patch; do
     patch -p1 --forward < "$p"
   done
   ```

   Or copy the twelve files from `gecko-157/modified/` over the tree. Either way, the
   changed files must then match the `result/` hashes in `gecko-157/SHA256SUMS`.
3. Build GeckoView for Android (arm64) by following Mozilla's documentation:
   - GeckoView contributor quick start: <https://firefox-source-docs.mozilla.org/mobile/android/geckoview/contributor/geckoview-quick-start.html>
   - Building Firefox for Android: <https://firefox-source-docs.mozilla.org/mobile/android/index.html>

   A minimal `mozconfig`:

   ```
   ac_add_options --enable-project=mobile/android
   ac_add_options --target=aarch64-linux-android
   ```

   then run `./mach bootstrap`, `./mach build` and `./mach gradle geckoview:assembleDebug`
   (or the release variant). The resulting AAR is under
   `<objdir>/gradle/build/mobile/android/geckoview/outputs/aar/`.

Veld built its engine from this exact source tree with the patched files in place,
using Mozilla's build system (`AB_CD=multi`), and kept Mozilla's published arm64
native libraries and `omni.ja` byte for byte, except that the single changed
JavaScript entry `modules/GeckoViewSettings.sys.mjs` inside `omni.ja` was replaced
with the modified file from this repository. A full
rebuild from the steps above yields functionally equivalent code. Byte-identical
reproduction is not guaranteed, because Mozilla's builds are not reproducible across
build machines.

## Unmodified Mozilla components

Veld also ships these MPL-2.0 components without modification. Their source is
available from Mozilla at the same revision:

| Component | Version | Source |
|---|---|---|
| GeckoView (all files not listed above), including bundled translation models | 157.0.20260924084938 | <https://hg.mozilla.org/releases/mozilla-release/file/8eb25af4acf031ab1e06abf1a912275083c820ed> or the archive above |
| Mozilla Android Components (`browser-engine-gecko`, `concept-engine`, `browser-state`, `browser-icons`, `feature-readerview` and their dependencies) | 157.0 | Same revision, directory `mobile/android/android-components/`: <https://hg.mozilla.org/releases/mozilla-release/file/8eb25af4acf031ab1e06abf1a912275083c820ed/mobile/android/android-components> |
| Public Suffix List (inside OkHttp 4.12.0) | as bundled in OkHttp 4.12.0 | <https://github.com/publicsuffix/list> (and the OkHttp 4.12.0 source at <https://github.com/square/okhttp/tree/parent-4.12.0>) |

Maven binaries and `-sources.jar` files for the Mozilla components are also
published at <https://maven.mozilla.org/maven2/org/mozilla/>.

## Desktop-panel viewport patch (not in this release)

`reference/adr-0024-viewport-gecko153/native-viewport-source-complete.patch` is a
separate Veld change (an opt-in desktop viewport mode). It was written against an
older Gecko revision, `3810496e857367d04cf4fa0c705df6995a48b860` (GeckoView
153.0.20260715202819), and is **not** contained in Veld 1.0. It is published here for
transparency only. If a future Veld release ships it, that release's source offer
will include the complete modified files for the matching revision.

## Licence

Every file in this repository is Mozilla source code or a change to it and is
licensed under the Mozilla Public License 2.0; see `LICENSE`. Mozilla's original
copyright and licence headers are retained in each file. "Mozilla", "Firefox" and
"GeckoView" are trademarks of the Mozilla Foundation; this repository grants no
trademark rights and is not endorsed by Mozilla.
