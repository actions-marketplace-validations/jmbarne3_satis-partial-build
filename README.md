# Satis - Partial Build Action
Used for partially building your own composer repository using [composer/satis](https://github.com/composer/satis).

## Inputs

| Input        | Description                                                                                             | Required                     |
|--------------|---------------------------------------------------------------------------------------------------------|------------------------------|
| token        | GitHub token that will be used for composer when invoked by satis.<br/>Currently auth is static for "github-oauth" | Yes                          |
| package      | The name of the package to rebuild | Yes |

## Outputs
None

## Usage

Example `.github/workflow/satis-partial-build.yaml`
```yaml
# eg. Can be used to host your internal composer package registry on github pages.
name: Satis Partial Build

on:
  - workflow_dispatch

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: UCF/satis-partial-build@v1
        with:
          token: ${{ GITHUB_TOKEN }} # App/OAuth token, PAT
          package: ${{ intputs.package }}
      - env:
          GIT_EMAIL: bot@github.com
          GIT_NAME: cool-bot
        run: |
          git config user.name $GIT_NAME
          git config user.email $GIT_EMAIL
          git add docs/
          git commit -m "Re-built repository assets"
          git push
```

Example `satis.json`
```json
{
  "name": "my-cool/repository",
  "homepage": "https://where-are-you/",
  "repositories": [
    {
      "packagist.org": false
    },
    {
      "type": "vcs",
      "url": "https://github.com/my-cool-org/the-repository"
    }
  ],
  "require-all": true,
  "output-dir": "docs"
}
```
