# nextcloud-podman-quadlets
nextcloud for podman, based on https://github.com/nextcloud/docker

> [!WARNING]
> IN PRODUCTION

## Steps

1. Either clone or manually write the quadlet files to `/etc/containers/systemd/nextcloud`:

   ```shell
   git clone https://github.com/linux-universe/nextcloud-podman-quadlets.git /etc/containers/systemd/nextcloud
   ```

> [!NOTE]
> For rootless Podman setups, clone the repository to `~/.config/containers/systemd/nextcloud` instead.

2. Make sure to pass in values for MYSQL_ROOT_PASSWORD and MYSQL_PASSWORD variables before you run this setup

3. Reload systemd:

   ```shell
   systemctl daemon-reload
   ```

4. Then start the nextcloud pod:

   ```shell
   systemctl start nextcloud-pod
   ```
> [!NOTE]
> Run steps 3 and 4 with the `--user` argument for rootless setups.
