# Veld: MPL-2.0 Source Code Form for modified GeckoView files

This repository is the source-code offer for the Mozilla Public License 2.0 (MPL-2.0)
files that the Veld browser for Meta Horizon OS ships in modified form, and for the
MPL-2.0 files of Veld's copy of Mozilla's Reader View extension. It satisfies
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

## Reader View (Android Components 157.0)

Veld ships its own copy of Mozilla's Reader View extension (the `readerview` built-in of
the Android Components module `feature-readerview` 157.0) under the id `reader-view@veld`,
version 1.5, to add its own colour schemes, a full-window video view and a loading view for Veld's Immersive Reader, and to read only the opened article of a page that holds several. The seven MPL-2.0 files of that copy are in
`android-components-157/reader-view/`, exactly as Veld ships them:

| File | Change from Mozilla's file |
|---|---|
| `manifest.json` | modified: extension id, name and version (1.5) only |
| `readerview.css` | modified: Mozilla's 319 lines unchanged, Veld's colour schemes appended, then the rules of the full-window video view, then the two rules of the loading view |
| `readerview.js` | modified: the list of accepted colour scheme names and its check, the video view (a `VeldVideoView` class and three cases in the message switch) the loading view (a `VeldLoadingView` class and one case in the same switch) and the article choice (a `veldKeepOpenedArticle` function and its one call at the start of `ReaderView.show`) |
| `readerview.html` | unmodified |
| `readerview-content.js` | unmodified |
| `readerview-background.js` | unmodified |
| `readability/JSDOMParser-0.4.2.js` | unmodified |

Version 1.5 adds the article choice: on a page that holds several articles (for example a page that appends the next articles below the one opened), the copy of the page Readability reads keeps only the article that is positively the page's own (its headline repeats the page's title metadata, or it names the page's canonical address) and otherwise stays as it is; `readerview.js` gains the `veldKeepOpenedArticle` function and one call in `ReaderView.show`, and `manifest.json` raises the version; the other five files are unchanged. Version 1.4 adds the loading view ("Loading Immersive Reader" shown instead of the article while Read in VR opens, is open or returns): `readerview.js` gains a `VeldLoadingView` class and one case in the message switch, `readerview.css` gains two rules, and `manifest.json` raises the version; the other four files are unchanged. Version 1.3 only changes the Sepia scheme (`veld-r1-sepia`) to a soft clay ground (`#e0c9b4`) and text colour (`#59443b`) in `readerview.css` and raises the version in `manifest.json`. Version 1.2 changed the same two files only, for a warmer, darker Sepia ground. Version 1.1 changes the same three files compared with version 1.0: `readerview.js` gains the
video view (it finds a video of the article, shows it alone filling the window, plays it and
ends the view again), `readerview.css` gains the rules of that view, and `manifest.json` only
raises the version. The other four files are unchanged.

The three modifications are also given as patches in `android-components-157/patches/reader-view/`
(unified diffs from Mozilla's file to Veld's). `android-components-157/SHA256SUMS` lists the
SHA-256 of every file here, so the result can be checked byte for byte.

Upstream origin: the nine files under
`mobile/android/android-components/components/feature/readerview/src/main/assets/extensions/readerview/`
at mozilla-release revision `8eb25af4acf031ab1e06abf1a912275083c820ed` (Firefox 157.0), as shipped in
`org.mozilla.components:feature-readerview:157.0` (AAR SHA-256
`81343920e7406834bd716a75c2aed475742e5e04c49dbe558421e6eda7d8070d`). In that AAR the build writes the
version into `manifest.json`, so the patch for `manifest.json` starts from the AAR's file.
Mozilla's headers are kept in each file that has one. The two Readability files of the
extension (`readability-0.4.2.js`, `readability-readerable-0.4.2.js`) are Apache-2.0, not MPL-2.0,
and unmodified; they are not part of this repository and are available from the same Mozilla
source and from <https://github.com/mozilla/readability/tree/0.4.2>.

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
available from Mozilla at the revisions listed:

| Component | Version | Source |
|---|---|---|
| GeckoView (all files not listed above), including bundled translation models | 157.0.20260924084938 | <https://hg.mozilla.org/releases/mozilla-release/file/8eb25af4acf031ab1e06abf1a912275083c820ed> or the archive above |
| Mozilla Android Components (`browser-engine-gecko`, `concept-engine`, `browser-state`, `browser-icons`, `feature-readerview`, `service-sync-logins`, `concept-storage`, `concept-sync`, `concept-base`, `lib-dataprotect`, `support-base` and their dependencies) | 157.0 | Same revision, directory `mobile/android/android-components/`: <https://hg.mozilla.org/releases/mozilla-release/file/8eb25af4acf031ab1e06abf1a912275083c820ed/mobile/android/android-components> |
| Mozilla application-services (`logins`, `init_rust_components`, `full-megazord`), used for saved passwords | 157.0.1 | <https://github.com/mozilla/application-services/tree/v157.0.1> (tag `v157.0.1`, commit `b2c6a64e19e793218c255e3c57beb034b761bb8a`) |
| Mozilla Glean SDK (`glean`), included by application-services and never started by Veld | 68.0.1 | <https://github.com/mozilla/glean/tree/v68.0.1> (tag `v68.0.1`, commit `92ecd18105fd2936e11936da749183369a491580`) |
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
