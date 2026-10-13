# Source-offer repository: publication review (2026-10-13)

## Summary for the owner

**What the repository contains.** Only Mozilla's own GeckoView source files that Veld
changed, in their changed form, plus the patches that show exactly what changed, a
README and the MPL-2.0 licence text. There are 12 changed Mozilla files in Gecko 157:
seven media files where Veld switches off debug logging of media details (the privacy
repair), one WebAuthn file that copes safely with Quest having no Google Play services,
and four files that add the page-zoom feature. All 12 are included as complete files,
and their SHA-256 values are listed so anyone can check them against Mozilla's
originals plus the patches. A
separate, clearly labelled patch for the desktop-panel viewport (ADR-0024) is included
for reference only: it targets an older Gecko (153) and is not in Veld 1.0.

**What it deliberately does not contain.** No Veld app code (Kotlin, Compose UI,
manifests, Gradle files), no build scripts or build outputs (no AAR, APK, omni.ja),
no Meta SDK code or binaries, no keys, tokens, signing configuration or keystore paths,
no device serials, IP addresses, e-mail addresses, user names or local folder paths,
no evidence, receipts, review records, test fixtures or tests, and nothing else from
`artifacts/`. Veld's internal build commands were not copied, because they contain
local machine paths. The README gives clean, path-free build steps instead.

**Why that satisfies the MPL.** MPL-2.0 section 3.2(a) requires that people who
receive Veld can get the Source Code Form of the MPL-covered files Veld distributes.
For files Veld did not change, Mozilla already publishes that source at the exact
revision, and the README links to it. For the 12 files Veld changed, this repository
provides them as complete files, with exact patches and checksums, at the exact upstream revision
`8eb25af4acf031ab1e06abf1a912275083c820ed`. Section 3.3 allows Veld's own app code to
stay proprietary because it is a separate "Larger Work" containing no copied MPL code,
so it is correctly left out. This is an engineering record, not legal advice.

**One recorded scanner exception (owner decision (b), 2026-10-13).** The project privacy
scanner flags web links whose host is not on its allow-list. It flagged 8 lines of
Mozilla's own unchanged comment text: 7 in `GeckoSession.java` (documentation and example
links such as `foo.com`, `my-referrer.com`, `webcompat.com` and `drafts.csswg.org`) and 1
in `GeckoHlsPlayer.java` (a `cs.android.com` code-search link). None is personal or
browsing data. Editing Mozilla's text would make the files differ from what Veld ships.
The owner chose option (b): "(b) include the two complete files as well, with a recorded
exception for those 9 lines of Mozilla's comments". The count of 9 in that quote came
from an earlier report that wrongly included a README link, which has since been
removed. The exception covers exactly the 8 upstream lines listed below and nothing else.

## How the contents were produced and checked

- Mozilla originals: all 12 were extracted from the official
  `firefox-157.0.source.tar.xz` (SHA-256 `259c564d…6d4e982`, whose `sourcestamp.txt`
  names mozilla-release revision `8eb25af4…83c820ed`, build `20260924084938`). Each
  matched the hashes in Veld's source-intake records exactly.
- Modified files: copied from the exact files Veld's shipped engine was built from
  (privacy repair `FND002-GECKO157-PRIVACY-01..08`, page zoom `ZOOM157-01..04`). The
  hashes match the build script's staged-input hashes and the copied-code ledger.
- Patches: regenerated as clean `diff -u` output (`a/` and `b/` labels, LF line endings,
  no timestamps or local paths) from the verified originals to the modified files.
  Patches 03 and 04 are byte-identical to `compliance/modified-source/.../zoom157-*.patch`.
  Patches 01 and 02 have the same content as Veld's local `privacy.patch` and
  `no-gms-carry.patch`, without the Windows line endings. Round trip: applying 01-04 to
  the originals reproduced all 12 modified files byte for byte (12/12 OK).
- `GeckoSession.java` and `GeckoHlsPlayer.java` were rebuilt for publication by applying
  patches 01 and 03 to Mozilla's originals. Their SHA-256 values match `SHA256SUMS` and
  the hashes the shipped engine build staged (`40c014fa…` and `fb65f3a7…`).
- `LICENSE`: the verbatim MPL-2.0 text from Mozilla's own tree
  (`mobile/android/LICENSE` in the same source archive).

## Scanner exception record

Scanner exceptions are matched on kind, path and the scanner's 10-hex-digit value
fingerprint, and use the `evidence_redaction_scan` fields `owner`, `approved_by`,
`reason` and `expires`. They are recorded in
`evidence/process/SOURCE_OFFER_SCAN_POLICY_2026-10-13.json`. That policy is the
unchanged repo policy `tools/dev/privacy_audit_policy.json` plus these entries only.
It is used with `--policy` for this folder only, so the repo-wide policy and audits
are unchanged. Each entry also pins the line numbers, the SHA-256 of each line and the
SHA-256 of the whole file. A changed file therefore no longer matches the record and
needs a new review.

