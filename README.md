# Unofficial SillyBunny Builds

This is a small, unofficial build repository for [SillyBunny](https://github.com/SillyBunnyTeam/SillyBunny). It is not affiliated with, owned by, or maintained by the SillyBunny project.

GitHub Actions builds the upstream repository's Dockerfile for both supported architectures:

- `Stable` follows upstream `main`.
- `Staging` follows upstream `staging` and may be less stable.

Images are published privately to GitHub Container Registry as:

```text
ghcr.io/<owner>/sillybunny-unofficial-builds:Stable
ghcr.io/<owner>/sillybunny-unofficial-builds:Staging
```

Replace `<owner>` with the owner of this repository. Because the package is private, authenticate with a GitHub personal access token that has `read:packages` permission before pulling:

```sh
echo "$CR_PAT" | docker login ghcr.io -u <github-username> --password-stdin
docker pull ghcr.io/<owner>/sillybunny-unofficial-builds:Stable
```

Each workflow run writes the immutable image digest to its job summary. Pin a deployment to that exact image with:

```text
ghcr.io/<owner>/sillybunny-unofficial-builds@sha256:<digest>
```

The `Stable` and `Staging` tags move as upstream changes; digest references do not.
