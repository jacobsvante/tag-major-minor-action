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
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com

          git tag -d v${{ steps.release.outputs.major }} || true
          git tag -d v${{ steps.release.outputs.major }}.${{ steps.release.outputs.minor }} || true

          git push origin :v${{ steps.release.outputs.major }} || true
          git push origin :v${{ steps.release.outputs.major }}.${{ steps.release.outputs.minor }} || true

          git tag -a v${{ steps.release.outputs.major }} -m "Release v${{ steps.release.outputs.major }}"
          git tag -a v${{ steps.release.outputs.major }}.${{ steps.release.outputs.minor }} -m "Release v${{ steps.release.outputs.major }}.${{ steps.release.outputs.minor }}"

          git push origin v${{ steps.release.outputs.major }}
          git push origin v${{ steps.release.outputs.major }}.${{ steps.release.outputs.minor }}
```
