# nextcloud-podman-quadlets
nextcloud for podman, based on https://github.com/nextcloud/docker

> [!NOTE]
> If you encounter any issues relating to this installation method, please raise an issue on this repository rather than bothering the official Nextcloud team.

## Installation steps

1. Either clone or manually write the quadlet files to `/etc/containers/systemd/nextcloud`:

> For rootless Podman setups, clone the repository to `~/.config/containers/systemd/nextcloud` instead.

   ```shell
   git clone https://github.com/linux-universe/nextcloud-podman-quadlets.git /etc/containers/systemd/nextcloud
   ```

2. Make sure to pass in values for `MYSQL_ROOT_PASSWORD` and `MYSQL_PASSWORD` variables before you run this setup

3. Reload systemd:

   ```shell
   systemctl daemon-reload
   ```

4. Then start the nextcloud pod:

   ```shell
   systemctl start nextcloud-pod
   ```

> Run steps 3 and 4 with the `--user` argument for rootless setups.

## Accessing the Nextcloud command-line interface (occ)

```console
$ podman exec -it --user www-data systemd-nextcloud php occ
```

## Reverse Proxy

It is highly recommended that you run this setup with a reverse proxy. To use it, uncomment the reverse proxy-related lines in `nextcloud.container`.

Make sure you configure the necessary redirects for service discovery: https://docs.nextcloud.com/server/stable/admin_manual/configuration_server/reverse_proxy_configuration.html#service-discovery.

It is also recommended that you configure HSTS. Please check the documentation for your reverse proxy for further information.

caddy example:
```Caddyfile
https://example.xyz:443 {
# HSTS
	header {
		Strict-Transport-Security max-age=31536000;
	}

# redirects
	redir /.well-known/carddav /remote.php/dav/ 301
	redir /.well-known/caldav /remote.php/dav/ 301

# reverse proxy
	reverse_proxy localhost:8080
}
```

## Install and configure the Nextcloud Talk High-performance backend

Follow the steps in the guide at https://nextcloud-talk.readthedocs.io/en/latest/quick-install/#installation-steps. For Step 2, however, follow these instructions instead:

1. Uncomment the additional exposed ports in `nextcloud.pod`

2. Create a new file named `nextcloud-talk-hpb.container`:
```systemd
[Container]
AutoUpdate=registry
Pod=nextcloud.pod
Image=ghcr.io/nextcloud-releases/aio-talk:latest
Environment=NC_DOMAIN=<your_domain>
Environment=TALK_HOST=<your_signaling_domain>
Environment=TALK_PORT=3478
Environment=TURN_SECRET=<your_turn_secret>
Environment=SIGNALING_SECRET=<your_signaling_secret>
Environment=INTERNAL_SECRET=<your_internal_secret>

[Service]
TimeoutStartSec=900
```

3. Restart your Pod with `systemctl daemon-reload && systemctl restart nextcloud-pod`
