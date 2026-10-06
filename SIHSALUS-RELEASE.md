# OpenConceptLab 3.2.0-sihsalus.1

This is a candidate SIH Salus maintenance release, not an official OpenMRS
release. It is not yet published or deployed.

## Provenance and scope

- Upstream base: OpenConceptLab 3.2.0, `6aecd4b8025f1a5467841a84fd603eb149fcfcb2`.
- Native fix: `ba032f5dc2d8d0265a9546c4864716dd4ed62938`.
- Upstream contribution: [OpenMRS #136](https://github.com/openmrs/openmrs-module-openconceptlab/pull/136).
- Downstream tracking: [content #255](https://github.com/sihsalus/sihsalus-content/pull/255).
- Maintenance owner: `@Duvet05`.

Changing the subscription host changes import URLs. Initializer correctly keeps
checksums for unchanged concept exports, leaving historical concept imports at
the old host. An unchanged cross-source answer or set-member mapping then cannot
resolve its target. This release reuses a successful mapping by stable external
UUID only when its update timestamp matches and its native relationship still
exists. Unknown, changed, undated or deleted relationships use normal resolution.
No import history, clinical identity, schema or content checksum is rewritten.

The alternative of retaining the previous OCL subscription was rejected because
the institutional server must remain selected. Reloading unchanged ZIPs or
rewriting OCL tables in the content package would conceal the importer defect.
Remove this maintenance release when an official compatible OpenMRS release
includes #136 and passes the same synthetic upgrade and restart tests.

## Verification and publication boundary

`mvn -B -ntp -pl api,omod -am verify` passes on Java 21 for the candidate version.
The API includes seven regression tests and reports 156 tests, zero failures or
errors, and eight pre-existing skips. The two positive origin-change regressions
fail with the original Saver and pass with the correction. OWA and OMOD packaging
use the native Maven reactor.

The content qualification workflow tests a synthetic installation, upgrade from
1.25.15 and operational upgrade. Its original baseline uses the unmodified OMOD;
candidate phases use the corrected binary. Record its final source SHA and
results before publication. This qualification is not clinical acceptance.

Publish only after review and explicit authorization. Publish a new immutable
tag and its native `openconceptlab-3.2.0-sihsalus.1.omod`, record SHA-256, then let
the distro consume that exact release. Do not overwrite an existing asset or
install the candidate manually into a running hospital instance.
