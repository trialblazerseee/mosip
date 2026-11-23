# MOSIP-SEC-SEV1-2026-001: Critical Vulnerability in MOSIP Pre-Registration 1.1.5.x/1.2.0.x

| Field | Value |
|-------|-------|
| **Advisory ID** | `MOSIP-SEC-SEV1-2026-001` |
| **Type** | Security |
| **Severity** | SEV1 – Critical |
| **CVE ID** (security only) |  |
| **CVSS score** (security only) |  |
| **Status** | `Draft` |
| **Published** | |
| **Last updated** | `` |

## Summary

A critical security vulnerability has been identified in the MOSIP Pre-Registration module affecting deployments that use the default JWT signing secret. Under certain circumstances, an attacker may be able to forge authentication tokens and gain unauthorized access to Pre-Registration resources.

MOSIP has developed a permanent fix, which will be included in an upcoming Pre-Registration release. All adopters are strongly advised to apply the recommended mitigation measures immediately and upgrade to the patched release as soon as it becomes available.

## Affected components

| Module / repository | Affected versions | Fixed versions |
|---------------------|-------------------|----------------|
| MOSIP Pre-Registration | `1.1.5.x` and `1.2.0.x` | Upcoming Pre-Registration release; version not provided in source document |
| `commons`, `mosip-infra` and `mosip-config` (V3 installation patch) | Not specified in source document | Upcoming patch release; versions not provided in source document |

This advisory applies to Pre-Registration deployments that are configured with the default JWT signing secret.

## Description

A critical security vulnerability has been identified in the MOSIP Pre-Registration module affecting deployments that use the default JWT signing secret. Under certain circumstances, an attacker may be able to forge authentication tokens and gain unauthorized access to Pre-Registration resources.

Both MOSIP 1.1.5.x and 1.2.0.x versions are affected.

## Impact

Successful exploitation of this vulnerability could allow unauthorized access to Pre-Registration data and operations, including:

- Viewing applicant demographic information.
- Accessing uploaded document metadata.
- Modifying application details.
- Changing application status.
- Deleting Pre-Registration applications.

## Resolution

MOSIP has implemented the below-mentioned fix as a permanent fix that addresses the underlying authentication weakness. The fix will be delivered in an upcoming Pre-Registration release.

Once the release is announced, all adopters are strongly encouraged to continue using deployment-specific secrets instead of default values.

### Immediate mitigation

As part of the mitigation measures, we are sharing the fix implemented in the V3 installation, which will be included in an upcoming patch release for commons, mosip-infra and mosip-config. Countries currently using the V3 installation are requested to apply the same fix at the earliest opportunity to mitigate the issue and plan to upgrade to the official patch release when it is published.

### For V3 installations

1. Generate a new random secret that is 32 bytes in length and Base64 encoded using the following command:

   ```bash
   openssl rand -base64 32
   ```

2. Add the generated secret to the `conf-secrets-various` secret in both the `config-server` and `conf-secrets` namespaces. Ensure the key-value pair is configured as follows:

   ```yaml
   prereg-auth-jwt-secret: <generated Base64-encoded secret>
   ```

3. Update the following property in `mosip-config/pre-registration-default.properties`:

   ```properties
   prereg.auth.jwt.secret=${prereg.auth.jwt.secret}
   ```

4. Update the `config-server` `deployment.yaml` by adding the following environment variable:

   ```yaml
   - name: SPRING_CLOUD_CONFIG_SERVER_OVERRIDES_PREREG_AUTH_JWT_SECRET
     valueFrom:
       secretKeyRef:
         key: prereg-auth-jwt-secret
         name: conf-secrets-various
         optional: false
   ```

5. Once the deployment is updated, the `config-server` will restart automatically.

6. After the `config-server` restart is complete, restart all Pre-Registration services to ensure the new secret is picked up by all components.

7. Upon successful restart of all services, perform sanity testing to verify that the Pre-Registration module is functioning as expected.

### For V2 installations

1. Generate a new cryptographically secure secret consisting of 32 random bytes encoded in Base64 using the following command:

   ```bash
   openssl rand -base64 32
   ```

2. Refer to the [encryption guide](https://github.com/mosip/mosip-infra/blob/v1.1.5.5-P1/deployment/sandbox-v2/utils/secrets/README.md) and encrypt the generated secret before updating it in the `mosip-config`.

3. Update the encrypted value for the following property in `mosip-config/pre-registration-default.properties`:

   ```properties
   prereg.auth.jwt.secret=<encrypted secret>
   ```

4. Restart all Pre-Registration services to apply the updated configuration.

5. Once the services are up and running, perform validation and sanity testing to ensure all Pre-Registration functionalities are working as expected.

**Important:** As there is currently no planned patch release for V2 deployments, all V2 adopters should implement the above configuration change as soon as possible.

## Workarounds / mitigations

- Replace the default JWT signing secret.
- Restart affected Pre-Registration services.
- Review logs for suspicious activity.
- Validate that no environment is using the default secret.
- Upgrade to the upcoming patched Pre-Registration release once available.

## Verification (optional)

- After the `config-server` restart is complete, restart all Pre-Registration services to ensure the new secret is picked up by all components (V3 installations).
- Upon successful restart of all services, perform sanity testing to verify that the Pre-Registration module is functioning as expected (V3 installations).
- Once the services are up and running, perform validation and sanity testing to ensure all Pre-Registration functionalities are working as expected (V2 installations).

## References

- [Encryption guide](https://github.com/mosip/mosip-infra/blob/v1.1.5.5-P1/deployment/sandbox-v2/utils/secrets/README.md).

## Contact

If you require assistance with implementing the mitigation or planning the upgrade, please reach out through the MOSIP support and community channels.

Additional guidance and release information will be shared as part of the forthcoming Pre-Registration release announcement.

## Revision history

| Date | Version | Change |
|------|---------|--------|
| `Not provided in source document` | 1.0 | Initial publication |
