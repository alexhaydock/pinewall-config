# pinewall-image

This repository houses the config package builder for my [Pinewall](https://github.com/alexhaydock/pinewall/) project. It's designed to be included in that project as a package.

This repo is built and published as a GitHub Pages site which is a fully-featured Alpine Linux package mirror.

Repo URL for use in `apko` build manifests or Alpine repo configs:
```text
https://alexhaydock.github.io/pinewall-config/
```

See the main Pinewall repo for build instructions and full documentation.

### GitHub Pages flow
When replicating this build-and-publish flow, don't forget to set the GitHub Pages settings for the repository to "Deploy from a branch" and select the `gh-pages` branch.

### Generating SOPS-protected keys
```sh
# Generate melange keys
melange keygen

# Generate age keys (we will use this to encrypt the melange key)
age-keygen -o ~/age-pinewall.key

# Use SOPS to encrypt a file using the newly-generated age pubkey as the "target"
sops --encrypt --age age1rdlvy0qy956mnuhg4ayfuvuc8w3k3wjpwxhyfskgynqmzayzmqrq68w3n5 --output melange.rsa.age melange.rsa

# Remove the unencrypted melange.rsa keyfile
rm -v melange.rsa

# Now, we can find the `AGE-SECRET-KEY-` string from the age keyfile
# and put it in the `SOPS_AGE_KEY` repo secret in GitHub
cat ~/age-pinewall.key
```
