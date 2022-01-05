GitHub action for create git tags for the specified major and minor version identifiers

Example, used together with the excellent [release-please-action](https://github.com/google-github-actions/release-please-action):

```yml
on:
    push:
        branches:
            - main

name: Main
jobs:
    release-please:
        runs-on: ubuntu-latest
        steps:
            - uses: google-github-actions/release-please-action@v3
              id: release
              with:
                  release-type: simple
                  package-name: my-package
                  bump-minor-pre-major: true
            - uses: actions/checkout@v2
            - name: Tag major and minor versions
              uses: jacobsvante/tag-major-minor-action
              if: ${{ steps.release.outputs.release_created }}
              with:
                  major: steps.release.outputs.major
                  minor: steps.release.outputs.minor
```
