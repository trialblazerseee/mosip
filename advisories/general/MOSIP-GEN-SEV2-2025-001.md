
# MOSIP-GEN-SEV2-2025-001: Urgent Migration Away from Bitnami Public Charts & Images

| Field | Value |
|-------|-------|
| **Advisory ID** | `MOSIP-GEN-SEV2-2025-001` |
| **Type** | `General` |
| **Severity** | `SEV2 – High` |
| **CVE ID** (security only) | `N/A` |
| **CVSS score** (security only) | `N/A` |
| **Status** | `Draft` |
| **Published** | `2025-09-19` |
| **Last updated** | `2025-09-19` |

## Summary

Bitnami (now under Broadcom) has announced major changes to its free/open-use catalog of container images and Helm charts. Many previously freely available images and Helm artifacts are being moved to legacy repositories without further updates or will require a subscription for continued access to the full catalog.

Key components commonly used in MOSIP deployments, such as MinIO, Keycloak, Kafka, PostgreSQL, Redis, and RabbitMQ, may be affected if they rely on Bitnami charts or container images.

MOSIP strongly advises all partners, implementing countries, and users to proactively migrate away from relying on Bitnami's free catalog for production-critical components.

## Affected components

| Module / repository | Affected versions | Fixed versions |
|---------------------|-------------------|----------------|
| Keycloak | Bitnami container images and Helm charts using affected versioned tags | Approved alternative images/charts or supported Bitnami Secure Images |
| MinIO | Bitnami container images and Helm charts using affected versioned tags | Official MinIO images/charts or approved alternatives |
| Kafka | Bitnami container images and Helm charts using affected versioned tags | Apache Kafka / Strimzi operator or approved alternatives |
| PostgreSQL, Redis, RabbitMQ | Bitnami container images and Helm charts using affected versioned tags | Maintained community/vendor images or managed services |
| Other MOSIP infrastructure components | All deployments referencing affected Bitnami images or charts | Supported and maintained alternatives |

## Description

Bitnami (now under Broadcom) has announced significant changes to its free/open-use catalog of container images and Helm charts.

The following changes have been announced:

### Deprecation of version-tagged images

- From August 28, 2025, most versioned tags for many applications are being moved out of the main `docker.io/bitnami` repository into the legacy repository `docker.io/bitnamilegacy`.
- Images moved to the legacy repository will no longer receive updates or security patches.
- Only a limited set of hardened images will remain available for free under the `latest` tag in a new community-oriented subset, primarily intended for development/non-production use.
- Access to the full catalog with versioned tags, updates, and enterprise support requires a subscription to Bitnami Secure Images.

### Helm charts

The Helm chart source code will continue to be publicly available on GitHub under the Apache-2.0 licence. However:

- Packaged OCI Helm chart artifacts may stop receiving updates or may contain image references pointing to deprecated or removed repositories.
- Chart defaults may reference images in `docker.io/bitnami` that will no longer be accessible or updated.

### Brownouts and registry deletion timeline

The following timeline was announced:

| Event | Date |
|-------|------|
| First scheduled brownout | August 28–29, 2025 |
| Second scheduled brownout | September 2–3, 2025 |
| Third scheduled brownout | September 17–18, 2025 |
| Planned final public catalog deletion/cutoff for many free assets | September 29, 2025 |

### Bitnami Commons Chart Status

Regarding the Bitnami Commons chart or similarly named shared/common charts:

- There is no explicit public indication in the source advisory that the Bitnami Commons chart is being revoked or removed entirely.
- Chart source code remains publicly available.
- However, packaged charts, image references, and defaults may refer to deprecated Bitnami image repositories.
- Implementers should audit chart dependencies and ensure that image repositories and tags can be overridden.

## Impact

Deployments using Bitnami charts or images for components such as Keycloak, MinIO, Kafka, PostgreSQL, Redis, RabbitMQ, and other Kubernetes/Docker infrastructure components may experience the following issues:

- **Image pull failures:** Errors such as `ErrImagePull` and `ImagePullBackOff` may occur when images are removed or moved.
- **Broken builds and deployments:** CI/CD pipelines may fail when expected versioned image tags are no longer available.
- **Security risks:** Legacy images will no longer receive security patches, leaving known and future vulnerabilities unaddressed.
- **Operational instability:** During autoscaling, node rescheduling, or upgrades, systems may attempt to pull unavailable images or tags, resulting in deployment failures.

## Resolution

MOSIP recommends that all partners, implementing countries, and users take immediate action to reduce dependency on Bitnami's free catalog and plan migration to supported alternatives.

### Immediate actions (Next Few Days / Weeks)

**1. Inventory every usage of Bitnami images/charts**

- List all services and components using Bitnami charts or container images, including Keycloak, Kafka, MinIO, etc.
- Identify versioned tags, image references, and Helm values that refer to `docker.io/bitnami`.

**2. Test fallback / patching**

