# SillyBunny Unofficial Docker Builds!

> Heads-up: this is a vibe-coded repository. I used GPT-5.6 Terra Light, and while I cannot promise everything is perfect, the container works for me 👍

SillyBunny has Docker support upstream, but building an image yourself is one more thing to do before you can start chatting. And if you are on a Raspberry Pi or another ARM machine… well, you can probably see where this is going.

This repository builds unofficial, ready-to-pull SillyBunny images for both regular 64-bit PCs and ARM64 devices. Add one to your Docker Compose file and let Docker do the rest.

> This project is not affiliated with, owned by, or maintained by the SillyBunny project. The images are built from [SillyBunnyTeam/SillyBunny](https://github.com/SillyBunnyTeam/SillyBunny).

## How to use

Create a `compose.yml` like this:

```yaml
services:
  sillybunny:
    container_name: sillybunny
    hostname: sillybunny
    image: ghcr.io/randomboixd/sillybunny-unofficial-builds:Stable
    environment:
      - NODE_ENV=production
      - FORCE_COLOR=1
      # Uncomment on memory-capped hosts to trade throughput for aggressive GC.
      # - SILLYBUNNY_BUN_SMOL=1
    ports:
      - "4444:4444"
    volumes:
      - "./config:/home/bun/app/config"
      - "./data:/home/bun/app/data"
      - "./plugins:/home/bun/app/plugins"
      - "./extensions:/home/bun/app/public/scripts/extensions/third-party"
    restart: unless-stopped
```

Then run:

```sh
docker compose up -d
```

Open <http://localhost:4444> when the container is running.

### Stable or Staging?

- `Stable` follows SillyBunny's upstream `main` branch. Use this unless you specifically want newer, untested changes.
- `Staging` follows upstream `staging`, which updates more often and may break between releases.

Change the final part of the `image:` value to choose one:

```text
ghcr.io/randomboixd/sillybunny-unofficial-builds:Stable
ghcr.io/randomboixd/sillybunny-unofficial-builds:Staging
```

## A few useful details

The images support both `linux/amd64` (most desktops and servers) and `linux/arm64` (including current Raspberry Pi OS 64-bit installs). Docker automatically pulls the right one.

On SELinux-based systems, bind mounts may need the `:z` suffix, for example `./data:/home/bun/app/data:z`.

If the GHCR package is private, authenticate once with a GitHub personal access token that has `read:packages` permission before your first pull:

```sh
echo "$CR_PAT" | docker login ghcr.io -u randomboixd --password-stdin
```

`Stable` and `Staging` are moving tags. Every workflow run also records its immutable OCI digest in the GitHub Actions job summary. To pin a deployment exactly, use the digest reference from that summary:

```text
ghcr.io/randomboixd/sillybunny-unofficial-builds@sha256:<digest>
```

Each image is labelled with the upstream branch and exact SillyBunny commit used to make it.

## Licensing and source

The original content in this repository—the README and GitHub Actions workflow—is licensed under the [MIT License](LICENSE).

SillyBunny itself is licensed under the [GNU AGPL-3.0](https://github.com/SillyBunnyTeam/SillyBunny/blob/main/LICENSE). These images contain SillyBunny and do **not** relicense it as MIT. If you redistribute an image or modify its upstream source, you are responsible for complying with the AGPL-3.0, including providing the corresponding source. The image labels and workflow summary identify the exact upstream commit used for each build.
