# LobeChat @ Bellroy

The repository has been configured to include Nix setup for development and deployment. The current code on the `main` branch is the one that is deployed
onto our staging and production servers.

Avoid modifying the source directly. This will make future merging in of the upstream repository more difficult. Try to make any modifications in files that are unlikely to clash with upstream changes.

## Running locally

```
$ pnpm install
$ pnpm dev
```