- Override image repositories in Helm values or Kubernetes manifests to use `docker.io/bitnamilegacy` or `bitnamisecure` (if available) when possible.
- Mirror or pull images locally into your own registry for critical services to ensure future resilience.

**3. Update CI/CD pipelines**

- Ensure pipelines do not fail due to missing versioned tags.
- Make image references parameterizable/configurable so that switching repositories and tags is easier.

**4. Run smoke tests during brownout windows**

- Use scheduled brownouts to simulate image availability failures.
- Ensure systems remain functional when certain Bitnami images are unavailable.

### Medium-term actions

**1. Evaluate alternatives to Bitnami**

- Use non-Bitnami charts and images from trustworthy open-source projects or community vendors.
- Self-host builds from source where possible.
- Consider images from Debian, Ubuntu, CentOS, other upstream distributions, or the respective projects' own container repositories.

**2. Select and standardize a replacement stack**

- Evaluate alternative Helm charts or operator patterns for each component, including Keycloak, Kafka, and MinIO.
- Define policies for version pinning, image security, and regular patching.

**3. Mirror critical images**

- Host critical images in your own private or public registries to avoid dependency on third-party availability.

**4. Communicate and update documentation**

- Ensure all MOSIP users and implementers are aware of the migration and expected changes.
- Update deployment guides, Helm values/templates, and Kubernetes manifests.

### Suggested alternatives / replacement options

The following are candidate alternatives mentioned in the original advisory. They should be evaluated based on deployment requirements.

| Component | Alternative Chart / Image Source | Pros | Cons / Considerations |
|-----------|----------------------------------|------|-----------------------|
| Keycloak | Official Keycloak Helm chart or community charts such as community-Keycloak operator / Quarkus builds | Maintained by the community/Keycloak team; avoids Bitnami vendor lock-in | Verify versioning, security patches, and operational support |
| MinIO | Official MinIO Helm chart / image | Simple, popular, and offers full control | May require additional work for backups, high availability, etc. |
| Kafka | Apache Kafka official operator / Strimzi operator | Well understood, strong community, flexible operator capabilities | More configuration needed; possible performance/operations overhead |
| PostgreSQL / Redis / RabbitMQ | Community/vendor-maintained images or cloud-managed services (RDS, Azure DB, etc.) | Offloads operations and provides managed security updates | Cost, latency, dependency, possible lock-in; ensure backups, etc. |

### Suggested migration timeline for MOSIP partners

| Timeline | Expected action |
|----------|-----------------|
| Before August 28, 2025 | Inventory usage; begin modifying deployments to override image repositories/tags; test fallbacks and plan migration strategies. |
| Between August 28 – September 29, 2025 | Migrate heavily used components; start using legacy/secure repositories; ensure no production component depends on deprecated versioned Bitnami tags without updates. |
| After September 29, 2025 | The free Bitnami catalog with versioned tags/images is expected to be removed or significantly limited. Production deployments should use supported, maintained images or approved alternatives. |

## Workarounds / mitigations

Until migration is completed:

- Override image repository references in Helm values or Kubernetes manifests to use available alternative repositories.
- Use `docker.io/bitnamilegacy` for existing images where required, while recognizing that these images will not receive further updates or security patches.
- Consider Bitnami Secure Images where a subscription is available and appropriate.
- Mirror critical images into private registries to reduce dependency on public registry availability.
- Pin image versions and validate deployment behavior during upgrades, scaling, and node rescheduling.

These measures are temporary mitigations and do not replace migration to actively maintained and supported images.

## Verification

Implementers should verify the following:

1. Identify all Bitnami image references in Kubernetes manifests, Helm charts, and CI/CD pipelines.
2. Confirm that production workloads no longer depend on unsupported or unavailable Bitnami versioned image tags.
3. Verify that replacement images are available in the configured container registry.
4. Ensure image repository and tag overrides are correctly applied in Helm values and deployment manifests.
5. Perform smoke tests to validate deployments, pod restarts, autoscaling, and node rescheduling.
6. Confirm that replacement images receive regular security updates and are supported.
7. Verify that critical images are mirrored in an organization-controlled registry where required.

## Additional files (optional)

None.

## References

- [Bitnami Official Website](https://bitnami.com/)
- [Bitnami Containers GitHub Repository](https://github.com/bitnami/containers)
- [Bitnami Charts GitHub Repository](https://github.com/bitnami/charts)
- [Bitnami Legacy Docker Hub Repository](https://hub.docker.com/u/bitnamilegacy)
- [Bitnami Secure Images](https://bitnami.com/secure)

## Credits (optional)

Not specified in the source document.

## Contact

MOSIP is ready to support partners in evaluating alternatives, testing migrations, and updating deployment templates/charts.

Please contact the MOSIP technical team for assistance, share your migration plan, and raise any blockers as early as possible.

For further assistance, contact the MOSIP Community at https://community.mosip.io.

For security matters, use the private reporting channel listed in [security/README.md](../security/README.md).

## Revision history

| Date | Version | Change |
|------|---------|--------|
| `2025-09-19` | 1.0 | Initial publication |
