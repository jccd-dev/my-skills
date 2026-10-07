---
name: vps-admin-deploy-access
description: Set up or review separate SSH access for human VPS administrators and CI deployment users. Use when provisioning a VPS or hardening deployment access; not for general application deployment.
---

# VPS Admin and CI Deploy Access

Create a durable separation between a human administrator and CI deployment
automation without risking loss of server access.

## Access model

Use distinct identities and distinct Ed25519 key pairs:

| Identity | Purpose | Privilege |
| --- | --- | --- |
| Administrator | Human maintenance and recovery | `sudo` only when required |
| Deploy user | CI release automation | No `sudo`; only the permissions needed to release the app |

Do not reuse the administrator private key in CI. Keep the deploy user's
password disabled and authenticate it only with its dedicated public key.

## Safe setup sequence

1. Confirm a provider-console or equivalent out-of-band recovery path exists.
2. Verify the VPS's Ed25519 host-key fingerprint out of band before accepting
   it locally. Do not trust `ssh-keyscan` by itself.
3. Create the administrator account and install its public key with `.ssh` mode
   `700` and `authorized_keys` mode `600`.
4. Create the deploy account without an interactive password. Install only the
   CI public key with the same SSH file permissions.
5. Open separate fresh SSH sessions and prove both accounts work before
   changing `sshd` settings.
6. Only after those tests pass, disable root and password authentication. Run
   `sshd -t` before reloading SSH, and keep one known-good session open until a
   new administrator login succeeds.

If an existing access path is uncertain, stop before changing SSH
authentication settings and ask the operator to confirm their recovery method.

## Deployment-user boundaries

- Do not add the deploy user to `sudoers`.
- Give the deploy user ownership only of the application release directory and
  its SSH files.
- Treat Docker group membership as root-equivalent. Grant it only when Docker
  Compose or Docker CLI access is genuinely required for releases, and document
  that tradeoff.
- Use a dedicated CI key and pin the host key in CI `known_hosts`; never disable
  host-key checking to make deployments work.
- Keep CI private keys and known-hosts data in the CI secret store, not in the
  repository or app environment file.

## Review checklist

Before declaring setup complete, verify:

- Root login and password authentication are disabled only after both accounts
  have been tested.
- The human admin key and CI deploy key are different.
- `deploy` has no `sudo` access.
- SSH files are owned by the target user and are not group/world-readable.
- Firewall rules expose only the intended SSH and application proxy ports.
- The deploy key can be revoked independently without locking out the
  administrator.

This skill guides access design and safe verification. It does not authorize
creating users, editing live SSH configuration, changing firewall rules, or
deploying an application without the user's explicit approval.