- owner: `LIC-002 MPL source offer`
- approved_by: project owner, 2026-10-13 (relayed by the coordinator), quote above
- reason: verbatim upstream Mozilla Gecko 157 public comment text in an MPL-2.0
  corresponding-source file; no personal or browsing data
- expires: `2027-04-13` (review date)

| File | Line | Scanner fingerprint | Line SHA-256 | File SHA-256 |
|---|---:|---|---|---|
| `.../gecko/media/GeckoHlsPlayer.java` | 922 | `db708deb9c` | `919d8135a39864256200681cae8871e5269ca15e6d2667d906bd48949fe00bfe` | `fb65f3a70383cd093c8dea4ee311cb89f415ba438a0cf140e99c1472a3b2c384` |
| `.../geckoview/GeckoSession.java` | 2177 | `4a24a80e67` | `3dfcdb704373671056d7e1da9b2af96b0427bb532c72e8e30aa817c3a495287b` | `40c014fa7597b8bd35bf65ecbc1c895f2a84a81aa95d6b1055cad2b67499ad31` |
| `.../geckoview/GeckoSession.java` | 3418 | `be7d278938` | `2f99aeaf426ed9790cad390840fb6b08fe9df8de59edaee84bb0afa9b8c21f3c` | `40c014fa7597b8bd35bf65ecbc1c895f2a84a81aa95d6b1055cad2b67499ad31` |
| `.../geckoview/GeckoSession.java` | 4096 | `c45b131aa8` | `a7ad87c0320a4b9d6b22e62a6f9beddd246f0320e35ec38136c7b247fb7863fc` | `40c014fa7597b8bd35bf65ecbc1c895f2a84a81aa95d6b1055cad2b67499ad31` |
| `.../geckoview/GeckoSession.java` | 7407 | `a9a9b569a6` | `922a8c339fb9c94a37fe6c1d9b343de06c3b228ba722f6ec16ba8a491de0fc8f` | `40c014fa7597b8bd35bf65ecbc1c895f2a84a81aa95d6b1055cad2b67499ad31` |
| `.../geckoview/GeckoSession.java` | 7408 | `a9a9b569a6` | `c4c82870c3452007930b1f5cc6a32f7b17ef4d531d3a33bea9ab4526ac0e7d3c` | `40c014fa7597b8bd35bf65ecbc1c895f2a84a81aa95d6b1055cad2b67499ad31` |
| `.../geckoview/GeckoSession.java` | 7412 | `a9a9b569a6` | `d795648e391e3e0603bc9fd8231fe997bef4ac001be3c70ec519f31a22fae7c3` | `40c014fa7597b8bd35bf65ecbc1c895f2a84a81aa95d6b1055cad2b67499ad31` |
| `.../geckoview/GeckoSession.java` | 7413 | `a9a9b569a6` | `3f9d2cf15e1131c7771b5aa36fc1d574ace5dc8a819e414539dda36e589e78a8` | `40c014fa7597b8bd35bf65ecbc1c895f2a84a81aa95d6b1055cad2b67499ad31` |

## Scan results (whole staging folder, all 21 files)

| Scan | Command | Result |
|---|---|---|
| Project redaction scanner with the source-offer exception policy | `.venv-bootstrap/Scripts/python.exe -I -B tools/dev/evidence_redaction_scan.py --policy evidence/process/SOURCE_OFFER_SCAN_POLICY_2026-10-13.json --paths artifacts/source-offer-staging` | PASS, blocking=0, warnings=0, excepted=8 (exactly the 8 lines above), no unused or stale exception |
| Project redaction scanner, unchanged repo policy (control) | same command without `--policy` | 8 blocking `url` findings, exactly the 8 excepted lines and no others |
| Dedicated publication grep (15 checks) | known Quest serial and serial shapes, adb serial arguments, IPv4, IPv6, e-mail, Windows drive paths, user, home, WSL and app-data folder paths, owner names and machine user, private-key headers, Android signing-config keys and keystore file extensions, token shapes (GitHub, AWS, Google, Slack, JWT, Bearer, OpenAI-style), quoted secret assignments, Veld app identifiers, Meta SDK packages | 0 hits. A canary file with a serial, IP, e-mail, user path, private-key header, signing-config key, a GitHub token shape and an API-key assignment produced 11 hits, so the grep does detect these. |

Words such as `password` or `token` appear in Mozilla's public API names (for example
`FLAG_IS_PASSWORD`, `onTokenRequest`). They are identifiers, not secret values, and the
quoted-assignment check found no literal secret.

