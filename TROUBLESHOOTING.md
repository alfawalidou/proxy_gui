# Troubleshooting

This checklist covers common deployment problems for this repository.

## GitHub Actions does not start or fails early

1. Open the repository **Actions** tab and inspect the failed workflow step.
2. Confirm that the required repository secrets exist:
   - `PUBLIC_IP`
   - `SSH_PRIVATE_KEY`
   - `HUSARNET_JOINCODE`
3. Make sure the SSH private key stored in `SSH_PRIVATE_KEY` matches the public key installed in `/root/.ssh/authorized_keys` on the target server.

## SSH connection fails

- Confirm the server is reachable on its public IP.
- Verify TCP port 22 is accessible from the runner/network path.
- Confirm the configured SSH key has no formatting damage after being copied into GitHub Secrets.
- Check `/root/.ssh/authorized_keys` on the target host.

## Husarnet does not join

- Verify that `HUSARNET_JOINCODE` is current and copied exactly.
- Review the workflow and Ansible logs for the first failing Husarnet task.
- Regenerate the join code if the existing code has been revoked or expired.

## Docker Compose problems

On the target host, validate the Compose file before restarting services:

```bash
docker compose config
docker compose ps
docker compose logs --tail=100
```

Use the service-specific logs to identify configuration, networking, or startup errors before redeploying.

## Backup or restore problems

Use the repository **Actions** tab to run the `Save Backup` or `Restore Backup` workflow manually. Check that `.backup.tgz` exists when restoring and inspect the workflow logs if the archive cannot be read or written.
