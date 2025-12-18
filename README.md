# nextcloud-podman-quadlets
nextcloud for podman, based on https://github.com/nextcloud/docker

> [!NOTE]
> If you encounter any issues relating to this installation method, please raise an issue on this repository rather than bothering the official Nextcloud team.

## Installation steps

1. Either clone or manually write the quadlet files to `/etc/containers/systemd/nextcloud`:

   ```shell
   git clone https://github.com/linux-universe/nextcloud-podman-quadlets.git /etc/containers/systemd/nextcloud
   ```

> [!NOTE]
> For rootless Podman setups, clone the repository to `~/.config/containers/systemd/nextcloud` instead.

2. Make sure to pass in values for `MYSQL_ROOT_PASSWORD` and `MYSQL_PASSWORD` variables before you run this setup

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

## Accessing the Nextcloud command-line interface (occ)

```console
$ podman exec -it --user www-data systemd-nextcloud php occ
```