## File table

Licence for every row: MPL-2.0. "Scan" means both scans above, run on the final folder.

| Path | Bytes | SHA-256 | Origin | Why it is safe to publish | Scan |
|---|---:|---|---|---|---|
| `LICENSE` | 16725 | `1f256ecad192880510e84ad60474eab7589218784b9a50bc7ceee34c2b91f1d5` | Upstream Mozilla (verbatim MPL-2.0 text) | Standard public licence text; no data | PASS / 0 hits |
| `README.md` | see final inventory | see final inventory | Veld-written documentation | Describes the repo; contains only public Mozilla and GitHub links, the public revision and artifact hashes; no paths, names or secrets | PASS / 0 hits |
| `PUBLICATION_REVIEW.md` | (this file) | not self-listed | Veld-written review record | Hashes, public revisions and scan results only; does not reproduce any flagged value, serial or path | PASS / 0 hits |
| `gecko-157/SHA256SUMS` | 4341 | `55ecdf21fdb36d5b32c33ae7d2aa9927cf6cbde7f65f2493ea6d7ed5ff543684` | Veld-generated checksums | SHA-256 values and repo-relative Mozilla paths only | PASS / 0 hits |
| `gecko-157/modified/mobile/android/geckoview/api.txt` | 196990 | `fdf5bb3c11cb25ffd7e60f61432c1980259c97353120f707bf4dfbd7dd7e0fc6` | Upstream Mozilla + Veld patch (ZOOM157-02) | Public API signature list; Veld added two method lines; no secrets or personal data | PASS / 0 hits |
| `gecko-157/modified/.../gecko/media/GeckoHLSDemuxerWrapper.java` | 4966 | `cccf75a6133b540618c4f12aba73429cbae98b94d8149e6b2342c8d5ae7354ad` | Upstream Mozilla + Veld patch (FND002-02) | Mozilla source with MPL header; change only disables debug logging | PASS / 0 hits |
| `gecko-157/modified/.../gecko/media/GeckoHLSResourceWrapper.java` | 3559 | `481600c9885c24fbda8be40d56dad4173ce67d7da112fb56125ca8461a56adb5` | Upstream Mozilla + Veld patch (FND002-05) | As above | PASS / 0 hits |
| `gecko-157/modified/.../gecko/media/GeckoHlsAudioRenderer.java` | 6979 | `46cf3b0ead0c1f64475f7dc8285bafed72f7f6f73cb33b739d22e42cb74b23d5` | Upstream Mozilla + Veld patch (FND002-01) | As above | PASS / 0 hits |
| `gecko-157/modified/.../gecko/media/GeckoHlsPlayer.java` | 38551 | `fb65f3a70383cd093c8dea4ee311cb89f415ba438a0cf140e99c1472a3b2c384` | Upstream Mozilla + Veld patch (FND002-03); rebuilt from patch 01 | Mozilla source with MPL header; change only disables debug logging and keeps the unmapped-track return; one upstream code-search link in a comment | PASS (1 line excepted) / 0 hits |
| `gecko-157/modified/.../gecko/media/GeckoHlsRendererBase.java` | 10882 | `17ed41c23e8d5a18c887488c3a61b00d2eddfb9c642caff09247e28b9bd1d66e` | Upstream Mozilla + Veld patch (FND002-04) | As above | PASS / 0 hits |
| `gecko-157/modified/.../gecko/media/GeckoHlsVideoRenderer.java` | 20775 | `35274b71bd670143e248e3beb230ff444e674e9a066a8e6751a7a3a58858299c` | Upstream Mozilla + Veld patch (FND002-06) | As above | PASS / 0 hits |
| `gecko-157/modified/.../gecko/media/MediaManager.java` | 2585 | `8af2099c3eef2d14dbd5e5bf7343994da4e60b1cc5964d3f13a32b691301ebf2` | Upstream Mozilla + Veld patch (FND002-07) | As above | PASS / 0 hits |
| `gecko-157/modified/.../geckoview/GeckoSession.java` | 320261 | `40c014fa7597b8bd35bf65ecbc1c895f2a84a81aa95d6b1055cad2b67499ad31` | Upstream Mozilla + Veld patch (ZOOM157-01); rebuilt from patch 03 | Mozilla source with MPL header; Veld added the two page-zoom methods and range constants; seven upstream documentation/example links in comments | PASS (7 lines excepted) / 0 hits |
| `gecko-157/modified/.../geckoview/WebAuthnTokenManager.java` | 36079 | `661010faf8cd8e115230430bb1aafb78bc79ed2d410fd7606ee673006942d6b1` | Upstream Mozilla + Veld patch (FND002-08) | Mozilla source with MPL header; change only adds class-presence checks; names public Google FIDO class names, contains no keys or credentials | PASS / 0 hits |
| `gecko-157/modified/.../geckoview/doc-files/CHANGELOG.md` | 158065 | `014640605d40b9bba14add4e98bb6265294d761e165a399e4ebbb2616623ebe0` | Upstream Mozilla + Veld patch (ZOOM157-03) | Public changelog; Veld added one v157 entry and the api-version hash | PASS / 0 hits |
| `gecko-157/modified/mobile/shared/modules/geckoview/GeckoViewSettings.sys.mjs` | 7312 | `933172041ea7b4be25fccae3f9912420d65b7e1154bd379554e39459a8c8dc67` | Upstream Mozilla + Veld patch (ZOOM157-04) | Mozilla source with MPL header; adds zoom set/get handlers only | PASS / 0 hits |
| `gecko-157/patches/01-privacy-diagnostics.patch` | 5066 | `2e7488035fd181cb68f4ee80417fb02f76f67147efbab230e29b78dd1e61ea10` | Veld patch (FND002-01..07) | Unified diff of Mozilla files only; `a/`/`b/` labels, no local paths or timestamps | PASS / 0 hits |
| `gecko-157/patches/02-webauthn-absent-gms.patch` | 9622 | `dd83e762d7e93ee88e6fa18df29ebb583fa01c05659a9e4eb6d097e834efdd27` | Veld patch (FND002-08) | As above | PASS / 0 hits |
| `gecko-157/patches/03-page-zoom-java.patch` | 4259 | `cf6aaa7860f0d25562ed80ff43ba5cbe5d452c38e5cc0ee2676139f0b33baddc` | Veld patch (ZOOM157-01..03) | As above; identical to the ledgered `zoom157-java.patch` | PASS / 0 hits |
| `gecko-157/patches/04-page-zoom-js-handler.patch` | 2183 | `29cc9966ae74196a3492f64dc290265c2bce2b138036ad197661994e29cd4fef` | Veld patch (ZOOM157-04) | As above; identical to the ledgered `zoom157-js-handler.patch` | PASS / 0 hits |
| `reference/adr-0024-viewport-gecko153/native-viewport-source-complete.patch` | 13857 | `fec09921e30e464530d91c55c66909d2ee16e11ab1d1d7d4b8b035ce1e7df6ed` | Veld patch (UI006-VIEWPORT-01..06) against Mozilla revision `3810496e857367d04cf4fa0c705df6995a48b860` | Diff of Mozilla files only (C++, WebIDL, Java, JS); git-style headers with repo-relative paths; not shipped in Veld 1.0; labelled as reference | PASS / 0 hits |

