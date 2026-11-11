
# MOSIP-GEN-SEV3-2026-001: MinIO Community Edition – Maintenance Mode Advisory

| Field | Value |
|-------|-------|
| **Advisory ID** | `MOSIP-GEN-SEV3-2026-001` |
| **Type** | `General` |
| **Severity** | `SEV3 – Medium` |
| **CVE ID** (security only) | `N/A` |
| **CVSS score** (security only) | `N/A` |
| **Status** | `Draft` |
| **Published** | `2026-03-29` |
| **Last updated** | `2026-03-29` |

## Summary

MinIO's open-source/community edition has entered maintenance mode, affecting countries and System Integrators considering or operating MinIO Community Edition for MOSIP deployments.

MOSIP is not tied to MinIO and requires an S3-compatible object storage interface. Countries should select object storage solutions with demonstrable support for a minimum five-year operational window aligned with national deployment timelines.

## Affected components

| Module / repository | Affected versions | Fixed versions |
|---------------------|-------------------|----------------|
| MOSIP object storage deployments using MinIO Community Edition | All MinIO Community Edition deployments | Not applicable – no MOSIP software fix/version identified |

## Description

This advisory is issued to all MOSIP country deployments in response to MinIO's announcement that its open-source/community edition has entered maintenance mode.

MinIO is referenced in MOSIP documentation as a sample implementation of an S3-compatible object store. MOSIP's architecture is not tied to MinIO; it requires an S3-compatible object storage interface.

The MinIO Community Edition has entered maintenance mode with the following restrictions:

- No new features, architectural changes, or issue reviews will be accepted for the community edition.
- Security patches and critical bug fixes will continue only for existing deployments – no forward development.
- Pre-compiled binaries and official Docker images have been discontinued.
- Distribution has shifted to source-only under the AGPLv3 licence, significantly increasing the operational burden on implementers.
- Countries deploying MinIO Community Edition for new projects face a product with no active development roadmap and increasing self-maintenance overhead.

## Impact

- New MOSIP deployments selecting MinIO Community Edition will rely on a component with no active development or roadmap.
- The absence of pre-compiled binaries and Docker images increases operational complexity and cost for country teams.
- Long-term security posture may degrade as the community edition will not receive forward-looking security hardening.
- AGPLv3 source-only distribution may introduce licence compliance considerations for government-sector deployments.
- System Integrators proposing MinIO Community Edition may be unable to fulfil SLA obligations over a five-year deployment window.

## Resolution

As documented in MOSIP Docs v1.2.0 (Object Store – Overview), MinIO is referenced only as a sample/reference implementation. MOSIP's object storage layer is designed against the S3-compatible API standard.

Any object store that exposes an S3-compatible interface is supported. Countries are not required to use MinIO and should evaluate alternatives that provide long-term, vendor-supported S3-compatible storage.

Countries and System Integrators are advised to adopt an S3-compatible object store that meets the following criteria:

| Criterion | Requirement |
|-----------|-------------|
| **Criterion 1 – S3-Compatible API** | The object store must expose an S3-compatible API. This is the only interface MOSIP depends upon. MinIO is one implementation; it is not the only one. |
| **Criterion 2 – 5-Year Support Window** | The chosen solution must have an active vendor or community support commitment covering at minimum a five-year operational window, aligned with country deployment timelines. |
| **Criterion 3 – Active Release Cycle** | The product must maintain an active release cycle with regular security patches, bug fixes, and published binaries or container images. |
| **Criterion 4 – Licence Compatibility** | The licence must be compatible with government and public-sector deployment policies. Assess whether AGPLv3 or alternative licences apply to your deployment context. |
| **Criterion 5 – SLA-Backed Support** | SLA-backed support must be available through a qualified vendor or System Integrator for the duration of the deployment. |
| **Criterion 6 – Integration Validation** | The chosen object store must be validated against the MOSIP object store interface through integration testing prior to production deployment. |

### Indicative S3-Compatible Alternatives

The following are illustrative examples of S3-compatible object store solutions. Countries must conduct their own due diligence and procurement evaluation. MOSIP does not endorse any specific vendor or product.

- **MinIO AIStor:** https://www.min.io/product/aistor
- **Ceph / Rook-Ceph:** Open-source, actively maintained, S3-compatible, suitable for on-premise deployments.
- **AWS S3:** Fully managed, enterprise SLA, native S3 API.
- **Azure Blob Storage (S3-compatible adapter):** Enterprise-grade managed storage with S3 compatibility.
- **Google Cloud Storage:** Managed object storage with S3-compatible XML API.
- **MinIO Enterprise (AGPL → Commercial):** Countries may opt for MinIO's commercial/enterprise edition with vendor SLA, which continues active development.

## Workarounds / mitigations

For existing deployments, the source advisory states that security patches and critical bug fixes will continue for MinIO Community Edition.

Countries should assess operational risk and plan a transition or support arrangement that satisfies the criteria specified in the Resolution section.

## Verification

Before production deployment, confirm the following:

1. The selected object store exposes an S3-compatible API.
2. The solution has passed integration testing against the MOSIP object store interface.
3. A documented minimum five-year support commitment is available.
4. The product maintains an active release cycle with regular security patches and bug fixes.
5. The licence is compatible with government and public-sector deployment policies.
6. SLA-backed support is available for the duration of the deployment.

## Additional files (optional)

None.

## References

- [MOSIP Documentation – Object Store Overview](https://docs.mosip.io/1.2.0/id-lifecycle-management/supporting-components/persistence/object-store)
- [MinIO Official Site](https://min.io)

## Credits (optional)

Not specified.

## Contact

Countries requiring further guidance may contact their System Integrators. They may also seek assistance from the MOSIP team via [community@mosip.io](mailto:community@mosip.io) or [MOSIP Community](https://community.mosip.io).

For security matters, use the private reporting channel listed in [security/README.md](../security/README.md).

## Revision history

| Date | Version | Change |
|------|---------|--------|
| `2026-03-29` | 1.0 | Initial publication |