`.../` abbreviates `mobile/android/geckoview/src/main/java/org/mozilla/`. Bytes for
`README.md` and `PUBLICATION_REVIEW.md` are given in the final inventory at the end of this file.

## Excluded on purpose

| Item | Reason |
|---|---|
| Mozilla originals of the 12 files | Unmodified, available from Mozilla at the exact revision (README links) |
| `compliance/modified-source/.../COMMANDS.md`, `outer-build.sh`, `repack_omni.py`, `prove*.py` | Contain local machine paths, WSL user and home directories; replaced by clean README steps |
| `proof-*.json`, build/receipt/review JSON, `reconstruction.json`, `DOWNSTREAM_NOTICE.txt` | Evidence and receipts with local paths; not source |
| The scanner exception policy file | Internal evidence; stays in this repository's `evidence/process/`, not in the public repo |
| Built AAR, `omni.ja`, `classes.jar`, APKs | Build outputs; the source is what MPL requires |
| `experiments/ui006-geckoview-page-zoom/` scripts, tests and fixtures | Veld tooling and test fixtures, not MPL source |
| All Veld app modules, Gradle files, manifests, signing configuration | Proprietary Veld code (MPL 3.3 Larger Work) and secrets |
| Meta Spatial SDK, Horizon OS UI Set, any Meta binary or header | Meta proprietary; must never be published |

## Final inventory

README.md: 7192 bytes, SHA-256 `eec5ce4e85f5aa11ea26af2c2cb110a56eb242467e7d23381192c95665ceff68`.

Folder total excluding this review: 20 files, 870249 bytes. With this review: 21 files.

Exception policy: `evidence/process/SOURCE_OFFER_SCAN_POLICY_2026-10-13.json`, SHA-256 `ce38f2d2dc499900bceef4c4bf1f021159679de504218cc859a948f308e204f1`.

After this file's last edit, both scans were rerun over all 21 files, including this review. Redaction scanner with the exception policy: PASS, 0 blocking, 0 warnings, 8 excepted. Dedicated grep: 0 hits.
